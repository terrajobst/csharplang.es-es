---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281962"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="51f05-101">Registra el trabajo en curso</span><span class="sxs-lookup"><span data-stu-id="51f05-101">Records Work-in-Progress</span></span>

<span data-ttu-id="51f05-102">A diferencia de las demás propuestas de registros, esto no es una propuesta en sí misma, sino un trabajo en curso diseñado para registrar las decisiones de diseño de consenso para la característica de registros.</span><span class="sxs-lookup"><span data-stu-id="51f05-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="51f05-103">Los detalles de la especificación se agregarán según sea necesario para resolver las preguntas.</span><span class="sxs-lookup"><span data-stu-id="51f05-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="51f05-104">La sintaxis de un registro se propone para agregarse de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="51f05-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="51f05-105">El `attributes` no terminal también permitirá un nuevo atributo contextual, `data`.</span><span class="sxs-lookup"><span data-stu-id="51f05-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="51f05-106">Una clase (struct) declarada con una lista de parámetros o un modificador de `data` se denomina clase de registro (struct de registros), cualquiera de los cuales es un tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="51f05-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="51f05-107">Es un error declarar un tipo de registro sin una lista de parámetros y el modificador `data`.</span><span class="sxs-lookup"><span data-stu-id="51f05-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="51f05-108">Miembros de un tipo de registro</span><span class="sxs-lookup"><span data-stu-id="51f05-108">Members of a record type</span></span>

<span data-ttu-id="51f05-109">Además de los miembros declarados en el cuerpo de clase, un tipo de registro tiene los siguientes miembros adicionales:</span><span class="sxs-lookup"><span data-stu-id="51f05-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="51f05-110">Constructor principal</span><span class="sxs-lookup"><span data-stu-id="51f05-110">Primary Constructor</span></span>

<span data-ttu-id="51f05-111">Un tipo de registro tiene un constructor público cuya firma corresponde a los parámetros de valor de la declaración de tipos.</span><span class="sxs-lookup"><span data-stu-id="51f05-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="51f05-112">Esto se denomina constructor principal para el tipo y hace que se suprima el constructor predeterminado declarado implícitamente.</span><span class="sxs-lookup"><span data-stu-id="51f05-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="51f05-113">Es un error tener un constructor principal y un constructor con la misma firma ya presente en la clase.</span><span class="sxs-lookup"><span data-stu-id="51f05-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="51f05-114">En tiempo de ejecución del constructor principal</span><span class="sxs-lookup"><span data-stu-id="51f05-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="51f05-115">ejecuta los inicializadores de campo de instancia que aparecen en el cuerpo de la clase; y, a continuación, invoca el constructor de clase base sin argumentos.</span><span class="sxs-lookup"><span data-stu-id="51f05-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="51f05-116">Inicializa los campos de respaldo generados por el compilador para las propiedades correspondientes a los parámetros de valor (si estas propiedades son proporcionadas por el compilador; consulte [propiedades sintetizadas](#Synthesized Properties)).</span><span class="sxs-lookup"><span data-stu-id="51f05-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="51f05-117">[] TODO: agregar sintaxis de llamada base y especificación sobre cómo elegir el constructor base mediante la resolución de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="51f05-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="51f05-118">Propiedades</span><span class="sxs-lookup"><span data-stu-id="51f05-118">Properties</span></span>

<span data-ttu-id="51f05-119">Para cada parámetro de registro de una declaración de tipo de registro, hay un miembro de propiedad público correspondiente cuyo nombre y tipo se toman de la declaración del parámetro value.</span><span class="sxs-lookup"><span data-stu-id="51f05-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="51f05-120">Si no hay ninguna propiedad concreta (es decir, no abstracta) con un descriptor de acceso get y con este nombre y tipo se declara o hereda explícitamente, el compilador lo genera de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="51f05-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="51f05-121">Para una clase de registro o de registro:</span><span class="sxs-lookup"><span data-stu-id="51f05-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="51f05-122">Se crea una propiedad automática de solo obtención pública.</span><span class="sxs-lookup"><span data-stu-id="51f05-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="51f05-123">Su valor se inicializa durante la construcción con el valor del parámetro del constructor principal correspondiente.</span><span class="sxs-lookup"><span data-stu-id="51f05-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="51f05-124">Se invalida cada descriptor de acceso get de la propiedad abstracta heredada "matching".</span><span class="sxs-lookup"><span data-stu-id="51f05-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="51f05-125">Miembros de igualdad</span><span class="sxs-lookup"><span data-stu-id="51f05-125">Equality members</span></span>

