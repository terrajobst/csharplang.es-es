---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488840"
---
# <a name="arrays"></a>Matrices

Una matriz es una estructura de datos que contiene una serie de variables que se accede mediante índices calculados. Las variables contenidas en una matriz, también conocida como elementos de la matriz, son todas del mismo tipo, y este tipo se denomina tipo de elemento de la matriz.

Una matriz tiene un rango que determina el número de índices asociados a cada elemento de matriz. El rango de matriz se conoce también como las dimensiones de la matriz. Una matriz con un rango de uno se denomina un ***matriz unidimensional***. Una matriz con un rango mayor que uno se denomina un ***matriz multidimensional***. Matrices multidimensionales de tamaño específicas a menudo se conocen como las matrices bidimensionales, las matrices tridimensionales y así sucesivamente.

Cada dimensión de una matriz tiene una longitud asociada que es un número entero mayor o igual que cero. Las longitudes de dimensión no forman parte del tipo de la matriz, pero se establecen cuando se crea una instancia del tipo de matriz en tiempo de ejecución. La longitud de una dimensión determina el intervalo válido de índices para esa dimensión: Para una dimensión de longitud `N`, los índices pueden oscilar entre `0` a `N - 1` inclusive. El número total de elementos de una matriz es el producto de las longitudes de cada dimensión de la matriz. Si una o varias de las dimensiones de la matriz tienen una longitud de cero, se dice que la matriz estar vacío.

El tipo de elemento de una matriz puede ser cualquiera, incluido un tipo de matriz.

## <a name="array-types"></a>Tipos de matriz

Un tipo de matriz se escribe como un *non_array_type* seguido de uno o varios *rank_specifier*s:

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

Un *non_array_type* es cualquier *tipo* que es no sí un *array_type*.

El rango de un tipo de matriz viene dado por el extremo izquierdo *rank_specifier* en el *array_type*: Un *rank_specifier* indica que la matriz es una matriz con un rango de uno más el número de "`,`" tokens en el *rank_specifier*.

El tipo de elemento de un tipo de matriz es el tipo resultante de eliminar el extremo izquierdo *rank_specifier*:

*  Un tipo de matriz del formulario `T[R]` es una matriz con rango `R` y un tipo de elemento que no son de matriz `T`.
*  Un tipo de matriz del formulario `T[R][R1]...[Rn]` es una matriz con rango `R` y un tipo de elemento `T[R1]...[Rn]`.

De hecho, el *rank_specifier*s se leen de izquierda a derecha antes del tipo de elemento final que no son de matriz. El tipo `int[][,,][,]` es una matriz unidimensional de las matrices tridimensionales de matrices bidimensionales de `int`.

En tiempo de ejecución, puede ser un valor de un tipo de matriz `null` o una referencia a una instancia de ese tipo de matriz.

### <a name="the-systemarray-type"></a>El tipo System.Array

El tipo `System.Array` es el tipo base abstracto de todos los tipos de matriz. Una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde cualquier tipo de matriz a `System.Array`y una conversión explícita de referencia ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde `System.Array` a cualquier tipo de matriz. Tenga en cuenta que `System.Array` no es un *array_type*. Se trata de un *class_type* de los cuales *array_type*se derivan.

En tiempo de ejecución, un valor de tipo `System.Array` puede ser `null` o una referencia a una instancia de cualquier tipo de matriz.

### <a name="arrays-and-the-generic-ilist-interface"></a>Matrices y la interfaz IList genérica

Una matriz unidimensional `T[]` implementa la interfaz `System.Collections.Generic.IList<T>` (`IList<T>` para abreviar) y sus interfaces base. En consecuencia, hay una conversión implícita de `T[]` a `IList<T>` y sus interfaces base. Además, si hay una conversión implícita de referencia de `S` a `T` , a continuación, `S[]` implementa `IList<T>` y hay una conversión implícita de referencia de `S[]` a `IList<T>` y sus interfaces base () [Conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)). Si no hay una conversión explícita de referencia de `S` a `T` , a continuación, hay una conversión explícita de referencia de `S[]` a `IList<T>` y sus interfaces base ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)). Por ejemplo:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

