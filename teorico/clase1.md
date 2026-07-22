# Sistemas de numeración

**Fecha:** 21-07-2026

## Sistemas con base

En los sistemas con base, un número cualquiera $N$, se representa mediante un polinomio de la forma:

$$
N=a_nb^n+\ldots+a_0b^0+a_{-1}b^{-1}+\ldots
$$

Donde $a_i$ es un símbolo del sistema, al que llamamos dígito, y $b$ es la base.

La base es igual a la cantidad de símbolos del sistema. Notando que los dígitos son la representación en el sistema de los números enteros, menores que la base tenemos que se cumple la condición $b>a_i\geq0$.

La base $b$ la representamos siempre, por convención, en el sistema decimal.
Habitualmente, la representación omite las potencias de la base y coloca un punto (o coma) para separar la parte de potencias positivas de la parte con las potencias negativas, quedando:

$$
N=a_na_{n-1}\ldots a_0,a_{-1}a_{-2}\ldots a_{-p}
$$

Las bases que utilizaremos en general en este curso son las siguientes:

1. **Sistema decimal:** Es el sistema de numeración utilizado en la vida cotidiana, cuya base es $10$, utilizando los conocidos símbolos $\{0,1,2,3,4,5,6,7,8,9\}$
2. **Sistema binario:** Es el sistema de base $2$, el cual utiliza los símbolos $\{0,1\}$ que reciben el nombre de **bit (binary digit)**
3. **Sistema octal:** Es el sistema de base $8$, el cual utiliza los símbolos $\{0,1,2,3,4,5,6,7\}$
4. **Sistema hexadecimal:** Es el sistema de base $16$, el cual utiliza los símbolos $\{0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F\}$

La base del sistema en el que se está representando un número se suele indicar con un subíndice al final del número. En los casos particulares de base $2$, base $8$ y base $16$ también se puede especificar con un subfijo con las letras $b,o$ y $h$ respectivamente. En el caso de base $16$ también se utiliza el prefijo $0\text{x}$. Si no se indica nada, se asume base decimal.
Veamos algunos ejemplos:

- $1101_{(2}=1101b=1\cdot2^3+1\cdot2^2+0\cdot2^1+1\cdot2^0=13$ (decimal)
- $1011.11_{(2}=1011.11b=1\cdot2^3+0\cdot2^2+1\cdot2^1+1\cdot2^0+1\cdot2^{-1}+1\cdot2^{-2}=11.75$ (decimal)
- $\text{A2F}_{(16}=\text{A2Fh}=0\text{xA2F}=10\cdot16^2+2\cdot16^1+15\cdot16^0=2607$ (decimal)

A continuación estudiaremos los algoritmos para que, dada la representación de un número en cierta base, podamos hallar la correspondiente representación en otra base dada.

## Conversión de base de números enteros

Nuestro deseo es, dado un número entero $N$ en una base $B$ representado por:

$$
N=A_nB^n+\ldots+A_0B^0
$$

Hallar su expresión en una base $b$ diferente de $B$.
En definitiva, lo que buscamos es hallar los valores de $a_m,a_{m-1},\ldots,a_0$

### Caso A

Queremos convertir de una base $B$ a una base $b$, usando la aritmética de la base $b$ **(muy útil para pasar de cualquier base, a la base $10$)**.

La conversión se hace a través del polinomio característico, expresando los símbolos $A_n\ldots,A_0$ y la base $B$ en la base $b$, evaluando el polinomio y realizando las operaciones en la base $b$. Todo esto se entiende a la perfección con un ejemplo.

#### Ejemplo

Supongamos que queremos convertir $\text{A2Fh}$ a decimal, entonces:

$$
\text{A2Fh}=10\cdot16^2+2\cdot16^1+15\cdot16^0=2607
$$

### Caso B

Queremos convertir de una base $B$ a una base $b$ usando la aritmética de la base $B$ **(muy útil para pasar de base $10$ a cualquier base)**.

Previamente notemos que cualquier número $N$ se puede expresar como:

$$
\begin{aligned}
&N=b\cdot N_1+a_0\\
&N_1=b\cdot N_2+a_1\\
&\vdots\\
&N_n=b\cdot N_{n+1}+a_n\\
\end{aligned}
$$

Donde estamos repitiendo la división entera entre $b$ hasta llegar a un $N_{n+1}$ que sea menor a $b$ (la próxima división daría como cociente $0$).
Por lo tanto, los valores $a_0,\ldots,a_n$ son los restos de las divisiones de $N$ entre $b$ realizadas en la aritmética de la base $B$. Esto también se entiende bien claro con un ejemplo.

#### Ejemplo

Supongamos que queremos convertir $653$ a binario, entonces:

$$
\begin{aligned}
&653=2\cdot326+1\\
&326=2\cdot163+0\\
&163=2\cdot81+1\\
&81=2\cdot40+1\\
&40=2\cdot20+0\\
&20=2\cdot10+0\\
&10=2\cdot5+0\\
&5=2\cdot2+1\\
&2=2\cdot1+0\\
&1=2\cdot0+1
\end{aligned}
$$

Y entonces simplemente tomamos los restos, para obtener el resultado que buscamos:

$$
653=1011000101\text{b}
$$

## Conversión de base de números con parte fraccionaria

Sea un número con parte fraccionaria:

$$
N=N_e+N_f=a_nb^n+\ldots+a_0b^0+a_{-1}b^{-1}+\ldots
$$

Donde $N_e$ y $N_f$ son la parte entera y la parte fraccionaria de $N$ respectivamente.
La parte fraccionaria sigue siempre a la parte entera en cualquier base; por lo tanto, $N_e$ puede convertirse igual que antes y $N_f$ se convierte por separado.

Estudiaremos entonces como convertir las partes fraccionarias de un número. Sean:

- $N_f=A_{-1}B^{-1}+A_{-2}B^{-2}+\ldots$ en base $B$
- $N_f=a_{-1}b^{-1}+a_{-2}b^{-2}+\ldots$ en base $b$

### Caso A

Queremos convertir desde una base $B$ a una base $b$ usando la aritmética de la base $b$ **(muy útil para pasar de cualquier base a base $10$)**

Para esto, desarrollamos el polinomio característico de $N_f$:

$$
P(x)=A_{-1}x^1+A_{-2}x^2+\ldots A_{-m}x^m
$$

Al calcular el valor numérico de $P(x)$ para $x=B^{-1}$ usando aritmética de $b$, obtendremos el valor buscado. Esto también se entiende bien claro con un ejemplo.

#### Ejemplo

Supongamos que queremos pasar $0.213_{(8}$ a base decimal, entonces tomamos su polinomio característico:

$$
P(x)=2x+x^2+3x^3
$$

Y ahora sustituimos $x$ por $B^{-1}=\frac{1}{8}$ para obtener el resultado que buscamos:

$$
P(x)=3(0.125)^3+(0.125)^2+2(0.125)=0.27148\ldots
$$

Esta última es la expresión decimal para el número $0.213_{(8}$.

### Caso B

Queremos convertir desde una base $B$ a una base $b$ operando con la aritmética de $B$ **(lo que la hace muy útil para pasar de base $10$ a cualquier base)**

Para determinar los coeficientes $a_{-1},a_{-2},\ldots$ para la base $b$, se observa que cada uno de tales coeficientes es un entero.

1. Primero multiplicamos $N_f$ por $b$ (con aritmética de $B$):

   $$
   b\cdot N_f=a_{-1}+a_{-2}b^{-1}+a_{-3}b^{-2}+\ldots
   $$

2. De donde observamos, que la parte entera de $b\cdot N_f$ es $a_{-1}$. Continuamos restando $a_{-1}$ y luego multiplicando por $b$ nuevamente:

   $$
   b(b\cdot N_f-a_{-1})=a_{-2}+a_{-3}b^{-1}+\ldots
   $$

3. Nuevamente observamos que la parte entera de $b(b\cdot N_f-a_1)$ es $a_{-2}$.
4. Podemos repetir esta idea cuantas veces queramos para obtener el número deseado de decimales. Puede suceder que este proceso no termine en ningún punto.

Veamos un ejemplo que es esclarecedor para entender el método.

#### Ejemplo

Supongamos que queremos convertir el decimal $653.61$ a base $2$, entonces:

$$
\begin{aligned}
&2(0.61)=1.22\Rightarrow a_{-1}=1\\
&2(0.22)=0.44\Rightarrow a_{-2}=0\\
&2(0.44)=0.88\Rightarrow a_{-3}=0\\
&2(0.88)=1.76\Rightarrow a_{-4}=1\\
&2(0.76)=1.52\Rightarrow a_{-5}=1\\
\end{aligned}
$$

Por lo tanto, usando la parte entera en binario que ya calculamos para los ejemplos anteriores de la clase, concluimos que:

$$
653.61=1011000101.10011\text{b}
$$

## Caso particular bases $8$ y $16$

La base $8$ y la base $16$ tienen una íntima relación con la base $2$. Puesto que $8=2^3$, entonces cada dígito octal se corresponde con $3$ dígitos binarios.
El procedimiento entonces para convertir un número binario en un número octal es dividir en grupos de $3$ bits a partir del punto binario, y asignando el dígito octal correspondiente a cada grupo.

Supongamos que queremos convertir $11001010011.111110011_{(2}$ a base octal:

$$
\underbrace{110}_{3}
\underbrace{010}_{1}
\underbrace{100}_{2}
\underbrace{011}_{3}
.
\underbrace{111}_{7}
\underbrace{110}_{6}
\underbrace{011}_{3}
=
3123.736_{(8}
$$

---

Por otra parte, la conversión de base $8$ a base $2$ se hace a la inversa, convirtiendo en binario cada dígito octal.

Supongamos que queremos convertir $732_{(8}$ a base binaria, entonces primero convertimos los dígitos octales a sus correspondientes versiones en binario:

$$
7_{(8}=111\text{b}\\
3_{(8}=011\text{b}\\
2_{(8}=010\text{b}\\
$$

Y ahora simplemente las juntamos en orden:

$$
732_{(8}=111011010\text{b}
$$

---

Para el caso hexadecimal, ambas operaciones son análogas, con la única diferencia que agrupamos de a $4$ bits en vez de a $3$ bits.

Aunque es muy fácil de deducir, se puede considerar una tabla de fácil acceso para transformar de una base a otra cuando trabajamos con estas tres que están muy relacionadas.

$$
\begin{array}{c|c|c|c}
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
\end{array}
$$
