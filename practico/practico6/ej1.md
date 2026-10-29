# Assembler para un microprocesador RISC de 16 bits

**Fecha:** 28-10-2026

## Consigna

| Instrucción | Descripción |
|-------------|-------------|
| NOP | No realiza ninguna operación |
| MOVI Inm, Reg1 | Carga el inmediato Inm en los bits menos significativos de Reg1, dejando los bits más significativos en 0 |
| MOVR Reg1, Reg2 | Carga en Reg2 el contenido de Reg1 |
| LOAD Reg1, Reg2 | Carga el contenido de memoria apuntado por Reg1 en Reg2 |
| SAVE Reg1, Reg2 | Guarda el registro Reg1 en la dirección de memoria apuntada por Reg2 |
| ADD Reg1, Reg2, Reg3 | Suma Reg1 y Reg2 y guarda el resultado en Reg3 |
| SUB Reg1, Reg2, Reg3 | Realiza la operación Reg1-Reg2 y guarda el resultado en Reg3 |
| SHL Reg1, Reg2, Reg3 | Desplaza Reg1 a la izquierda Reg2 lugares y guarda el resultado en Reg3 |
| SHR Reg1, Reg2, Reg3 | Desplaza Reg1 a la derecha Reg2 lugares y guarda el resultado en Reg3 |
| AND Reg1, Reg2, Reg3 | Realiza un AND bit a bit entre Reg1 y Reg2 y guarda el resultado en Reg3 |
| OR Reg1, Reg2, Reg3 | Realiza un OR bit a bit entre Reg1 y Reg2 y guarda el resultado en Reg3 |
| XOR Reg1, Reg2, Reg3 | Realiza un XOR bit a bit entre Reg1 y Reg2 y guarda el resultado en Reg3 |
| NOT Reg1, Reg2 | Realiza un NOT bit a bit de Reg1 y guarda el resultado en Reg2 |
| JMP Reg1 | Hace un salto incondicional a la dirección apuntada por Reg1 |
| JC Reg1 | Hace un salto a la dirección apuntada por Reg1 si la bandera de $C$ vale 1 |
| JZ Reg1 | Hace un salto a la dirección apuntada por Reg1 si la bandera de $Z$ vale 1 |

**Aclaraciones:**
- Las flags $Z$ (cero) y $C$ (acarreo) son afectadas por las operaciones aritméticas y lógicas.
- Para el caso de los desplazamientos, la flag $C$ corresponde al último bit que fue desplazado hacia afuera del registro.
- Los 16 registros llevan el nombre R00 hasta R15.

1. Defina un formato de instrucción para la CPU a través de los siguientes pasos:

   1. Indique el largo de instrucción.
   2. Indique cuántos bits son necesarios para codificar cada registro. Asigne un código binario para cada registro de la cantidad de bits.
   3. Indique cuántos bits son necesarios para codificar el código de instrucción. Asigne un código binario para cada instrucción.
   4. Agrupe las instrucciones según qué tipo de información precisan codificar (ej: SUB/ADD: opcode + 3 registros).
   5. Defina un subformato de instrucción para cada grupo. Indique qué instrucción utiliza cada tipo de subformato especificado.

2. Con el formato que definió, ¿cuántos bits quedan disponibles para el inmediato MOVI? ¿Qué rangos de valores se puede cargar con una única instrucción MOVI?
3. Escriba la secuencia de instrucciones necesaria para dejar la constante 0xABCD en el registro R01.
4. Implemente en alto nivel un programa que sume el contenido de memoria de las direcciones comprendidas en el rango 0xABCD-0xEF00 y almacene en la dirección 0x00FF el triple de dicha suma.
5. Implemente en lenguaje ensamblador el programa de la parte anterior.
6. Implemente en lenguaje máquina las primeras 7 instrucciones del programa de la parte anterior (ensamblador).

**Notas:**
- El programa se debe implementar con los nombres de operación dados. Las direcciones y constantes deben representarse en hexadecimal.
- El programa estará cargado en memoria a partir de la dirección 0x0000.

## Resolución

### Parte 1

- Defina un formato de instrucción para la CPU a través de los siguientes pasos:

   1. Indique el largo de instrucción.
   2. Indique cuántos bits son necesarios para codificar cada registro. Asigne un código binario para cada registro de la cantidad de bits.
   3. Indique cuántos bits son necesarios para codificar el código de instrucción. Asigne un código binario para cada instrucción.
   4. Agrupe las instrucciones según qué tipo de información precisan codificar (ej: SUB/ADD: opcode + 3 registros).
   5. Defina un subformato de instrucción para cada grupo. Indique qué instrucción utiliza cada tipo de subformato especificado.

#### Subparte 1

- Indique el largo de instrucción.

La parte más sencilla del ejercicio. Como trabajamos con una arquitectura de 16 bits que es RISC, el largo de instrucción está dado por este número.

Concluimos que las instrucciones tienen largo fijo de 16.

#### Subparte 2

