---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483905"
---
# <a name="primary-constructors"></a>Constructores principales

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Las clases pueden tener una lista de parámetros y, cuando lo hacen, su especificación de clase base puede tener una lista de argumentos.
Los parámetros del constructor principal están en el ámbito de la declaración de clase y, si los captura un miembro de función o una función anónima, se almacenan como campos privados en la clase.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Es habitual tener muchos reutilizables en el código de inicialización del programa. En el caso general, una parte determinada de `x` de datos se menciona muchas veces:

- Como un campo privado `_x`
- Como parámetro `x` a un constructor
- En un `_x = x;` de asignación del campo del parámetro del constructor
- Como una propiedad `X`
- En el establecedor de propiedad `x = value;`
- En el `return x;` captador de propiedad

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

En el caso de las propiedades que no requieren validación o cálculo, el tediosas tareas se puede reducir mediante propiedades automáticas, lo que reduce la necesidad de declarar un campo de respaldo explícito para la propiedad. Pero si la propiedad requiere cualquier tipo de lógica más allá de lo que proporciona una propiedad automática, lo mejor es lo que hace.

En su lugar, los constructores principales reducen la sobrecarga mediante la colocación de los argumentos del constructor directamente en el ámbito de la clase, lo que vuelve a obviar la necesidad de declarar explícitamente un campo de respaldo. Por lo tanto, el ejemplo anterior sería:

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

En este ejemplo, el constructor principal reduce el número de entidades con nombre para `x` de tres a dos, obviando el campo de respaldo `_x`. Quita dos declaraciones de miembro de tres (manteniendo solo la propia declaración de propiedad) y reduce el número total de menciones de `x`/`_x`/`X` de ocho a cinco.


## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Las clases pueden tener una lista de parámetros:

``` c#
public class C(int i, string s)
{
    ...
}
```

La lista de parámetros hace que se declare implícitamente un constructor para la clase, con la misma accesibilidad que la propia clase.

``` c#
new C(5, "Hello");
```

Los parámetros del constructor principal están en el ámbito de todo el cuerpo de la clase. Si se capturan mediante un miembro de función o una función anónima, se almacenan como campos privados en la clase. Si solo se utilizan durante la inicialización, no se almacenarán en el objeto.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Si una clase con un constructor principal tiene una especificación de clase base, ésta puede tener una lista de argumentos. Actúa como la lista de argumentos para un `base(...)` inicializador del constructor declarado implícitamente. Si no se proporciona ninguna lista de argumentos, se supone que es una vacía.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
La clase también puede tener constructores definidos explícitamente, pero todos ellos tienen que utilizar un inicializador de `this(...)`. Esto garantiza que siempre se llama al constructor principal cuando se crea una nueva instancia.

Todos los inicializadores del cuerpo de la clase se convertirán en asignaciones en el constructor generado. Esto significa que, a diferencia de otras clases, los inicializadores se ejecutarán *después* de que se haya invocado el constructor base, no antes de. Además, la clase generada contendrá asignaciones para inicializar los campos privados que se generaron para almacenar los parámetros de constructor primario capturados por los miembros. Estos miembros se reescriben para utilizar el campo privado en lugar del parámetro de una manera similar a los cierres de las expresiones lambda. Los campos principales generados se inicializan primero y, a continuación, las asignaciones generadas por el inicializador se ejecutan en el orden de aparición en la clase.

En el ejemplo anterior, el efecto de la declaración de clase es como si se reescribira de la siguiente manera:

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

La captura tiene restricciones similares a la captura de variables locales mediante expresiones lambda. Por ejemplo, los parámetros `ref` y `out` se permiten en los constructores principales, pero no se pueden capturar en los cuerpos de los miembros.


## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

En orden aproximado de importancia.

