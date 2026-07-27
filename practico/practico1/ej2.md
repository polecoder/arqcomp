# Código de Hamming

**Fecha:** 27-07-2026

## Consigna

Se desea transmitir dígitos decimales en código BCD a través de un canal con ruido. Con ese objetivo se genera a partir del código BCD un código de Hamming de 7 bits. Decodificar el siguiente mensaje asumiendo que a lo sumo ha ocurrido un único error en cada palabra del código. Considere que en esta codificación no se incluyen la marca de fin ni la codificación del signo.

$$
1001000 - 0000000 - 1110100 - 0001111
$$

- Orden de los bits: $m_4m_3m_2p_3m_1p_2p_1$
- Dígito BCD: $m_4m_3m_2m_1$

## Resolución

Para este ejercicio, nuevamente recordemos la tabla vista en el teórico que será de ayuda para construir $S$.

$$
\begin{array}{c|ccccccc}
&a_4&a_3&a_2&p_3&a_1&p_2&p_1\\
\hline
S_0&x&&x&&x&&x\\
S_1&x&x&&&x&x&\\
S_2&x&x&x&x&&&\\
\end{array}
$$

Ahora, vayamos por sección del código dado.

### Sección #1

- $1001000$

Construyamos $S$ para esta sección, y verifiquemos si hay error en la tira.

- $S_2=a_4\oplus a_3\oplus a_2\oplus p_3=1\oplus0\oplus0\oplus1=0$
- $S_1=a_4\oplus a_3\oplus a_1\oplus p_2=1\oplus0\oplus0\oplus0=1$
- $S_0=a_4\oplus a_2\oplus a_1\oplus p_1=1\oplus0\oplus0\oplus0=1$

Entonces $S=011$, por lo que el error se encuentra en la posición $3$. La tira de Hamming original debió haber sido $1001100$.
Concluimos que el mensaje de esta sección es $1001$.

### Sección #2

- $0000000$

Construyamos $S$ para esta sección, y verifiquemos si hay error en la tira.

- $S_2=a_4\oplus a_3\oplus a_2\oplus p_3=0\oplus0\oplus0\oplus0=0$
- $S_1=a_4\oplus a_3\oplus a_1\oplus p_2=0\oplus0\oplus0\oplus0=0$
- $S_0=a_4\oplus a_2\oplus a_1\oplus p_1=0\oplus0\oplus0\oplus0=0$

Entonces $S=000$, por lo que no tenemos error para esta sección.
Concluimos que el mensaje de esta sección es $0000$.

### Sección #3

- $1110100$

Construyamos $S$ para esta sección, y verifiquemos si hay error en la tira.

- $S_2=a_4\oplus a_3\oplus a_2\oplus p_3=1\oplus1\oplus1\oplus0=1$
- $S_1=a_4\oplus a_3\oplus a_1\oplus p_2=1\oplus1\oplus1\oplus0=1$
- $S_0=a_4\oplus a_2\oplus a_1\oplus p_1=1\oplus1\oplus1\oplus0=1$

Entonces $S=111$, por lo que el error se encuentra en la posición $7$. La tira de Hamming original debió haber sido $0110100$.
Concluimos que el mensaje de esta sección es $0111$.

### Sección #4

- $0001111$

Construyamos $S$ para esta sección, y verifiquemos si hay error en la tira.

- $S_2=a_4\oplus a_3\oplus a_2\oplus p_3=0\oplus0\oplus0\oplus1=1$
- $S_1=a_4\oplus a_3\oplus a_1\oplus p_2=0\oplus0\oplus1\oplus1=0$
- $S_0=a_4\oplus a_2\oplus a_1\oplus p_1=0\oplus0\oplus1\oplus1=0$

Entonces $S=100$, por lo que el error se encuentra en la posición $4$. La tira de Hamming original debió haber sido $0000111$.
Concluimos que el mensaje de esta sección es $0001$.

### Conclusión

Juntando lo que hicimos por cada sección, el mensaje final es:

$$
1001-0000-0111-0001
$$

Esto concluye el ejercicio.