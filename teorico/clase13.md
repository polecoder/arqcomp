# Arquitectura de Von Neumann

**Fecha:** 19-10-2026

## Introducción

Esta arquitectura fue la propuesta por el matemático John Von Neumann para la construcción de la computadora EDVAC en 1949, sucesora de la que se considera la primer computadora electrónica, la ENIAC (1946).
Los conceptos que propuso Von Neumann han tenido una vigencia mucho más allá de la esperada en una industria como la de las tecnologías de la información. Por más que llame la atención, todos los computadores modernos disponibles comercialmente poseen en el fondo la misma arquitectura que la EDVAC, definida en 1945.
Mucho se ha adelantado en materia de los circuitos lógicos (la organización) que implementan la arquitectura, pero casi nada en los principios de diseño que planteó Von Neumann hace ya más de 80 años.

La idea de Von Neumann se apoya fuertemente en el hecho de que una operación compleja normalmente puede dividirse como una secuencia ordenada de operaciones más simples. En otras palabras, lo que propuso fue construir una máquina capaz de ejecutar algorítmos en forma explícita. Para ello introdujo el concepto de "programa almacenado" como una "secuencia lógicamente ordenada de instrucciones", siendo las "instrucciones" las operaciones básicas que implementa el hardware a través de sus circuitos lógicos.

Una arquitectura de Von Neumann tiene tres bloques constructivos básicos: la Unidad Central de Proceso (CPU), la Memoria y la Entrada/Salida.
Las funciones de cada bloque son:

- **CPU:** se encarga de ejecutar los programas.
- **Memoria:** almacena el programa (conjunto de instrucciones ordenado lógicamente) y los datos (operadores y resultados de la ejecución de las instrucciones)
- **Entrada/Salida:** comunica al computador con el mundo exterior, permitiendo la interacción con los usuarios y con otras computadoras.

La **Memoria** se implementa como ya vimos en las clases anteriores. En un próximo capítulo veremos en detalle el sub-sistema de **Entrada/Salida**. En este tramo de clases, nos concentraremos en analizar la **CPU**.

Estos tres sub-sistemas se interconectan por medio de un **bus**, que contiene líneas de **datos**, **dirección** y **control** respectivamente.

## CPU

La **unidad central de proceso** en la encargada de la ejecución de las **instrucciones**.
Para ello la arquitectura de Von Neumann propone que la misma cuente con los siguientes elementos básicos:

- **Unidad Aritmética-Lógica (ALU):** circuito lógico que implementa operaciones de aritmética binaria: típicamente las operaciones básicas para las representaciones "binario" y "complemento a 2"; además de AND, OR, XOR y NOT bit a bit. También es habitual que implemente operaciones de desplazamiento y/o rotación de bits.
- **Unidad de control:** circuito secuencial que implementa el denominado "ciclo de instrucción", permitiendo acceder a la siguiente instrucción de un programa, leer sus operandos, efectuar la operación indicada en la ALU y guardar el resultado de la misma
- **Banco (Set/Conjunto) de registros:** una serie de posiciones especiales de memoria, ubicadas físicamente dentro de la propia CPU, que permiten un acceso a operandos y lugares de almacenamiento de resultados mucho más veloz que si estuvieran en el sistema de memoria normal. Algunos de estos registros son de uso interno de la propia CPU (más precisamente de su unidad de control) y otros son accesibles y utilizables por el programador.

Notemos que el banco de registros no es un elemento esencial de la arquitectura. Es un elemento de carácter tecnológico que tiene que ver con dos aspectos:

- por razones vinculadas a la electrónica, la velocidad de transferencia de información entre dos circuitos está limitada por la distancia geográfica entre los mismos.
- la memoria rápida (usada en los pocos registros de la CPU) es más cara que la memoria más lenta (usada en las posiciones de memoria principal de forma más abundante).

De allí que se utilice poca memoria rápida (por el costo) y que se ubique en las cercanías de donde se va a utilizar (dentro de la CPU) para minimizar su tiempo de acceso.

## Caracterización de una arquitectura Von Neumann

