---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484025"
---
# <a name="readonly-references"></a>Referencias readonly

* [x] propuesto
* [x] prototipo
* [x] implementación: iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

La característica "referencias de solo lectura" es realmente un grupo de características que aprovechan la eficacia de pasar variables por referencia, pero sin exponer los datos a modificaciones:  
- `in` de parámetros
- Devoluciones de `ref readonly`
- Structs `readonly`
- `ref`/métodos de extensión `in`
- `ref readonly` variables locales
- `ref` de expresiones condicionales

## <a name="passing-arguments-as-readonly-references"></a>Pasar argumentos como referencias de solo lectura.

Existe una propuesta existente que toca este tema https://github.com/dotnet/roslyn/issues/115 como un caso especial de parámetros de solo lectura sin entrar en muchos detalles.
Aquí solo quiero reconocer que la idea por sí misma no es muy nueva.

### <a name="motivation"></a>Motivación

Antes de esta característica C# no tenía una manera eficaz de expresar el deseo de pasar variables de struct a llamadas a métodos con fines de solo lectura sin intención de modificar. El paso normal de los argumentos por valor implica la copia, lo que agrega costos innecesarios.  Que dirige a los usuarios para que usen el paso de argumentos por referencia y que se basen en comentarios o documentación para indicar que no se supone que el destinatario de la llamada modifique los datos. No es una buena solución por muchas razones.  
Los ejemplos son numerosos operadores matemáticos-vector/Matrix en las bibliotecas de gráficos, como [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , se sabe que tienen operandos Ref únicamente debido a consideraciones de rendimiento. Hay código en el propio compilador Roslyn que usa estructuras para evitar asignaciones y, a continuación, los pasa por referencia para evitar la copia de costos.

### <a name="solution-in-parameters"></a>Solución (parámetros de`in`)

Del mismo modo que los parámetros de `out`, `in` parámetros se pasan como referencias administradas con garantías adicionales del destinatario.  
A diferencia de `out` parámetros que _debe_ asignar el destinatario antes de cualquier otro uso, el destinatario no puede asignar los parámetros `in`.

Como resultado `in` los parámetros permiten la eficacia del paso de argumentos indirectos sin exponer argumentos a mutaciones por parte del destinatario.

### <a name="declaring-in-parameters"></a>Declaración de parámetros `in`

`in` parámetros se declaran mediante `in` palabra clave como un modificador en la Signatura del parámetro.

Para todos los propósitos, el parámetro `in` se trata como una variable `readonly`. La mayoría de las restricciones en el uso de los parámetros de `in` dentro del método son las mismas que con los campos de `readonly`.

> En realidad, un parámetro `in` puede representar un campo `readonly`. La similitud de las restricciones no es una coincidencia.

Por ejemplo, los campos de un parámetro `in` que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- `in` parámetros se permiten en cualquier lugar donde se permitan los parámetros de ByVal normales. Esto incluye indizadores, operadores (incluidas las conversiones), delegados, expresiones lambda, funciones locales.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` no se permite en combinación con `out` o con cualquier cosa con la que `out` no se combina.

- No se puede sobrecargar en `ref`/`out``in` /diferencias.

- Se permite sobrecargar las diferencias ordinarias de ByVal y `in`.

- Para el propósito de OHI (sobrecargar, ocultar, implementar), `in` se comporta de forma similar a un parámetro `out`.
Se aplican las mismas reglas.
Por ejemplo, el método de reemplazo tendrá que coincidir con `in` parámetros con `in` parámetros de un tipo que se pueda convertir en identidades.

- Para el propósito de las conversiones de grupo de delegado/lambda/método, `in` se comporta de forma similar a un parámetro de `out`.
Las expresiones lambda y los candidatos de conversión de grupos de métodos aplicables tendrán que coincidir con `in` parámetros del delegado de destino con `in` parámetros de un tipo que se pueda convertir en identidades.

- Para el propósito de la varianza genérica, `in` parámetros no son variantes.

> Nota: no hay advertencias en `in` parámetros que tengan tipos de referencia o primitivos.
Puede que no sea puntual en general, pero en algunos casos el usuario debe/desea pasar primitivos como `in`. Ejemplos: invalidar un método genérico como `Method(in T param)` cuando `T` se sustituye por `int`, o cuando tiene métodos como `Volatile.Read(in int location)`
>
> Es posible tener un analizador que advierte en casos de uso ineficaz de los parámetros de `in`, pero las reglas para dicho análisis serían demasiado aproximados para formar parte de una especificación de lenguaje.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Uso de `in` en sitios de llamada. (argumentos`in`)

Hay dos maneras de pasar argumentos a `in` parámetros.

#### <a name="in-arguments-can-match-in-parameters"></a>`in` argumentos pueden coincidir con `in` parámetros:

Un argumento con un modificador `in` en el sitio de llamada puede coincidir con `in` parámetros.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` argumento debe ser un valor l (*) _legible_ .
Ejemplo: `M1(in 42)` no es válido

> (*) La noción de valor [l/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) varía entre los lenguajes.  
Aquí, por valor l I significa una expresión que representa una ubicación a la que se puede hacer referencia directamente.
Y RValue significa una expresión que produce un resultado temporal que no se conserva por sí mismo.  

- En concreto, es válido pasar `readonly` campos, `in` parámetros u otras variables `readonly` de forma formal como argumentos de `in`.
Ejemplo: `dictionary[in Guid.Empty]` es legal. `Guid.Empty` es un campo de solo lectura estático.

- `in` argumento debe tener la _identidad_ del tipo que se pueda convertir al tipo del parámetro.
Ejemplo: `M1<object>(in Guid.Empty)` no es válido. `Guid.Empty` no es _convertible en una identidad_ en `object`

La motivación de las reglas anteriores es que `in` argumentos garantizan el _alias_ de la variable de argumento. El destinatario siempre recibe una referencia directa a la misma ubicación que representa el argumento.

- en situaciones excepcionales en las que los argumentos de `in` deben estar desbordados por pila debido a las expresiones de `await` que se usan como operandos de la misma llamada, el comportamiento es el mismo que con los argumentos `out` y `ref`: Si la variable no se puede desbordar de manera referencial y transparente, se muestra un error.

Ejemplos:
1. `M1(in staticField, await SomethingAsync())` es válido.
`staticField` es un campo estático al que se puede tener acceso más de una vez sin efectos secundarios observables. Por lo tanto, se pueden proporcionar el orden de los efectos secundarios y los requisitos de alias.
2. `M1(in RefReturningMethod(), await SomethingAsync())` producirá un error.
`RefReturningMethod()` es un método que devuelve `ref`. Una llamada al método puede tener efectos secundarios observables, por lo que debe evaluarse antes que el operando `SomethingAsync()`. Sin embargo, el resultado de la invocación es una referencia que no se puede conservar en el punto de suspensión de `await`, lo que hace imposible el requisito de referencia directa.

> Nota: los errores de desbordamiento de pila se consideran limitaciones específicas de la implementación. Por lo tanto, no tienen efecto en la resolución de sobrecarga o la inferencia lambda.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Los argumentos ByVal normales pueden coincidir `in` parámetros:

Los argumentos normales sin modificadores pueden coincidir con los parámetros de `in`. En tal caso, los argumentos tienen las mismas restricciones distendidas que los argumentos ByVal normales.

La motivación para este escenario es que `in` parámetros de las API pueden dar lugar a inconvenientes para el usuario cuando los argumentos no se pueden pasar como una referencia directa: por ejemplo, los literales, los resultados calculados o `await`-Ed o los argumentos que tienen tipos más específicos.  
Todos estos casos tienen una solución trivial de almacenar el valor del argumento en una local temporal del tipo adecuado y pasar ese local como un argumento `in`.  
Para reducir la necesidad de este compilador de código reutilizable puede realizar la misma transformación, si es necesario, cuando `in` modificador no está presente en el sitio de llamada.  

Además, en algunos casos, como la invocación de operadores o `in` métodos de extensión, no hay ninguna manera sintáctica de especificar `in`. Esto solo requiere especificar el comportamiento de los argumentos ByVal normales cuando coinciden `in` parámetros.

En concreto:

- es válido para pasar RValues.
En tal caso, se pasa una referencia a un temporal.
Ejemplo:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- se permiten las conversiones implícitas.

> En realidad, se trata de un caso especial de pasar un valor r  

En ese caso, se pasa una referencia a un valor convertido temporal que contiene.
Ejemplo:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- en un caso de un receptor de un método de extensión `in` (en lugar de `ref` métodos de extensión), se permiten las _conversiones_ de RValues o implícitas.
En ese caso, se pasa una referencia a un valor convertido temporal que contiene.
Ejemplo:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
En este documento se proporciona más información sobre `ref`/métodos de extensión `in`.

- la desbordación de argumentos debido a `await` operandos podría sobrepasar "por valor", si es necesario.
En escenarios en los que no es posible proporcionar una referencia directa al argumento debido a que intervienen `await` se vuelca una copia del valor del argumento.  
Ejemplo:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Dado que el resultado de una invocación de efectos secundarios es una referencia que no se puede conservar en `await` suspensión, se conservará en su lugar un temporal que contenga el valor real (tal como lo haría en un caso de parámetro ByVal normal).

#### <a name="omitted-optional-arguments"></a>Argumentos opcionales omitidos

Se permite que un parámetro `in` especifique un valor predeterminado. Esto hace que el argumento correspondiente sea opcional.

Si se omite el argumento opcional en el sitio de llamada, se pasa el valor predeterminado a través de un temporal.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Comportamiento de alias en general

Al igual que las variables `ref` y `out`, las variables de `in` son referencias o alias a las ubicaciones existentes.

Aunque no se permite que el destinatario escriba en ellos, leer un parámetro `in` puede observar valores diferentes como un efecto secundario de otras evaluaciones.

Ejemplo:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` parámetros y la captura de variables locales.  
Para la captura de lambda/Async `in` parámetros se comportan igual que los parámetros `out` y `ref`.

- `in` parámetros no se pueden capturar en un cierre
- no se permiten parámetros `in` en métodos de iterador
- no se permiten parámetros `in` en métodos Async

### <a name="temporary-variables"></a>Variables temporales.  
Algunos usos de `in` paso de parámetros pueden requerir el uso indirecto de una variable local temporal:  
- `in` argumentos se pasan siempre como alias directos cuando Call-site utiliza `in`. No se usa nunca en este caso.
- `in` argumentos no deben ser alias directos cuando Call-site no usa `in`. Si el argumento no es un valor l, se puede usar un temporal.
- `in` parámetro puede tener un valor predeterminado. Cuando se omite el argumento correspondiente en el sitio de la llamada, el valor predeterminado se pasa a través de un temporal.
- `in` argumentos pueden tener conversiones implícitas, incluidas las que no conservan la identidad. En esos casos se usa un temporal.
- no se puede escribir en los receptores de llamadas de struct normales LValues (**caso existente**). En esos casos se usa un temporal.

La duración del argumento objetos temporales coincide con el ámbito más cercano del sitio de llamada.

La duración formal de las variables temporales es semánticamente significativa en escenarios que implican el análisis de escape de las variables devueltas por referencia.

### <a name="metadata-representation-of-in-parameters"></a>Representación de metadatos de parámetros de `in`.
Cuando se aplica `System.Runtime.CompilerServices.IsReadOnlyAttribute` a un parámetro ByRef, significa que el parámetro es un parámetro `in`.

Además, si el método es *abstracto* o *virtual*, la firma de estos parámetros (y solo estos parámetros) debe tener `modreq[System.Runtime.InteropServices.InAttribute]`.

**Motivación**: esto se hace para asegurarse de que, en un caso de que el método invalide e implemente los parámetros de `in` coincidan.

Los mismos requisitos se aplican a los métodos de `Invoke` en los delegados.

**Motivación**: esto es para asegurarse de que los compiladores existentes no pueden simplemente omitir `readonly` al crear o asignar delegados.

## <a name="returning-by-readonly-reference"></a>Devolución por referencia de solo lectura.

### <a name="motivation"></a>Motivación
La motivación de esta subcaracterística es aproximadamente simétrica a las razones del `in` los parámetros, evitando la copia, pero en el lado devuelto. Antes de esta característica, un método o un indexador tenían dos opciones: 1) devuelven por referencia y se exponen a posibles mutaciones o 2) devuelven por valor, lo que da como resultado la copia.

### <a name="solution-ref-readonly-returns"></a>Solución (`ref readonly` devuelve)  
La característica permite que un miembro devuelva variables por referencia sin exponerlas a mutaciones.

### <a name="declaring-ref-readonly-returning-members"></a>Declarar `ref readonly` devolver miembros

Una combinación de modificadores `ref readonly` en la firma devuelta se utiliza para indicar que el miembro devuelve una referencia de solo lectura.

Para todos los propósitos, un miembro de `ref readonly` se trata como una variable `readonly`, similar a `readonly` campos y `in` parámetros.

Por ejemplo, los campos de `ref readonly` miembro que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`. -Se permite pasarlos como argumentos `in`, pero no como argumentos `ref` o `out`.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- se permiten `ref readonly` devoluciones en los mismos lugares en los que se permiten `ref` devoluciones.
Esto incluye indexadores, delegados, expresiones lambda y funciones locales.

- No se puede sobrecargar en `ref`/de `ref readonly`/diferencias.

- Se permite sobrecargar los valores de ByVal y `ref readonly` devuelven diferencias normales.

- Para el propósito de OHI (sobrecargar, ocultar, implementar), `ref readonly` es similar, pero distinto de `ref`.
Por ejemplo, un método que invalide `ref readonly` uno, debe ser `ref readonly` y tener un tipo que se pueda convertir en identidades.

- En el caso de las conversiones de grupo de delegado/lambda/método, `ref readonly` es similar, pero distinto de `ref`.
Las expresiones lambda y los candidatos de conversión de grupos de métodos aplicables deben coincidir `ref readonly` devolución del delegado de destino con `ref readonly` devolución del tipo que se puede convertir en identidades.

- Para el propósito de la varianza genérica, `ref readonly` devuelve son no variantes.

> Nota: no hay ninguna advertencia en `ref readonly` devoluciones que tengan tipos de referencia o primitivos.
Puede que no sea puntual en general, pero en algunos casos el usuario debe/desea pasar primitivos como `in`. Ejemplos: invalidar un método genérico como `ref readonly T Method()` cuando `T` se sustituyó por `int`.
>
>Es posible tener un analizador que advierte en casos de uso ineficaz de `ref readonly` devuelve, pero las reglas para dicho análisis serían demasiado aproximadas para formar parte de una especificación de lenguaje.

### <a name="returning-from-ref-readonly-members"></a>Devolver desde `ref readonly` miembros
Dentro del cuerpo del método, la sintaxis es la misma que con las devoluciones de referencia normales. El `readonly` se deduce del método contenedor.

La motivación es que `return ref readonly <expression>` es una duración innecesaria y solo permite discrepancias en la parte `readonly` que siempre daría lugar a errores.
Sin embargo, el `ref` es necesario para la coherencia con otros escenarios en los que se pasa algo a través de un alias estricto frente a un valor.

> A diferencia de lo que ocurre con los parámetros de `in`, `ref readonly` devuelve nunca a través de una copia local. Es posible que la copia deje de existir inmediatamente después de que se devuelva este procedimiento. Por consiguiente `ref readonly` Returns son siempre referencias directas.

Ejemplo:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- Un argumento de `return ref` debe ser un valor l (**regla existente**)
- Un argumento de `return ref` debe ser "Safe to Return" (**regla existente**)
- En un miembro de `ref readonly`, _no es necesario que_ el argumento de `return ref` sea grabable.
Por ejemplo, este miembro puede devolver una referencia a un campo de solo lectura o a uno de sus parámetros de `in`.

### <a name="safe-to-return-rules"></a>Safe para devolver reglas.
La seguridad normal de las reglas de devoluciones para las referencias se aplicará también a las referencias de solo lectura.

Tenga en cuenta que se puede obtener un `ref readonly` a partir de un `ref` normal/parámetro/Return, pero no al revés. De lo contrario, la seguridad de las devoluciones de `ref readonly` se deduce de la misma manera que para las devoluciones normales de `ref`.

Teniendo en cuenta que RValues se puede pasar como `in` parámetro y devolverse como `ref readonly` necesitamos una regla más: **RValues no es seguro para devolver por referencia**.

> Considere la situación en la que se pasa un valor r a un parámetro `in` a través de una copia y, a continuación, se devuelve en forma de un `ref readonly`. En el contexto del llamador, el resultado de dicha invocación es una referencia a los datos locales y, como tal, no es seguro devolver.
> Una vez que RValues no se pueden devolver de forma segura, la regla existente `#6` ya controla este caso.

Ejemplo:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

Reglas de `safe to return` actualizadas:

1.  **las referencias a variables en el montón se pueden devolver de forma segura**
2.  **los parámetros Ref/in son seguros para devolver**
`in` los parámetros de forma natural solo se pueden devolver como ReadOnly.
3.  **los parámetros out son seguros para devolver** (pero se deben asignar definitivamente, ya que ya es el caso hoy)
4.  **los campos de struct de instancia se pueden devolver de forma segura siempre que el receptor sea seguro para devolver**
5.  **' this ' no es seguro para devolver los miembros de struct**
6.  **una referencia, devuelta desde otro método, es segura para devolver si todas las referencias que se pasan a ese método como parámetros formales eran seguras de devolver.** 
*específicamente es irrelevante si el receptor es seguro para devolver, independientemente de si el receptor es un struct, una clase o un tipo de parámetro de tipo genérico.*
7.  **No es seguro devolver RValues por referencia.** 
*específicamente RValues se pueden pasar como parámetros in.*

> Nota: existen otras reglas relacionadas con la seguridad de las devoluciones que entran en juego cuando se implican tipos de referencia y reasignaciones de referencia.
> Las reglas se aplican igualmente a los miembros `ref` y `ref readonly` y, por lo tanto, no se mencionan aquí.

### <a name="aliasing-behavior"></a>Comportamiento de alias.
`ref readonly` miembros proporcionan el mismo comportamiento de alias que los miembros de `ref` ordinarios (excepto para ser de solo lectura).
Por lo tanto, para la captura en lambdas, Async, iteradores, desbordamiento de pila, etc. se aplican las mismas restricciones. es decir,. debido a la incapacidad de capturar las referencias reales y debido a la naturaleza de efectos secundarios de la evaluación de miembros, tales escenarios no se permiten.

> Se permite y es necesario realizar una copia cuando `ref readonly` Return es un receptor de métodos struct normales, que toman `this` como una referencia ordinaria que se puede escribir. Históricamente, en todos los casos en los que estas invocaciones se aplican a la variable de solo lectura, se realiza una copia local.

### <a name="metadata-representation"></a>Representación de metadatos.
Cuando se aplica `System.Runtime.CompilerServices.IsReadOnlyAttribute` a la devolución de un método que devuelve ByRef, significa que el método devuelve una referencia de solo lectura.

Además, la firma de resultados de estos métodos (y solo esos métodos) debe tener `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.

**Motivación**: esto es para asegurarse de que los compiladores existentes no pueden simplemente omitir `readonly` al invocar métodos con `ref readonly` devuelve.

## <a name="readonly-structs"></a>Structs de solo lectura
En Resumen, es una característica que hace `this` parámetro de todos los miembros de instancia de un struct, excepto para los constructores, un parámetro `in`.

### <a name="motivation"></a>Motivación
El compilador debe asumir que cualquier llamada al método en una instancia de struct puede modificar la instancia. En realidad, se pasa una referencia grabable al método como `this` parámetro y habilita completamente este comportamiento. Para permitir tales invocaciones en `readonly` variables, las invocaciones se aplican a las copias temporales. Esto podría ser poco intuitivo y a veces obliga a los usuarios a abandonar `readonly` por motivos de rendimiento.  
Ejemplo: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

Después de agregar compatibilidad para `in` parámetros y `ref readonly` devuelve el problema de la copia defensiva se verá peor, ya que las variables ReadOnly serán más comunes.

### <a name="solution"></a>Solución
Permita `readonly` modificador en las declaraciones de struct, lo que daría lugar a `this` tratarse como un parámetro de `in` en todos los métodos de instancia de estructura, salvo para los constructores.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Restricciones en miembros de struct de solo lectura
- Los campos de instancia de un struct de solo lectura deben ser de solo lectura.  
**Motivación:** solo se puede escribir externamente, pero no a través de miembros.
- Las autopropiedades de instancia de un struct de solo lectura deben ser Get-only.  
**Motivación:** consecuencia de la restricción en los campos de instancia.
- La estructura ReadOnly no puede declarar eventos similares a campos.  
**Motivación:** consecuencia de la restricción en los campos de instancia.

### <a name="metadata-representation"></a>Representación de metadatos.
Cuando `System.Runtime.CompilerServices.IsReadOnlyAttribute` se aplica a un tipo de valor, significa que el tipo es un `readonly struct`.

En concreto:
-  La identidad del tipo `IsReadOnlyAttribute` no es importante. De hecho, el compilador puede incrustarlo en el ensamblado contenedor si es necesario.

## <a name="refin-extension-methods"></a>`ref`/métodos de extensión `in`
En realidad, existe una propuesta (https://github.com/dotnet/roslyn/issues/165) y la solicitud de incorporación de prototipo correspondiente (https://github.com/dotnet/roslyn/pull/15650).
Simplemente deseo confirmar que esta idea no es totalmente nueva. Sin embargo, es relevante aquí, ya que `ref readonly` quita de forma elegante el problema más polémico sobre tales métodos: Qué hacer con los receptores de valor r.

La idea general es permitir que los métodos de extensión tomen el parámetro `this` por referencia, siempre que se sepa que el tipo es un tipo de estructura.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Los motivos para escribir estos métodos de extensión son principalmente:  
1.  Evitar la copia cuando el receptor es un struct grande
2.  Permitir métodos de extensión mutables en Structs

Los motivos por los que no queremos permitir esto en las clases  
1.  Tendría una finalidad muy limitada.
2.  Interrumpiría la invariante de larga duración que una llamada al método no puede convertir un receptor que no sea de`null` para que se `null` después de la invocación.
> De hecho, actualmente no se puede convertir en una variable que no sea de`null` `null` a menos que `ref` o `out`no la asigne o pase _explícitamente_ .
> Esto ayuda en gran medida a la legibilidad u otras formas de análisis "puede ser un valor nulo aquí".
3.  Sería difícil conciliar con la semántica "evaluar una vez" de los accesos condicionales null.
Ejemplo: `obj.stringField?.RefExtension(...)`-necesita capturar una copia de `stringField` para que la comprobación de valores NULL sea significativa, pero las asignaciones a `this` dentro de RefExtension no se reflejarán de nuevo en el campo.

Una capacidad para declarar métodos de extensión en **Structs** que toman el primer argumento por referencia era una solicitud de larga duración. Una de las consideraciones de bloqueo fue "¿Qué ocurre si el receptor no es un valor l?".

- Existe un precedente que también se podría llamar a cualquier método de extensión como método estático (a veces es la única manera de resolver la ambigüedad). Esto indicaría que no se permiten los receptores de valor r.
- Por otro lado, existe la posibilidad de efectuar la invocación en una copia en situaciones similares cuando intervienen los métodos de instancia de struct.

La razón por la que existe la "copia implícita" es que la mayoría de los métodos struct no modifican realmente el struct mientras no es capaz de indicarlo. Por lo tanto, la solución más práctica era simplemente hacer la invocación en una copia, pero esta práctica se conoce para perjudicar el rendimiento y provocar errores.

Ahora, con la disponibilidad de `in` parámetros, es posible que una extensión señale el intento. Por lo tanto, el dilema se puede resolver mediante la exigencia de que las extensiones de `ref` se llamen con receptores grabables, mientras que las extensiones `in` permiten la copia implícita si es necesario.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` extensiones y genéricos.
El propósito de `ref` métodos de extensión es mutar el receptor directamente o invocar a los miembros mutantes. Por lo tanto, se permiten las extensiones `ref this T` siempre que `T` esté restringido a un struct.

Por otro lado `in` métodos de extensión existen específicamente para reducir la copia implícita. Sin embargo, cualquier uso de un parámetro `in T` tendrá que realizarse a través de un miembro de interfaz. Dado que todos los miembros de interfaz se consideran mutables, cualquier uso de este tipo requeriría una copia. -En lugar de reducir la copia, el efecto sería el contrario. Por lo tanto, no se permite `in this T` cuando `T` es un parámetro de tipo genérico, independientemente de las restricciones.

### <a name="valid-kinds-of-extension-methods-recap"></a>Tipos válidos de métodos de extensión (Resumen):
Ahora se permiten las siguientes formas de declaración de `this` en un método de extensión:
1) `this T arg`: extensión ByVal normal. (**caso existente**)
- T puede ser cualquier tipo, incluidos los tipos de referencia o los parámetros de tipo.
La instancia será la misma variable después de la llamada.
Permite conversiones implícitas de este tipo de _conversión de argumentos_ .
Se puede llamar a en RValues.

