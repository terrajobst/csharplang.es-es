---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483569"
---
# <a name="declaration-expressions"></a>Expresiones de declaración

Admitir las asignaciones de declaraciones como expresiones.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Permita la inicialización en el punto de declaración en más casos, simplifique el código y permita el uso de `var`.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

Permita las declaraciones de `ref` argumentos, similar a `out var`.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Las expresiones se extienden para incluir la asignación de declaración. La prioridad es la misma que la asignación.

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

La asignación de declaración es de un solo local.

El tipo de una expresión de asignación de declaración es el tipo de la declaración.
Si el tipo es `var`, el tipo deducido es el tipo de la expresión de inicialización. 

La expresión de asignación de declaración puede ser un valor l, para `ref` valores de argumento en particular.

Si la expresión de asignación de declaración declara un tipo de valor y la expresión es un valor r, el valor de la expresión es una copia.

La expresión de asignación de declaración puede declarar un `ref` local.
Existe una ambigüedad cuando se usa `ref` para una expresión de declaración en un argumento de `ref`.
El inicializador de variable local determina si la declaración es una `ref` local.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

El ámbito de las variables locales declaradas en expresiones de asignación de declaración es el mismo que el ámbito de las expresiones de declaración correspondientes de C # 7.0.

Es un error en tiempo de compilación hacer referencia a una variable local en el texto que precede a la expresión de declaración.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives
Sin cambios. Esta característica es simplemente sintácticamente breve después de todo.

Expresiones de secuencias más generales: vea [#377](https://github.com/dotnet/csharplang/issues/377).

Para permitir el uso de `var` en más casos, permita una declaración y asignación independientes de `var` variables locales y deduzca el tipo de las asignaciones de todas las rutas de acceso de código.

## <a name="see-also"></a>Consulte también
[see-also]: #see-also
Vea expresión de declaración básica en [#595](https://github.com/dotnet/csharplang/issues/595).

Vea declaración de desconstrucción en la característica de [desconstrucción](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .
