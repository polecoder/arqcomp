# Representación interna de datos

**Fecha:** 26-07-2026

## Representación punto fijo para tipos fraccionarios

Para representar números con parte fraccional, se puede utilizar un número determinado de bits (por ejemplo: $16$, $32$ o $64$); asignando un cierto número fijo de bits para representar la parte entera del número y el resto para la parte fraccional. La parte entera siempre ocupa los bits más significativos del código.

La representación se obtiene representando en complemento a dos el número resultante de multiplicar $N$ por $2^f$, siendo $f$ el número de bits dedicados a la parte fraccional.

### Ejemplo

Supogamos que queremos convertir el número $15.25_{10}$ a punto fijo de $10$ bits, utilizando $5$ bits para la parte fraccional.
Primero tendríamos que multiplicar por $2^5$, pero recordemos que esta operación es mucho más sencillo cuando lo realizamos directamente en base binaria, pues se transforma en la operación coloquialmente conocida como "correr la coma". Entonces, primero convertimos $15.25_{10}$ a base binaria:

$$
N_e=15.25_{10}=1111b
$$

Por otra parte, calculamos la parte fraccionaria:

$$
\begin{aligned}
&2(0.25)=0.50\Rightarrow a_{-1}=0\\
&2(0.50)=1.00\Rightarrow a_{-2}=1\\
&\text{Concluimos que }N_f=0.25_{10}=0.01b
\end{aligned}
$$

Y entonces, concluimos que $15.25_{10}=1111.01b$. Ahora si podemos ir concretamente a la representación punto fijo, multiplicando lo obtenido por $2^5$ y pasando a representación complemento a dos.

$$
1111.01b\times(2^5)_{10}=111101000
$$

Ahora, rellenaríamos con ceros a la izquierda para llegar a los bits que elegimos para la representación ($10$ en nuestro caso):

$$
0111101000
$$

Y como el número es positivo, se mantiene con la representación complemento de dos. En caso de que el número fuera negativo $-15.25$, bastaría con pasar este resultado a complemento de dos para finalizar.

### Operaciones en punto fijo

Para sumar y restar con esta representación, se opera normalmente con las representaciones y el resultado es consistente.
Por otra parte, para la multiplicación, se requiere corregir el resultado dividiendo entre $2^f$; mientras que para la división se debe multiplicar el resultado por $2^f$.

## Representación punto flotante

En las distintas arquitecturas de computadoras, se han utilizado diversas representaciones para expresar números reales, todas ellas basadas en la siguiente notación:

$$
N=(-1)^s\cdot b^e\cdot M
$$

En donde:

- $s$ es el signo
- $e$ es el exponente
- $M$ es la mantisa
- $b$ es la base de representación

Las bases utilizadas han sido normalmente $10$ y $2$. Dado que esta representación es ambigua (existen varias representaciones para un mismo número) se utiliza una versión restringida que se llama **normalizada**. Los números normalizados son aquellos en que el bit más significativo de **la mantisa es distinta de cero**, o lo que es equivalente, son **aquellos en que la mantisa sea máxima**.

### Estándar IEEE 754 de punto flotante

Para que los números representados en punto flotante puedan ser intercambiables entre distintas arquitecturas se establece el estándar IEEE 754 que define el formato y las operaciones con estos. El estándar IEEE 754 usa la base $2\ (b=2)$ y su mantisa normalizada es de la forma $1.F$. La representación utiliza el signo, el exponente (codificado usando representación desplazamiento) y la parte fraccional de la mantisa (lo que está después de la "coma binaria").
En definitiva el número se representa por una terna de códigos binarios:

- $S$ (codificación del signo)
- $E$ (codificación del exponente en desplazamiento)
- $F$ (codificación de la parte fraccional de la mantisa en binario)

El estándar define tres formatos (en función de la cantidad de bits utilizados):

$$
\begin{array}{c|cccc}
&\text{S (bits)}&\text{E (bits)}&\text{F (bits)}&\text{Total (bytes)}\\
\hline
\text{Precisión simple}&1&8&23&4\\
\text{Precisión doble}&1&11&52&8\\
\text{Precisión extendida}&1&15&64&10\\
\text{Precisión media}&1&5&10&2\\
\end{array}
$$

**Nota:** La precisión extendida se utiliza para resultados intermedios de operaciones, pero no para almacenamiento permanente.
La precisión media por otra parte, no se utiliza en la práctica; pero si usaremos en el curso.