<span data-ttu-id="51f05-126">Los tipos de registros producen implementaciones sintetizadas para los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="51f05-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="51f05-127">invalidación de `object.GetHashCode()`, a menos que esté sellada o proporcionada por el usuario</span><span class="sxs-lookup"><span data-stu-id="51f05-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="51f05-128">invalidación de `object.Equals(object)`, a menos que esté sellada o proporcionada por el usuario</span><span class="sxs-lookup"><span data-stu-id="51f05-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="51f05-129">`T Equals(T)` método, donde `T` es el tipo actual</span><span class="sxs-lookup"><span data-stu-id="51f05-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="51f05-130">`T Equals(T)` se especifica para realizar la igualdad de valores, comparando la propiedad con el mismo nombre que cada parámetro del constructor principal con la propiedad correspondiente del otro tipo.</span><span class="sxs-lookup"><span data-stu-id="51f05-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="51f05-131">`object.Equals` realiza el equivalente de</span><span class="sxs-lookup"><span data-stu-id="51f05-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="51f05-132">Expresión `with`</span><span class="sxs-lookup"><span data-stu-id="51f05-132">`with` expression</span></span>

<span data-ttu-id="51f05-133">Una expresión de `with` es una expresión nueva con la sintaxis siguiente.</span><span class="sxs-lookup"><span data-stu-id="51f05-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="51f05-134">Una expresión `with` permite la "mutación no destructiva", diseñada para producir una copia de la expresión del receptor con modificaciones en las propiedades enumeradas en el `anonymous_object_initializer`.</span><span class="sxs-lookup"><span data-stu-id="51f05-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="51f05-135">Una expresión `with` válida tiene un receptor con un tipo que no es void.</span><span class="sxs-lookup"><span data-stu-id="51f05-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="51f05-136">El tipo de receptor debe contener un método de instancia accesible denominado `With` con los parámetros adecuados y el tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="51f05-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="51f05-137">Es un error si hay varios métodos `With` no invalidados.</span><span class="sxs-lookup"><span data-stu-id="51f05-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="51f05-138">Si hay varias invalidaciones de `With`, debe haber un método de `With` no invalidado, que es el método de destino.</span><span class="sxs-lookup"><span data-stu-id="51f05-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="51f05-139">De lo contrario, debe haber exactamente un método `With`.</span><span class="sxs-lookup"><span data-stu-id="51f05-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="51f05-140">En el lado derecho de la expresión de `with` es una `anonymous_object_initializer` con una secuencia de asignaciones con un campo o propiedad del receptor en el lado izquierdo de la asignación, y una expresión arbitraria en el lado derecho, que se puede convertir implícitamente al tipo del lado izquierdo del mismo.</span><span class="sxs-lookup"><span data-stu-id="51f05-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="51f05-141">Dado un método de `With` de destino, el tipo de valor devuelto debe ser el tipo del tipo de expresión de receptor o un tipo base de este.</span><span class="sxs-lookup"><span data-stu-id="51f05-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="51f05-142">Para cada parámetro del método `With`, debe haber un campo de instancia correspondiente accesible o una propiedad legible en el tipo de receptor con el mismo nombre y el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="51f05-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="51f05-143">Cada propiedad o campo del lado derecho de la expresión with también debe corresponder a un parámetro con el mismo nombre en el método `With`.</span><span class="sxs-lookup"><span data-stu-id="51f05-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="51f05-144">Dado un método `With` válido, la evaluación de una expresión de `with` es equivalente a llamar al método `With` con las expresiones de la `anonymous_object_initializer` sustituida por el parámetro con el mismo nombre que la propiedad del lado izquierdo.</span><span class="sxs-lookup"><span data-stu-id="51f05-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="51f05-145">Si no hay ninguna propiedad coincidente para un parámetro determinado en el `anonymous_object_initializer`, el argumento es la evaluación del campo o propiedad del mismo nombre en el receptor.</span><span class="sxs-lookup"><span data-stu-id="51f05-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="51f05-146">El orden de evaluación de los efectos secundarios es el siguiente, y cada expresión se evalúa exactamente una vez:</span><span class="sxs-lookup"><span data-stu-id="51f05-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="51f05-147">Expresión de receptor</span><span class="sxs-lookup"><span data-stu-id="51f05-147">Receiver expression</span></span>

2. <span data-ttu-id="51f05-148">Expresiones en el `anonymous_object_initializer`, en orden léxico</span><span class="sxs-lookup"><span data-stu-id="51f05-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="51f05-149">Evaluación de las propiedades que coinciden con los parámetros del método `With`, en orden de definición de los parámetros del método `With`.</span><span class="sxs-lookup"><span data-stu-id="51f05-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>