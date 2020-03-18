---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483953"
---
# <a name="ranges"></a>Intervalos

## <a name="summary"></a>Resumen

Esta característica trata sobre la entrega de dos nuevos operadores que permiten construir `System.Index` y `System.Range` objetos y usarlos para indexar y segmentar colecciones en tiempo de ejecución.

## <a name="overview"></a>Información general

### <a name="well-known-types-and-members"></a>Tipos y miembros conocidos

Para usar las nuevas formas sintácticas para `System.Index` y `System.Range`, es posible que se necesiten nuevos tipos y miembros conocidos, en función de las formas sintácticas que se utilicen.

Para usar el operador "Hat" (`^`), se requiere lo siguiente:

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

Para usar el tipo de `System.Index` como argumento en un acceso de elemento de matriz, se requiere el siguiente miembro:

```csharp
int System.Index.GetOffset(int length);
```

La sintaxis de `..` para `System.Range` requerirá el tipo de `System.Range`, así como uno o varios de los miembros siguientes:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

La sintaxis de `..` permite que estén ausentes, ambos o ninguno de sus argumentos. Independientemente del número de argumentos, el constructor de `Range` siempre es suficiente para utilizar la sintaxis de `Range`. Sin embargo, si alguno de los demás miembros está presente y faltan uno o varios de los argumentos `..`, se puede sustituir el miembro adecuado.

Por último, para un valor de tipo `System.Range` que se va a usar en una expresión de acceso de elemento de matriz, debe existir el siguiente miembro:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. index

C#no tiene forma de indizar una colección desde el final, sino que la mayoría de los indizadores usan la noción "desde Start" o una expresión "length-i". Se presenta una nueva expresión de índice que significa "desde el final". La característica introducirá un nuevo operador unario de prefijo "Hat". Su único operando debe ser convertible a `System.Int32`. Se reducirá en el `System.Index` adecuado Factory Method llamada.

Aumentamos la gramática de *unary_expression* con el siguiente formato de sintaxis adicional:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Se llama al operador *index from end* . El índice predefinido de los operadores *finales* es el siguiente:

```csharp
    System.Index operator ^(int fromEnd);
```

El comportamiento de este operador solo se define para los valores de entrada mayores o iguales que cero.

Ejemplos:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#no tiene ninguna forma sintáctica de tener acceso a "Ranges" o "slices" de las colecciones. Normalmente, los usuarios se ven obligados a implementar estructuras complejas para filtrar o operar en segmentos de memoria, o bien recurrir a métodos LINQ como `list.Skip(5).Take(2)`. Con la adición de `System.Span<T>` y otros tipos similares, es más importante que este tipo de operación se admita en un nivel más profundo en el lenguaje o en tiempo de ejecución, y que la interfaz sea unificada.

El lenguaje introducirá un nuevo operador de intervalo `x..y`. Es un operador binario de infijo que acepta dos expresiones. Se puede omitir cualquiera de los operandos (ejemplos siguientes) y deben poder convertirse en `System.Index`. Se reducirá al `System.Range` adecuado Factory Method llamada.

-Reemplazamos las C# reglas de gramática de *multiplicative_expression* por lo siguiente (para introducir un nuevo nivel de prioridad):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

Todas las formas del *operador Range* tienen la misma precedencia. Este nuevo grupo de precedencia es inferior a los *operadores unarios* y superior a los *operadores aritméticos mulitiplicative*.

Llamamos al operador de `..` el *operador de intervalo*. El operador de intervalo integrado se puede entender aproximadamente para que se corresponda con la invocación de un operador integrado de este formulario:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Ejemplos:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Además, `System.Index` debe tener una conversión implícita de `System.Int32`, por lo que no es necesario sobrecargar para mezclar enteros e índices en firmas multidimensionales.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Agregar compatibilidad con índices y rangos a tipos de biblioteca existentes

### <a name="implicit-index-support"></a>Compatibilidad con índices implícitos

El lenguaje proporcionará un miembro de indexador de instancia con un parámetro único de tipo `Index` para los tipos que cumplen los criterios siguientes:

- El tipo es recuento.
- El tipo tiene un indizador de instancia accesible que toma un único `int` como argumento.
- El tipo no tiene un indizador de instancia accesible que toma un `Index` como primer parámetro. El `Index` debe ser el único parámetro o los parámetros restantes deben ser opcionales.

Un tipo se puede ***contar*** si tiene una propiedad denominada `Length` o `Count` con un captador accesible y un tipo de valor devuelto de `int`. El lenguaje puede hacer uso de esta propiedad para convertir una expresión de tipo `Index` en un `int` en el punto de la expresión sin necesidad de usar el tipo `Index` en absoluto. En caso de que existan `Length` y `Count`, se preferirá `Length`. Para facilitar la simplicidad, la propuesta usará el nombre `Length` para representar `Count` o `Length`.

Para estos tipos, el lenguaje actuará como si hubiera un miembro de índice con el formato `T this[Index index]` donde `T` es el tipo de valor devuelto del indexador basado en `int`, incluidas las anotaciones de estilo de `ref`. El nuevo miembro tendrá el mismo `get` y `set` miembros con accesibilidad coincidente que el indizador `int`. 

