---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484037"
---

# <a name="discriminated-unions--enum-class"></a>Uniones/`enum class` discriminadas

`enum class`es un nuevo tipo de declaración de tipos, que a veces se denomina uniones discriminadas, donde cada instancia posible del tipo se muestra y cada instancia no se superpone.

Un `enum class` se define con la sintaxis siguiente:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Sintaxis de ejemplo:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Semántica

Una definición de `enum class` define un tipo raíz, que es una clase abstracta del mismo nombre que la declaración de `enum class`, y un conjunto de miembros, cada uno de los cuales tiene un tipo que es un subtipo del tipo raíz. Si hay varias definiciones de `partial enum class`, todos los miembros se considerarán miembros de la definición de clase enum. A diferencia de una definición de clase abstracta definida por el usuario, el tipo de raíz `enum class` es parcial de forma predeterminada y se define para tener un constructor *privado* predeterminado sin parámetros.

Tenga en cuenta que, dado que el tipo raíz se define como una clase abstracta parcial, también se pueden agregar definiciones parciales del *tipo raíz* , donde se permiten los formatos de sintaxis estándar para un cuerpo de clase.
Sin embargo, ningún tipo puede heredar directamente del tipo raíz en cualquier declaración, aparte de los especificados como miembros de `enum class`. Además, no se permiten constructores definidos por el usuario para el tipo raíz.

Hay cuatro tipos de declaraciones de miembros de `enum class`:

* Miembros de clase simples

* miembros de clase complejos

* miembros de `enum class`

* miembros de valor.

### <a name="simple-class-members"></a>Miembros de clase simples

Una declaración de miembro de clase simple define una nueva clase de "registro" anidada (no definida intencionadamente en este documento) con el mismo nombre. La clase anidada hereda del tipo raíz.

Dado el código de ejemplo anterior,

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

la declaración de `enum class` tiene una semántica equivalente a la siguiente declaración

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>miembros de clase complejos

También puede anidar una declaración de clase completa debajo de una declaración de `enum class`. Se tratará como una clase anidada del tipo raíz. La sintaxis permite cualquier declaración de clase, pero es necesaria para que el miembro de clase compleja herede de la declaración de `enum class` contenedora directa. 

### <a name="enum-class-members"></a>miembros de `enum class`

`enum classes` se pueden anidar entre sí, por ejemplo,

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

Esto es casi idéntico a la semántica de un `enum class`de nivel superior, salvo que la clase de enumeración anidada define un tipo de raíz anidada y todo lo que está debajo de la clase de enumeración anidada es un subtipo del tipo de raíz anidado, en lugar del tipo raíz de nivel superior.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Miembros de valor

`enum classes` también puede contener miembros de valor. Los miembros de valor definen propiedades estáticas Get-Only públicas en el tipo raíz que también devuelven el tipo raíz, por ejemplo,

```C#
enum class Color
{
    Red,
    Green
}
```

tiene propiedades equivalentes a

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

La semántica completa se considera un detalle de implementación, pero se garantiza que se devolverá una instancia única para cada propiedad, y se devolverá la misma instancia en las invocaciones repetidas.


### <a name="switch-expression-and-patterns"></a>Cambiar expresión y patrones

Hay algunos ajustes propuestos para la coincidencia de patrones y la expresión switch para controlar `enum classes`. Las expresiones switch ya pueden coincidir con tipos a través del patrón variable, pero para actualmente para tipos de referencia, no se considera que ningún conjunto de brazos de conmutador de la expresión switch está completo, salvo para la coincidencia con el tipo estático del argumento o un subtipo.

Las expresiones switch se cambiarán de modo que, si el tipo raíz de un `enum class` es el tipo estático del argumento de la expresión switch, y hay un conjunto de patrones que coinciden con todos los miembros de la enumeración, el modificador se considerará exhaustivo.

Dado que los miembros de valor no son constantes y no definen nuevos tipos estáticos, actualmente no pueden coincidir con el patrón. Para que esto sea posible, se agregará un nuevo patrón con la sintaxis de patrón de constante para permitir la coincidencia con `enum class` miembros de valor. La coincidencia se define para que se realice correctamente si y solo si el argumento de la coincidencia de patrón y el valor devuelto por el miembro de valor de `enum class` serían iguales a la referencia, aunque la implementación no es necesaria para realizar esta comprobación.


## <a name="open-questions"></a>Preguntas abiertas

- [] ¿Qué indica el algoritmo de tipo común sobre `enum class` miembros? ¿Es este código válido?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] Agregar un nuevo patrón solo para los miembros de valor parece pesado. ¿Hay una construcción de versión más general que tenga sentido?
    - [] Los miembros de valor tampoco se asignan correctamente a una construcción de clase anidada paralela debido a esto

- [] Se está cambiando a un argumento con un tipo estático `enum class` que se garantiza que sea constante.

- [] ¿Hay una manera de hacer que `enum class`no se consideren completos en la expresión switch? ¿Tiene `virtual`?

- [] ¿Qué modificadores se deben permitir en `enum class`?