Como ya expresamos, todas las computadoras actuales tienen en esencia la misma arquitectura. Sin embargo dentro de la idea general de Von Neumann que todas respetan, se diferencian entre si por decisiones de diseño que afectan la cantidad y cualidad de sus elementos componentes.

Así una arquitectura particular (ej: Intel x86, PowerPC, SPARC, MIPS, etc.) establece en forma diferenciada los siguientes elementos característicos, los que deben ser conocidos por los programadores "de bajo nivel" para poder escribir programas para una de esas arquitecturas:

- **Set de instrucciones:** La cantidad de instrucciones disponibles; la calidad y complejidad de las operaciones implementadas en el hardware de la CPU.
- **Formato de instrucción:** La forma  en que se codifican las instrucciones.
- **Set de registros:** la cantidad de registros disponibles al programador, así como la función que pueden cumplir (ej: origen o destino de operaciones, almacenamiento de direcciones de operandos).
- **Modos de direccionamiento:** formas de generar las direcciones para hallar los operandos o almacenar los resultados de las operaciones.
- **Manejo de la Entrada/Salida:** forma de comunicación con los "periféricos" (dispositivos que implementan la interacción con el mundo exterior).
- **Manejo de interrupciones:** manejo de una forma particular de invocar a ciertas sub-rutinas de los programas que estudiaremos más adelante.

### Set de instrucciones

Históricamente desde la EDVAC hasta principios de la década de los '80, la tendencia fue ir, progresivamente, aumentando la cantidad de instrucciones disponibles en el hardware y su complejidad.
En ese momento, se pensaba que cuanto más "potente" era el hardware (en cuanto a la variedad de operaciones disponibles). más eficientemente se podrían ejecutar los programas, logrando una mayor "performance" del sistema.

Sin embargo, en 1980 se publicaron, casi simultáneamente, trabajos de investigación de las universidades de Berkeley y Stanford (EE.UU.) que proponían un enfoque radicalmente distinto: lo mejor era disponer de un conjunto mínimo de instrucciones que estuvieran implementadas en forma óptima.
Estos trabajos acuñaron el término **RISC** (**R**educed **I**nstruction **S**et **C**omputer) para referirse a los diseños basados en este concepto.
A partir de este momento, el resto de las arquitecturas pasaron a denominarse **CISC** (**C**omplex **I**nstruction **S**et **C**omputer).

No hay una respuesta definitiva sobre que arquitectura es mejor: si las basadas en la filosofía RISC o las CISC.
Si bien las primeras implementaciones prácticas de RISC demostraron ser más eficientes, el mercado siguió siendo de las CISC, en particular de la que se convirtió en un estándar de facto: la **arquitectura x86 de Intel**.
Hoy en día, se puede decir que un procesador Intel es, en el fondo, una máquina RISC que ejecuta un conjunto de "micro-rutinas" que "emulan" las instrucciones de una máquina de arquitectura x86.

### Formato de instrucciones

El formato de las instrucciones refiere a la codificación de las distintas instrucciones para su almacenamiento en la memoria del sistema.
Al igual que para los demás tipos de datos manipulados que vimos, los computadores trabajan con una representación de las instrucciones mediante un código binario.

El código binario reserva una serie de bits para identificar la operación realizada por la instrucción, otros indican los operandos a utilizar y sus direcciones, así como la indicación de donde se almacena el resultado.

Los atributos que definen el formato de las instrucciones (es decir los que corresponde a la **caracterización** de la arquitectura) incluyen aspectos tales como si los códigos binarios asociados son de largo fijo o variable, y si las instrucciones tienen operandos y destino independientes (en cuyo caso se habla de "arquitectura de 3 direcciones") ó solapados (correspondiente a "arquitectura de 2 direcciones").

Las arquitecturas RISC promocionaron la utilización de formatos de instrucción de largo fijo, como forma de simplificar la circuitería de decodificación y permitir la utilización eficaz de técnicas de optimización (pipeline, superescalaridad) que se verán más adelante. Por otra parte las arquitecturas CISC suelen tener instrucciones de largo variable.
Las arquitecturas RISC también utilizan formatos de "3 direcciones", mientras que en las CISC es más común encontrarse con implementaciones de "2 direcciones" (como es el caso de Intel x86).

