# Sumas de productos canónicos

**Fecha:** 14-09-2026

## Consigna

Expresar las siguientes funciones booleanas como suma de productos canónicos:

1. $f_1(a,b,c)=\Sigma(1,4,5,6)$
2. $f_2(a,b,c,d)=\Sigma(0,1,4,6,8,9,12,14)$
3. $f_3(a,b,c)=\Pi(2,3,6,7)$

## Resolución

Será muy útil para estos ejercicios tener una tabla de la representación en binario de los números hasta por lo menos $14$.
Esto nos permitirá ver directamente que variables aparecen normales y cuales aparecen complementadas para cada valor de las funciones.

$$
\begin{array}{|c|c|c|c|}
\hline
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
\hline
\end{array}
$$

### Función #1

- $f_1(a,b,c)=\Sigma(1,4,5,6)$

Utilizando la tabla, tenemos que:

$$
f_1(a,b,c)=\overline{ab}c+a\overline{bc}+a\overline{b}c+\overline{ab}c
$$

### Función #2

- $f_2(a,b,c,d)=\Sigma(0,1,4,6,8,9,12,14)$

Utilizando la tabla, tenemos que:

$$
\begin{aligned}
f_2(a,b,c,d)=\ &\overline{abcd}+\overline{abc}d+\overline{a}b\overline{cd}+\overline{a}bc\overline{d}\ +\\
&a\overline{bcd}+a\overline{bc}d+ab\overline{cd}+abc\overline{d}
\end{aligned}
$$

### Función #3

- $f_3(a,b,c)=\Pi(2,3,6,7)$

Notemos que $\Pi(2,3,6,7)=\Sigma(0,1,4,5)$. Con esto podemos utilizar la tabla y hallar la expresión que buscamos:

$$
f_3(a,b,c)=\overline{abc}+\overline{ab}c+a\overline{bc}+a\overline{b}c
$$

---

Esto concluye el ejercicio.