Los números normalizados son de la forma: $(-1)^s\cdot 2^e\cdot(1.F)$, donde el bit más significativo de la mantisa es un $1$.
Como todos los números normalizados tienen en uno el bit más significativo, el estándar define una representación que omite este bit (para optimizar). Esta consiste en: un $1$ implícito, una coma implícita y luego la parte "fraccional" de la mantisa
De esta forma, la representación queda como:

- $N'=(-1)^s\cdot2^{e+127}\cdot(1.F)$ para precisión simple
- $N'=(-1)^s\cdot2^{e+1023}\cdot(1.F)$ para precisión doble

**Observación:** Los números sumados a los exponentes vienen justamente de la codificación desplazamiento que se eligió para la representación.

---

El conjunto de valores posibles puede ser dividido en las siguientes categorías:

- Cero
- Números normalizados
- Números desnormalizados
- Infinitos
- NaN (no es un número, por ejemplo la raíz cuadrada de un número negativo)

Las clases se distinguen principalmente por el valor de $E$ (exponente), siendo modificada ésta por el campo $F$ (fracción). Cada clase se representa con los siguientes rangos de valores:

$$
\begin{array}{c|cc}
&\text{E (exponente)}&\text{F (fracción)}\\
\hline
\text{Normalizados}&00\ldots0<\text{Exp}<11\ldots1&\text{Cualquier combinación}\\
\text{Desnormalizados}&00\ldots0&\neq0\\
\text{Cero}&00\ldots0&0\\
\text{Infinito}&11\ldots1&0\\
\text{Not a number}&11\ldots1&\neq0\\
\end{array}
$$

Los números desnormalizados sirven para operar con números menores que el menor número normalizado representable. Estos números asumen un $0$ implícito en vez del $1$ implícito de los números normalizados.
Por lo tanto, cuando tenemos un número en notación punto flotante desnormalizado estamos representando el número (por ejemplo para precisión simple):

$$
(-1)^s\cdot2^{-126}\cdot(0.F)
$$

Donde el exponente coincide con el exponente del número más negativo normalizado.

### Operaciones en punto flotante

Las operaciones (suma, resta, multiplicación) se realizan mediante algoritmos especializados para la representación.

Para la suma se debe:

- Alinear los exponentes (llevando el menor al mayor), adecuando concordadamente la mantisa (teniendo en cuenta el $1$ omitido)
- Sumar las mantisas teniendo en cuenta los signos
- Normalizar, acomodando la mantisa y el exponente si corresponde

Para la multiplicación se debe:

- Determinar el signo en base a los signos de los operandos
- Sumar los exponentes
- Multiplicar las mantisas
- Normalizar, acomodando la mantisa y el exponente si corresponde

### Normalización en punto flotante

Como vimos, los números normalizados son de la forma $1.F$, donde el bit más significativo de la mantisa es un $1$.
Todos los números deben ser representados en su forma normalizada (siempre que se pueda) y los resultados finales se deben normalizar.

El método de normalización utilizado es mover la coma a la parte más significativa de la cifra, es decir, variando el peso aritmético de los dígitos que lo componen.

#### Ejemplo

Supongamos que queremos representar el número decimal $-118.625$ usando el sistema de la IEEE 754.

1. Dado que es un número negativo, el signo $S$ es $1$.
2. Ahora tenemos que escribir el número sin signo usando su expresión en base $2$. El resultado es $1110110.101\text{b}$.
3. Luego, movemos la coma "binaria" a la izquierda, dejando sólo un dígito a su izquierda.

    $$
    1110110.101=1.1101110101\times2^6
    $$
    
    Esto es un número en punto flotante normalizado.
4. La parte fraccional de la mantisa, es la parte a la derecha de la coma binaria, rellenada con ceros a la derecha hasta obtener los $23$ bits. Es decir:

    $$
    F=11011101010000000000000
    $$

5. El exponente es $6$, que debe ser representado en desplazamiento. 
Para el formato IEEE 754 de $32$ bits, el desplazamiento $d=127$, entonces sería $6+127=133$, que en binario es:

    $$
    E=10000101
    $$

6. Con esto ya tenemos el resultado pronto:

    $$
    \begin{array}{c|c|c}
    S&E&F\\
    \hline
    1&10000101&11011101010000000000000
    \end{array}
    $$