* La propuesta utiliza la sintaxis que también se ha propuesto para los registros posicionales. Si deseamos ambas características, se requiere algún ajuste. Por ejemplo, se ha propuesto un modificador `data` en los registros.
* El tamaño de asignación de los objetos construidos es menos obvio, ya que el compilador determina si se debe asignar un campo para un parámetro de constructor principal basado en el texto completo de la clase. Este riesgo es similar a la captura implícita de variables mediante expresiones lambda.
* Una tentación común (o un patrón accidental) podría ser capturar el "mismo" parámetro en varios niveles de herencia a medida que se pasa la cadena del constructor en lugar de allotting explícitamente un campo protegido en la clase base, lo que conduce a asignaciones duplicadas. para los mismos datos en los objetos. Esto es muy similar al riesgo actual de invalidar propiedades automáticas con propiedades automáticas. 
* Tal y como se ha propuesto anteriormente, no hay ningún lugar para la lógica adicional que normalmente se expresaría en los cuerpos del constructor. La extensión "cuerpos del constructor principal" que se encuentra debajo de direcciones.
* Como se propone, la semántica de orden de ejecución es ligeramente diferente de la de los constructores ordinarios. Probablemente, esto podría ser solucionado, pero a costa de algunas de las propuestas de extensión (en particular, "cuerpos de constructor primario").
* La propuesta solo funciona cuando un solo constructor puede designarse como principal.
* No hay ninguna manera de tener accesibilidad independiente de la clase y del constructor principal. Por ejemplo, en situaciones en las que todos los constructores públicos se delegan a un constructor privado "Build-It-All" que sería necesario. Si es necesario, la sintaxis podría proponerse más adelante.


## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Los registros posicionales completos pueden ser una alternativa, o pueden coexistir con constructores principales, en función de los detalles. Permiten *más* abreviaturas en un *menor* número de escenarios. Por tanto, los dos son potencialmente útiles, pero tener ambos pueden ser exagerados, a menos que se integren perfectamente entre sí.


## <a name="possible-extensions"></a>Posibles extensiones
[extensions]: #possible-extensions

Se trata de variaciones o adiciones a la propuesta de núcleo que se pueden considerar en conjunción con ella o en una fase posterior, si se considera útil.

### <a name="primary-constructor-bodies"></a>Cuerpos del constructor principal

A menudo, los constructores contienen lógica de validación de parámetros u otro código de inicialización no trivial que no se puede expresar como inicializadores.

Los constructores principales se pueden extender para permitir que los bloques de instrucciones aparezcan directamente en el cuerpo de la clase. Esas instrucciones se insertarían en el constructor generado en el punto en el que aparecen entre las asignaciones de inicialización y, por tanto, se ejecutaría intercalada con inicializadores. Por ejemplo:

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a>Campos de inicializador y funciones de inicializador

En una clase con un constructor principal, podríamos considerar las declaraciones de campo y método sin modificadores de accesibilidad para que sean más similares a las variables locales y las funciones locales:

* Al igual que los parámetros de constructor principal, los "campos de inicializador" solo se capturan en un campo privado real si se usaron en miembros de función.
* Las "funciones de inicializador" solo se considerarían para capturar los parámetros del constructor principal y los campos de inicializador si se utilizaban en otros miembros de función. Si no se capturan, podrían generarse de manera más óptima, como las funciones locales.
* Al igual que los parámetros del constructor principal, no estarán disponibles a través del acceso a miembros, sino solo como un nombre simple.

Se puede usar para las variables temporales y las funciones auxiliares que solo son relevantes para la inicialización:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

Esto puede ser demasiado sutil, especialmente porque la ausencia de modificadores de accesibilidad en otro lugar simplemente significa `private`. 

### <a name="initializer-statements"></a>Instrucciones de inicializador

Una combinación radical de las extensiones anteriores sería permitir simplemente instrucciones directamente en el cuerpo de la clase. Estas instrucciones son exactamente iguales que los cuerpos de constructor intercalados propuestos anteriormente, salvo que no es necesario que se incluyan en `{ }`. Para que esto sea suficientemente útil, las variables y las funciones auxiliares "locales" también tendrían que ser expresables en el nivel superior de la clase, de la manera que se explora en la extensión "campos de inicializador y funciones de inicializador" anterior.


### <a name="member-access"></a>Acceso a miembros

La propuesta principal trata los parámetros del constructor principal como parámetros a los que solo se puede hacer referencia como nombres simples. Una alternativa consiste en permitir que se haga referencia a ellas como si fueran campos privados, es decir, con un acceso a miembros, *aunque* a veces no se generen como campos. Esto permitiría hacer referencia a ellas como `this.x` cuando las prevalecen las variables locales y se podía obtener acceso a ellas desde una instancia diferente como `other.x`.

Si se aplica a la extensión "campos de inicializador y funciones de inicializador", esto también reduciría el grado en el que eran diferentes de los miembros privados normales. La única diferencia sería que el compilador es libre de elide desde el objeto si solo se utiliza durante la inicialización.

