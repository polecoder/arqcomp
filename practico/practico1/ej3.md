# Enteros sin signo

**Fecha:** 27-07-2026

## Consigna

Escribir los números (decimales) $1023$, $45216$ y $71822$ en representación de enteros sin signo binario de 16 bits.

## Resolución

### Número #1

- $1023$

Este caso es bastante sencillo ya que $1023=2^{10}-1$. Entonces, su representación es la siguiente:

$$
000000111111111
$$

Donde los ceros a la izquierda se rellenan para llegar a la cantidad de bits deseada.

### Número #2

- $45216$

Para este caso, tendremos que realizar las divisiones enteras para expresar el resultado.

$$
\begin{aligned}
&45216=2\cdot 22608+0\\
&22608=2\cdot 11304+0\\
&11304=2\cdot 5652+0\\
&5652=2\cdot 2826+0\\
&2826=2\cdot 1413+0\\
&1413=2\cdot 706+1\\
&706=2\cdot 353+0\\
&353=2\cdot 176+1\\
&176=2\cdot 88+0\\
&88=2\cdot 44+0\\
&44=2\cdot 22+0\\
&22=2\cdot 11+0\\
&11=2\cdot 5+1\\
&5=2\cdot 2+1\\
&2=2\cdot 1+0\\
&1=2\cdot 0+1\\
\end{aligned}
$$

Entonces, la representación es la siguiente:

$$
1011000010100000
$$

### Número #3

- $71822$

Para este caso, también tendremos que realizar las divisiones enteras para expresar el resultado.

$$
\begin{aligned}
&71822=2\cdot 35911+0\\
&35911=2\cdot 17955+1\\
&17955=2\cdot 8977+1\\
&8977=2\cdot 4488+1\\
&4488=2\cdot 2244+0\\
&2244=2\cdot 1122+0\\
&1122=2\cdot 561+0\\
&561=2\cdot 280+1\\
&280=2\cdot 140+0\\
&140=2\cdot 70+0\\
&70=2\cdot 35+0\\
&35=2\cdot 17+1\\
&17=2\cdot 8+1\\
&8=2\cdot 4+0\\
&4=2\cdot 2+0\\
&2=2\cdot 1+0\\
&1=2\cdot 0+1\\
\end{aligned}
$$

Entonces, la representación es la siguiente:

$$
10001100010001110
$$

---

Esto concluye el ejercicio.