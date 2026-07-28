# Representación punto flotante

**Fecha:** 28-07-2026

## Consigna

Sumar los números $\text{3EE00000h}$ y $\text{3D800000h}$, representados en punto flotante, formato IEEE de precisión simple (1 bit para el signo, 8 bits para el exponente y 23 bits para la mantisa) y expresar el resultado normalizado en hexadecimal.

## Resolución

Para empezar este ejercicio, convertimos a binario los números dados:

- $\text{3EE00000h}=00111110111000000000000000000000\text{b}$
- $\text{3D800000h}=00111101100000000000000000000000\text{b}$

Ahora para ambos números, debemos recordar que significa la representación punto flotante.

- $\text{3EE00000h}$ tiene:
    - Signo $s=0$, el número es positivo
    - Exponente $e=01111101=125_{10}$
    - Mantisa $M=1.11000000000000000000000$
    - Entonces esta expresión representa el número $N=2^{125-127}\cdot(1.11000000000000000000000)$
- $\text{3D800000h}$ tiene:
    - Signo $s=0$, el número es positivo
    - Exponente $e=01111011=123_{10}$
    - Mantisa $M=1.00000000000000000000000$
    - Entonces esta expresión representa el número $N=2^{123-127}\cdot(1.00000000000000000000000)$

Con estos datos, podemos recordar el teórico que nos indica como sumar números bajo esta representación:

- Alinear los exponentes (llevando el menor al mayor), adecuando concordadamente la mantisa (teniendo en cuenta el $1$ omitido)
- Sumar las mantisas teniendo en cuenta los signos
- Normalizar, acomodando la mantisa y el exponente si corresponde

### Paso #1 - Alinear los exponentes

Queremos llevar el exponente más chico al exponente más grande. Para esto, movemos la coma de $\text{3D800000h}$ en dos lugares, de modo que su representación queda como:

- $\text{3D800000h}=2^{125-127}\cdot(0.01000000000000000000000)$

Con esto tenemos los exponentes alineados, ambos los exponentes reales son $-2$.

### Paso #2 - Sumar las mantisas

Ahora estamos en condiciones de sumar las mantisas:

$$
\begin{alignedat}{2}
&&11.10000000000000000000000\\
&&1.11000000000000000000000\\
+&&0.01000000000000000000000\\
\hline
&&10.00000000000000000000000\\
\end{alignedat}
$$

### Paso #3 - Normalizar

Llegados a este punto, normalizar (para la mantisa), significará correr la coma hasta solo tener un $1$ del lado izquierdo.
La forma normalizada para la mantisa que obtuvimos es:

$$
1.000000000000000000000000\cdot2^{1}
$$

Donde hay que realizar dos observaciones:

1. Como en la suma del paso anterior, necesitamos un bit extra para almacenar la información, tendremos que eliminar el bit menos significativo para "acomodar" esta situación. Como es cero en nuestro caso, no necesitamos redondear.
2. Notemos que multiplicamos por $2^1$, esto implica un acomodo en el exponente final:
    - Antes: $125-127$
    - Ahora: $126-127$

---

Con todo este razonamiento, expresemos el resultado:

- Signo $s=0$, el número es positivo por ser suma de positivos.
- Exponente $e=126_{10}=01111110$
- Mantisa $M=1.00000000000000000000000$

Podemos concluir entonces que el resultado final es:

$$
00111111000000000000000000000000\text{b}
$$

Y en hexadecimal:

$$
\text{3F000000h}
$$

Esto concluye el ejercicio.