### Set de registros

Los registros son, como mencionamos, posiciones "especializadas" de memoria, ubicadas dentro de la propia CPU y que poseen una manera de direccionarlas de manera distinta de la memoria "normal" del sistema.

Los primeros diseños siguieron el ejemplo de la EDVAC de Von Neumann y utilizaron registros "con personalidad". En la propuesta original de Von Neumann, la computadora poseía (entre otros):

- Un registro **Acumulador** para las operaciones aritméticas,
- Un registro **Contador** para operaciones que implicaran contar y
- Un registro **Indice** para ser utilizado para contener direcciones de operandos de memoria.

Estas ideas son las que llevaron a Intel a nombrar los registros de su primer microprocesador 8080 con letras:

- A (Acumulador)
- B (Base, para direcciones)
- C (Contador)
- D (Data, para almacenar datos, es decir operandos en general)

Luego la arquitectura x86 de 16 bits tomó esos nombres y le agregó una X de eXtended (en referencia a que pasaron de 8 bits a 16 bits), quedando entonces AX, BX, CX y DX; a los que se agregaron SI (source index) y DI (destination index) entre otros.
La característica de los registros "con personalidad" es que su función dentro de las  instrucciones (como operandos fuente, operandos destino, contadores o parte del direccionamiento de los operandos) está condicionada y no todos sirven para cualquier función. Su aplicabilidad es restringida y específica.

Las arquitecturas RISC promovieron desde el comienzo el uso de los registros "despersonalizados", de uso general, aplicables todos y cada uno a cualquiera de las funciones antedichas. Intel incorporó la idea a partir del procesador 80386, primer representante de 32 bits de su familia arquitectónica. Los registros de 32 bits de la arquitectura x86 se denominan EAX, EBX, ECX, EDX, ESI, EDI, etc (con la E por "Enhanced") y pueden intercambiarse casi completamente en su uso en las instrucciones.

### Modos de direccionamiento

Los modos de direccionamiento establecen las formas en las que se puede, a nivel de las instrucciones, especificar la dirección de un operando o del lugar donde colocar el resultado de la operación correspondiente a la instrucción.

Existen tres modos de direccionamiento básicos: Inmediato, Directo e Indirecto. A continuación presentaremos las características de cada uno:

- **Inmediato:** en este modo en la instrucción se encuentra el propio operando (su valor). Se usa típicamente para constantes (ya que las instrucciones no se pueden modificar).
- **Directo:** en este modo en la instrucción se encuentra la dirección del operando. Se pueden distinguir dos tipos:
    - *Directo a registro:* El operando está almacenado en un registro y la instrucción contiene el identificador del registro.
    - *Directo a memoria:* El operando está almacenado en memoria y la instrucción contiene la dirección de memoria donde se encuentra.
- **Indirecto:** en este modo en la instrucción se encuentra en la dirección del lugar donde se encuentra la dirección del operando. También se pueden distinguir dos tipos:
    - *Indirecto por registro:* El operando se encuentra en una posición de memoria cuya dirección se almacena en un registro y la instrucción contiene el identificador del registro. Este es el tipo de indirecto habitualmente implementado, utilizando más de de un registro (sumando sus contenidos) para formar la dirección. En algunos casos la arquitectura prevé multiplicar el contenido del registro por el tamaño en bytes del operando. En ese caso se habla de direccionamiento indizado.
    - *Indirecto por memoria:* El operando está almacenado en una posición de memoria cuya dirección está en otra posición de memoria y la instrucción contiene la dirección de esta última. Esta variante no es implementada en las arquitecturas prácticas disponibles.

Los modos directo e indirecto (especialmente en su modalidad "Indizado") se suelen combinar para establecer la dirección de un operando o de un destino. La dirección se forma sumando el contenido de uno o más registros identificados en la instrucción con la "dirección base" contenida en la misma.
Esto permite implementar fácilmente el acceso a estructuras tipo array. Se coloca la dirección de comienzo de la estructura en la instrucción (modo directo) y se cambia el contenido de un registro "índice" (que también debe estar referenciado en la instrucción) para ir recorriendo el array haciendo un "loop" y reutilizando la misma instrucción.