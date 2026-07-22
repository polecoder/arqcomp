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
Esto sucede porque la única condición para que el algorítmo termine, es que el nuevo número hallado sea cero; esta situación no necesariamente sucede, ya que siempre múltiplicamos un número un número por dos y nos quedamos con su parte fraccionaria, pero nada garantiza que en algún momento lleguemos a cero.

Considerando un número $N$ cualquiera en base $B$, podemos expresar su parte fraccionaria $N_f$ con $k$ dígitos como la fracción exacta $N/B^k$.
La conversión a la base $b$ solo termina si al simplificar la fracción mencionada, todos los factores primos del denominador dividen también a $b$. En particular, si $B$ y $b$ tienen los mismos factores primos (por ejemplo ambas potencias de $2$), toda fracción exacta en una, se convierte en una cantidad finita de dígitos en la otra.

**Nota:** La respuesta está dada con IA y realmente no termino de entender el concepto. Lo revisaré más adelante
