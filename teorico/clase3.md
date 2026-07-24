# Representación interna de datos

**Fecha:** 24-07-2026

## Representación ASCII

El **ASCII** es un código de $7$ bits que especifica la representación de las letras y símbolos especiales usados en el idioma inglés norteamericano, incluyendo también los números.

De los $128$ valores posibles del código ($7$ bits), tenemos que:

- $10$ se utilizan para los dígitos decimales (del $\text{30h}$ al $\text{39h}$)
- $26$ para letras minúsculas del $\text{61h}$ al $\text{7Ah}$
- $34$ para símbolos especiales (espacio, !, #, etc.)
- Los primeros $32$ que se denominan caracteres de control; éstos se utilizan para la comunicación de datos y con los fines de dar formato a los textos en impresoras y pantallas de video

La forma más habitual para representar el código ASCII, y en general todos los sistemas de codificación de caracteres, es a través de una matriz cuyas columnas están asociadas a los $3$ bits más significativos del código, y sus filas a los $4$ menos significativos, como se ve en la siguiente tabla.

![Figura 1](./img/clase3fig1.png)

## Representación de tipo natural (número entero sin signo)

### Representación binaria

Los enteros sin signo (siempre positivos, incluyendo el $0$), poseen la representación más simple: su código binario coincide con su expresión en base $2$ restringida a número fijo de bits.
Se utilizan para contadores, direcciones, punteros y para derivar otros tipos.

Las operaciones elementales de este tipo son las cuatro usuales para los números enteros $(+,-,\times,/)$.

Por ejemplo, en la suma de $2$ enteros sin signo, se aplica el algoritmo usual para los números binarios. Veamos algunos casos con representaciones de $8$ bits.

**Ejemplo 1:**

$$
\begin{alignedat}{2}
&&000110000 &\Rightarrow \text{carry final}=0\\
25 &&00011001\\
+74 &&01001010\\
\hline
99 &&01100011
\end{alignedat}
$$

**Ejemplo 2:**

$$
\begin{alignedat}{2}
&&111110000 &\Rightarrow \text{carry final}=1\\
25 &&00011001\\
+234 &&11101010\\
\hline
259 &&01100011 &\Rightarrow \text{la representación no es correcta}\\
\end{alignedat}
$$

Los bits de carry (acarreo) de las operaciones anteriores se presentan en la primera línea de la operación.
Observemos que si el último bit de acarreo (denominado bit de acarreo o **carry** de la operación) es $1$, el código binario resultante no representa al resultado de la operación (como pasó en el ejemplo 2).
En este caso, decimos que ha ocurrido un **overflow**, que significa que nos salimos del rango de representación.

Este es el problema que trae esta representación, estamos restringidos en el tamaño de números con los que podemos trabajar.
Los tamaños usuales para esta representación son:

- El byte: $0$ a $255$.
- Dos bytes: $0$ a $2^{16}-1$
- Cuatro bytes: $0$ a $2^{32}-1$
- Ocho bytes: $0$ a $2^{64}-1$
- En general para $n$ bits: $0\leq N\leq2^n-1$

## Representación de tipo entero (con signo)

### Representación valor absoluto y signo

Si tenemos $n$ bits para representar el número, tomamos uno de ellos, en particular, el primero de ellos para el signo y el resto representa el valor absoluto del número en binario.

- Si el primer bit es $1$, entonces el número representado es negativo.
- En caso contrario, el número representado es positivo.

**Ejemplo:**

$$
0110\Rightarrow6\\
1110\Rightarrow-6\\
$$

Para $n$ bits, el rango del número representado es:

- $-(2^{n-1}-1)\leq N\leq 2^{n-1}-1$

Con esta representación existen dos códigos para el $0$; $1000$ y $0000$ ($4$ bits). Esto puede verse como un inconveniente.

Además, las operaciones no trabajan directamente con la representación, sino que deben interpretarse en base a los signos relativos.

### Representación complemento a uno

Para esta representación, los números positivo se representan en binario, mientras que los números negativos, se representan como su valor absoluto pero complementado bit a bit.

**Nota:** Complementado significa que si en un bit hay un $0$, entonces su valor complementado es $1$ y viceversa.

Para $n$ bits, el rango representado es:

- $-(2^{n-1}-1)\leq N\leq 2^{n-1}-1$

Por ejemplo, la siguiente tabla describe esta representación para $4$ bits:

$$
\begin{array}{c|c||c|c}
-7&1000&7&0111\\
-6&1001&6&0110\\
-5&1010&5&0101\\
-4&1011&4&0100\\
-3&1100&3&0011\\
-2&1101&2&0010\\
-1&1110&1&0001\\
0&1111&0&0000\\
\end{array}
$$

El orden en binario (interpretando los códigos como si fueran números en base $2$ aunque no lo son) no corresponde al orden de los números que representa. Otra desventaja es que aquí también existen dos representaciones para el cero.

### Representación desplazamiento

La representación por desplazamiento supone un corrimiento de los valores a representar según un valor $d$ (llamado desplazamiento); posteriormente se le aplica el módulo para que pueda ser almacenada en el tamaño de la representación deseada.
Para el desplazamiento, se supone que el valor codificado (resultado de la operación $N+d$) es un número que para $n$ bits es un valor entre $0$ y $2^{n-1}$, por lo que permite representar valores desde $-d$ hasta $2^n-d-1$.
En general, para representar $2^n$ números diferentes, se asigna a $d$ el valor $2^{n-1}$ o $2^{n-1}-1$.

**Ejemplo:** Sea $n=4$ el número de bits y $d=8$ el desplazamiento elegido, entonces:

$$
\begin{aligned}
-8&\Rightarrow0000\\
-7&\Rightarrow0001\\
&\ \ \vdots\\
-1&\Rightarrow0111\\
0&\Rightarrow1000\\
1&\Rightarrow1001\\
&\ \ \vdots\\
6&\Rightarrow1110\\
7&\Rightarrow1111\\
\end{aligned}
$$

La propiedad más importante de esta representación, es que los códigos conservan el órden de los números, con o sin signo. En particular, toda representación de un número negativo es menor que cualquiera de un número positivo. Otra ventaja es que existe una sola representación para el cero.

Por otra parte, la gran desventaja de esta representación es que los algoritmos para las operaciones usuales son más complejos.

### Representación complemento a dos

Los números positivos se representan directamente en binario y para conseguir el código de los negativos, se complementa el valor absoluto y se los incrementa en uno.

Por ejemplo, sea $70=01000110$, entonces para obtener la representación de $-70$:

1. Hacemos la negación bit a bit: $10111001$.
2. Le sumamos $1$, obteniendo: $10111010$.

Las propiedades más importantes de esta representación son:

- Mantiene la suma (la suma con signo o sin signo es la misma operación, es decir que el algoritmo es el mismo). Es decir que la suma de las representaciones da la representación de la suma de los números representados, sean estos positivos o negativos.
- Es coherente la representación del cero, esto es que existe una sola representación para este número.
- Se pierde la relación de orden. El algoritmo de comparación de $A$ con $B$ depende de los signos de estos números.
- La resta se hace sumando el negativo del sustraendo, con lo que también se mantiene.

    $$
    A-B=A+(-B)=A+\neg B+1
    $$

Al igual que en toda la representación de largo fijo las operaciones pueden generar overflow. Pero a diferencia del caso de la representación binaria (entero sin signo), en este caso el bit de carry no indica por si solo esta condición.
Veamos a continuación algunos ejemplos ilustrativos de todos los casos posibles.

#### Ejemplo 1

$$
\begin{alignedat}{2}
&&000110000 &\Rightarrow \text{carry final}=0\\
25 &&00011001\\
+74 &&01001010\\
\hline
99 &&01100011 &\Rightarrow \text{representación correcta, overflow}=0\\
\end{alignedat}
$$

#### Ejemplo 2

$$
\begin{alignedat}{2}
&&111110000 &\Rightarrow \text{carry final}=1\\
25 &&00011001\\
-22 &&11101010\\
\hline
3 &&00000011 &\Rightarrow \text{representación correcta, overflow}=0\\
\end{alignedat}
$$

#### Ejemplo 3

$$
\begin{alignedat}{2}
&&011100000 &\Rightarrow \text{carry final}=0\\
25 &&00011001\\
+114 &&01110010\\
\hline
139 &&10001011 &\Rightarrow -117\text{ representación incorrecta, overflow}=1\\
\end{alignedat}
$$

#### Ejemplo 4

$$
\begin{alignedat}{2}
&&100010000 &\Rightarrow \text{carry final}=1\\
-120 &&10001000\\
-22&&11101010\\
\hline
-142 &&01110010 &\Rightarrow 114\text{ representación incorrecta, overflow}=1\\
\end{alignedat}
$$

En esta representación, para saber si hubo **overflow** al final de la operación hay que verificar la existencia de acarreo en los dos bits más significativos. Podemos verificar esto en los $4$ ejemplos.

---

Por otra parte, para el caso de la multiplicación sucede algo paradójico: si bien la representación mantiene también dicha operación, la forma usual de implementar la operación no lo hace.
El problema es que la multiplicación binaria se implementa normalmente dando el resultado en el doble de bits que los operandos. Esto es: si los operandos son de $n$ bits, entonces el resultado de la multiplicación se calcula en $2n$ bits (esto es porque si mantuvieramos los $n$ bits rápidamente nos iríamos de los límites de la representación al multiplicar números pequeños).

Al extender el resultado de la operación a $2n$ bits la propiedad de mantener la multiplicación deja de ser cumplida por la representación **complemento a dos**.
En resumen, en los casos prácticos que trabajaremos daremos por entendido que esta representación funciona correctamente con la suma de la forma que conocemos, pero no así con la multiplicación.