- Indique cuántos bits son necesarios para codificar cada registro. Asigne un código binario para cada registro de la cantidad de bits.

Recordemos que: *los 16 registros llevan el nombre R00 hasta R15*.
Lo primero es que tenemos 16 registros, por lo que nos es suficiente con 4 bits. Por otra parte, podemos utilizar los números del nombre de cada registro para asignar de forma inteligente un código binario; por ejemplo: R6 => 0110.

Esto nos deja con la siguiente tabla:

$$
\begin{array}{c|c}
\text{Registro}&\text{Binario}\\
\hline
R0&0000\\
R1&0001\\
R2&0010\\
R3&0011\\
R4&0100\\
R5&0101\\
R6&0110\\
R7&0111\\
R8&1000\\
R9&1001\\
R10&1010\\
R11&1011\\
R12&1100\\
R13&1101\\
R14&1110\\
R15&1111\\
\end{array}
$$

#### Subparte 3

- Indique cuántos bits son necesarios para codificar el código de instrucción. Asigne un código binario para cada instrucción.

Tenemos 16 instrucciones, por lo que nuevamente 4 bits serán suficientes. Lo asignaremos de acuerdo al órden de la tabla de la letra:

$$
\begin{array}{c|c}
\text{Codigo}&\text{Binario}\\
\hline
\text{NOP}&0000\\
\text{MOVI}&0001\\
\text{MOVR}&0010\\
\text{LOAD}&0011\\
\text{SAVE}&0100\\
\text{ADD}&0101\\
\text{SUB}&0110\\
\text{SHL}&0111\\
\text{SHR}&1000\\
\text{AND}&1001\\
\text{OR}&1010\\
\text{XOR}&1011\\
\text{NOT}&1100\\
\text{JMP}&1101\\
\text{JC}&1110\\
\text{JZ}&1111\\
\end{array}
$$

#### Subparte 4

- Agrupe las instrucciones según qué tipo de información precisan codificar (ej: SUB/ADD: opcode + 3 registros).

Agruparemos sencillamente por cantidad de registros o información asociada a cada instrucción.

- **Sin registros:** NOP
- **1 registro:** JMP, JC, JZ
- **2 registros:** MOVR, LOAD, SAVE, NOT
- **3 registros:** ADD, SUB, SHL, SHR, AND, OR, XOR
- **MOVI:** se separa de los demás ya que tiene un solo registro y un inmediato

#### Subparte 5

- Defina un subformato de instrucción para cada grupo. Indique qué instrucción utiliza cada tipo de subformato especificado.

**Sin registros:**

$$
0000\
0000\
0000\
\underbrace{0000}_{\text{opcode}}
$$

**1 registro:**

$$
0000\
0000\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**2 registros:**

