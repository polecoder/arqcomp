# Preguntas teóricas

**Fecha:** 26-10-2026

## Consigna

1. Explique el propósito de los registros IP e IR.
2. ¿Qué implica decir que una arquitectura es de $n$ bits? ¿Los tamaños de cuáles recursos de hardware quedan determinados?
3. Indique al menos tres aspectos fundamentales a tener en cuenta en el diseño de máquinas RISC.
4. Describa las etapas del ciclo de instrucción e indique qué registros de la CPU intervienen en cada una.

## Resolución

### Pregunta #1

- Explique el propósito de los registros IP e IR.

El registro IP (Instruction Pointer) o como hemos visto en el teórico, conocido como PC (Program Counter) es el registro que almacena la dirección de la próxima instrucción a ejecutarse.
Por otro lado, el registro IR (Instruction Register) se utiliza para guardar una copia de la dirección almacenada por el registro PC.

### Pregunta #2

- ¿Qué implica decir que una arquitectura es de $n$ bits? ¿Los tamaños de cuáles recursos de hardware quedan determinados?

Que la arquitectura sea de $n$ bits implica el tamaño de los siguientes recursos de hardware:

1. **ALU:** 2 entradas de $n$ bits, 1 salida de $n$ bits.
2. **Shifter:** 1 entrada de $n$ bits, 1 salida de $n$ bits.
3. **Registros:** Cada registro tiene un tamaño de $n$ bits de palabra.
4. **Memoria:** Cualquier dirección de memoria tiene un tamaño de $n$ bits de palabra.
5. **Largo de instrucciones:** SOLO para arquitecturas RISC

### Pregunta #3

- Indique al menos tres aspectos fundamentales a tener en cuenta en el diseño de máquinas RISC.

Recordemos que RISC significa **Reduced Instruction Set Computer**, esto nos lleva al primer ascecto fundamental de su diseño:

1. Las máquinas RISC cuentan con un conjunto de instrucciones reducido y elemental.
2. Las arquitecturas RISC utilizan formatos de instrucciones de largo fijo, además de utilizar un sistema de "3 direcciones": esto es que cada instrucción especifica las direcciones de sus operandos y de su destino explícitamente.
3. Los registros de las máquinas RISC son despersonalizados. Esto significa que podemos usar cualquier registro para almacenar cualquier dato que sea necesario.

### Pregunta #4

- Describa las etapas del ciclo de instrucción e indique qué registros de la CPU intervienen en cada una.

Las etapas del ciclo de instrucción son las siguientes en el órden dado: *fetch, decode, read, execute, write*. Los describimos a continuación.

- *Fetch:* se lee la próxima instrucción a ejecutarse.
- *Decode:* se decodifica el código obtenido en el fetch para determinar que operación se debe realizar.
- *Read:* se accede a memoria para traer los operandos.
- *Execute:* se ejecuta la operación a realizar en la ALU sobre los operandos.
- *Write:* no todas las operaciones escriben en memoria el resultado, pero si se hace es en este paso.

Ahora especificamos que registros intervienen en cada etapa:

- *Fetch:* PC, MAR (si la instrucción está en memoria), MBR (si la instrucción está en memoria), IR, +1 (para actualizar el PC)
- *Decode:* IR
- *Read:* IR, MAR (si los operandos están en memoria), MBR (si los operandos están en memoria), A...F (si se guarda en algún registro)
- *Execute:* MBR (si hay un operando en memoria), A...F (cualquier registro donde haya almacenado un operando)
- *Write*: IR, MAR (si se escribe en memoria), MBR (si se escribe en memoria), A...F (si se guarda en algún registro)