- `in this T self`extensión de `in` de  - .
T debe ser un tipo de struct real.
La instancia será la misma variable después de la llamada.
Permite conversiones implícitas de este tipo de _conversión de argumentos_ .
Se puede llamar a en RValues (se puede invocar en un Temp si es necesario).

- `ref this T self`extensión de `ref` de  - .
T debe ser un tipo de estructura o un parámetro de tipo genérico restringido para ser un struct.
La invocación puede escribir en la instancia.
Solo permite conversiones de identidad.
Se debe llamar al método LValue grabable. (nunca se invoca a través de un Temp).

## <a name="readonly-ref-locals"></a>Variables locales de referencia de solo lectura.

### <a name="motivation"></a>Razones.
Una vez que se introdujeron `ref readonly` miembros, era evidente que se debían emparejar con el tipo de local adecuado. La evaluación de un miembro puede producir o observar efectos secundarios, por lo tanto, si el resultado se debe usar más de una vez, debe almacenarse. Las variables locales de `ref` normales no ayudan aquí, ya que no se les puede asignar una referencia `readonly`.   

### <a name="solution"></a>Solución.
Permitir declarar `ref readonly` variables locales. Se trata de un nuevo tipo de `ref` variables locales que no se van a escribir. Como resultado `ref readonly` las variables locales pueden aceptar referencias a variables de solo lectura sin exponer estas variables a Escrituras.

