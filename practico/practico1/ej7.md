# Representación punto fijo

**Fecha:** 27-07-2026

## Consigna

Se tiene un número fraccional expresado en punto fijo, asumiendo que se disponen de 16 bits en total y 6 bits para la parte fraccional.

1. Indique el mayor número representable.
2. Represente $5.125$ y $-8.75$.
3. Dada la siguiente tira de bits: $1100100011000000$, provea el resultado numérico resultante de interpretar la tira provista como punto fijo (8 bits para la parte fraccionaria en este caso).

## Resolución

### Parte 1

- Indique el mayor número representable.

Recordemos que la representación se obtiene representando en complemento a dos el número resultante de multiplicar $N$ por $2^f$, siendo $f$ el número de bits dedicados a la parte fraccional.

El mayor número representable sería aquel con los 22 bits valiendo $1$, a excepción del primero que valdría $0$ para mantener el signo positivo. Entonces el mayor número representable para esta representación sería:

$$
0111111111111111
$$

Que es equivalente a $0111111111.111111\times 2^6$. O en base decimal equivalente a $511.984375\times2^6$.

Por lo tanto, el mayor número representable es $511.984375$.

### Parte 2

- Represente $5.125$ y $-8.75$.

Separemos este ejercicio por cada número.

#### Número #1

- $N=5.125$

Separando el número en su parte entera y fraccionaria, tenemos que: $N=N_e+N_f$, donde podemos calcular ambas partes usando las estrategias que ya conocemos:

- $N_e=0000000101$ ($10$ bits)

Para la parte fraccionaria:

$$
\begin{aligned}
&2(0.125)=0.25\Rightarrow a_{-1}=0\\
&2(0.25)=0.50\Rightarrow a_{-2}=0\\
&2(0.50)=1.00\Rightarrow a_{-3}=1\\
\end{aligned}
$$

Entonces $N_f=0.001000$ ($6$ bits).
Sumamos las partes obtenidas para obtener el resultado:

$$
N_e+N_f=0000000101.001000
$$

Y recordemos que la representación punto fijo es aquel número que representa $N$ multiplicado por $2^f$. En este caso ese número sería:

$$
0000000101001000
$$

Faltaría pasar a la representación complemento a dos, pero ya está hecho pues el número es positivo. Concluimos que el resultado final es:

$$
0000000101001000
$$

#### Número #2

- $N=-8.75$

Separando el número en su parte entera y fraccionaria, tenemos que: $N=N_e+N_f$, donde podemos calcular ambas partes usando las estrategias que ya conocemos:

- $N_e=0000001000$ ($10$ bits)

Para la parte fraccionaria:

$$
\begin{aligned}
&2(0.75)=1.50\Rightarrow a_{-1}=1\\
&2(0.50)=1.00\Rightarrow a_{-2}=1\\
\end{aligned}
$$

Entonces $N_f=0.110000$ ($6$ bits).
Sumamos las partes obtenidas para obtener el resultado:

$$
N_e+N_f=0000001000.110000
$$

Ahora tenemos que multiplicar por $2^6$, obteniendo:

$$
0000001000110000
$$

Y en este caso, el número que tenemos si es negativo, por lo tanto, hacemos el complemento a dos del mismo.

$$
\begin{aligned}
&1111110111001111+1\\
&=\scriptstyle{(\text{operatoria})}\\
&1111110111010000\\
\end{aligned}
$$

Entonces el resultado final es:

$$
1111110111010000
$$

Esto concluye esta parte.

### Parte 3

- Dada la siguiente tira de bits: $1100100011000000$, provea el resultado numérico resultante de interpretar la tira provista como punto fijo (8 bits para la parte fraccionaria en este caso).

Recordemos que en el proceso de obtención de un número en representación punto fijo, multiplicamos por $2^f$. En este caso empezaríamos por dividir por este número, para poder operar para obtener la parte entera y fraccionaria del número.

$$
11001000.11000000\times 2^8
$$

Y ahora, tenemos dos conversiones que hacer para obtener el resultado; la parte entera está en representación complemento de dos, mientras que la parte fraccionaria es simplemente un entero en binario.

#### Parte entera

- Queremos hallar la parte entera del número, recordando que está en complemento de dos.

Ya que el número es negativo, la estrategia será realizar la operación inversa que la que utilizamos para calcular el negativo de un número, es decir:

1. Complementamos el número.
2. Sumamos $1$.
3. Cambiamos el signo para obtener el resultado final.

Entonces:

$$
\begin{aligned}
&11001000\\
&\Rightarrow\scriptstyle{(\text{complementando})}\\
&00110111\\
&\Rightarrow\scriptstyle{(\text{sumando }1)}\\
&00111000\\
\end{aligned}
$$

E interpretando $00111000\text{b}$ en base decimal, tenemos que:

$$
32+16+8=56
$$

Entonces la parte entera del número es:

$$
N_e=-56
$$

#### Parte fraccionaria

- Queremos hallar la parte fraccionaria del número.

Para esto simplemente utilizamos las estrategias que conocemos desde el práctico cero:

1. Consideramos el polinomio característico.
2. Lo evaluamos en $1/b$.

Recordemos el número que queremos convertir a decimal antes de empezar: $0.11000000$

$$
\begin{aligned}
&N_f\\
&=\scriptstyle{(\text{estrategia elegida})}\\
&P(1/2)\\
&=\scriptstyle{(\text{definición del polinomio característico})}\\
&x+x^2\\
&=\scriptstyle{(\text{reemplazando})}\\
&1/2+1/4\\
&=\scriptstyle{(\text{operatoria})}\\
&3/4\\
&=\scriptstyle{(\text{operatoria})}\\
&0.75\\
\end{aligned}
$$

#### Conclusión

Juntando lo que obtuvimos en las dos partes, tenemos que el número convertido es:

$$
N=N_e+N_f=-56+0.75=-55.25
$$

Esto concluye el ejercicio.