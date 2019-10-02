---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704061"
---
# <a name="unsafe-code"></a>Código no seguro

El lenguaje C# principal, tal como se define en los capítulos anteriores, difiere principalmente de C++ C y en su omisión de punteros como un tipo de datos. En su lugar C# , proporciona referencias y la capacidad de crear objetos administrados por un recolector de elementos no utilizados. Este diseño, junto con otras características, proporciona C# un lenguaje mucho más seguro que C C++o. En el lenguaje C# principal, simplemente no es posible tener una variable no inicializada, un puntero "pendiente" o una expresión que indexa una matriz más allá de sus límites. Por lo tanto, se eliminan todas las categorías C++ de errores que normalmente plagan C y programas.

Aunque prácticamente todas las construcciones de tipo de puntero C++ de C o tienen un tipo C#de referencia homólogo en, sin embargo, hay situaciones en las que el acceso a los tipos de puntero se convierte en una necesidad. Por ejemplo, la interacción con el sistema operativo subyacente, el acceso a un dispositivo asignado a la memoria o la implementación de un algoritmo crítico en el tiempo puede no ser posible ni práctico sin acceso a los punteros. Para satisfacer esta necesidad, C# proporciona la capacidad de escribir ***código no seguro***.

En código no seguro es posible declarar y operar en punteros, para realizar conversiones entre punteros y tipos enteros, para tomar la dirección de las variables, etc. En cierto sentido, escribir código no seguro es muy similar a escribir código de C C# dentro de un programa.

En realidad, el código no seguro es una característica "segura" desde la perspectiva de los desarrolladores y los usuarios. El código no seguro debe estar claramente marcado con el modificador `unsafe`, por lo que los desarrolladores no pueden usar las características no seguras accidentalmente, y el motor de ejecución funciona para asegurarse de que el código no seguro no se puede ejecutar en un entorno que no es de confianza.

## <a name="unsafe-contexts"></a>Contextos no seguros

Las características no seguras de C# solo están disponibles en contextos no seguros. Un contexto no seguro se introduce mediante la inclusión de un modificador `unsafe` en la declaración de un tipo o miembro, o mediante la utilización de un *unsafe_statement*:

*  Una declaración de una clase, un struct, una interfaz o un delegado puede incluir un modificador `unsafe`, en cuyo caso toda la extensión textual de esa declaración de tipos (incluido el cuerpo de la clase, el struct o la interfaz) se considera un contexto no seguro.
*  Una declaración de un campo, un método, una propiedad, un evento, un indexador, un operador, un constructor de instancia, un destructor o un constructor estático puede incluir un modificador `unsafe`, en cuyo caso toda la extensión textual de dicha declaración de miembro se considera un contexto no seguro.
*  Un *unsafe_statement* habilita el uso de un contexto no seguro dentro de un *bloque*. Toda la extensión textual del *bloque* asociado se considera un contexto no seguro.

A continuación se muestran las producciones de gramática asociadas.

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

el modificador `unsafe` especificado en la declaración de estructura hace que toda la extensión textual de la declaración del struct se convierta en un contexto no seguro. Por lo tanto, es posible declarar los campos `Left` y `Right` para que sean de un tipo de puntero. También se puede escribir el ejemplo anterior.

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Aquí, los modificadores `unsafe` en las declaraciones de campo hacen que esas declaraciones se consideren contextos no seguros.

Además de establecer un contexto no seguro, lo que permite el uso de tipos de puntero, el modificador `unsafe` no tiene ningún efecto en un tipo o miembro. En el ejemplo

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

el modificador `unsafe` en el método `F` de `A` hace que la extensión textual de `F` se convierta en un contexto no seguro en el que se pueden usar las características no seguras del lenguaje. En la invalidación de `F` en `B`, no hay necesidad de volver a especificar el modificador `unsafe`, a menos que, por supuesto, el método `F` en `B` necesite tener acceso a características no seguras.

La situación es ligeramente diferente cuando un tipo de puntero forma parte de la Signatura del método.

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

Aquí, dado que la firma de `F` incluye un tipo de puntero, solo se puede escribir en un contexto no seguro. Sin embargo, el contexto no seguro se puede introducir haciendo que la clase completa sea no segura, como sucede en `A`, o incluyendo un modificador `unsafe` en la declaración del método, como es el caso de `B`.

## <a name="pointer-types"></a>Tipos de puntero

En un contexto no seguro, un *tipo* ([tipos](types.md)) puede ser un *pointer_type* , así como un *value_type* o un *reference_type*. Sin embargo, un *pointer_type* también se puede usar en una expresión `typeof` ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) fuera de un contexto no seguro, ya que dicho uso no es seguro.

```antlr
type_unsafe
    : pointer_type
    ;
```

Un *pointer_type* se escribe como una *unmanaged_type* o la palabra clave `void`, seguida de un token `*`:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

El tipo especificado antes del `*` en un tipo de puntero se denomina ***tipo referente*** del tipo de puntero. Representa el tipo de la variable a la que apunta un valor del tipo de puntero.

A diferencia de las referencias (valores de tipos de referencia), el recolector de elementos no utilizados no realiza un seguimiento de los punteros; el recolector de elementos no utilizados no tiene conocimiento de los punteros y los datos a los que apuntan. Por esta razón, no se permite que un puntero señale a una referencia o a un struct que contenga referencias, y el tipo referente de un puntero debe ser un *unmanaged_type*.

