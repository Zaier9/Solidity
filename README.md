# Solidity

## Variables y tipos de datos

### Tipos de datos

* int | uint <8-256>: Son números enteros, que pueden ser sin signo o con signo, y que pueden tener una capacidad de 8 a 256 bits.
* bool: Verdadero o flaso
* address: Guarda direcciones de ETH de 160 bits (20 bytes), y puede tener métodos extra como .transfer o .balance
* string: Cadena de texto
bytes<8-256>: Cadena de bytes

### Tipos de variables

* Variables locales: Son aquellas que ocurren durante la ejecución. En la EVM es la parte correspondiente a memoria volátil
* Variables de estado: Son variables que se almacenan en la parte de la ROM de la EVM. Es memoria persistente

### Variables globales

* msg: Toda transacción es un mensaje firmado. En este objeto vienen los datos de dicho mensaje (sender, value, etc.)
* tx: Represena la transacción, es distitna respecto a msg porque cosas como el sender van variando conforme se concatenan llamadas entre contratos
* block: Información respecto al bloque

## Structs y Arrays

Structs:
    Son estructuras de datos complejas, que agrupan una serie de datos.

Arreglos:
    Son listas de datos, incluso se pueden hacer listas de Structs, añadiendo estructuras más complejas

Funciones Array Dinámico

* push: Se usa para adicionar elementos al final de un array
* pop: Se usa para remover el último elemento de un array

## Mappings y enums

### Mapas

Los mapas son estructuras de datos de tipo llave-valor, que permiten apuntar un tipo de dato a otro en forma de diccionario.

El tipo de la llave puede ser cualquier tipo de dato elemental, (por ejemplo, uint), y el tipo de dato del valor puede ser cualquier dato elemental o complejo, (se pueden inclusive hacer estructuras multidimensionales).

mapping(<key type> => <value type>) <visibility> <name>;

### Enum

Representa una lista de valores posibles creados por el usuario, una variable del tipo de enum declarado sólo puede tomar los valores enumerados.

enum { <value1>, ..., <valueN> }

## Estructuras de Control

* if/else: Estructura condicional. Ejecuta un bloque u otro dependiendo de una evaluación booleana
* for: Estructura cíclica que ejecuta un bloque de instrucciones un número determinado de veces
* while: Estructura cíclica que repite un bloque mientras se cumpla una condición
* do while: Estructura cíclica que se asmilia al while, con la diferencia que siempre se ejecuta almenos una vez

## Eventos

Los eventos son un tipo de dato que sirve para emitir avisos de que ocurrió alguna acción en particular.

Puede ser utilizado por clientes para escuchar cambios importantes, y también pueden utilizarse para indexar información.

Protocolos como TheGraph utilizan indexación de eventos para agregación de informacións

## Funciones

Son piezas de código definidas por un nombre, parámetros y modificadores.

function <name>(<type> <parameter>..., [..., ]>) 
  <access modifiers>
  <mutability modifiers>
  <user defined modifiers> 
  <returns>(<type>) {
    <content>
}

* name: Nombre de la función
* type: Tipo de dato
* parameter: Nombre del parámetro
* access modifier: public, private, internal, external
* mutability modifier: view, pure. Ambas hacen que la función no cobre gas, y si se omite, se asume que es una función que escribe en el storage de la EVM
* user defined modifiers: Son modificadores definidos por el usuario, son opcionales y se presentan en la siguiente clase
* returns: Indica que la función retornará uno o más datos

## Modificadores

Los modificadores son funciones especiales por el usuario y que se añaden a otra función para envolver su funcionamiento

modifier <name>(<type> <parameter>..., [,...]) {
  <content>
}
El guión bajo

El guión bajo (también conocido como placeholder), es una instrucción especial del modificador que indica dónde se va a ejecutar el código de la función inicial que envuelve al modifier.

Por ejemplo

// Primero valida y luego ejecuta
modifier isOwner() {
  if(<condicion>) revert()
  _;
}

// Primero ejecuta y luego valida
modifier isOwner() {
   _;
  if(<condicion>) revert()
}

// Ejecuta, valida y vuelve a ejecutar
modifier isOwner() {
   _;
  if(<condicion>) revert()
   _;
}

## Manejo de Errores

* assert: Se utiliza para pruebas, compara dos valores
* revert: Es un error que regresa todas las modificaciones de estado realizadas durante la ejecución de la función. Recibe por parámetro un mensaje de error
* require: Es una variación del revert que recibe por parámetro una expresión booleana y revierte si esta expresión es falsa.

Cabe destacar que cualquier consumo de gas ejecutado hasta el momento de un revert se debe pagar, porque el cómputo fué utilizado.

## Tipos de Almacenamiento

* Storage: Memoria persistente. Es el más costoso. Similar a la memoria ROM
* Memory: Variables temporales durante ejecución. Se asimila a la RAM
* Calldata: Son constantes definidas en el entorno de ejecución de una variable. No son modificables.

Memoria dinámica

