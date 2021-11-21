# Solidity

## Variables y tipos de datos

Tipos de datos

* int | uint <8-256>: Son números enteros, que pueden ser sin signo o con signo, y que pueden tener una capacidad de 8 a 256 bits.
* bool: Verdadero o flaso
* address: Guarda direcciones de ETH de 160 bits (20 bytes), y puede tener métodos extra como .transfer o .balance
* string: Cadena de texto
bytes<8-256>: Cadena de bytes

Tipos de variables

* Variables locales: Son aquellas que ocurren durante la ejecución. En la EVM es la parte correspondiente a memoria volátil
* Variables de estado: Son variables que se almacenan en la parte de la ROM de la EVM. Es memoria persistente

Variables globales

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

Mapas

Los mapas son estructuras de datos de tipo llave-valor, que permiten apuntar un tipo de dato a otro en forma de diccionario.

El tipo de la llave puede ser cualquier tipo de dato elemental, (por ejemplo, uint), y el tipo de dato del valor puede ser cualquier dato elemental o complejo, (se pueden inclusive hacer estructuras multidimensionales).

mapping(<key type> => <value type>) <visibility> <name>;

Enum

Representa una lista de valores posibles creados por el usuario, una variable del tipo de enum declarado sólo puede tomar los valores enumerados.

enum { <value1>, ..., <valueN> }

## Estructuras de Control

* if/else: Estructura condicional. Ejecuta un bloque u otro dependiendo de una evaluación booleana
* for: Estructura cíclica que ejecuta un bloque de instrucciones un número determinado de veces
* while: Estructura cíclica que repite un bloque mientras se cumpla una condición
* do while: Estructura cíclica que se asmilia al while, con la diferencia que siempre se ejecuta almenos una vez