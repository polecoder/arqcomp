# Corrección de errores y sistema de Hamming

**Fecha:** 27-07-2026

## Consigna

1. Explicar si los siguientes sistemas son capaces de corregir errores:

   - Código de Hamming
   - Paridad

2. Codificar en el sistema de Hamming la tira $1001$.

## Resolución

### Parte 1

- Explicar si los siguientes sistemas son capaces de corregir errores:

   - Código de Hamming
   - Paridad

Como ya vimos en las preguntas teóricas, el sistema de paridad no es capaz por si solo de corregir errores, ya que no identifica la posición en la que sucede el cambio de bit no deseado.
Por otra parte, el código de Hamming está diseñado para identificar la posición exacta donde ocurre el error, por lo que si es capaz de corregir errores a través del valor de $S$. Si este es cero, entonces **no hay error**. Por otro lado, si el valor es distinto de cero, entonces $S$ indica **la posición** en la que se efectuó un cambio de bit

### Parte 2

- Codificar en el sistema de Hamming la tira $1001$.

Recordemos la siguiente tabla, que es fundamental para obtener el resultado deseado en el sistema de Hamming:

$$
\begin{array}{c|ccccccc}
&a_4&a_3&a_2&p_3&a_1&p_2&p_1\\
\hline
S_0&x&&x&&x&&x\\
S_1&x&x&&&x&x&\\
S_2&x&x&x&x&&&\\
\end{array}
$$

Por lo tanto:

- $p_1=a_4\oplus a_2\oplus a_1=0$
- $p_2=a_4\oplus a_3\oplus a_1=0$
- $p_3=a_4\oplus a_3\oplus a_2=1$

Concluimos que la tira $1001$ es la siguiente en el sistema de Hamming:

$$
1001100
$$

Esto concluye el ejercicio.