### <a name="declaring-and-using-ref-readonly-locals"></a>Declarar y usar `ref readonly` variables locales.

La sintaxis de tales variables locales usa modificadores `ref readonly` en el sitio de la declaración (en ese orden específico). De forma similar a las variables locales de `ref` normales, las variables locales de `ref readonly` se deben inicializar con referencia a la declaración. A diferencia de las variables locales de `ref` estándar, las variables locales de `ref readonly` pueden hacer referencia a `readonly` LValues como `in` parámetros, `readonly` campos `ref readonly` métodos.

Para todos los propósitos, una `ref readonly` local se trata como una variable `readonly`. La mayoría de las restricciones en el uso son las mismas que con `readonly` campos o parámetros de `in`.

Por ejemplo, los campos de un parámetro `in` que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>Restricciones en el uso de `ref readonly` variables locales
Salvo por su naturaleza `readonly`, `ref readonly` variables locales se comportan como `ref` variables locales normales y están sujetas a las mismas restricciones.  
Por ejemplo, las restricciones relacionadas con la captura en cierres, la declaración en `async` métodos o el análisis de `safe-to-return` se aplican igualmente a `ref readonly` variables locales.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>Expresiones de `ref` ternarios. (también conocido como "LValues condicional")

### <a name="motivation"></a>Motivación
El uso de `ref` y `ref readonly` variables locales expone una necesidad de inicializar las variables locales con una u otra variable de destino basada en una condición.

