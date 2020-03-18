---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483671"
---
# <a name="conditional-ref-expressions"></a>Expresiones de referencia condicional

El patrón de enlazar una variable de referencia a una u otra expresión condicional no se expresa actualmente en C#.

La solución habitual es introducir un método como:

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

Tenga en cuenta que esto no es un reemplazo exacto de ternario, ya que todos los argumentos deben evaluarse en el sitio de llamada.

Lo siguiente no funcionará según lo esperado:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

La sintaxis propuesta sería la siguiente:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

El intento anterior con "Choice" se puede escribir _correctamente_ con Ref ternario como:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

La diferencia de Choice es que se tiene acceso a las expresiones de consecuencia y alternativas de una manera _realmente_ condicional, por lo que no vemos un bloqueo si ```arr == null```

La referencia ternaria es simplemente un ternario, donde tanto la alternativa como la consecuencia son refs. De forma natural, será necesario que los operandos de consecuencia/alternativos sean LValues. También se requerirá que la consecuencia y la alternativa tengan tipos que sean convertibles entre sí.

El tipo de la expresión se calculará de forma similar a la del ternario normal. es decir,. en caso de que la consecuencia y la alternativa tengan identidad convertible, pero tipos diferentes, se aplicarán las reglas de combinación de tipos existentes.

De forma segura a la devolución se asumirán con cautela los operandos condicionales. Si no es seguro devolver todo lo que no es seguro para devolver.

Ref ternario es un valor l y, como tal, se puede pasar, asignar o devolver por referencia;

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
```

La referencia ternaria se puede usar también en un contexto normal (no Ref). Aunque no sería común, ya que también podía usar un ternario normal.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Notas de implementación: 

La complejidad de la implementación parece ser el tamaño de una corrección de errores de moderado a grande. -I. E no es muy caro.
No creo que sea necesario realizar cambios en la sintaxis o el análisis.
No hay ningún efecto en los metadatos o la interoperabilidad. La característica se basa completamente en expresiones.
No hay ningún efecto en la depuración/PDB