El nuevo indexador se implementará mediante la conversión del argumento de tipo `Index` en un `int` y la emisión de una llamada al indizador basado en `int`. Para fines de análisis, use el ejemplo de `receiver[expr]`. La conversión de `expr` en `int` se realizará de la siguiente manera:

- Cuando el argumento tiene el formato `^expr2` y el tipo de `expr2` es `int`, se traducirá a `receiver.Length - expr2`.
- De lo contrario, se traducirá como `expr.GetOffset(receiver.Length)`.

Esto permite a los desarrolladores usar la característica `Index` en tipos existentes sin necesidad de modificarla. Por ejemplo:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

Las expresiones `receiver` y `Length` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez. Por ejemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Este código imprimirá "obtener longitud 3".

### <a name="implicit-range-support"></a>Compatibilidad de intervalo implícita

El lenguaje proporcionará un miembro de indexador de instancia con un parámetro único de tipo `Range` para los tipos que cumplen los criterios siguientes:

- El tipo es recuento.
- El tipo tiene un miembro accesible denominado `Slice` que tiene dos parámetros de tipo `int`.
- El tipo no tiene un indizador de instancia que toma un único `Range` como primer parámetro. El `Range` debe ser el único parámetro o los parámetros restantes deben ser opcionales.

Para estos tipos, el lenguaje se enlazará como si hubiera un miembro de índice con el formato `T this[Range range]` donde `T` es el tipo de valor devuelto del método `Slice`, incluidas las anotaciones de estilo de `ref`. El nuevo miembro también tendrá accesibilidad coincidente con `Slice`. 

Cuando el indexador basado en `Range` se enlaza a una expresión llamada `receiver`, se reducirá al convertir la expresión `Range` en dos valores que se pasarán al método `Slice`. Para fines de análisis, use el ejemplo de `receiver[expr]`.

El primer argumento de `Slice` se obtendrá convirtiendo la expresión con tipo de la siguiente manera:

- Cuando `expr` tiene el formato `expr1..expr2` (donde `expr2` se puede omitir) y `expr1` tiene el tipo `int`, se emitirá como `expr1`.
- Cuando `expr` tiene el formato `^expr1..expr2` (donde `expr2` se puede omitir), se emitirá como `receiver.Length - expr1`.
- Cuando `expr` tiene el formato `..expr2` (donde `expr2` se puede omitir), se emitirá como `0`.
- De lo contrario, se emitirá como `expr.Start.GetOffset(receiver.Length)`.

Este valor se volverá a usar en el cálculo del segundo argumento `Slice`. Al hacerlo, se hará referencia a él como `start`. El segundo argumento de `Slice` se obtendrá convirtiendo la expresión con tipo de intervalo de la siguiente manera:

- Cuando `expr` tiene el formato `expr1..expr2` (donde `expr1` se puede omitir) y `expr2` tiene el tipo `int`, se emitirá como `expr2 - start`.
- Cuando `expr` tiene el formato `expr1..^expr2` (donde `expr1` se puede omitir), se emitirá como `(receiver.Length - expr2) - start`.
- Cuando `expr` tiene el formato `expr1..` (donde `expr1` se puede omitir), se emitirá como `receiver.Length - start`.
- De lo contrario, se emitirá como `expr.End.GetOffset(receiver.Length) - start`.

Las expresiones `receiver`, `Length` y `expr` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez. Por ejemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Este código imprimirá "obtener longitud 2".

El lenguaje será especial en caso de que se trate de los siguientes tipos conocidos: 

- `string`: se usará el `Substring` de método en lugar de `Slice`.
- `array`: se usará el `System.Reflection.CompilerServices.GetSubArray` de método en lugar de `Slice`.

## <a name="alternatives"></a>Alternativas

Los nuevos operadores (`^` y `..`) son el azúcar sintáctico. La funcionalidad se puede implementar mediante llamadas explícitas a `System.Index` y `System.Range` métodos de generador, pero dará como resultado un código mucho más reutilizable y la experiencia no será intuitiva.

## <a name="il-representation"></a>Representación de IL

Estos dos operadores se reducirán a las llamadas normales de indexador/método, sin cambios en los niveles posteriores del compilador.

## <a name="runtime-behavior"></a>Comportamiento en tiempo de ejecución

- El compilador puede optimizar los indizadores para los tipos integrados como matrices y cadenas, y reducir la indización a los métodos existentes correspondientes.
- `System.Index` producirá si se construye con un valor negativo.
- `^0` no inicia, pero se convierte en la longitud de la colección o Enumerable en la que se proporciona.
- `Range.All` es semánticamente equivalente a `0..^0`y se puede desconstruir en estos índices.

## <a name="considerations"></a>Consideraciones

### <a name="detect-indexable-based-on-icollection"></a>Detectar indexable basada en ICollection

La inspiración para este comportamiento era inicializadores de colección. Usar la estructura de un tipo para indicar que ha participado en una característica. En el caso de los tipos de inicializadores de colección, puede participar en la característica implementando la interfaz `IEnumerable` (no genérica).

