# Preguntas teóricas

**Fecha:** 24-09-2026

## Consigna

1. Explique cómo se pueden utilizar los operadores `&` y `>>` para extraer un conjunto de bits dentro de una tira.
2. Explique cómo se pueden utilizar las operaciones `/` (división entera) y `%` (módulo) para extraer un conjunto de bits dentro de una tira.
3. Explique qué puede cambiar en un programa al definir una variable como `unsigned char` en lugar de `char`.

## Resolución

### Pregunta 1

- Explique cómo se pueden utilizar los operadores `&` y `>>` para extraer un conjunto de bits dentro de una tira.

Supongamos que tenemos una tira de 16 bits de los cuales queremos extraer un conjunto de 8 bits desde la posición 3 hasta la posición 10 de la tira original.
Entonces, primero hacemos un shift hacia la derecha para mover el primer bit que nos interesa a la posición 0.
Luego hacemos un `&` bit a bit con la tira `0xFF`, esto nos devolverá exactamente los 8 bits de la tira original.

```c
unsigned char extraer_bits(short tira) {
    return (tira >> 3) & 0xFF;
}
```

En resumen, el operador `>>` mueve el bit que nos interesa a la posición 0, mientras que el operador `& 0xFF` nos ayuda a extraer la cantidad de bits que necesitamos.

### Pregunta 3

- Explique qué puede cambiar en un programa al definir una variable como `unsigned char` en lugar de `char`.

La diferencia principal es como se interpretan los bits de la varible:

1. `unsigned char` representa el rango $[0,255]$, es decir un número binario "puro"
2. `char` sin embargo, representa el rango $[-128,127]$, permite la interpretación negativa.

Al operar con las tiras no debería haber inconvenientes para las operaciones que utilizamos (suma, resta, AND, OR, XOR), pero al interpretar el valor de la tira de bits seguramente no representen al mismo valor.
