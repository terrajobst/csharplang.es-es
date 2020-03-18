---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484061"
---
# <a name="lambda-discard-parameters"></a>Parámetros de descarte de lambda

## <a name="summary"></a>Resumen

Permite el uso de Descartes (`_`) como parámetros de expresiones lambda y métodos anónimos.
Por ejemplo:
- lambdas: `(_, _) => 0`, `(int _, int _) => 0`
- métodos anónimos: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Motivación

No es necesario que los parámetros no usados tengan nombre. La intención de los descartes es clara, es decir, no se usan ni se descartan.

## <a name="detailed-design"></a>Diseño detallado

[Parámetros de método](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) En la lista de parámetros de una expresión lambda o un método anónimo con más de un parámetro denominado `_`, estos parámetros son descartar parámetros.
Nota: Si un solo parámetro se denomina `_`, es un parámetro normal por motivos de compatibilidad con versiones anteriores.

Los parámetros discard no introducen ningún nombre en los ámbitos.
Tenga en cuenta que esto implica que no se ocultan los nombres de `_` (subrayado).

[Nombres simples](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Si `K` es cero y el *simple_name* aparece dentro de un *bloque* y si el espacio de declaración de variable local del *bloque*[(o](basic-concepts.md#declarations)el de un *bloque*de inclusión) contiene una variable local, un parámetro (con la excepción de los parámetros de descarte) o una constante con el nombre `I`, el *simple_name* hace referencia a esa variable local, parámetro o constante y se clasifica como una variable o valor.

[Ámbitos](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) de A excepción de los parámetros de descarte, el ámbito de un parámetro declarado en una *lambda_expression* ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) es el *anonymous_function_body* de ese *lambda_expression* con la excepción de los parámetros de descarte, el ámbito de un parámetro declarado en una *anonymous_method_expression* ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) es el *bloque* de ese *anonymous_method_expression*.

## <a name="related-spec-sections"></a>Secciones de especificaciones relacionadas
- [Parámetros correspondientes](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
