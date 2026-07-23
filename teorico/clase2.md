# Códigos y errores

**Fecha:** 23-07-2026

## Detección y corrección de errores

Todos los sistemas de codificación que permiten detección y corrección de errores se basan en una misma idea: redundancia.

El fundamento es sencillo: para poder distinguir si un valor es correcto o no (o sea, representa un objeto válido del sistema o no) debo agregar información adicional al código.
Entonces en todo sistema de codificación con capacidad de detectar errores, el número de objetos representados es siempre menor que el número de valores posibles del código binario utilizado.

Esta afirmación quedará más clara cuando veamos las distintas estrategias utilizadas. De todas formas un ejemplo sencillo puede ser de utilidad:

- Supongamos que tenemos $16$ objetos a representar; en principio, con un código binario de $4$ bits alcanzaría, pues $2^4=16$. El tema es que con $4$ bits no estaríamos en condiciones de detectar errores, puesto que todos los valores posibles del código binario utilizado se corresponden con un objeto válido del sistema. De esta manera, si hubiera un error en un bit del código que representa a un objeto $A$, se transformaría en el código que representa a un objeto $B$, por lo que no habría posiblidad de detectarlo.

En este caso, para disponer de la posibilidad de distinguir un código binario correcto, de uno incorrecto, deberíamos utilizar un código binario de, por ejemplo, $6$ bits. 
Con $6$ bits tendríamos $64$ valores posibles, de los cuales sólo $16$ representarían objetos reales.
Si elegimos convenientemente los códigos asociados a cada objeto, estamos en condiciones de detectar errores producidos por el cambio de un bit en el código que representa a un objeto.

## Distancia entre representaciones binarias

### Definición

La distancia entre dos representaciones binarias se define como el número de bits entre los dos códigos.
Es decir que si tenemos dos códigos binarios $a$ y $b$:

- $a=(a_0,a_1,\ldots,a_n)$
- $b=(b_0,b_1,\ldots,b_n)$

La distancia entre ellos $d(a,b)$ está dada por:

$$
d(a,b)=(a_0\oplus b_0)+(a_1\oplus b_1)+\ldots+(a_n\oplus b_n)
$$

Donde $\oplus$ representa a la operación **O Exclusivo (XOR)**.
Por ejemplo, los códigos:

$$
01101\\
10110\\
$$

Tienen una distancia de cuatro.

### Propiedades

Así como la definimos la "distancia" tal cual la hemos definido tiene las siguientes propiedades (que no vamos a demostrar):

1. $d(a,b)=d(b,a)$
2. $d(a,b)=0\iff a=b$
3. $d(a,b)+d(b,c)\geq d(a,c)$

---

Se pueden generar sistemas de codificación binarios que tengan una determinada distancia. En el caso de un sistema de codificación, se llama "distancia" del código a la mínima distancia que exista entre dos valores válidos del código.
La posibilidad de detectar y corregir errores está fuertemente vínculada, como es de suponer, a la distancia del código utilizado.

Consideremos un sistema de códificación de distancia $d$ y dos objetos válidos del código llamados $A$ y $B$. Consideremos el conjunto de valores posibles del sistema de codificación (no necesariamente válidos) que se obtienen de modificar hasta $e$ bits del valor $A$. Idem para los del valor $B$.

Si estos conjuntos relacionados con los objetos $A$ y $B$ no comparten ningún valor, podemos no solo detectar la existencia de un error, sino que además podemos reconocer a que código correcto corresponde un valor "degenerado" dado, permitiendo por lo tanto la corrección del error.
Si los conjuntos tienen puntos en común en cambio, no podremos corregir, pero si detectar errores, siempre y cuando dentro del conjunto de puntos asociado al valor $A$ no se incluya al valor $B$, puesto que si no, la alteración de un código válido puede conducir a otro válido, impidiendo así detectar el error.

De este razonamiento, concluimos dos condiciones importantes:

- Para que se puedan **detectar errores**, necesitamos que $e<d$.
- Para que se puedan **corregir errores**, necesitamos que $e<d/2$.

## Bits de redundancia

Se puede demostrar que para generar códigos de distancia $3$ para objetos representables en $k$ bits, se necesitan usar $p$ bits adicionales, llamados bits de redundancia o de paridad, tal que se cumpla:

$$
2^p\geq p+k+1
$$

El razonamiento detrás de esta afirmación, es que para poder identificar el bit que tiene error dispondremos de $2^p$ combinaciones posibles, generadas por los $p$ bits.
Con esas combinaciones debemos poder señalar cual es el bit errado de los $p+k$ bits que tendrá el código completo. Además precisaremos tener una combinación para indicar que no hay error, de ahí surge que el lado derecho de la expresión es $p+k+1$.

Por ejemplo, supongamos que queremos diseñar un sistema de memoria de una computadora de $16$ bits, de manera que sea capaz de corregir errores de hasta $1$ bit. De acuerdo a la desigualdad que vimos, necesitamos agregar $5$ bits de redundancia para que esto sea posible.

$$
2^5=32\geq5+16+1=22
$$

Notemos que esto no se cumple para $p=4$.

Veamos a continuación una tabla que muestra los valores de $p$ (bits agregados) para distintos valores de $k$ (bits del código original).

