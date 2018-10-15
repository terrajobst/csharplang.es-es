# <a name="exceptions"></a>Excepciones

Las excepciones en C# proporcionan una forma estructurada, uniforme y con seguridad de tipos de control de nivel de sistema y nivel de aplicación las condiciones de error. El mecanismo de excepciones en C# es bastante similar a la de C++, con algunas diferencias importantes:

*  En C#, todas las excepciones deben representarse mediante una instancia de un tipo de clase derivado de `System.Exception`. En C++, cualquier valor de cualquier tipo puede utilizarse para representar una excepción.
*  En C#, un bloque finally ([la instrucción try](statements.md#the-try-statement)) puede usarse para escribir código de terminación que se ejecute en una ejecución normal y condiciones excepcionales. Dicho código es difícil de escribir en C++ sin duplicar código.
*  En C#, las excepciones de nivel de sistema, como desbordamiento, división por cero y null desreferencia también han definido las clases de excepción y se encuentran en un mismo nivel que las condiciones de error de nivel de aplicación.

## <a name="causes-of-exceptions"></a>Causas de excepciones

Se puede producir la excepción de dos maneras diferentes.

*  Un `throw` instrucción ([la instrucción throw](statements.md#the-throw-statement)) produce una excepción inmediatamente y de forma incondicional. Control nunca llega a la instrucción inmediatamente posterior a la `throw`.
*  Ciertas condiciones excepcionales que surgen durante el procesamiento de instrucciones de C# y expresión provocan una excepción en determinadas circunstancias, cuando la operación no se puede completar con normalidad. Por ejemplo, una operación de división de enteros ([operador de división](expressions.md#division-operator)) produce una `System.DivideByZeroException` si el denominador es cero. Consulte [clases de excepciones comunes](exceptions.md#common-exception-classes) para obtener una lista de las diferentes excepciones que pueden producirse en este modo.

## <a name="the-systemexception-class"></a>La clase System.Exception

La `System.Exception` clase es el tipo base de todas las excepciones. Esta clase tiene algunas propiedades importantes que comparten todas las excepciones:

*  `Message` es una propiedad de solo lectura de tipo `string` que contiene una descripción legible de la causa de la excepción.
*  `InnerException` es una propiedad de solo lectura de tipo `Exception`. Si su valor es distinto de null, hace referencia a la excepción que produjo la excepción actual, es decir, en que se ha producido la excepción actual en un bloque catch controla la `InnerException`. En caso contrario, su valor es null, que indica que esta excepción no fue causada por otra excepción. El número de objetos de excepción que se encadenan de esta manera puede ser arbitrario.

El valor de estas propiedades se puede especificar en las llamadas al constructor de instancia para `System.Exception`.

## <a name="how-exceptions-are-handled"></a>¿Cómo se controlan las excepciones

Las excepciones se controlan mediante un `try` instrucción ([la instrucción try](statements.md#the-try-statement)).

Cuando se produce una excepción, el sistema busca más cercano `catch` cláusula que pueda controlar la excepción, según lo determinado por el tipo de tiempo de ejecución de la excepción. En primer lugar, se busca el método actual para incluir léxicamente `try` instrucción y las cláusulas catch asociada de la instrucción try se consideran en orden. Si se produce un error, se busca el método que llamó al método actual para incluir léxicamente `try` instrucción que contenga el punto de la llamada al método actual. Esta búsqueda continúa hasta que un `catch` encontrar una cláusula que pueda controlar la excepción actual, una clase de excepción que sea de la misma clase o una clase base del tipo de tiempo de ejecución de la excepción de asignación de nombres. Un `catch` cláusula que no asigne nombre a una clase de excepción puede controlar cualquier excepción.

Una vez que se encuentra una cláusula catch coincidente, el sistema se prepara para transferir el control a la primera instrucción de la cláusula catch. Antes de comenzar la ejecución de la cláusula catch, el sistema ejecuta primero, en orden, cualquiera `finally` cláusulas que se asociaron con instrucciones try más anidados que a la que se detectó la excepción.

Si no se encuentra ninguna cláusula catch coincidente, se produce una de estas dos cosas:

*  Si la búsqueda de una cláusula catch coincidente llega a un constructor estático ([constructores estáticos](classes.md#static-constructors)) o un inicializador de campo estático, un `System.TypeInitializationException` se produce en el momento que desencadenó la invocación del constructor estático. La excepción interna de la `System.TypeInitializationException` contiene la excepción que se inició originalmente.
*  Si el código que inicialmente se inició el subproceso llega a la búsqueda de cláusulas catch coincidentes, se termina la ejecución del subproceso. El impacto de cuando dicha terminación es definido por la implementación.

Las excepciones que se producen durante la ejecución de un destructor merecen una mención especial. Si se produce una excepción durante la ejecución de un destructor y no se detecta esa excepción, se termina la ejecución de dicho destructor y se llama al destructor de la clase base (si existe). Si no hay ninguna clase base (como en el caso de los `object` tipo) o si no hay ningún destructor de clase base, a continuación, se descarta la excepción.

## <a name="common-exception-classes"></a>Clases de excepciones comunes

Las excepciones siguientes se producen ciertas operaciones de C#.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Una clase base para las excepciones que se producen durante las operaciones aritméticas, como `System.DivideByZeroException` y `System.OverflowException`. | 
| `System.ArrayTypeMismatchException`  | Se produce cuando se produce un error en un almacén en una matriz porque el tipo real del elemento almacenado es incompatible con el tipo real de la matriz. | 
| `System.DivideByZeroException`       | Se produce cuando se produce un intento de dividir un valor integral por cero. | 
| `System.IndexOutOfRangeException`    | Se produce cuando se intenta indexar una matriz a través de un índice que es menor que cero o fuera de los límites de la matriz. | 
| `System.InvalidCastException`        | Se produce cuando se produce un error en una conversión explícita de un tipo base o interfaz a un tipo derivado en tiempo de ejecución. | 
| `System.NullReferenceException`      | Se produce cuando un `null` referencia se utiliza de forma que hace que el objeto que se hace referencia sea necesaria. | 
| `System.OutOfMemoryException`        | Se produce cuando un intento de asignar memoria (a través de `new`) se produce un error. | 
| `System.OverflowException`           | Se inicia cuando se desborda una operación aritmética en un contexto `checked`. | 
| `System.StackOverflowException`      | Se produce cuando se agota la pila de ejecución por tener demasiadas llamadas a métodos pendientes; Normalmente es indicativa de recursividad muy profunda o ilimitada. | 
| `System.TypeInitializationException` | Se produce cuando un constructor estático inicia una excepción y no `catch` cláusulas existe para capturarla. | 