La asignación `lst2 = oa1` genera un error en tiempo de compilación desde la conversión de `object[]` a `IList<string>` es una conversión explícita, no implícita. La conversión `(IList<string>)oa1` provocará una excepción en tiempo de ejecución desde `oa1` referencias una `object[]` y no un `string[]`. Sin embargo la conversión `(IList<string>)oa2` no provocará una excepción que se produzca desde `oa2` referencias una `string[]`.

Cada vez que hay una conversión implícita o explícita de referencia de `S[]` a `IList<T>`, también hay una conversión explícita de referencia de `IList<T>` y sus interfaces base a `S[]` ([referencia explícita las conversiones](conversions.md#explicit-reference-conversions)).

Cuando un tipo de matriz `S[]` implementa `IList<T>`, algunos de los miembros de la interfaz implementada pueden producir excepciones. El comportamiento exacto de la implementación de la interfaz queda fuera del ámbito de esta especificación.

## <a name="array-creation"></a>creación de matriz

Se crean instancias de la matriz por *array_creation_expression*s ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) o por el campo o declaraciones de variable local que incluyen un *array_initializer*([Inicializadores de matriz](arrays.md#array-initializers)).

Cuando se crea una instancia de matriz, el rango y la longitud de cada dimensión se establecen y, a continuación, permanecen constantes para toda la duración de la instancia. En otras palabras, no es posible cambiar el rango de una instancia existente de la matriz, ni es posible cambiar el tamaño de sus dimensiones.

Una instancia de matriz siempre es un tipo de matriz. El `System.Array` tipo es un tipo abstracto que no pueden crearse instancias.

Elementos de las matrices creadas por *array_creation_expression*s siempre se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).

## <a name="array-element-access"></a>Acceso de elemento de matriz

Elementos de la matriz son accesibles mediante *element_access* expresiones ([matriz acceso](expressions.md#array-access)) del formulario `A[I1, I2, ..., In]`, donde `A` es una expresión de un tipo de matriz y cada `Ix` es un expresión de tipo `int`, `uint`, `long`, `ulong`, o se pueden convertir implícitamente a uno o varios de estos tipos. El resultado de un acceso de elemento de matriz es una variable, es decir, el elemento de matriz seleccionado por los índices.

Se pueden enumerar los elementos de una matriz mediante un `foreach` instrucción ([la instrucción foreach](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Miembros de la matriz

Cada tipo de matriz hereda los miembros declarados por el `System.Array` tipo.

## <a name="array-covariance"></a>Covarianza de matrices

Para dos *reference_type*s `A` y `B`, si una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) o una conversión explícita de referencia ([ Conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde `A` a `B`, a continuación, también existe la misma conversión de referencia del tipo de matriz `A[R]` para el tipo de matriz `B[R]`, donde `R` es cualquier determinado *rank_specifier* (pero el mismo para ambos tipos de matriz). Esta relación se conoce como ***covarianza de matrices***. Covarianza de matrices significa que en concreto, un valor de un tipo de matriz `A[R]` realmente puede ser una referencia a una instancia de un tipo de matriz `B[R]`, siempre existe una conversión implícita de referencia de `B` a `A`.

Debido a la covarianza de matrices, las asignaciones a elementos de matrices de tipos de referencia incluyen una comprobación en tiempo de ejecución que garantiza que el valor asignado al elemento de matriz es realmente de un tipo permitido ([asignación Simple](expressions.md#simple-assignment)). Por ejemplo:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

La asignación a `array[i]` en el `Fill` método incluye implícitamente una comprobación en tiempo de ejecución que garantiza que el objeto al que hace referencia `value` sea `null` o una instancia que es compatible con el tipo de elemento real de `array`. En `Main`, las dos primeras invocaciones de `Fill` se realiza correctamente, pero las causas de invocación tercer un `System.ArrayTypeMismatchException` que se produzca una vez ejecutada la primera asignación a `array[i]`. La excepción se produce porque una conversión boxing `int` no pueden almacenarse en un `string` matriz.

Covarianza de matrices específicamente no se extiende a las matrices de *value_type*s. Por ejemplo, no existe ninguna conversión que permite realizar un `int[]` a tratarse como un `object[]`.

## <a name="array-initializers"></a>Inicializadores de matriz

Los inicializadores de matriz se pueden especificar en las declaraciones de campo ([campos](classes.md#fields)), las declaraciones de variable locales ([declaraciones de variable Local](statements.md#local-variable-declarations)) y las expresiones de creación de matriz ([creación de matrices las expresiones](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Un inicializador de matriz consta de una secuencia de inicializadores de variables, delimitadas por "`{`"y"`}`"tokens y separados por"`,`" tokens. Cada inicializador de variable es una expresión o, en el caso de una matriz multidimensional, un inicializador de matriz anidados.

El contexto en el que se usa un inicializador de matriz determina el tipo de la matriz que se está inicializando. En una expresión de creación de matriz, el tipo de matriz inmediatamente precede al inicializador o se deduce de las expresiones de inicializador de matriz. En un campo o una declaración de variable, el tipo de matriz es el tipo del campo o variable que se declara. Cuando se usa un inicializador de matriz en un campo o una declaración de variable, como:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
es simplemente la versión abreviada de una expresión de creación de matriz equivalente:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

En una matriz unidimensional, el inicializador de matriz debe constar de una secuencia de expresiones que son compatibles con asignación del tipo de elemento de la matriz. Las expresiones de inicializan los elementos de matriz en orden ascendente, empezando por el elemento en el índice cero. El número de expresiones en el inicializador de matriz determina la longitud de la instancia de matriz que se va a crear. Por ejemplo, el inicializador de matriz anterior crea un `int[]` instancia de longitud 5 y, a continuación, se inicializa la instancia con los siguientes valores:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Para una matriz multidimensional, el inicializador de matriz debe tener tantos niveles de anidamiento como dimensiones de la matriz. El nivel de anidamiento superior corresponde a la dimensión del extremo izquierdo y el nivel de anidamiento inferior se corresponde con la dimensión situada más. La longitud de cada dimensión de la matriz viene determinada por el número de elementos en el nivel de anidamiento correspondiente en el inicializador de matriz. Cada inicializador de matriz anidados, el número de elementos debe ser igual que los otros inicializadores de matriz del mismo nivel. El ejemplo:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
crea una matriz bidimensional con una longitud de cinco para la dimensión del extremo izquierdo y una longitud de dos para la dimensión situada más:
```csharp
int[,] b = new int[5, 2];
```
y, a continuación, se inicializa la matriz de instancia con los siguientes valores:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

Si una dimensión que no sea la más a la derecha no se especifica con longitud cero, se supone que las dimensiones subsiguientes también tiene longitud cero. El ejemplo:
```csharp
int[,] c = {};
```
crea una matriz bidimensional con una longitud de cero para el extremo izquierdo y la dimensión situada más:
```csharp
int[,] c = new int[0, 0];
```

Cuando una expresión de creación de matriz incluye tanto longitudes de dimensión explícitas como un inicializador de matriz, las longitudes deben ser expresiones constantes y el número de elementos en cada nivel de anidamiento debe coincidir con la longitud de la dimensión correspondiente. A continuación se muestran algunos ejemplos:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Aquí, el inicializador para `y` da como resultado un error en tiempo de compilación porque la expresión de longitud de la dimensión no es una constante y el inicializador para `z` da como resultado un error en tiempo de compilación porque la longitud y el número de elementos en el inicializador no coinciden.
