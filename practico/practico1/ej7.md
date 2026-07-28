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

Separando el número en su parte entera y fraccionaria, tenemos que: $N=N_e+N_f$.