La razón por la que un string necesita un sufijo que indique el uso de memoria, es debido a que es memoria dinámica, por lo que calldata no puede alocar una cantidad definida de memoria, por lo que tenemos que indicarle que esa variable la pase por la memoria volátil (RAM/memory), para que la función la pueda manejar correctamente.

Este efecto ocurre con cualquier cosa que sea de tamaño no definido, por ejemplo:

* Un arreglo
* Un string

## Gas y Comisiones

El gas es una unidad de medida para el procesamiento de la EVM. Se mide en unidades de gas, y es constante para las mismas operaciones.

* gasPrice: Es la cantidad de ETH que pagamos por unidad de gas. Es decir, aunque el gas sea constante, la demanda por ese gas puede subir el precio.
* gasCost: Es la cantidad de unidades de gas que generó la ejecución
* gasFee: Gas cost * Gas Price

Priority fee

A partir del EIP1559 , se realizaron cambios importantes al mercado de gas, y se contempla el priority fee, que es el extra que menciona Sebastián, y es una propina para el minero con la cuál se obtiene prioridad en la ejecución

## Transferencias de ether desde un contrato

* send: Envía un monto a una dirección y retorna false si la transferencia no se realiza
* transfer: Envía un monto y revierte si no se puede realizar
* call: Esta es más complicada, pero básicamente realiza una llamada hacia una dirección. Incluso se pueden llamar funciones de otro contrato si se le pasa un address válido y la llamada dentro del parámetro data. No obstante, al ser un mensaje, puede llevar ether, y por eso se usa para envíos. Retorna el resultado de la función llamada (si es que fué el caso)

## Recibir ether desde un contrato

* Receive: Recibe el saldo de trasferencias sin parámetros.
* FallBack: Recibe información adjunta a la trasferencia por medio de los parámetros.
* Función Payable: Se especifica el tipo payable a una función que puede recibir trasferencias.

  Cuando se envía una transferencia lo primero que se intenta es verificar si existe una función con la firma especificada.

  Si no se encuentra, se buscará una función **fallback** que reciba parámetros.

## Manejo de dependencias y librerias

  * Con la sentencia **"import"** podemos hacer referencia a un contrato que esté definido en el mismo ámbito en el que estamos trabajando.
  * También podemos importar contratos que se encuentren en un repositorio o en un paquete como **npm**

Además de contratos, podemos importar librerias que son similares a los contratos, pero no contienen estado y solo brindan utilidad.

## Herencia

* El concepto es similar al de la programación orientada a objetos y la identificaremos con la sentencia **is**.
* Si un contrato tiene un constructor con parámetros, debemos indicar **qué valores debe tomar ese constructor para poder derivarse**.

## Override

* Podemos definir funciones de tipo **virtual** para indicar que pueden ser redefinidas en sus contratos derivados.
* Cuando se redefine una función de debe indicar el tipo **override** en su declaración.
* Si una función virtual no define implementación, el contrato se convierte en un **contrato abstracto**.

## Interfaces

* Nos permiten definir el comportamiento que queremos que tenga un contrato.
* Se implementa igual que la herencia de contratos.
* Sus funciones no tienen implementación, solo encabezados.

## Super

Podemos hacer referencia a algún elemento o función de la clase superior por medio de la sentencia **super**.

## Instanciar un contrato en otro contrato

* Dentro de un contrato podemos hacer referencia a otro contrato ya implemetado en la red a través de su dirección.
* Para esto podemos utilizar el tipo de contrato al que referenciamos o bien alguna de sus clases superiores.

## Polimorfismo

* Capacidad de poder utilizar contratos derivados en una estructura.

## Tokens

* Son una representación de un elemento que tiene un valor en un contexto determinado.
* En general ésta representación tiene menos valor que el objeto original.
* Existen diferentes clasificaciones para los tokens.

## Clasificación

* **Fungibles:** son elementos reemplazables por otros con las mismas características. 
  Además son divisibles y podemos consumir sólo una fracción.
* **No fungibles:** son elementos que pueden variar de valor con respecto de elementos de iguales características.
  NO son divisibles.

## ERC-20

* Es el estándar que representa a los tokens fungibles.
* Al ser un estándar no cuenta con una implementación, sino que solo define una interfase.
* Existen otros estándares que implementan tokens fungibles, pero todos ellos mantienen la compatibilidad contra el token ERC-20.

## ERC-721

* Es el estándar que representa a los tokens no fungibles.
* Al igual que el ERC-20 solo define una interfase.
* A diferencia de los tokens fungibles, éstos tienen un identificador único conocido como **tokenId**.

## TokenUri

* Contiene información no técnica acerca de un token.
* En general tiene que ver con información estética o visual relacionada.
* Fue introducida por el estándar ERC-721, pero se utiliza en otros tipos de token.

## Application Binary Interface(ABI)

* **ABI** viene de Application Binary Interface.
* Es una interfase que nos detalla qué **definiciones tiene el contrato y sus funciones**, sin conocer su implementación.
* Nos permite saber qué forma tiene un contrato para poder interactuar con sus funciones, especialmente si estamos contruyendo la capa de usuario.
