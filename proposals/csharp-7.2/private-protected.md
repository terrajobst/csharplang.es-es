---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484079"
---
# <a name="private-protected"></a>privado protegido

* [x] propuesto
* [x] prototipo: [completo](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] implementación: [completa](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] Especificación: [completa](#detailed-design)

## <a name="summary"></a>Resumen
[summary]: #summary

Exponga el nivel de accesibilidad `protectedAndInternal` de C# CLR en como `private protected`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Hay muchas circunstancias en las que una API contiene miembros que solo están diseñados para ser implementados y utilizados por las subclases contenidas en el ensamblado que proporciona el tipo. Aunque CLR proporciona un nivel de accesibilidad para ese propósito, no está disponible en C#. Por lo tanto, los propietarios de la API deben usar la protección de `internal` y autodisciplina o un analizador personalizado, o bien usar `protected` con documentación adicional que explique que, mientras que el miembro aparece en la documentación pública para el tipo, no pretende formar parte de la API pública.  Para ver ejemplos de este último, consulte los miembros del `CSharpCompilationOptions` de Roslyn cuyos nombres empiezan por `Common`.

Proporcionar compatibilidad directamente con este nivel de acceso C# en permite expresar estas circunstancias de forma natural en el idioma.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>`private protected` modificador de acceso

Se propone agregar una nueva combinación de modificadores de acceso `private protected` (que puede aparecer en cualquier orden entre los modificadores). Esto se asigna a la noción de CLR de protectedAndInternal y toma en préstamo la misma sintaxis que se usa actualmente en [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility).

Se puede tener acceso a un miembro declarado `private protected` dentro de una subclase de su contenedor si esa subclase está en el mismo ensamblado que el miembro.

Modificamos la especificación del lenguaje como se indica a continuación (adiciones en negrita). Los números de sección no se muestran a continuación, ya que pueden variar en función de la versión de la especificación en la que está integrado.

-----

> La accesibilidad declarada de un miembro puede ser una de las siguientes:
- Público, que se selecciona mediante la inclusión de un modificador Public en la declaración de miembro. El significado intuitivo de Public es "acceso no limitado".
- Protected, que se selecciona mediante la inclusión de un modificador protegido en la declaración de miembro. El significado intuitivo de Protected es el "acceso limitado a la clase contenedora o a los tipos derivados de la clase contenedora".
- Internal, que se selecciona mediante la inclusión de un modificador Internal en la declaración de miembro. El significado intuitivo de Internal es el "acceso limitado a este ensamblado".
- Protected internal, que se selecciona mediante la inclusión de un modificador protegido y otro interno en la declaración de miembro. El significado intuitivo de protected internal es "accesible dentro de este ensamblado, así como los tipos derivados de la clase contenedora".
- **Private Protected, que se selecciona mediante la inclusión de un modificador Private y protected en la declaración de miembro. El significado intuitivo de Private Protected es "accesible dentro de este ensamblado por los tipos derivados de la clase contenedora".**

-----

> Dependiendo del contexto en el que tenga lugar una declaración de miembro, solo se permiten determinados tipos de accesibilidad declarada. Además, cuando una declaración de miembro no incluye modificadores de acceso, el contexto en el que se produce la declaración determina la accesibilidad declarada predeterminada. 
- Los espacios de nombres tienen implícitamente una accesibilidad declarada pública. No se permiten modificadores de acceso en las declaraciones de espacio de nombres.
- Los tipos declarados directamente en unidades de compilación o espacios de nombres (en lugar de dentro de otros tipos) pueden tener accesibilidad declarada pública o interna y tener como valor predeterminado la accesibilidad declarada interna.
- Los miembros de clase pueden tener cualquiera de los cinco tipos de accesibilidad declarada y tienen como valor predeterminado la accesibilidad declarada privada. [Nota: un tipo declarado como miembro de una clase puede tener cualquiera de los cinco tipos de accesibilidad declarada, mientras que un tipo declarado como miembro de un espacio de nombres solo puede tener acceso declarado público o interno. finalizar Nota]
- Los miembros de struct pueden tener accesibilidad declarada pública, interna o privada y de forma predeterminada en Private, ya que los Structs están sellados implícitamente. Los miembros de estructura introducidos en un struct (es decir, no heredados por ese struct) no pueden tener accesibilidad declarada protegida *,* ~~interna protegida~~ **o privada** . [Nota: un tipo declarado como miembro de un struct puede tener una accesibilidad declarada pública, interna o privada, mientras que un tipo declarado como miembro de un espacio de nombres solo puede tener una accesibilidad declarada pública o interna. finalizar Nota]
- Los miembros de interfaz tienen implícitamente una accesibilidad declarada pública. No se permiten modificadores de acceso en las declaraciones de miembros de interfaz.
- Los miembros de enumeración tienen implícitamente una accesibilidad declarada pública. No se permiten modificadores de acceso en las declaraciones de miembros de enumeración.

-----

> El dominio de accesibilidad de un miembro anidado M declarado en un tipo T dentro de un programa P, se define de la manera siguiente (teniendo en cuenta que el propio M podría ser un tipo):
- Si la accesibilidad declarada de M es pública, el dominio de accesibilidad de M es el dominio de accesibilidad de T.
- Si la accesibilidad declarada de M es interna protegida, permita que D sea la Unión del texto del programa de P y el texto del programa de cualquier tipo derivado de T, que se declara fuera de P. El dominio de accesibilidad de M es la intersección del dominio de accesibilidad de T con D.
- **Si la accesibilidad declarada de M es privada protegida, permita que D sea la intersección del texto del programa de P y el texto del programa de cualquier tipo derivado de T. El dominio de accesibilidad de M es la intersección del dominio de accesibilidad de T con D.**
- Si la accesibilidad declarada de M está protegida, permita que D sea la Unión del texto del programa de T y el texto del programa de cualquier tipo derivado de T. El dominio de accesibilidad de M es la intersección del dominio de accesibilidad de T con D.
- Si la accesibilidad declarada de M es interna, el dominio de accesibilidad de M es la intersección del dominio de accesibilidad de T con el texto de programa de P.
- Si la accesibilidad declarada de M es privada, el dominio de accesibilidad de M es el texto de programa de T.

-----

> Cuando se tiene acceso a un miembro de instancia **protegido o privado** protegido fuera del texto del programa de la clase en la que se declara, y cuando se tiene acceso a un miembro de instancia interno protegido fuera del texto del programa en el que se declara, el acceso se realiza dentro de una declaración de clase que se deriva de la clase en la que se declara. Además, el acceso debe realizarse a través de una instancia de ese tipo de clase derivada o de un tipo de clase construido a partir de él. Esta restricción evita que una clase derivada tenga acceso a miembros protegidos de otras clases derivadas, incluso cuando los miembros se heredan de la misma clase base.

-----

> Los modificadores de acceso permitidos y el acceso predeterminado de una declaración de tipo dependen del contexto en el que se produce la declaración (§ 9.5.2):
- Los tipos declarados en unidades de compilación o espacios de nombres pueden tener acceso público o interno. El valor predeterminado es acceso interno.
- Los tipos declarados en las clases pueden tener acceso public, protected internal, **Private**Protected, Protected, Internal o Private. El valor predeterminado es Private Access.
- Los tipos declarados en Structs pueden tener acceso público, interno o privado. El valor predeterminado es Private Access.

-----

> Una declaración de clase estática está sujeta a las siguientes restricciones:
- Una clase estática no debe incluir un modificador Sealed o Abstract. (Sin embargo, puesto que no se puede crear una instancia de una clase estática o derivarse de ella, se comporta como si fuera Sealed y Abstract).
- Una clase estática no debe incluir una especificación de clase base (§ 16.2.5) y no puede especificar explícitamente una clase base o una lista de interfaces implementadas. Una clase estática hereda implícitamente del tipo Object.
- Una clase estática solo debe contener miembros estáticos (§ 16.4.8). [Nota: todas las constantes y los tipos anidados se clasifican como miembros estáticos. finalizar Nota]
- Una clase estática no debe tener miembros con accesibilidad declarada interna protegida **, privada** o protegida.

> Es un error en tiempo de compilación infringir cualquiera de estas restricciones. 

-----

> Una declaración de miembro de clase puede tener cualquiera de los ~~cinco~~**tipos posibles de** accesibilidad declarada (§ 9.5.2): Public, **Private Protected**, protected internal, Protected, Internal o Private. A excepción de las combinaciones **protegidas internas y privadas** **protegidas, se**trata de un error en tiempo de compilación para especificar más de un modificador de acceso. Cuando una declaración de miembro de clase no incluye modificadores de acceso, se supone Private.

-----

> Los tipos no anidados pueden tener una accesibilidad declarada pública o interna y tener la accesibilidad declarada interna de forma predeterminada. Los tipos anidados también pueden tener estas formas de accesibilidad declarada, además de una o varias formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o un struct:
- Un tipo anidado que se declara en una clase puede tener ~~cinco~~**formas de** accesibilidad declarada (Public, **Private Protected**, protected internal, Protected, Internal o Private) y, al igual que otros miembros de clase, tiene como valor predeterminado la accesibilidad declarada privada.
- Un tipo anidado que se declara en un struct puede tener cualquiera de las tres formas de accesibilidad declarada (Public, Internal o Private) y, al igual que otros miembros de struct, tiene como valor predeterminado la accesibilidad declarada privada.

-----

> El método invalidado por una declaración de invalidación se conoce como el método base invalidado para un método de invalidación M declarado en una clase C, el método base invalidado se determina examinando cada tipo de clase base de C, empezando por el tipo de clase base directa de C y Continuando con cada tipo de clase base directa sucesiva, hasta que en un tipo de clase base determinado se encuentra al menos un método accesible que tiene la misma firma que M después de la sustitución de los argumentos de tipo. Con el fin de localizar el método base invalidado, se considera que un método es accesible si es público, si está protegido, si está **protegido internamente** o si es interno **o privado protegido** y declarado en el mismo programa que C.

-----

> El uso de los modificadores de descriptor de acceso se rige por las siguientes restricciones:
- No se puede usar un modificador de descriptor de acceso en una interfaz o en una implementación explícita de un miembro de interfaz.
- En el caso de una propiedad o un indizador que no tenga modificador override, solo se permite un modificador de descriptor de acceso si la propiedad o indizador tiene un descriptor de acceso get y set y, a continuación, solo se permite en uno de estos descriptores de acceso.
- En el caso de una propiedad o indizador que incluya un modificador override, un descriptor de acceso debe coincidir con el modificador de descriptor de acceso, si existe, del descriptor de acceso que se va a reemplazar.
- El modificador de descriptor de acceso debe declarar una accesibilidad que sea estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el propio indexador. Para ser precisos:
  - Si la propiedad o el indexador tienen una accesibilidad declarada Public, el modificador de descriptor de acceso puede ser **Private Protected**, Internal Protected, Internal, Protected o Private.
  - Si la propiedad o indizador tiene una accesibilidad declarada de Internal Protected, el modificador de descriptor de acceso puede ser **Private Protected**, Internal, Protected o Private.
  - Si la propiedad o el indexador tienen una accesibilidad declarada de Internal o Protected, el modificador de descriptor de acceso debe ser **Private Protected o** Private.
  - **Si la propiedad o indizador tiene una accesibilidad declarada de Private Protected, el modificador de descriptor de acceso debe ser privado.**
  - Si la propiedad o indizador tiene una accesibilidad declarada de Private, no se puede usar ningún modificador de descriptor de acceso.

-----

> Puesto que no se admite la herencia para Structs, la accesibilidad declarada de un miembro de struct no puede ser Protected, **Private Protected**o protected internal.

-----

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Una alternativa sería el aprovisionamiento de una API que combina un atributo y un analizador. El programador coloca el atributo en un miembro de `internal` para indicar que el miembro está diseñado para usarse solo en las subclases y el analizador comprueba que se obedecen esas restricciones. 

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

La implementación se completa en gran medida. El único elemento de trabajo abierto está borrando una especificación correspondiente para VB.

## <a name="design-meetings"></a>Design Meetings

TBD
