# Interrupciones

**Fecha:** 16-11-2026

## Introducción

En este capítulo veremos el último de los componentes esenciales de la arquitectura Von Neumann: el manejo de interrupciones.
Este concepto es habitualmente díficil de entender en los cursos básicos de computación porque se aparta de lo que intuitivamente pensamos de la ejecución de un programa: siempre pensamos que la próxima instrucción (o sentencia) que se ejecuta de un programa es la que está escrita a continuación en el código, o siendo más precisos, la que está a continuación en la lógica de ejecución del programa.
Sin embargo esto no es cierto justamente debido al mecanismo de interrupciones que se utiliza en todos los sistemas que se basan en los conceptos de programa almacenado.

## Definición

Una definición de interrupción puede ser:

> Una interrupción consiste en un mecanismo que provoca la alteración del orden lógico de ejecución de instrucciones como respuesta a un evento externo, generado por el hardware de entrada/salida en forma asincrónica al programa que está siendo ejecutado y fuera de su control.

En forma alternativa, se puede decir que:

> Una interrupción consiste en un mecanismo que le permite al hardware la invocación de una rutina fuera del control del programa que está siendo ejecutado.

Analicemos el contenido de la primera definición que hemos dado. Por un lado decimos que se trata de un mecanismo, cuyos detalles veremos más adelante, que provoca **la alteración del órden lógico de ejecución de instrucciones**. Esto significa que cuando ocurre una interrupción, la próxima instrucción a ser ejecutada **no** es la que corresponde a la secuencia lógica de instrucciones del programa que se está ejecutando, que vendría a ser la apuntada por IP, sino que dicha secuencia se modifica y se pasa a ejecutar otra instrucción (concretamente la primera de la **rutina de servicio de la interrupción** correspondiente al pedido atendido).

Por otro lado nos referimos a que es en respuesta a un *evento externo*, que está generado por el hardware de entrada/salida, es decir que no es el programa que lo genera (al menos no en forma directa), siendo provocado por el hardware, en particular el asociado a los dispositivos de entrada/salida, más concretamente: los controladores de E/S.

Y finalmente se menciona que es *asincrónico y fuera de su control*, lo que significa que el programa que está siendo ejecutado no tiene control sobre el momento en que este mecanismo se dispara, ni sobre la propia ocurrencia del fenómeno. Es decir que en particular, no hay sincronismo entre la ejecución del programa y la eventual invocación a esa rutina "especial" disparada por la interrupción.

## Pedido de interrupción

El mecanismo de interrupción comienza con el pedido de interrupción (interrupt request) generado por un controlador de entrada/salida.

Este pedido se genera a raíz de alguna condición detectada por el controlador (ej: dispone de un dato para ser leído por la CPU, terminó de ejecutar la lectura del sector de disco solicitada, hay una condición de error en el byte recibido por la línea de comunicaciones, etc.).

Las condiciones que generan el pedido varían de acuerdo al tipo de controlador que se trate y de acuerdo a como esté configurado el mismo. Hay controladores capaces de solicitar iterrupciones ante determinadas y diferentes condiciones de error, pero que pueden ser programados para no hacerlo a través de bits que habilitan/deshabilitan la posibilidad que una situación particular pueda o no generar una interrupción.

Los controladores de E/S disponen de una señal de **salida** (habitualmente denominada INT, INTR o IRQ) que toma el valor "1" cuando el controlador interrumpe. Normalmente esa salida es el reflejo de hardware de un bit del registro de ESTADO. Es decir que el nivel actual (0 o 1) de la señal INT del controlador de E/S puede leerse en un bit de dicho registro. Este bit de "pedido de interrupción pendiente" y su correspondiente reflejo en el hardware permanece en nivel alto hasta que la rutina de atención de la interrupción realice alguna operación sobre los registros del controlador de E/S que satisfaga el pedido, con lo cual el controlador procederá a pasar ese bit (y su salida INT) a 0. Cual es esa acción dependerá del controlador, y posiblemente de la condición que provocó el pedido. Algunos pedidos de interrupción se satisfacen con la lectura del registro de estado, otros requieren de la lectura de algún registro adicional, otros requieren de la escritura de un registro, etc.

En las CPUs también existe esta señal, que recibe denominaciones similares, pero que es de **entrada**. Esta entrada es la que el hardware de la CPU consulta para determinar si hay algún pedido de interrupción por parte de algún controlador de E/S.

Luego lo veremos en mayor detalle, pero adelantaremos que la CPU chequea la existencia de un pedido de interrupción al finalizar la ejecución de cada instrucción. Si detecta una solicitud, procede a desencadenar el mecanismo que desemboca en la ejecución de la rutina de serivicio de la interrupción, luego de la cual se pasa a ejecutar la próxima instrucción del programa interrumpido, siempre que no exista un nuevo pedido de interrupción, donde en caso de existir, se desencadenará un nuevo proceso de invocación a la rutina de servicio que corresponda.

De acuerdo a la forma en que la CPU detecta que hay un pedido de interrupción se distinguen dos casos: detección por nivel y detección por flanco.

### Detección por nivel

En este caso la CPU reconocerá que hay un pedido de interrupción pendiente, mientras su entrada INT esté en el nivel lógico alto ("1").
Esto significa que toda vez que la CPU detecte esa entrada en alto, desencadenará la invocación a la rutina de atención, incluso cuando haya terminado recién de ejecutar una rutina de interrupción.

### Detección por flanco

En este caso lo que nos interesa a los efectos de determinar que existe un requerimiento de interrupción es la existencia de un cambio de nivel bajo ("0") a alto ("1"), es decir un **flanco** en la entrada INT.

Es decir que no importa el valor absoluto actual de la entrada INT, lo que se tiene en cuenta es si existió un cambio de 0 a 1. Si hubo flanco y aún no ha sido invocada la rutina de atención, la CPU lo hace y da por cumplido el pedido.

Luego veremos que esto tiene sus implicancias en el diseño de rutinas de servicio a las interrupciones.
