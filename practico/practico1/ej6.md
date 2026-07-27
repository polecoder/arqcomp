# Operaciones en complemento a dos

**Fecha:** 27-07-2026

## Consigna

Realizar las siguientes operaciones en complemento a 2, indicando el valor de los bits de condición $Z$ (cero), $N$ (negativo), $C$ (acarreo) y $V$ (overflow).

1. $\text{0x2977}+\text{0x5689}$
2. $\text{0xCAFE}+\text{0xB007}$
3. $\text{0xF21C}+\text{0x0DE4}$
4. $\text{0x5789}-\text{0x021F}$

## Resolución

Antes de empezar, expliquemos detalladamente que representa cada bit de condición indicado por la consigna:

- $Z$ vale $1$ si todos los bits del resultado son $0$. En otro caso vale $0$.
- $N$ copia el valor del bit más significativo del resultado, además indica el signo del resultado.
- $C$ es el valor del último carry de la suma.
- $V$ indica si hubo overflow en la operación, se calcula como con la operación lógica XOR sobre los últimos dos dígitos del carry

Por otra parte, también usaremos mucho la tabla para convertir de hexadecimal a binario:

$$
\begin{array}{c|c|c|c}
0000&0&1000&8\\
\hline
0001&1&1001&9\\
\hline
0010&2&1010&A\\
\hline
0011&3&1011&B\\
\hline
0100&4&1100&C\\
\hline
0101&5&1101&D\\
\hline
0110&6&1110&E\\
\hline
0111&7&1111&F\\
\end{array}
$$

### Operación #1

- $\text{0x2977}+\text{0x5689}$

Primero, convertimos los números de la operación a binario, obteniendo:

- $\text{0x2977}=0010100101110111\text{b}$
- $\text{0x5689}=0101011010001001\text{b}$

Ahora si, podemos realizar la operación normalmente:

$$
\begin{alignedat}{2}
&&01111111111111110\\
\text{0x2977}&&0010100101110111\\
+\text{0x5689}&&0101011010001001\\
\hline
&&1000000000000000\\
\end{alignedat}
$$

Con esto, podemos detallar los valores de los bits condición:

- $Z=0$
- $N=1$
- $C=0$
- $V=1$, por lo que hay overflow

El bit $V=1$ como vimos, indica overflow: dado que ambos operandos son positivos, se esperaría un resultado positivo; pero el resultado obtenido es negativo, pues $N=1$.
Esto significa que la suma verdadera excede el rango representable en $16$ bits, por lo que la interpretación del con signo del resultado no es válida.

### Operación #2

- $\text{0xCAFE}+\text{0xB007}$

Primero, convertimos los números de la operación a binario, obteniendo:

- $\text{0xCAFE}=1100101011111110\text{b}$
- $\text{0xB007}=1011000000000111\text{b}$

Ahora si, podemos realizar la operación normalmente:

$$
\begin{alignedat}{2}
&&10000000111111100\\
\text{0xCAFE}&&1100101011111110\\
+\text{0xB007}&&1011000000000111\\
\hline
&&0111101100000101\\
\end{alignedat}
$$

Con esto, podemos detallar los valores de los bits condición:

- $Z=0$
- $N=0$
- $C=1$
- $V=1$, por lo que hay overflow

El bit $V=1$ como vimos, indica overflow: dado que ambos operandos son negativos (el bit más significativo de ambos es $1$), se esperaría un resultado negativo; pero el resultado obtenido es positivo, pues $N=0$.
Esto significa que la suma verdadera excede el rango representable en $16$ bits, por lo que la interpretación del con signo del resultado no es válida.

### Operación #3

- $\text{0xF21C}+\text{0x0DE4}$

Primero, convertimos los números de la operación a binario, obteniendo:

- $\text{0xF21C}=1111001000011100\text{b}$
- $\text{0x0DE4}=0000110111100100\text{b}$

Ahora si, podemos realizar la operación normalmente:

$$
\begin{alignedat}{2}
&&11111111111111000\\
\text{0xF21C}&&1111001000011100\\
+\text{0x0DE4}&&0000110111100100\\
\hline
&&0000000000000000\\
\end{alignedat}
$$

Con esto, podemos detallar los valores de los bits condición:

- $Z=1$
- $N=0$
- $C=1$
- $V=0$, por lo que no hay overflow

Como no hay overflow, el resultado de esta suma es exactamente $0$.

### Operación #4

- $\text{0x5789}-\text{0x021F}$

Primero, convertimos los números de la operación a binario, obteniendo:

- $\text{0x5789}=0101011110001001\text{b}$
- $\text{0x021F}=0000001000011111\text{b}$

A diferencia de los demás casos, queremos hallar el negativo de $\text{0x021F}$, por lo que nos tomamos un paso intermedio:

- $-\text{0x021F}=1111110111100000\text{b}+1=1111110111100001\text{b}$

Ahora si, podemos realizar la operación normalmente:

$$
\begin{alignedat}{2}
&&11111111100000010\\
\text{0x5789}&&0101011110001001\\
-\text{0x021F}&&1111110111100001\\
\hline
&&0101010101101010\\
\end{alignedat}
$$

Con esto, podemos detallar los valores de los bits condición:

- $Z=0$
- $N=0$
- $C=1$
- $V=0$, por lo que no hay overflow

Como no hay overflow, el resultado de esta suma es exactamente el interpretado, es decir $0101010101101010\text{b}=\text{0x556A}$.