Esta propuesta requiere inicialmente que los tipos implementen `ICollection` para poder calificarse como indexable. Sin embargo, esto requería un número de casos especiales:

- `ref struct`: estos no pueden implementar interfaces todavía tipos como `Span<T>` son ideales para la compatibilidad con índices y rangos. 
- `string`: no implementa `ICollection` y agregar ese `interface` tiene un gran costo.

Esto significa que la compatibilidad con tipos de clave especiales ya es necesaria. El uso de mayúsculas y minúsculas especiales de `string` es menos interesante, ya que el lenguaje lo hace en otras áreas (`foreach` bajar, constantes, etc.). El uso de mayúsculas y minúsculas especiales de `ref struct` es más en lo que se refiere a una clase completa de tipos. Se etiquetan como indexables si simplemente tienen una propiedad denominada `Count` con un tipo de valor devuelto de `int`. 

Después de tener en cuenta que se ha normalizado el diseño para indicar que cualquier tipo que tiene una propiedad `Count` / `Length` con un tipo de valor devuelto de `int` es indexable. Esto quita todas las mayúsculas y minúsculas especiales, incluso para `string` y matrices.

### <a name="detect-just-count"></a>Detectar solo recuento

La detección de los nombres de propiedad `Count` o `Length` complica el diseño un poco. La selección de solo una para estandarizar no es suficiente, ya que termina excluyendo un gran número de tipos:

- Use `Length`: excluye prácticamente todas las colecciones de los subespacios de nombres System. Collections y sub-namespaces. Tienden a derivarse de `ICollection` y, por tanto, prefieren `Count` de la longitud.
- Usar `Count`: excluye `string`, matrices, `Span<T>` y la mayoría de los tipos basados en `ref struct`

La complicación adicional en la detección inicial de los tipos indexables se compensa por su simplificación en otros aspectos.

### <a name="choice-of-slice-as-a-name"></a>Elección del segmento como nombre

Se eligió el nombre `Slice` como es el nombre estándar de facto para las operaciones de estilo de segmento en .NET. A partir de netcoreapp 2.1, todos los tipos de estilo de span usan el nombre `Slice` para las operaciones de segmentación. Antes de netcoreapp 2.1, en realidad no hay ningún ejemplo de segmentación para ver un ejemplo. Los tipos como `List<T>`, `ArraySegment<T>``SortedList<T>` serían ideales para la segmentación, pero el concepto no existía cuando se agregaban tipos. 

Por lo tanto, `Slice` ser el único ejemplo, se eligió como nombre.

### <a name="index-target-type-conversion"></a>Conversión de tipo de destino de índice

Otra manera de ver el `Index` transformación en una expresión de indexador es como una conversión de tipo de destino. En lugar de enlazar como si hubiera un miembro del formulario `return_type this[Index]`, en su lugar, el lenguaje asigna una conversión con tipo de destino a `int`. 

Este concepto podría generalizarse para todos los accesos a miembros en tipos que se pueden contar. Siempre que se utiliza una expresión con el tipo `Index` como argumento para la invocación de un miembro de instancia y el receptor es recuento, la expresión tendrá una conversión de tipo de destino a `int`. Las invocaciones de miembro aplicables para esta conversión incluyen métodos, indexadores, propiedades, métodos de extensión, etc. Solo se excluyen los constructores, ya que no tienen ningún receptor. 

La conversión de tipo de destino se implementará como se indica a continuación para cualquier expresión que tenga un tipo de `Index`. Para fines de análisis, use el ejemplo de `receiver[expr]`:

- Cuando `expr` tiene el formato `^expr2` y se `int`el tipo de `expr2`, se traducirá a `receiver.Length - expr2`.
- De lo contrario, se traducirá como `expr.GetOffset(receiver.Length)`.

Las expresiones `receiver` y `Length` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez. Por ejemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Este código imprimirá "obtener longitud 3". 

Esta característica sería beneficiosa para cualquier miembro que tuviera un parámetro que representase un índice. Por ejemplo, `List<T>.InsertAt`. Esto también tiene la posibilidad de confusión, ya que el lenguaje no puede proporcionar ninguna orientación sobre si una expresión está pensada para la indización o no. Todo lo que puede hacer es convertir cualquier expresión `Index` en `int` al invocar un miembro en un tipo que se puede contar. 

Restricciones:

- Esta conversión solo es aplicable cuando la expresión con el tipo `Index` es directamente un argumento para el miembro. No se aplicaría a ninguna expresión anidada.

## <a name="decisions-made-during-implementation"></a>Decisiones tomadas durante la implementación

- Todos los miembros del patrón deben ser miembros de instancia
- Si se encuentra un método de longitud pero tiene un tipo de valor devuelto incorrecto, continúe buscando el recuento.
- El indexador utilizado para el patrón de índice debe tener exactamente un parámetro int
- El método slice que se usa para el patrón de intervalo debe tener exactamente dos parámetros int
- Al buscar los miembros del patrón, buscamos definiciones originales, no miembros construidos

## <a name="design-meetings"></a>Design Meetings

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>Preguntas
