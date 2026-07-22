# Conversión entre bases

**Fecha:** 22-07-2026

## Consigna

Dados los siguientes números, convertir:

1. $64_{10}$ a base 2
2. $63_{10}$ a base 2
3. $56_{10}$ a base 16
4. $722_{10}$ a base 16
5. $90_{10}$ a base 2
6. $1011_2$ a base 16
7. $90_{10}$ a base 16
8. $65535_{10}$ a base 16
9. $1011_6$ a base 2
10. $\text{CAFE}_{16}$ a base 2
11. $10000_2$ a base 16

## Resolución

### Conversión #1

- $64_{10}$ a base 2

Notemos que $64=2^6$, esto nos deja la conversión a base $2$ en bandeja:

$$
64_{10}=100000\text{b}
$$

### Conversión #2

- $63_{10}$ a base 2

Este caso es bastante similar al anterior, pues $63=2^6-1$, la conversión a base $2$ sería:

$$
63_{10}=011111\text{b}
$$

### Conversión #3

- $56_{10}$ a base 16

Acá ya no es tan fácil, operemos dividiendo por la base.

$$
\begin{aligned}
&56=16\cdot3+8\\
&3=16\cdot0+3
\end{aligned}
$$

Recordemos, los restos en sentido inverso. Entonces el resultado sería:

$$
56_{10}=\text{0x}38
$$

### Conversión #4

- $722_{10}$ a base 16

Operemos dividiendo por la base.

$$
\begin{aligned}
&722=16\cdot45+2\\
&45=16\cdot2+13\\
&2=16\cdot0+2\\
\end{aligned}
$$

Entonces el resultado sería:

$$
722_{10}=\text{0x2C2}
$$

### Conversión #5

- $90_{10}$ a base 2

Operemos dividiendo por la base.

$$
\begin{aligned}
&90=2\cdot45+0\\
&45=2\cdot22+1\\
&22=2\cdot11+0\\
&11=2\cdot5+1\\
&5=2\cdot2+1\\
&2=2\cdot1+0\\
&1=2\cdot0+1\\
\end{aligned}
$$

Entonces el resultado sería:

$$
90_{10}=1011010\text{b}
$$

### Conversión #6

- $1011_2$ a base 16

El cambio con los anteriores para este caso es que tenemos la representación binaria. Recordando el teórico, 4 dígitos binarios equivalen a 1 dígito hexadecimal, por lo tanto:

$$
1011_{2}=\text{0xB}
$$

### Conversión #7

- $90_{10}$ a base 16

Ya tenemos la conversión a binario de la parte 5, tenemos que $90_{10}=1011010\text{b}$. Entonces podemos usar el mismo razonamiento que la conversión anterior, obteniendo:

$$
90_{10}=1011010\text{b}=\text{0x5A}
$$

### Conversión #8

- $65535_{10}$ a base 16

Operemos dividiendo por la base.

$$
\begin{aligned}
&65536=16\cdot4096+0\\
&4096=16\cdot256+0\\
&256=16\cdot16+0\\
&16=16\cdot1+0\\
&1=16\cdot0+1\\
\end{aligned}
$$

Entonces el resultado sería:

$$
\begin{aligned}
&65536_{10}=\text{0x10000}
\end{aligned}
$$

### Conversión #9

- $1011_6$ a base 2

Primero convertimos a base 10, para luego pasar a base 2 más facilmente. Operemos:

$$
1011_6=1\cdot6^3+1\cdot6+1=216+6+1=223
$$

Ahora si, convertimos a base 2.

$$
\begin{aligned}
&223=2\cdot111+1\\
&111=2\cdot55+1\\
&55=2\cdot27+1\\
&27=2\cdot13+0\\
&13=2\cdot6+1\\
&6=2\cdot3+0\\
&3=2\cdot1+1\\
&1=2\cdot0+1\\
\end{aligned}
$$

Entonces el resultado sería:

$$
1011_6=223_{10}=11010111\text{b}
$$

### Conversión #10

- $\text{CAFE}_{16}$ a base 2

Para esta parte recordemos la tabla de conversión que relaciona las bases 2, 8 y 16.

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

Utilizando la tabla, expresamos directamente el resultado:

$$
\text{CAFE}_{16}=1100101011111110\text{b}
$$

### Conversión #11

- $10000_2$ a base 16

Recordando el teórico, 4 dígitos binarios equivalen a 1 dígito hexadecimal, por lo tanto:

$$
10000_2=\text{0x10}
$$

---

Esto concluye el ejercicio.