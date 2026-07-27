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
