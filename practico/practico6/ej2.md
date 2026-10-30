# Definir formatos de instrucción

**Fecha:** 30-10-2026

## Consigna

Sea una CPU de 16 bits que tiene 8 registros de uso general de 16 bits y en la que las instrucciones se codifican en 16 bits. La CPU posee las siguientes instrucciones:

| Instrucción | Descripción |
|-------------|-------------|
| LOAD REG1, REG2 | Guarda en REG2 el contenido de la dirección de memoria almacenada en REG1 |
| STORE REG1, REG2 | Escribe el contenido de REG1 en la dirección de memoria almacenada en REG2 |
| NOP | No realiza ninguna operación |
| SETLO INM, REG | Carga el valor constante INM de 8 bits en la parte baja de REG |
| SETHI INM, REG | Carga el valor constante INM de 8 bits en la parte alta de REG |
| ADD REG1, REG2, REG3 | Suma REG1 y REG2, guarda el resultado en REG3 |
| SUB REG1, REG2, REG3 | Resta REG2 a REG1, guarda el resultado en REG3 |
| AND REG1, REG2, REG3 | And bit a bit entre REG1 y REG2, guarda el resultado en REG3 |
| OR REG1, REG2, REG3 | Or bit a bit entre REG1 y REG2, guarda el resultado en REG3 |
| NOT REG1, REG2 | Guarda en REG2 el resultado de hacerle un NOT al REG1 |
| SL REG1, INM, REG2 | Shift a la izquierda de tantos bits de REG1 como indique el inmediato INM (de 4 bits) y guarda el resultado en REG2 |
| SR REG1, INM, REG2 | Shift a la derecha de tantos bits de REG1 como indique el inmediato INM (de 4 bits) y guarda el resultado en REG2. Expande el signo. |
| CMP REG1, REG2 | Instrucción de comparación que realiza la resta REG1-REG2 sin guardar el resultado y actualiza las banderas de condición |
| JZ INM | Si la bandera $Z$ está en 1 salta INM (INM es un número con signo de 12 bits) instrucciones desde la posición actual |
| JN INM | Si la bandera $N$ está en 1 salta INM (INM es un número con signo de 12 bits) instrucciones desde la posición actual |
| JMP INM | Salto incondicional de INM instrucciones (INM es un número con signo de 12 bits) desde la posición actual |

Se pide: Diseñe el formato de instrucción presentado e indique la codificación de cada instrucción.

## Resolución

Lo que nos piden en este ejercicio es el diseño del formato de instrucción. Esto cuenta de varias partes, por lo que iremos una a una:

### 1. Largo de instrucción

Ya está dado en la letra, cada instrucción es de 16 bits.

### 2. Nomenclatura y codificación de registros

La letra nos da 8 registros de uso general, por lo que los nomenclamos y codificamos de forma estándar (por ejemplo R05 -> 101).
Concluimos que se necesitan 3 bits para codificar cada registro.

### 3. Codificación de instrucciones

Para codificar las instrucciones creamos una tabla que asocie a cada instrucción un binario. Como tenemos 16 instrucciones, las codificaciones serán de 4 bits.

$$
\begin{array}{c|c}
\text{Opcode}&\text{Binario}\\
\hline
\text{LOAD}&0000\\
\text{STORE}&0001\\
\text{NOP}&0010\\
\text{SETLO}&0011\\
\text{SETHI}&0100\\
\text{ADD}&0101\\
\text{SUB}&0110\\
\text{AND}&0111\\
\text{OR}&1000\\
\text{NOT}&1001\\
\text{SL}&1010\\
\text{SR}&1011\\
\text{CMP}&1100\\
\text{JZ}&1101\\
\text{JN}&1110\\
\text{JMP}&1111\\
\end{array}
$$

### 4. Agrupación de instrucciones y subformatos de instrucción

En esta parte, agruparemos las instrucciones según cuántos registros se utilizan en cada operación, y según los tamaños de los inmediatos.

A continuación, especificamos las instrucciones que solamente cuentan con registros:

- **0 registros (NOP):**

$$
0000\
0000\
0000\
\underbrace{0000}_{\text{opcode}}
$$

- **2 registros (LOAD, STORE, NOT, CMP):**

$$
0000\
\underbrace{0000}_{\text{REG2}}\
\underbrace{0000}_{\text{REG1}}\
\underbrace{0000}_{\text{opcode}}
$$

- **3 registros (ADD, SUB, AND, OR):**

$$
\underbrace{0000}_{\text{REG3}}\
\underbrace{0000}_{\text{REG2}}\
\underbrace{0000}_{\text{REG1}}\
\underbrace{0000}_{\text{opcode}}
$$


Y ahora, especificamos las instrucciones que cuentan con un inmediato, distinguiendo por cantidad de registros y tamaño del inmediato:

- **12 bits con signo, 0 registros (JZ, JN, JMP):**

$$
\underbrace{0000\ 0000\ 0000}_{\text{INM}}\
\underbrace{0000}_{\text{opcode}}
$$

- **8 bits, 1 registro (SETLO, SETHI):**

$$
\underbrace{0000\ 0000}_{\text{INM}}\
\underbrace{0000}_{\text{REG1}}\
\underbrace{0000}_{\text{opcode}}
$$

- **4 bits, 2 registros (SL, SR):**

$$
\underbrace{0000}_{\text{REG2}}\
\underbrace{0000}_{\text{INM}}\
\underbrace{0000}_{\text{REG1}}\
\underbrace{0000}_{\text{opcode}}
$$

---

Esto concluye el ejercicio.
