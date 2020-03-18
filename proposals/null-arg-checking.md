---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483545"
---
# <a name="simplified-null-argument-checking"></a>Comprobación de argumentos null simplificada

## <a name="summary"></a>Resumen
Esta propuesta proporciona una sintaxis simplificada para validar que los argumentos de método no se `null` y producir `ArgumentNullException` adecuadamente.

## <a name="motivation"></a>Motivación
El trabajo de diseño de tipos de referencia que aceptan valores NULL ha provocado el examen del código necesario para `null` la validación de los argumentos. Dado que NRT no afecta a los desarrolladores de ejecución de código, todavía debe agregar `if (arg is null) throw` código de placa de la caldera incluso en los proyectos que están totalmente `null` limpios. Esto nos dio el deseo de explorar una sintaxis mínima para la validación de argumentos `null` en el lenguaje. 

Aunque se espera que esta sintaxis de validación de parámetros `null` se empareje con frecuencia con NRT, la propuesta es totalmente independiente. La sintaxis se puede usar independientemente de las directivas de `#nullable`.

## <a name="detailed-design"></a>Diseño detallado 

### <a name="null-validation-parameter-syntax"></a>Sintaxis de parámetro de validación null
El operador de signo de exclamación, `!`, se puede colocar después de un nombre de parámetro en una C# lista de parámetros y esto hará que el compilador emita el código de comprobación de `null` estándar para ese parámetro. Esto se conoce como `null` sintaxis de parámetros de validación. Por ejemplo:

``` csharp
void M(string name!) {
    ...
}
```

Se traducirá en:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

La comprobación de `null` generada se realizará antes que cualquier código creado por el desarrollador en el método. Cuando varios parámetros contienen el operador `!`, las comprobaciones se producirán en el mismo orden en que se declaran los parámetros.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

La comprobación será específica para la igualdad de referencia en `null`, no invoca `==` ni ningún operador definido por el usuario. Esto también significa que el operador `!` solo se puede Agregar a los parámetros cuyo tipo se puede probar para determinar si son iguales en `null`. Esto significa que no se puede usar en un parámetro cuyo tipo se sabe que es un tipo de valor.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

En el caso de un constructor, la validación de `null` se realizará antes que cualquier otro código del constructor. Esto incluye: 

- Encadenar a otros constructores con `this` o `base` 
- Inicializadores de campo que se producen implícitamente en el constructor

Por ejemplo:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

Se traducirá aproximadamente a lo siguiente:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Nota: este no es un C# código válido, sino solo una aproximación de lo que hace la implementación. 

La sintaxis del parámetro de validación `null` también será válida en las listas de parámetros lambda. Esto es válido incluso en la sintaxis de parámetro único que carece de paréntesis.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

La sintaxis también es válida en los parámetros de los métodos de iterador. A diferencia de otro código en el iterador, la `null` validación se producirá cuando se invoque el método iterador, no cuando se recorre el enumerador subyacente. Esto se aplica a los iteradores tradicionales o `async`.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

El operador `!` solo se puede usar para las listas de parámetros que tienen un cuerpo de método asociado. Esto significa que no se puede utilizar en una definición de método `abstract`, `interface`, `delegate` o `partial`.

### <a name="extending-is-null"></a>La extensión es null
Los tipos para los que la expresión `is null` es válida se extenderán para incluir parámetros de tipo sin restricciones. Esto le permitirá rellenar el intento de comprobar `null` en todos los tipos que sean válidos para una comprobación de `null`. En concreto, los tipos que no se conocen definitivamente como tipos de valor. Por ejemplo, los parámetros de tipo que están restringidos a `struct` no se pueden usar con esta sintaxis.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

El comportamiento de `is null` en un parámetro de tipo será el mismo que el de `== null` hoy. En los casos en los que se crea una instancia del parámetro de tipo como un tipo de valor, el código se evaluará como `false`. En los casos en los que se trata de un tipo de referencia, el código realizará una comprobación de `is null` adecuada.