$$
\begin{array}{c|c}
k&p\\
\hline
4&3\\
8&4\\
16&5\\
32&6\\
64&7\\
128&8
\end{array}
$$

## Paridad

Uno de los mecanismos para generar sistemas de codificación con capacidad de detectar errores es el de la paridad, o codificación con bit de paridad.
La idea es agregar a cada código binario de $n$ bits que representan objetos válidos, un nuevo bit calculado en función de los restantes. La forma en que se calcula el bit de redundancia (de paridad) es tal que la cantidad de "unos" en el código completo (original + bit de paridad) sea par, en cuyo caso hablamos de **paridad par**. Si por otra parte, la cantidad es impar, estamos en un caso de **paridad impar**.

Cuando se genera el código (se almacena o se transmite) se calcula este bit mediante uno de los dos criterios expuestos a partir de los $n$ bits originales. Cuando se recupera el código (se lee o se recibe), se recalcula el bit y se chequea con el almacenado o transmitido. En caso de no coincidir estamos ante un error, mientras que si el chequeo cierra, podemos decir que para nuestro sistema no hubo errores.

**Observación:** Notemos que no podemos afirmar en forma absoluta la ausencia de error, aunque el bit de paridad coincida con el bit recuperado. Esto es una regla general para todo sistema de codificación con detección de errores.
En este caso la regla se ve claramente: si cambian a la vez $2$ bits del código, o en general, un número par de ellos, el sistema de detección de errores por paridad no funciona, en el sentido de que no reporta el error.

Los sistemas de codificación con capacidad de detección (o detección y corrección) de errores funcionan bien bajo ciertas hipótesis. Estas son bastante intuitivas, y son las siguientes:

- La probabilidad de que falle un bit es baja.
- Las fallas de bits son sucesos independientes, es decir que la posibilidad de que falle un bit no tiene relación alguna con la falla de algún otro bit.

Con esto en mente, la probabilidad de que falle más de un bit es baja, donde entonces funcionaría bien nuestro sistema con bit de paridad.

---

Recordando la definición de la operación lógica XOR y su propiedad asociativa podemos observar (clarísimo planteando un ejemplo) que el bit de paridad se puede expresar como:

$$
P=b_0\oplus b_1\oplus\ldots\oplus b_n\quad\text{(caso par)}\\
P=\neg(b_0\oplus b_1\oplus\ldots\oplus b_n)\quad\text{(caso impar)}
$$

Con esta definición para el bit de paridad $P$, el chequeo de la corrección de un código recuperado se puede realizar evaluando la expresión:

$$
P\oplus b_0\oplus b_1\oplus\ldots\oplus b_n
$$

Con los valores $P,b_0,b_1,\ldots,b_n$ recuperados. Si el resultado es $0$, entonces no hubo error; en caso contrario, si es $1$ detectamos un error.

## Códigos de Hamming

Los códigos de Hamming son una forma práctica de generar códigos de distancia $3$. Los veremos a través de un ejemplo en concreto; supongamos que tenemos $16$ objetos representados, en principio, en binario:

$$
a_4a_3a_2a_1
$$

De acuerdo a lo que vimos anteriormente en la sección de [bits de redundancia](#bits-de-redundancia), precisamos $3$ de estos bits (pues $2^3=8\geq3+4+1$).

$$
p_3p_2p_1
$$

Armamos el código (inteligentemente) de la siguiente manera, donde abajo de cada dígito se encuentra su posición en binario.

$$
\underbrace{a_4}_{111}
\underbrace{a_3}_{110}
\underbrace{a_2}_{101}
\underbrace{p_3}_{100}
\underbrace{a_1}_{011}
\underbrace{p_2}_{010}
\underbrace{p_1}_{001}
$$

**Nota:** Esta decisión no es casualidad, posicionamos $p_3,p_2,p_1$ justamente en las posiciones $4,2,1$ respectivamente, que son las que representan potencias de $2$ exactas. Esto hará que la decodificación sea trivial, y lo veremos en los siguientes pasos.

Además, también se definen $p_3,p_2$ y $p_1$ de la siguiente forma:

- $p_1=a_4\oplus a_2\oplus a_1$
- $p_2=a_4\oplus a_3\oplus a_1$
- $p_3=a_4\oplus a_3\oplus a_2$

Al recuperar el código, podemos calcular los bits $S_j$ que están vinculados con las posiciones de los bits:

$$
\begin{array}{c|ccccccc}
&a_4&a_3&a_2&p_3&a_1&p_2&p_1\\
\hline
S_0&x&&x&&x&&x\\
S_1&x&x&&&x&x&\\
S_2&x&x&x&x&&&\\
\end{array}
$$

**Nota:** La relación es que $S_j$ incluye a la posición $i$ si y solo si el bit $j$ (contando desde el menos significativo, empezando en cero) de la representación binaria de $i$ es $1$.

Estos bits $S_j$ se juntan para formar un número binario:

$$
S=S_2S_1S_0
$$

Si al calcular $S$ se da que es cero, no hay errores (para nuestro sistema de codificación). Si $S$ da distinto de cero, entonces el número resultante indica la posición de que bit está siendo errado.
De esta manera es posible reconstruir el valor correcto del código, cambiando el bit que identificamos como corrupto.