Un *unmanaged_type* es cualquier tipo que no sea un tipo *reference_type* o construido y que no contenga campos de tipo *reference_type* o construido en ningún nivel de anidamiento. En otras palabras, un *unmanaged_type* es uno de los siguientes:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 o 2.
*  Cualquier *enum_type*.
*  Cualquier *pointer_type*.
*  Cualquier *struct_type* definido por el usuario que no sea un tipo construido y solo contenga campos de *unmanaged_type*s.

La regla intuitiva para la combinación de punteros y referencias es que los referentes de referencias (objetos) pueden contener punteros, pero los referentes de punteros no pueden contener referencias.

En la tabla siguiente se proporcionan algunos ejemplos de tipos de puntero:

| __Ejemplo__ | __Descripción__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Puntero a `byte`                             |
| `char*`     | Puntero a `char`                             |
| `int**`     | Puntero al puntero a `int`                   |
| `int*[]`    | Matriz unidimensional de punteros a `int` |
| `void*`     | Puntero a un tipo desconocido                       |

Para una implementación determinada, todos los tipos de puntero deben tener el mismo tamaño y representación.

A diferencia de C C++y, cuando se declaran varios punteros en la misma C# declaración, en el `*` se escribe junto con el tipo subyacente, no como un signo de puntuación de prefijo en cada nombre de puntero. Por ejemplo

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

El valor de un puntero que tiene el tipo `T*` representa la dirección de una variable de tipo `T`. El operador de direccionamiento indirecto de puntero `*` ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) se puede usar para tener acceso a esta variable. Por ejemplo, dada una variable `P` de tipo `int*`, la expresión `*P` denota la variable `int` que se encuentra en la dirección contenida en `P`.

Al igual que una referencia de objeto, un puntero puede ser `null`. Si se aplica el operador de direccionamiento indirecto a un puntero `null`, se producirá un comportamiento definido por la implementación. Un puntero con valor `null` se representa mediante All-bits-cero.

El tipo `void*` representa un puntero a un tipo desconocido. Dado que el tipo referente es desconocido, el operador de direccionamiento indirecto no se puede aplicar a un puntero de tipo `void*`, ni se puede realizar ninguna operación aritmética en dicho puntero. Sin embargo, un puntero de tipo `void*` se puede convertir a cualquier otro tipo de puntero (y viceversa).