Una solución habitual es introducir un método como:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Tenga en cuenta que `Choice` no es un reemplazo exacto de un ternario, ya que _todos los_ argumentos deben evaluarse en el sitio de llamada, lo que ha provocado errores y comportamientos inintuitivos.

Lo siguiente no funcionará según lo esperado:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Solución
Permite un tipo especial de expresión condicional que se evalúa como una referencia a uno de los argumentos LValue basados en una condición.

### <a name="using-ref-ternary-expression"></a>Usar `ref` expresión ternaria.

La sintaxis del tipo de `ref` de una expresión condicional es `<condition> ? ref <consequence> : ref <alternative>;`

Al igual que con la expresión condicional ordinaria, solo `<consequence>` o `<alternative>` se evalúa según el resultado de la expresión de condición booleana.

A diferencia de la expresión condicional ordinaria, `ref` expresión condicional:
- requiere que `<consequence>` y `<alternative>` sean LValues.
- `ref` expresión condicional es un valor l y
- `ref` expresión condicional es grabable si tanto `<consequence>` como `<alternative>` son grabables LValues

Ejemplos:  
`ref` ternario es un valor l y, como tal, se puede pasar, asignar o devolver por referencia;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Ser un valor l; también se puede asignar a.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

Se puede usar como receptor de una llamada al método y omitir la copia si es necesario.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` ternario se puede usar también en un contexto normal (no Ref).
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Puedo ver dos argumentos principales contra la compatibilidad mejorada para referencias y referencias de solo lectura:

1) Los problemas que se solucionan aquí son muy antiguos. ¿Por qué solucionarlos repentinamente ahora, especialmente porque no ayudaría el código existente?

Como encontramos C# y .net se usan en los dominios nuevos, algunos problemas están más destacados.  
Como ejemplos de entornos que son más críticos que el promedio de las sobrecargas de cálculo, puedo Mostrar

* escenarios de Cloud/Datacenter en los que se factura el cálculo y la capacidad de respuesta es una ventaja competitiva.
* Juegos/VR/AR con requisitos de tiempo real en latencia     

Esta característica no sacrifica ninguno de los puntos fuertes existentes, como la seguridad de tipos, a la vez que permite reducir las sobrecargas en algunos escenarios comunes.

2) ¿Podemos garantizar razonablemente que el destinatario de la llamada reproducirá las reglas cuando opte por `readonly` contratos?

Tenemos confianza similar cuando se usa `out`. Una implementación incorrecta de `out` puede provocar un comportamiento no especificado, pero en realidad apenas sucede.  

El hecho de que las reglas de comprobación formal estén familiarizadas con `ref readonly` mitigaría aún más el problema de confianza.

### <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

En realidad, el diseño de la competencia principal es "no hacer nada".

### <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Design Meetings

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
