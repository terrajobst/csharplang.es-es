---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483491"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>Suprimir la emisión de `localsinit` marca.

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Permite suprimir la emisión de `localsinit` marca mediante `SkipLocalsInitAttribute` atributo. 

## <a name="motivation"></a>Motivación
[motivation]: #motivation


### <a name="background"></a>Información previa
Por las variables locales de especificación de CLR que no contienen referencias no se inicializan en un valor determinado por la máquina virtual o JIT. La lectura de estas variables sin inicialización tiene seguridad de tipos, pero, de lo contrario, el comportamiento es indefinido y específico de la implementación. Normalmente, las variables locales sin inicializar contienen los valores que se dejaron en la memoria que ocupa ahora el marco de pila. Esto podría provocar un comportamiento no determinista y difícil reproducir errores. 

Hay dos maneras de "asignar" una variable local: 
- almacenando un valor o 
- al especificar `localsinit` marca que fuerza a todo lo que se asigna, el grupo de memoria local debe inicializarse con ceros Nota: Esto incluye variables locales y datos de `stackalloc`.    

No se recomienda el uso de datos no inicializados y no se permite en código comprobable. Aunque podría ser posible demostrar que, por medio del análisis de flujo, se permite que el algoritmo de comprobación sea conservador y simplemente se requiere que se establezca `localsinit`.

Históricamente C# , el compilador emite `localsinit` marca en todos los métodos que declaran variables locales.

Aunque C# emplea el análisis de asignación desfinita que es más estricto que el que requeriría la especificaciónC# de CLR (también debe considerar el ámbito de las variables locales), no se garantiza estrictamente que el código resultante se pueda comprobar formalmente:
- Es posible C# que las reglas y CLR no estén de acuerdo en si pasar un argumento local como `out` es un `use`.
- Es posible C# que las reglas y CLR no concuerden en el tratamiento de ramas condicionales cuando se conocen las condiciones (propagación constante).
- CLR podría simplemente requerir `localinits`, ya que se permite.  

### <a name="problem"></a>Problema
En las aplicaciones de alto rendimiento, el costo de la inicialización de cero forzada podría ser apreciable. Es especialmente evidente cuando se usa `stackalloc`.

En algunos casos, JIT puede Elide la inicialización cero inicial de las variables locales individuales cuando dicha inicialización se "elimina" mediante asignaciones posteriores. No todas JITs lo hacen y esta optimización tiene límites. No ayuda con `stackalloc`.

Para ilustrar que el problema es real, hay un error conocido en el que un método que no contiene ninguna `IL` variables locales no tendría `localsinit` marca. Los usuarios ya están aprovechando el error colocando `stackalloc` en estos métodos, intencionalmente para evitar los costos de inicialización. Es decir, a pesar de que la ausencia de `IL` variables locales es una métrica inestable y puede variar en función de los cambios en la estrategia CODEGEN. El error debe corregirse y los usuarios deben obtener una forma más documentada y confiable de suprimir la marca. 

## <a name="detailed-design"></a>Diseño detallado

Permite especificar `System.Runtime.CompilerServices.SkipLocalsInitAttribute` como una manera de indicar al compilador que no emita `localsinit` marca.
 
El resultado final de este es que las variables locales no pueden inicializarse en cero mediante el JIT, que en la mayoría de los casos no se puede C#observar en.  
Además de que `stackalloc` datos no se inicializarán en cero. Eso es claramente observable, pero también es el escenario más motivador.

Los destinos de atributo permitidos y reconocidos son: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface``Constructor`. Sin embargo, el compilador no requerirá que el atributo esté definido con los destinos enumerados, ni será de interés en qué ensamblado está definido el atributo. 

Cuando se especifica el atributo en un contenedor (`class`, `module`, que contiene el método para un método anidado,...), la marca afecta a todos los métodos contenidos en el contenedor.

Los métodos sintetizados "heredan" la marca del contenedor lógico/propietario. 

La marca solo afecta a la estrategia CODEGEN para los cuerpos de método reales. es decir,. la marca no tiene ningún efecto en los métodos abstractos y no se propaga a los métodos de reemplazo/implementación.

Esto es explícitamente una **_característica del compilador_** y **_no una característica del lenguaje_** .  
Del mismo modo que los modificadores de línea de comandos del compilador, la característica controla los detalles de implementación de una C# estrategia de CODEGEN determinada y no necesita ser necesaria para la especificación.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

- Es posible que los compiladores anteriores y otros no respeten el atributo.
Omitir el atributo es un comportamiento compatible. Solo puede dar lugar a una ligera disminución del rendimiento.

- El código sin `localinits` marca puede desencadenar errores de comprobación.
Los usuarios que solicitan esta característica generalmente no se ven preocupadas por la capacidad de comprobación. 
 
- Aplicar el atributo en niveles superiores a un método individual tiene un efecto no local, que es observable cuando se utiliza `stackalloc`. Aún así, este es el escenario más solicitado.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- omitir `localinits` marca cuando el método se declara en `unsafe` contexto. Esto podría provocar un cambio de comportamiento silencioso y peligroso de determinista a no determinista en el caso de `stackalloc`.

- omitir la marca de `localinits` siempre.
Incluso peor que el anterior.

- omita `localinits` marca a menos que se use `stackalloc` en el cuerpo del método.
No aborda el escenario más solicitado y puede desactivar el código sin ninguna opción para revertirlo de nuevo.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- ¿Se debe emitir realmente el atributo a los metadatos? 

## <a name="design-meetings"></a>Design Meetings

Ninguno todavía. 