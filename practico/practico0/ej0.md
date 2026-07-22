# Preguntas teóricas

**Fecha:** 21-07-2026

## Consigna

1. Se desea convertir un número en base 2 a base 10. Justifique qué procedimiento utilizaría:
   1. Conversión de una base $B$ a una base $b$ usando la aritmética de la base $b$
   2. Conversión de una base $B$ a una base $b$ usando la aritmética de la base $B$

2. ¿Cuál o cuáles de los procedimientos de conversión puede no terminar? Indique qué criterio(s) puede(n) usarse para determinar su finalización.

3. Sea $N$ un número en base 10 que al pasarse a base 2 requiere de una cantidad infinita de dígitos. ¿Ocurre lo mismo al pasarlo a base 8? Justifique.

## Resolución

### Pregunta #1

- Se desea convertir un número en base 2 a base 10. Justifique qué procedimiento utilizaría:
  1.  Conversión de una base $B$ a una base $b$ usando la aritmética de la base $b$
  2.  Conversión de una base $B$ a una base $b$ usando la aritmética de la base $B$

La respuesta sería la opción 1: _Conversión de una base $B$ a una base $b$ usando la aritmética de la base $b$_.
Expandiríamos el número en base dos en su forma polinómica:

$$
N=a_n\cdot2^n+\ldots+a_1\cdot2+a_0
$$

Donde $N$ sería la expresión en base decimal.

### Pregunta #2

- ¿Cuál o cuáles de los procedimientos de conversión puede no terminar? Indique qué criterio(s) puede(n) usarse para determinar su finalización.

El único procedimiento de conversión que vimos, que puede no terminar es la _Conversión de una base $B$ a una base $b$ usando la aritmética de la base $B$_ para la parte fraccionaria de un número $N$.
Esto sucede porque la única condición para que el algorítmo termine, es que el nuevo número hallado sea cero; esta situación no necesariamente sucede, ya que siempre múltiplicamos un número por la base $b$ y nos quedamos con su parte fraccionaria, pero nada garantiza que en algún momento lleguemos a cero.

Considerando un número con parte fraccionaria cualquiera en base $B$, podemos expresarlo con $k$ dígitos como la fracción exacta $N/B^k$ para algún $N$ entero.
El criterio de finalización para este método es directo, necesitamos poder reescribir la fracción que mencionamos anteriormente como $M/b^m$ para algún entero $m$; entonces necesitamos que la siguiente igualdad se cumpla:

$$
\frac{N}{B^k}=\frac{M}{b^m}\quad(*_1)
$$

**Nota:** La fracción $M/b^m$ simplemente quiere decir que podemos expresar exactamente el número $N$ para la base $b$.

Ahora, reducimos la fracción $N/B^k$ lo máximo posible, de modo que obtenemos una fracción resultante $p/q$ con $\gcd(p,q)=1$. Veamos el siguiente razonamiento partiendo de $*_1$.

$$
\begin{aligned}
&\frac{N}{B^k}=\frac{M}{b^m}\\
&\implies\scriptstyle{(\text{reducimos la fracción lo máximo posible})}\\
&\frac{p}{q}=\frac{M}{b^m}\\
&\implies\scriptstyle{(\text{operatoria})}\\
&pb^m=Mq\quad(*_2)\\
\end{aligned}
$$

A partir de esta igualdad, seguiremos un razonamiento usando factores primos para ver que condición nos impone esta ecuación sobre $q$.

Sea $r$ un factor primo cualquiera de $q$, como trivialmente $q\mid Mq$ y $r\mid q$, podemos concluir que $r\mid Mq$.
Por la igualdad $*_2$, tenemos que concluir también que $r\mid pb^m$.

Entonces o bien $r\mid p$ o bien $r\mid b^m$; pero sabemos que $p$ y $q$ son coprimos, no pueden compartir factores primos: esto nos permite concluir que $r\mid b^m$; además por propiedades de divisibilidad como $r$ es primo se tiene que $r\mid b$.

---

Esto nos deja con una conclusión bien clara, para que la igualdad $*_1$ se cumpla (nuestra condición de finalización), entonces **todos los factores primos del denominador de $N/B^k$ simplificado, $q$, tienen que dividir a $b$**.
Refinando aún más, esto significa que **$b$ debe contener todos los factores primos de $q$ en su descomposición**.

### Pregunta #3

- Sea $N$ un número en base 10 que al pasarse a base 2 requiere de una cantidad infinita de dígitos. ¿Ocurre lo mismo al pasarlo a base 8? Justifique.

Intentemos explicar esto utilizando la condición que hallamos en la parte anterior del ejercicio.
Recordemos la expresión fraccionaria del número es $N/10^k$ para los $k$ dígitos que tiene.

Si $N/10^k$ es un número en base $10$, que al pasarse a base $2$, requiere una cantidad infinita de dígitos, entonces **la base $2$ no contiene todos los factores primos de su denominador simplificado $q$**.

La descomposición de $2=2^1$, mientras que la de $8=2^3$. Entonces, si $2$ no contiene todos los factores primos de $q$, tampoco lo hará $2^3=8$, ya que los factores primos de estos números son los mismos.

Con este argumento, podemos concluir que efectivamente, ocurre lo mismo al pasarlo a base $8$.