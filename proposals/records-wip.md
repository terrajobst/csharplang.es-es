---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2020
ms.locfileid: "79484133"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="a69ad-101">Registra el trabajo en curso</span><span class="sxs-lookup"><span data-stu-id="a69ad-101">Records Work-in-Progress</span></span>

<span data-ttu-id="a69ad-102">A diferencia de las demás propuestas de registros, esto no es una propuesta en sí misma, sino un trabajo en curso diseñado para registrar las decisiones de diseño de consenso para la característica de registros.</span><span class="sxs-lookup"><span data-stu-id="a69ad-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="a69ad-103">Los detalles de la especificación se agregarán según sea necesario para resolver las preguntas.</span><span class="sxs-lookup"><span data-stu-id="a69ad-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="a69ad-104">La sintaxis de un registro se propone para agregarse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="a69ad-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="a69ad-105">El `attributes` no terminal también permitirá un nuevo atributo contextual, `data`.</span><span class="sxs-lookup"><span data-stu-id="a69ad-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="a69ad-106">Una clase (struct) declarada con una lista de parámetros o un modificador de `data` se denomina clase de registro (struct de registros), cualquiera de los cuales es un tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="a69ad-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="a69ad-107">Es un error declarar un tipo de registro sin una lista de parámetros y el modificador `data`.</span><span class="sxs-lookup"><span data-stu-id="a69ad-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="a69ad-108">Miembros de un tipo de registro</span><span class="sxs-lookup"><span data-stu-id="a69ad-108">Members of a record type</span></span>

<span data-ttu-id="a69ad-109">Además de los miembros declarados en el cuerpo de clase, un tipo de registro tiene los siguientes miembros adicionales:</span><span class="sxs-lookup"><span data-stu-id="a69ad-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="a69ad-110">Constructor principal</span><span class="sxs-lookup"><span data-stu-id="a69ad-110">Primary Constructor</span></span>

<span data-ttu-id="a69ad-111">Un tipo de registro tiene un constructor público cuya firma corresponde a los parámetros de valor de la declaración de tipos.</span><span class="sxs-lookup"><span data-stu-id="a69ad-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="a69ad-112">Esto se denomina constructor principal para el tipo y hace que se suprima el constructor predeterminado declarado implícitamente.</span><span class="sxs-lookup"><span data-stu-id="a69ad-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="a69ad-113">Es un error tener un constructor principal y un constructor con la misma firma ya presente en la clase.</span><span class="sxs-lookup"><span data-stu-id="a69ad-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="a69ad-114">En tiempo de ejecución del constructor principal</span><span class="sxs-lookup"><span data-stu-id="a69ad-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="a69ad-115">ejecuta los inicializadores de campo de instancia que aparecen en el cuerpo de la clase; y, a continuación, invoca el constructor de clase base sin argumentos.</span><span class="sxs-lookup"><span data-stu-id="a69ad-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="a69ad-116">Inicializa los campos de respaldo generados por el compilador para las propiedades correspondientes a los parámetros de valor (si estas propiedades son proporcionadas por el compilador; consulte [propiedades sintetizadas](#Synthesized Properties)).</span><span class="sxs-lookup"><span data-stu-id="a69ad-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="a69ad-117">[] TODO: agregar sintaxis de llamada base y especificación sobre cómo elegir el constructor base mediante la resolución de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="a69ad-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="a69ad-118">Propiedades</span><span class="sxs-lookup"><span data-stu-id="a69ad-118">Properties</span></span>

<span data-ttu-id="a69ad-119">Para cada parámetro de registro de una declaración de tipo de registro, hay un miembro de propiedad público correspondiente cuyo nombre y tipo se toman de la declaración del parámetro value.</span><span class="sxs-lookup"><span data-stu-id="a69ad-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="a69ad-120">Si no hay ninguna propiedad concreta (es decir, no abstracta) con un descriptor de acceso get y con este nombre y tipo se declara o hereda explícitamente, el compilador lo genera de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="a69ad-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="a69ad-121">Para una clase de registro o de registro:</span><span class="sxs-lookup"><span data-stu-id="a69ad-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="a69ad-122">Se crea una propiedad automática de solo obtención pública.</span><span class="sxs-lookup"><span data-stu-id="a69ad-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="a69ad-123">Su valor se inicializa durante la construcción con el valor del parámetro del constructor principal correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a69ad-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="a69ad-124">Se invalida cada descriptor de acceso get de la propiedad abstracta heredada "matching".</span><span class="sxs-lookup"><span data-stu-id="a69ad-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="a69ad-125">Miembros de igualdad</span><span class="sxs-lookup"><span data-stu-id="a69ad-125">Equality members</span></span>

<span data-ttu-id="a69ad-126">Los tipos de registros producen implementaciones sintetizadas para los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a69ad-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="a69ad-127">invalidación de `object.GetHashCode()`, a menos que esté sellada o proporcionada por el usuario</span><span class="sxs-lookup"><span data-stu-id="a69ad-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="a69ad-128">invalidación de `object.Equals(object)`, a menos que esté sellada o proporcionada por el usuario</span><span class="sxs-lookup"><span data-stu-id="a69ad-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="a69ad-129">`T Equals(T)` método, donde `T` es el tipo actual</span><span class="sxs-lookup"><span data-stu-id="a69ad-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="a69ad-130">`T Equals(T)` se especifica para realizar la igualdad de valores, comparando la propiedad con el mismo nombre que cada parámetro del constructor principal con la propiedad correspondiente del otro tipo.</span><span class="sxs-lookup"><span data-stu-id="a69ad-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="a69ad-131">`object.Equals` realiza el equivalente de</span><span class="sxs-lookup"><span data-stu-id="a69ad-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