$$
0000\
\underbrace{0000}_{\text{Reg2}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**3 registros:**

$$
\underbrace{0000}_{\text{Reg3}}\
\underbrace{0000}_{\text{Reg2}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**MOVI:**

$$
\underbrace{0000 \ 0000}_{\text{Inm}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

---

Esto concluye la parte 1.

### Parte 2

- Con el formato que definió, ¿cuántos bits quedan disponibles para el inmediato MOVI? ¿Qué rangos de valores se puede cargar con una única instrucción MOVI?

Con el formato definido, como vimos anteriormente, quedan disponibles 8 bits para el inmediato. 

Por otra parte, viendo la especificación de la instrucción MOVI:

> Carga el inmediato Inm en los bits menos significativos de Reg1, dejando los bits más significativos en 0

Concluimos que los únicos bits que aportan a cambiar el número son los 8 de Inm, por lo que el rango de valores que se puede cargar con MOVI es desde 0x00 a 0xFF.

### Parte 3

- Escriba la secuencia de instrucciones necesaria para dejar la constante 0xABCD en el registro R01.

Esto no tiene mayor complicación que razonar que instrucciones van en que orden. Lo único que quizás no es tan inmediato es que hay que utilizar MOVI para insertar los primeros valores a la CPU.

```
MOVI 0xAB R00
MOVI 0x08 R01 // preparo el shift
SHL R00 R01 R02
MOVI 0xCD R00
OR R02 R00 R01 // ya almaceno el resultado en R01
```

Esta rutina deja la constante 0xABCD en R01.

### Parte 4

- Implemente en alto nivel un programa que sume el contenido de memoria de las direcciones comprendidas en el rango 0xABCD-0xEF00 y almacene en la dirección 0x00FF el triple de dicha suma.

Un detalle importante para esta parte, es que no tenemos el detalle exacto del largo de palabra de una dirección de memoria. Por lo tanto se asume que se mantiene de 16 bits como los registros.

```c
void suma_memoria(short* MEM) {
    short resultado = 0;
    for (short direccion = 0xABCD; direccion <= 0xEF00; direccion++) {
        resultado += MEM[direccion];
    }
    // multiplicación x3 amigable con las instrucciones que tenemos
    MEM[0xFF] = (resultado << 1) + resultado; 
}
```

Donde recibimos en `MEM` lo que hay almacenado en memoria.

### Parte 5

- Implemente en lenguaje ensamblador el programa de la parte anterior.

Esta parte es bastante más interesante, tenemos que traducir la lógica de la parte 4 a ensamblador.
Durante el proceso de "programarlo", tenemos que establecer el rol de los registros:

- R00: lo usaremos como acumulador.
- R01: dirección actual, inicia en 0xABCD.
- R02: dirección final: 0xEF00.
- R03: incrementador +1
- R04: valor actual sacado de memoria
- R05: condición de parada
- R06: para almacenar el resultado
- R07: dirección para almacenar el resultado
- R08: dirección del inicio del bucle
- R09: dirección de la siguiente instrucción post-bucle.

```
// almacenar la dirección inicial en R01
0x0000: MOVI 0xAB R15
0x0001: MOVI 0x08 R14
0x0002: SHL R15 R14 R13
0x0003: MOVI 0xCD R15
0x0004: OR R13 R15 R01
// almacenar la dirección final en R02
0x0005: MOVI 0xEF R15
0x0006: SHL R15 R14 R02
// almacenar la dirección para el resultado en R07
0x0007: MOVI 0xFF R07

// inicializamos el incrementador +1
0x0008: MOVI 0x01 R03
// inicializamos el acumulador
0x0009: MOVI 0x00 R00

// dirección inicial del bucle
0x000A: MOVI 0x0C R08
// dirección para salida del bucle
0x000B: MOVI 0x12 R09

// bucle
0x000C: LOAD R01 R04
0x000D: ADD R04 R00 R00 // agregar valor actual al acumulador

0x000E: SUB R02 R01 R05 // condición de parada
0x000F: JZ R09 // salida del bucle si corresponde

0x0010: ADD R01 R03 R01 // actualizar dirección actual
0x0011: JMP R08 // volver a empezar el bucle

// carga del resultado en memoria
0x0012: SHL R00 R03 R06
0x0013: ADD R00 R06 R06
0x0014: SAVE R06 R07
```

**Nota:** Las direcciones de salto para esta arquitectura conviene calcularlas al final, ya que las instrucciones van desde el 0x0000 para adelante.

### Parte 6

- Implemente en lenguaje máquina las primeras 7 instrucciones del programa de la parte anterior (ensamblador).

Recordemos las primeras 7 instrucciones de la parte anterior:

```
0x0000: MOVI 0xAB R15
0x0001: MOVI 0x08 R14
0x0002: SHL R15 R14 R13
0x0003: MOVI 0xCD R15
0x0004: OR R13 R15 R01
0x0005: MOVI 0xEF R15
0x0006: SHL R15 R14 R02
```

Por otra parte, tenemos las codificaciones que hicimos en la parte 1 del ejercicio:

$$
\begin{array}{c|c||c|c}
\text{Registro}&\text{Binario}&\text{Código}&\text{Binario}\\
\hline
R0&0000&\text{NOP}&0000\\
R1&0001&\text{MOVI}&0001\\
R2&0010&\text{MOVR}&0010\\
R3&0011&\text{LOAD}&0011\\
R4&0100&\text{SAVE}&0100\\
R5&0101&\text{ADD}&0101\\
R6&0110&\text{SUB}&0110\\
R7&0111&\text{SHL}&0111\\
R8&1000&\text{SHR}&1000\\
R9&1001&\text{AND}&1001\\
R10&1010&\text{OR}&1010\\
R11&1011&\text{XOR}&1011\\
R12&1100&\text{NOT}&1100\\
R13&1101&\text{JMP}&1101\\
R14&1110&\text{JC}&1110\\
R15&1111&\text{JZ}&1111\\
\end{array}
$$

Por otra parte, veamos el órden de los bits para las instrucciones:

**Sin registros:**

$$
0000\
0000\
0000\
\underbrace{0000}_{\text{opcode}}
$$

**1 registro:**

$$
0000\
0000\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**2 registros:**

$$
0000\
\underbrace{0000}_{\text{Reg2}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**3 registros:**

$$
\underbrace{0000}_{\text{Reg3}}\
\underbrace{0000}_{\text{Reg2}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$

**MOVI:**

$$
\underbrace{0000 \ 0000}_{\text{Inm}}\
\underbrace{0000}_{\text{Reg1}}\
\underbrace{0000}_{\text{opcode}}
$$


Con esta información ya podemos pasar cada instrucción a lenguaje máquina:

```
0x0000: MOVI 0xAB R15   -> 0xABF1
0x0001: MOVI 0x08 R14   -> 0x08E1
0x0002: SHL R15 R14 R13 -> 0xFED7
0x0003: MOVI 0xCD R15   -> 0xCDF1
0x0004: OR R13 R15 R01  -> 0xDF1A
0x0005: MOVI 0xEF R15   -> 0xEFF1
0x0006: SHL R15 R14 R02 -> 0xFE27
```

---

Esto concluye el ejercicio.