Los tipos de puntero son una categoría independiente de tipos. A diferencia de los tipos de referencia y los tipos de valor, los tipos de puntero no heredan de `object` y no existen conversiones entre los tipos de puntero y `object`. En concreto, no se admiten las conversiones boxing y unboxing ([conversión boxing y unboxing](types.md#boxing-and-unboxing)) para los punteros. Sin embargo, se permiten conversiones entre diferentes tipos de puntero y entre tipos de puntero y tipos enteros. Esto se describe en [conversiones de puntero](unsafe-code.md#pointer-conversions).

Un *pointer_type* no se puede usar como un argumento de tipo ([tipos construidos](types.md#constructed-types)) y se produce un error en la inferencia de tipos ([inferencia de tipos](expressions.md#type-inference)) en las llamadas de método genérico que habrían deducido un argumento de tipo para que sea un tipo de puntero.

Un *pointer_type* se puede usar como el tipo de un campo volátil ([campos volátiles](classes.md#volatile-fields)).

Aunque los punteros se pueden pasar como parámetros `ref` o `out`, hacerlo puede provocar un comportamiento indefinido, ya que el puntero se puede establecer para que apunte a una variable local que ya no existe cuando el método llamado devuelve o al objeto fijo al que se ha utilizado para apuntar , ya no se ha corregido. Por ejemplo:

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

Un método puede devolver un valor de algún tipo y ese tipo puede ser un puntero. Por ejemplo, cuando se proporciona un puntero a una secuencia contigua de `int`s, el recuento de elementos de esa secuencia y otros valores `int`, el método siguiente devuelve la dirección de ese valor en esa secuencia, si se produce una coincidencia; en caso contrario, devuelve `null`:

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

En un contexto no seguro, hay varias construcciones disponibles para trabajar en punteros:

*  El operador `*` se puede usar para realizar la direccionamiento indirecto del puntero ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)).
*  El operador `->` se puede usar para tener acceso a un miembro de un struct a través de un puntero ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)).
*  El operador `[]` se puede usar para indizar un puntero ([acceso al elemento de puntero](unsafe-code.md#pointer-element-access)).
*  El operador `&` se puede usar para obtener la dirección de una variable ([el operador Address-of](unsafe-code.md#the-address-of-operator)).
*  Los operadores `++` y `--` se pueden usar para aumentar y disminuir punteros ([incremento y decremento del puntero](unsafe-code.md#pointer-increment-and-decrement)).
*  Los operadores `+` y `-` se pueden usar para realizar aritmética de punteros ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).
*  Los operadores `==`, `!=`, `<`, `>`, `<=` y `=>` se pueden usar para comparar los punteros ([comparación de punteros](unsafe-code.md#pointer-comparison)).
*  El operador `stackalloc` se puede usar para asignar memoria de la pila de llamadas ([búferes de tamaño fijo](unsafe-code.md#fixed-size-buffers)).
*  La instrucción `fixed` se puede usar para corregir temporalmente una variable para que se pueda obtener su dirección ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Variables fijas y móviles

El operador Address-of ([el operador Address-of](unsafe-code.md#the-address-of-operator)) y la instrucción `fixed` ([instrucción Fixed](unsafe-code.md#the-fixed-statement)) dividen las variables en dos categorías: ***Variables fijas*** y ***variables móviles***.

Las variables fijas residen en ubicaciones de almacenamiento que no se ven afectadas por el funcionamiento del recolector de elementos no utilizados. (Algunos ejemplos de variables fijas incluyen variables locales, parámetros de valor y variables creadas mediante punteros de desreferencia). Por otro lado, las variables móviles residen en ubicaciones de almacenamiento que están sujetas a reubicación o eliminación por parte del recolector de elementos no utilizados. (Ejemplos de variables móviles incluyen campos en objetos y elementos de matrices).

El operador `&` ([el operador Address-of](unsafe-code.md#the-address-of-operator)) permite obtener la dirección de una variable fija sin restricciones. Sin embargo, dado que una variable móvil está sujeta a la reubicación o eliminación por parte del recolector de elementos no utilizados, la dirección de una variable móvil solo se puede obtener mediante una instrucción `fixed` ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)) y esa dirección solo es válida para el duración de esa instrucción @no__t 2.

En términos precisos, una variable fija es una de las siguientes:

*  Variable resultante de un *simple_name* ([nombres simples](expressions.md#simple-names)) que hace referencia a una variable local o un parámetro de valor, a menos que una función anónima Capture la variable.
*  Variable resultante de un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `V.I`, donde `V` es una variable fija de un *struct_type*.
*  Una variable resultante de un *pointer_indirection_expression* ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) con el formato `*P`, un *pointer_member_access* ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) con el formato `P->I` o un *pointer_element_access* ( [Acceso a elementos de puntero](unsafe-code.md#pointer-element-access)) del formulario `P[E]`.

Todas las demás variables se clasifican como variables móviles.

Tenga en cuenta que un campo estático se clasifica como una variable móvil. Tenga en cuenta también que un parámetro `ref` o `out` se clasifica como una variable móvil, incluso si el argumento especificado para el parámetro es una variable fija. Por último, tenga en cuenta que una variable generada al desreferenciar un puntero siempre se clasifica como una variable fija.

## <a name="pointer-conversions"></a>Conversiones de puntero

En un contexto no seguro, el conjunto de conversiones implícitas disponibles ([conversiones implícitas](conversions.md#implicit-conversions)) se extiende para incluir las siguientes conversiones de puntero implícitas:

*  De cualquier *pointer_type* al tipo `void*`.
*  Del literal `null` a cualquier *pointer_type*.

Además, en un contexto no seguro, el conjunto de conversiones explícitas disponibles ([conversiones explícitas](conversions.md#explicit-conversions)) se extiende para incluir las siguientes conversiones de puntero explícitas:

*  De cualquier *pointer_type* a cualquier otro *pointer_type*.
*  Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong` a cualquier *pointer_type*.
*  De cualquier *pointer_type* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`.

Por último, en un contexto no seguro, el conjunto de conversiones implícitas estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) incluye la siguiente conversión de puntero:

*  De cualquier *pointer_type* al tipo `void*`.

Las conversiones entre dos tipos de puntero nunca cambian el valor del puntero real. En otras palabras, una conversión de un tipo de puntero a otro no tiene ningún efecto en la dirección subyacente proporcionada por el puntero.

Cuando un tipo de puntero se convierte en otro, si el puntero resultante no está alineado correctamente para el tipo señalado, el comportamiento es indefinido si se desreferencia el resultado. En general, el concepto "correctamente alineado" es transitivo: Si un puntero al tipo `A` está alineado correctamente para un puntero al tipo `B`, que, a su vez, está alineado correctamente para un puntero al tipo `C`, un puntero al tipo `A` se alinea correctamente para un puntero al tipo `C`.

Considere el siguiente caso en el que se tiene acceso a una variable con un tipo a través de un puntero a un tipo diferente:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Cuando un tipo de puntero se convierte en un puntero a byte, el resultado apunta al byte más bajo direccionable de la variable. Los incrementos sucesivos del resultado, hasta el tamaño de la variable, proporcionan punteros a los bytes restantes de esa variable. Por ejemplo, el siguiente método muestra cada uno de los ocho bytes en un tipo Double como un valor hexadecimal:

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

Por supuesto, la salida generada depende de modos endian.

Las asignaciones entre punteros y enteros están definidas por la implementación. Sin embargo, en las arquitecturas de CPU de 32 * y 64 bits con un espacio de direcciones lineal, las conversiones de punteros a o desde tipos enteros normalmente se comportan exactamente igual que las conversiones de los valores `uint` o `ulong`, respectivamente, a o desde esos tipos enteros.

### <a name="pointer-arrays"></a>Matrices de puntero

En un contexto no seguro, se pueden construir matrices de punteros. En las matrices de puntero solo se permiten algunas de las conversiones que se aplican a otros tipos de matriz:

*  La conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) de cualquier *array_type* a `System.Array` y las interfaces que implementa también se aplican a las matrices de punteros. Sin embargo, cualquier intento de obtener acceso a los elementos de la matriz a través de `System.Array` o las interfaces que implementa producirá una excepción en tiempo de ejecución, ya que los tipos de puntero no se pueden convertir a `object`.
*  Las conversiones de referencia implícitas y explícitas (conversiones de[referencia implícita](conversions.md#implicit-reference-conversions), [conversiones de referencia explícitas](conversions.md#explicit-reference-conversions)) de un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base genéricas nunca se aplican a las matrices de punteros. Dado que los tipos de puntero no se pueden usar como argumentos de tipo y no hay conversiones de tipos de puntero en tipos que no son de puntero.
*  La conversión de referencia explícita ([conversiones de referencia explícita](conversions.md#explicit-reference-conversions)) de `System.Array` y las interfaces que implementa en cualquier *array_type* se aplican a las matrices de punteros.
*  Las conversiones de referencia explícitas ([conversiones de referencia explícita](conversions.md#explicit-reference-conversions)) de `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]` nunca se aplican a las matrices de puntero, ya que los tipos de puntero no se pueden usar como argumentos de tipo y hay no hay conversiones de tipos de puntero a tipos que no sean de puntero.

Estas restricciones implican que la expansión de la instrucción `foreach` sobre las matrices descritas en [la instrucción foreach](statements.md#the-foreach-statement) no se puede aplicar a las matrices de punteros. En su lugar, una instrucción foreach con el formato

```csharp
foreach (V v in x) embedded_statement
```

donde el tipo de `x` es un tipo de matriz con el formato `T[,,...,]`, @no__t 2 es el número de dimensiones menos 1 y `T` o `V` es un tipo de puntero, se expande mediante bucles for anidados como se indica a continuación:

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

Las variables `a`, `i0`, `i1`,..., `iN` no están visibles o son accesibles para `x` o el *embedded_statement* o cualquier otro código fuente del programa. La variable `v` es de solo lectura en la instrucción insertada. Si no hay una conversión explícita ([conversiones de puntero](unsafe-code.md#pointer-conversions)) de `T` (el tipo de elemento) a `V`, se genera un error y no se realiza ningún paso más. Si `x` tiene el valor `null`, se `System.NullReferenceException` produce una excepción en tiempo de ejecución.

## <a name="pointers-in-expressions"></a>Punteros en expresiones

En un contexto no seguro, una expresión puede producir un resultado de un tipo de puntero, pero fuera de un contexto no seguro, es un error en tiempo de compilación para que una expresión sea de un tipo de puntero. En términos precisos, fuera de un contexto no seguro, se produce un error en tiempo de compilación si *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)), *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)) o  *element_access* ([acceso a elementos](expressions.md#element-access)) es de un tipo de puntero.

En un contexto no seguro, las producciones *primary_no_array_creation_expression* ([expresiones primarias](expressions.md#primary-expressions)) y *unary_expression* ([operadores unarios](expressions.md#unary-operators)) permiten las siguientes construcciones adicionales:

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

Estas construcciones se describen en las secciones siguientes. La gramática implica la prioridad y la asociatividad de los operadores no seguros.

### <a name="pointer-indirection"></a>Direccionamiento indirecto de puntero

Un *pointer_indirection_expression* consta de un asterisco (`*`) seguido de un *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

El operador unario `*` denota el direccionamiento indirecto del puntero y se usa para obtener la variable a la que señala un puntero. El resultado de evaluar `*P`, donde `P` es una expresión de un tipo de puntero `T*`, es una variable de tipo `T`. Es un error en tiempo de compilación aplicar el operador unario `*` a una expresión de tipo `void*` o a una expresión que no es de un tipo de puntero.

El efecto de aplicar el operador unario `*` a un puntero `null` está definido por la implementación. En concreto, no hay ninguna garantía de que esta operación produzca una `System.NullReferenceException`.

Si se ha asignado un valor no válido al puntero, el comportamiento del operador unario `*` es indefinido. Entre los valores no válidos para desreferenciar un puntero por el operador unario `*` hay una dirección alineada incorrectamente para el tipo señalado (vea el ejemplo de [conversiones de puntero](unsafe-code.md#pointer-conversions)) y la dirección de una variable después del final de su período de duración.

A efectos del análisis de asignación definitiva, una variable generada al evaluar una expresión con el formato `*P` se considera asignada inicialmente ([variables asignadas inicialmente](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Acceso a miembros de puntero

Un *pointer_member_access* consta de un *primary_expression*, seguido de un token "`->`", seguido de un *identificador* y un *type_argument_list*opcional.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

En un acceso de miembro de puntero con el formato `P->I`, `P` debe ser una expresión de un tipo de puntero distinto de `void*` y `I` debe indicar un miembro accesible del tipo al que `P` puntos.

Un acceso de miembro de puntero con el formato `P->I` se evalúa exactamente como `(*P).I`. Para obtener una descripción del operador de direccionamiento indirecto de puntero (`*`), vea [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection). Para obtener una descripción del operador de acceso a miembros (`.`), consulte [acceso a miembros](expressions.md#member-access).

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

el operador `->` se utiliza para tener acceso a los campos e invocar un método de un struct a través de un puntero. Dado que la operación `P->I` es precisamente equivalente a `(*P).I`, el método `Main` podría haberse escrito igualmente correctamente:

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

### <a name="pointer-element-access"></a>Acceso a elementos de puntero

Un *pointer_element_access* consta de un *primary_no_array_creation_expression* seguido de una expresión encerrada en "`[`" y "`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

En un elemento de puntero el acceso del formulario `P[E]`, `P` debe ser una expresión de un tipo de puntero distinto de `void*` y `E` debe ser una expresión que se pueda convertir implícitamente a `int`, `uint`, `long` o `ulong`.

Un acceso de elemento de puntero del formulario `P[E]` se evalúa exactamente como `*(P + E)`. Para obtener una descripción del operador de direccionamiento indirecto de puntero (`*`), vea [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection). Para obtener una descripción del operador de adición de puntero (`+`), vea [aritmética de punteros](unsafe-code.md#pointer-arithmetic).

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

un acceso de elemento de puntero se usa para inicializar el búfer de caracteres en un bucle @no__t 0. Dado que la operación `P[E]` es precisamente equivalente a `*(P + E)`, el ejemplo se podría haber escrito igualmente correctamente:

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

El operador de acceso a elementos de puntero no comprueba los errores fuera de los límites y el comportamiento cuando el acceso a un elemento fuera de los límites es indefinido. Es lo mismo que C y C++.

### <a name="the-address-of-operator"></a>Operador Address-of

Un *addressof_expression* consta de un símbolo de y comercial (`&`) seguido de un *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Dada una expresión `E`, que es de un tipo `T` y se clasifica como una variable fija ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)), la construcción `&E` calcula la dirección de la variable proporcionada por `E`. El tipo del resultado es `T*` y se clasifica como un valor. Se produce un error en tiempo de compilación si `E` no está clasificado como una variable, si `E` está clasificado como una variable local de solo lectura, o si `E` denota una variable móvil. En el último caso, se puede usar una instrucción fixed ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)) para "corregir" temporalmente la variable antes de obtener su dirección. Como se indica en [acceso a miembros](expressions.md#member-access), fuera de un constructor de instancia o un constructor estático para una estructura o clase que define un campo `readonly`, ese campo se considera un valor, no una variable. Como tal, no se puede tomar su dirección. Del mismo modo, no se puede tomar la dirección de una constante.

El operador `&` no requiere que el argumento esté asignado definitivamente, pero después de una operación `&`, la variable a la que se aplica el operador se considera definitivamente asignada en la ruta de acceso de ejecución en la que se produce la operación. Es responsabilidad del programador asegurarse de que la inicialización correcta de la variable realmente se realiza en esta situación.

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

`i` se considera definitivamente asignada después de la operación `&i` que se usa para inicializar `p`. La asignación a `*p` en vigor inicializa `i`, pero la inclusión de esta inicialización es responsabilidad del programador y no se produciría ningún error en tiempo de compilación si se quitase la asignación.

Las reglas de asignación definitiva para el operador `&` existen para evitar la inicialización redundante de variables locales. Por ejemplo, muchas API externas toman un puntero a una estructura que se rellena mediante la API. Las llamadas a estas API normalmente pasan la dirección de una variable de struct local y sin la regla, sería necesaria la inicialización redundante de la variable de estructura.

### <a name="pointer-increment-and-decrement"></a>Incremento y decremento de puntero

En un contexto no seguro, los operadores `++` y `--` (operadores de[incremento y decremento de sufijo](expressions.md#postfix-increment-and-decrement-operators) y [operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)) se pueden aplicar a las variables de puntero de todos los tipos excepto `void*`. Por lo tanto, para cada tipo de puntero `T*`, se definen implícitamente los siguientes operadores:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Los operadores producen los mismos resultados que `x + 1` y `x - 1`, respectivamente ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)). En otras palabras, para una variable de puntero de tipo `T*`, el operador `++` agrega `sizeof(T)` a la dirección contenida en la variable y el operador `--` resta `sizeof(T)` de la dirección incluida en la variable.

Si una operación de incremento o decremento de puntero desborda el dominio del tipo de puntero, el resultado se define en la implementación, pero no se genera ninguna excepción.

### <a name="pointer-arithmetic"></a>Aritmética de puntero

En un contexto no seguro, los operadores `+` y `-` (operador de[suma](expressions.md#addition-operator) y [resta](expressions.md#subtraction-operator)) se pueden aplicar a los valores de todos los tipos de puntero excepto `void*`. Por lo tanto, para cada tipo de puntero `T*`, se definen implícitamente los siguientes operadores:

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

Dada una expresión `P` de un tipo de puntero `T*` y una expresión @no__t 2 de tipo `int`, `uint`, `long` o `ulong`, las expresiones `P + N` y `N + P` calculan el valor de puntero de tipo `T*` que es el resultado de agregar 0 a la dirección proporcionado por 1. Del mismo modo, la expresión `P - N` calcula el valor de puntero de tipo `T*` que es el resultado de restar `N * sizeof(T)` de la dirección proporcionada por `P`.

Dadas dos expresiones, `P` y `Q`, de un tipo de puntero `T*`, la expresión `P - Q` calcula la diferencia entre las direcciones proporcionadas por `P` y `Q` y, a continuación, divide esa diferencia por `sizeof(T)`. El tipo del resultado es siempre `long`. En efecto, `P - Q` se calcula como `((long)(P) - (long)(Q)) / sizeof(T)`.

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

que genera el resultado:

```console
p - q = -14
q - p = 14
```

Si una operación aritmética de puntero desborda el dominio del tipo de puntero, el resultado se trunca en un modo definido por la implementación, pero no se genera ninguna excepción.

### <a name="pointer-comparison"></a>Comparación de punteros

En un contexto no seguro, los operadores `==`, `!=`, `<`, `>`, `<=` y `=>` ([operadores relacionales y de prueba de tipos](expressions.md#relational-and-type-testing-operators)) se pueden aplicar a los valores de todos los tipos de puntero. Los operadores de comparación de punteros son:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Dado que existe una conversión implícita de cualquier tipo de puntero al tipo `void*`, se pueden comparar los operandos de cualquier tipo de puntero mediante estos operadores. Los operadores de comparación comparan las direcciones proporcionadas por los dos operandos como si fueran enteros sin signo.

### <a name="the-sizeof-operator"></a>El operador sizeof

El operador `sizeof` devuelve el número de bytes ocupados por una variable de un tipo determinado. El tipo especificado como operando para `sizeof` debe ser un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

El resultado del operador `sizeof` es un valor de tipo `int`. Para determinados tipos predefinidos, el operador `sizeof` produce un valor constante, tal como se muestra en la tabla siguiente.


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

En el caso de todos los demás tipos, el resultado del operador `sizeof` está definido por la implementación y se clasifica como un valor, no como una constante.

No se especifica el orden en que los miembros se empaquetan en un struct.

A efectos de alineación, puede haber un relleno sin nombre al principio de un struct, dentro de un struct y al final de la estructura. El contenido de los bits usados como relleno es indeterminado.

Cuando se aplica a un operando que tiene el tipo de estructura, el resultado es el número total de bytes en una variable de ese tipo, incluido el relleno.

## <a name="the-fixed-statement"></a>Instrucción Fixed

En un contexto no seguro, la producción de *embedded_statement* ([instrucciones](statements.md)) permite una construcción adicional, la instrucción `fixed`, que se usa para "corregir" una variable móvil de modo que su dirección permanece constante mientras dure la instrucción. .

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

Cada *fixed_pointer_declarator* declara una variable local del *pointer_type* determinado e inicializa esa variable local con la dirección calculada por el *fixed_pointer_initializer*correspondiente. Una variable local declarada en una instrucción `fixed` es accesible en cualquier *fixed_pointer_initializer*que se produzca a la derecha de la declaración de la variable y en el *embedded_statement* de la instrucción `fixed`. Una variable local declarada por una instrucción `fixed` se considera de solo lectura. Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar esta variable local (a través de la asignación o los operadores `++` y `--`) o pasarla como un parámetro `ref` o `out`.

Un *fixed_pointer_initializer* puede ser uno de los siguientes:

*  El token "`&`" seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) a una variable móvil ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)) de un tipo no administrado `T`, siempre que el tipo `T*` sea se pueden convertir implícitamente al tipo de puntero proporcionado en la instrucción `fixed`. En este caso, el inicializador calcula la dirección de la variable especificada y se garantiza que la variable permanece en una dirección fija mientras dure la instrucción `fixed`.
*  Una expresión de un *array_type* con elementos de un tipo no administrado `T`, siempre que el tipo `T*` sea implícitamente convertible al tipo de puntero proporcionado en la instrucción `fixed`. En este caso, el inicializador calcula la dirección del primer elemento de la matriz y se garantiza que toda la matriz permanece en una dirección fija mientras dure la instrucción `fixed`. Si la expresión de matriz es null o si la matriz tiene cero elementos, el inicializador calcula una dirección igual a cero.
*  Expresión de tipo `string`, siempre que el tipo `char*` sea implícitamente convertible al tipo de puntero proporcionado en la instrucción `fixed`. En este caso, el inicializador calcula la dirección del primer carácter de la cadena y se garantiza que toda la cadena permanece en una dirección fija mientras dure la instrucción `fixed`. El comportamiento de la instrucción `fixed` está definido por la implementación si la expresión de cadena es NULL.
*  Un *simple_name* o *member_access* que hace referencia a un miembro de búfer de tamaño fijo de una variable móvil, siempre que el tipo del miembro de búfer de tamaño fijo se pueda convertir implícitamente al tipo de puntero proporcionado en la instrucción `fixed`. En este caso, el inicializador calcula un puntero al primer elemento del búfer de tamaño fijo ([búferes de tamaño fijo en expresiones](unsafe-code.md#fixed-size-buffers-in-expressions)) y se garantiza que el búfer de tamaño fijo permanece en una dirección fija mientras dure la instrucción `fixed`.

Para cada dirección calculada por un *fixed_pointer_initializer* , la instrucción `fixed` garantiza que la variable a la que hace referencia la dirección no esté sujeta a reubicación o eliminación por parte del recolector de elementos no utilizados mientras dure la instrucción `fixed`. Por ejemplo, si la dirección calculada por un *fixed_pointer_initializer* hace referencia a un campo de un objeto o un elemento de una instancia de matriz, la instrucción `fixed` garantiza que la instancia del objeto contenedor no se reubique o se elimine durante el duración de la instrucción.

Es responsabilidad del programador garantizar que los punteros creados por @no__t instrucciones-0 no sobreviven más allá de la ejecución de esas instrucciones. Por ejemplo, cuando se pasan punteros creados por @no__t instrucciones-0 a las API externas, es responsabilidad del programador asegurarse de que las API no conservan memoria de estos punteros.

Los objetos fijos pueden provocar la fragmentación del montón (porque no se pueden moverse). Por ese motivo, los objetos deben corregirse solo cuando sea absolutamente necesario y solo para la cantidad de tiempo más corta posible.

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

muestra varios usos de la instrucción `fixed`. La primera instrucción corrige y obtiene la dirección de un campo estático, la segunda instrucción corrige y obtiene la dirección de un campo de instancia, y la tercera instrucción corrige y obtiene la dirección de un elemento de matriz. En cada caso, se habría producido un error al usar el operador `&` normal, ya que todas las variables se clasifican como variables móviles.

La cuarta instrucción `fixed` del ejemplo anterior genera un resultado similar al tercero.

En este ejemplo de la instrucción `fixed` se usa `string`:

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

En un contexto no seguro, los elementos de las matrices unidimensionales se almacenan en un orden de índice creciente, empezando por index `0` y terminando por index `Length - 1`. En el caso de las matrices multidimensionales, los elementos de matriz se almacenan de forma que los índices de la dimensión situada más a la derecha aumenten primero, después la dimensión izquierda siguiente y así sucesivamente hacia la izquierda. Dentro de una instrucción `fixed` que obtiene un puntero `p` a una instancia de matriz `a`, los valores de puntero que van desde `p` hasta `p + a.Length - 1` representan direcciones de los elementos de la matriz. Del mismo modo, las variables que van desde `p[0]` hasta `p[a.Length - 1]` representan los elementos reales de la matriz. Dado el modo en que se almacenan las matrices, podemos tratar una matriz de cualquier dimensión como si fuera lineal.

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

que genera el resultado:

```console
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

una instrucción `fixed` se usa para corregir una matriz, por lo que su dirección se puede pasar a un método que toma un puntero.

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

una instrucción Fixed se usa para corregir un búfer de tamaño fijo de una estructura, por lo que su dirección se puede usar como puntero.

Un valor `char*` generado al corregir una instancia de cadena siempre apunta a una cadena terminada en NULL. Dentro de una instrucción Fixed que obtiene un puntero `p` a una instancia de cadena `s`, los valores de puntero que van desde `p` hasta `p + s.Length - 1` representan direcciones de los caracteres de la cadena y el valor de puntero `p + s.Length` siempre apunta a un carácter nulo (el carácter con valor `'\0'`).

La modificación de objetos de tipo administrado a través de punteros fijos puede tener como resultado un comportamiento indefinido. Por ejemplo, dado que las cadenas son inmutables, es responsabilidad del programador asegurarse de que los caracteres a los que hace referencia un puntero a una cadena fija no se modifican.

La terminación automática de las cadenas es especialmente útil cuando se llama a las API externas que esperan cadenas de "estilo C". Sin embargo, tenga en cuenta que se permite que una instancia de cadena contenga caracteres nulos. Si estos caracteres nulos están presentes, la cadena aparecerá truncada cuando se trate como una `char*` terminada en NULL.

## <a name="fixed-size-buffers"></a>Búferes de tamaño fijo

Los búferes de tamaño fijo se usan para declarar matrices en línea de "estilo C" como miembros de Structs y son principalmente útiles para interactuar con las API no administradas.

### <a name="fixed-size-buffer-declarations"></a>Declaraciones de búfer de tamaño fijo

Un ***búfer de tamaño fijo*** es un miembro que representa el almacenamiento para un búfer de longitud fija de variables de un tipo determinado. Una declaración de búfer de tamaño fijo introduce uno o más búferes de tamaño fijo de un tipo de elemento determinado. Los búferes de tamaño fijo solo se permiten en declaraciones de struct y solo se pueden producir en contextos no seguros ([contextos no seguros](unsafe-code.md#unsafe-contexts)).

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

Una declaración de búfer de tamaño fijo puede incluir un conjunto de atributos ([atributos](attributes.md)), un modificador `new` ([Modificadores](classes.md#modifiers)), una combinación válida de los cuatro modificadores de acceso ([parámetros de tipo y restricciones](classes.md#type-parameters-and-constraints)) y un modificador `unsafe` ([Unsafe contextos](unsafe-code.md#unsafe-contexts)). Los atributos y modificadores se aplican a todos los miembros declarados por la declaración de búfer de tamaño fijo. Es un error que el mismo modificador aparezca varias veces en una declaración de búfer de tamaño fijo.

No se permite una declaración de búfer de tamaño fijo para incluir el modificador `static`.

El tipo de elemento de búfer de una declaración de búfer de tamaño fijo especifica el tipo de elemento de los búferes introducidos por la declaración. El tipo de elemento de búfer debe ser uno de los tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 o 1.

El tipo de elemento de búfer va seguido de una lista de declaradores de búfer de tamaño fijo, cada uno de los cuales incorpora un nuevo miembro. Un declarador de búfer de tamaño fijo consta de un identificador que denomina al miembro, seguido de una expresión constante encerrada en `[` y `]` tokens. La expresión constante denota el número de elementos del miembro introducido por ese declarador de búfer de tamaño fijo. El tipo de la expresión constante se debe poder convertir implícitamente al tipo `int`, y el valor debe ser un entero positivo distinto de cero.

Se garantiza que los elementos de un búfer de tamaño fijo se organizan secuencialmente en la memoria.

Una declaración de búfer de tamaño fijo que declara varios búferes de tamaño fijo es equivalente a varias declaraciones de una única declaración de búfer de tamaño fijo con los mismos atributos y tipos de elemento. Por ejemplo

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

La búsqueda de miembros ([operadores](expressions.md#operators)) de un miembro de búfer de tamaño fijo continúa exactamente como la búsqueda de miembros de un campo.

Se puede hacer referencia a un búfer de tamaño fijo en una expresión usando *simple_name* ([inferencia de tipo](expressions.md#type-inference)) o *member_access* ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Cuando se hace referencia a un miembro de búfer de tamaño fijo como un nombre simple, el efecto es el mismo que un acceso a miembro del formulario `this.I`, donde `I` es el miembro de búfer de tamaño fijo.

En un acceso de miembro con el formato `E.I`, si `E` es de un tipo de struct y una búsqueda de miembro de `I` en ese tipo de estructura identifica un miembro de tamaño fijo, entonces `E.I` se evalúa como se indica a continuación:

*  Si la expresión `E.I` no se produce en un contexto no seguro, se produce un error en tiempo de compilación.
*  Si `E` se clasifica como un valor, se produce un error en tiempo de compilación.
*  De lo contrario, si `E` es una variable móvil ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)) y la expresión `E.I` no es *fixed_pointer_initializer* ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)), se produce un error en tiempo de compilación.
*  De lo contrario, `E` hace referencia a una variable fija y el resultado de la expresión es un puntero al primer elemento del miembro de búfer de tamaño fijo `I` en `E`. El resultado es de tipo `S*`, donde `S` es el tipo de elemento de `I` y se clasifica como un valor.

Se puede tener acceso a los elementos subsiguientes del búfer de tamaño fijo mediante operaciones de puntero desde el primer elemento. A diferencia del acceso a las matrices, el acceso a los elementos de un búfer de tamaño fijo es una operación no segura y no se comprueba el intervalo.

En el ejemplo siguiente se declara y se utiliza un struct con un miembro de búfer de tamaño fijo.

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

Los búferes de tamaño fijo no están sujetos a la comprobación de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)) y los miembros de búfer de tamaño fijo se omiten para la comprobación de asignación definitiva de variables de tipo struct.

Cuando la variable de estructura contenedora más externa de un miembro de búfer de tamaño fijo es una variable estática, una variable de instancia de una instancia de clase o un elemento de matriz, los elementos del búfer de tamaño fijo se inicializan automáticamente con sus valores predeterminados ([valor predeterminado). valores](variables.md#default-values)). En todos los demás casos, el contenido inicial de un búfer de tamaño fijo es indefinido.

## <a name="stack-allocation"></a>Asignación de la pila

En un contexto no seguro, una declaración de variable local ([declaraciones de variables locales](statements.md#local-variable-declarations)) puede incluir un inicializador de asignación de pila que asigna memoria de la pila de llamadas.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* indica el tipo de los elementos que se almacenarán en la ubicación recién asignada y la *expresión* indica el número de estos elementos. En conjunto, estos especifican el tamaño de asignación requerido. Dado que el tamaño de una asignación de pila no puede ser negativo, se trata de un error en tiempo de compilación para especificar el número de elementos como *constant_expression* que se evalúa como un valor negativo.

Un inicializador de asignación de pila con el formato `stackalloc T[E]` requiere que `T` sea un tipo no administrado ([tipos de puntero](unsafe-code.md#pointer-types)) y que `E` sea una expresión de tipo `int`. La construcción asigna `E * sizeof(T)` bytes de la pila de llamadas y devuelve un puntero de tipo `T*` al bloque recién asignado. Si `E` es un valor negativo, el comportamiento es indefinido. Si `E` es cero, no se realiza ninguna asignación y el puntero devuelto está definido por la implementación. Si no hay suficiente memoria disponible para asignar un bloque del tamaño determinado, se produce una `System.StackOverflowException`.

El contenido de la memoria recién asignada está sin definir.

No se permiten inicializadores de asignación de pila en los bloques `catch` o `finally` ([la instrucción try](statements.md#the-try-statement)).

No hay ninguna manera de liberar explícitamente la memoria asignada mediante `stackalloc`. Todos los bloques de memoria asignados a la pila creados durante la ejecución de un miembro de función se descartan automáticamente cuando se devuelve ese miembro de función. Esto corresponde a la función `alloca`, una extensión que se encuentra normalmente en C++ C y las implementaciones.

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

se usa un inicializador `stackalloc` en el método `IntToString` para asignar un búfer de 16 caracteres en la pila. El búfer se descarta automáticamente cuando el método vuelve.

## <a name="dynamic-memory-allocation"></a>Asignación de memoria dinámica

Excepto en el caso del operador @no__t- C# 0, no proporciona construcciones predefinidas para administrar la memoria de recolección de elementos no utilizados. Estos servicios se proporcionan normalmente mediante bibliotecas de clases auxiliares o importados directamente desde el sistema operativo subyacente. Por ejemplo, la clase `Memory` siguiente ilustra cómo se puede tener acceso a las funciones del montón de un sistema operativo subyacente C#desde:

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

A continuación se muestra un ejemplo en el que se usa la clase `Memory`:

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

En el ejemplo se asignan 256 bytes de memoria a través de `Memory.Alloc` e inicializa el bloque de memoria con valores que aumentan de 0 a 255. A continuación, asigna una matriz de bytes de elemento 256 y usa `Memory.Copy` para copiar el contenido del bloque de memoria en la matriz de bytes. Por último, el bloque de memoria se libera usando `Memory.Free` y el contenido de la matriz de bytes se genera en la consola.
