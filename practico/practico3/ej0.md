# Preguntas teóricas

**Fecha:** 14-09-2026

## Consigna

1. Para una función de $n$ variables, ¿cuántas filas tiene su tabla de verdad?
2. ¿Qué es un conjunto de operadores lógicamente completos?
3. ¿Existe alguna relación entre la representación $\Sigma$ y la suma de productos canónicos?
4. ¿Existe alguna relación entre la representación $\Pi$ y el producto de sumas canónicas?

## Resolución

### Pregunta #1

- Para una función de $n$ variables, ¿cuántas filas tiene su tabla de verdad?

La respuesta es directa, $2^n$. Esto es porque necesitamos saber cuánto vale la función para todos los "números" representados en binario.

### Pregunta #2

- ¿Qué es un conjunto de operadores lógicamente completos?

Podemos definir un conjunto de operadores lógicamente completos como aquel donde se puede representar cualquier función booleana deseada.

### Pregunta #3

- ¿Existe alguna relación entre la representación $\Sigma$ y la suma de productos canónicos?

La relación entre estos elementos es directa. La representación $\Sigma$ nos dice exactamente en que puntos la función vale $1$; nosotros necesitamos esa información para expresar la suma de productos canónicos de la función, ya que los valores donde la función vale $1$ son aquellos que representaremos.

### Pregunta #4

- ¿Existe alguna relación entre la representación $\Pi$ y el producto de sumas canónicas?

La respuesta es análoga a la anterior, pero en este caso $\Pi$ nos devuelve los puntos donde la función vale $0$, que es lo que necesitamos para el producto de sumas canónicas.