### <a name="intersection-with-nullable-reference-types"></a>Intersección con tipos de referencia que aceptan valores NULL
Cualquier parámetro que tenga un operador de `!` aplicado a su nombre comenzará con el estado que acepta valores NULL que no se `null`. Esto es así incluso si el tipo del propio parámetro es potencialmente `null`. Esto puede ocurrir con un tipo que acepta valores NULL explícitamente, como por ejemplo `string?`, o con un parámetro de tipo sin restricciones. 

Cuando una sintaxis de `!` en parámetros se combina con un tipo que acepta valores NULL explícitamente en el parámetro, el compilador emitirá una advertencia:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Problemas abiertos
None

## <a name="considerations"></a>Consideraciones

### <a name="constructors"></a>Constructores
La generación de código para constructores significa que hay un cambio de comportamiento pequeño pero observable al pasar de la validación de `null` estándar hoy y la sintaxis del parámetro de validación `null` (`!`). La comprobación de `null` en la validación estándar se produce después de los inicializadores de campo y de las llamadas a `base` o `this`. Esto significa que un desarrollador no puede migrar necesariamente el 100% de la validación de `null` a la nueva sintaxis. Los constructores requieren al menos alguna inspección.

Después de la explicación, se decidió que es muy poco probable que se produzcan problemas de adopción significativos. Es más lógico que la comprobación de `null` se ejecute antes que cualquier lógica del constructor. Puede volver a visitar si se detectan problemas de compatibilidad significativos.

### <a name="warning-when-mixing--and-"></a>¿ADVERTENCIA al mezclar? etc!
Hubo una explicación larga sobre si se debe emitir o no una advertencia cuando se aplica la sintaxis de `!` a un parámetro que se escribe explícitamente en un tipo que acepta valores NULL. En la superficie, parece una declaración de absurdo del desarrollador, pero hay casos en los que las jerarquías de tipos podrían obligar a los desarrolladores a realizar esta situación. 

Considere la siguiente jerarquía de clase en una serie de ensamblados (suponiendo que todos se compilan con la comprobación de `null` habilitada):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

Aquí el autor de `C3` decidió agregar la validación de `null` al `o`de parámetros. Esto está completamente en línea con la finalidad de usar la característica.

Ahora Imagine en una fecha posterior, el autor de Assembly2 decide agregar la siguiente invalidación:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

Esto lo permiten los tipos de referencia que aceptan valores NULL, ya que es válido para que el contrato sea más flexible para las posiciones de entrada. En general, la característica NRT permite una co/contravarianza razonable en el parámetro o la nulabilidad. Sin embargo, el lenguaje realiza la comprobación de la co/contravarianza basándose en la invalidación más específica, no en la declaración original. Esto significa que el autor de Assembly3 recibirá una advertencia sobre el tipo de `o` no coincidente y tendrá que cambiar la firma a lo siguiente para eliminarla: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

En este momento, el autor de Assembly3 tiene algunas opciones:

- Pueden aceptar o suprimir la advertencia sobre `object?` y `object` no coinciden.
- Pueden aceptar o suprimir la advertencia sobre `object?` y `!` no coinciden.
- Solo pueden quitar la comprobación de validación del `null` (eliminar `!` y realizar la comprobación explícita)

Este es un escenario real, pero por ahora la idea es avanzar con la advertencia. Si la advertencia se produce con más frecuencia de lo previsto, podemos quitarla más adelante (el valor inverso no es cierto).

### <a name="implicit-property-setter-arguments"></a>Argumentos del establecedor de propiedad implícito
El argumento `value` de un parámetro es implícito y no aparece en ninguna lista de parámetros. Esto significa que no puede ser un destino de esta característica. La sintaxis del establecedor de propiedad se puede extender para incluir una lista de parámetros que permita aplicar el operador `!`. Pero esto reduce la idea de esta característica, lo que simplifica la validación de `null`. Como tal, el argumento `value` implícito no funcionará con esta característica.

## <a name="future-considerations"></a>Consideraciones futuras
None
