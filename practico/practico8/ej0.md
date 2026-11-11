# Preguntas teóricas

**Fecha:** 11-11-2026

## Consigna

1. Explique por qué es importante salvar el contexto en una rutina recursiva en assembler Intel 8086.
2. Explique detalladamente qué realizan las instrucciones `call` y `ret`.
3. ¿Qué relación hay entre el tamaño de un segmento y el máximo de llamadas recursivas a una función?

## Resolución

### Pregunta #1

- Explique por qué es importante salvar el contexto en una rutina recursiva en assembler Intel 8086.

Salvar el contexto en una rutina es fundamental: imaginemos una rutina que recibe en AX un parámetro.
Supongamos dos cosas durante la ejecución de la rutina:

1. AX en algún momento se modifica
2. La rutina es recursiva: se vuelve a llamar a si misma en algún momento de la ejecución.

Si estas dos cosas suceden y el contexto no se guarda, la modificación de AX en una de las rutinas impactaría directamente en el uso del parámetro de la otra.

### Pregunta #2

- Explique detalladamente qué realizan las instrucciones `call` y `ret`.

Detallamos sobre cada instrucción:

- `call` en primera instancia guarda en el stack la dirección siguiente al call: la dirección de retorno, luego setea IP con el parámetro pasado (suele ser una etiqueta).
- `ret` deshace los cambios hechos por `call`, por lo que hace un pop devolviendo el tope del stack al registro IP

Además existe el concepto de `call far` que consiste en una llamada en la cual se modifica también CS.
En este caso, cambia ligeramente `call`: al principio hace también un push de CS. También cambia el `ret` ligeramente: hace un pop sobre CS al final.

Una observación fundamental para el funcionamiento de estas instrucciones es que el programador haga buen uso del stack: siempre que se llama a `ret` lo que hay en el tope del stack es justamente la dirección de retorno; de lo contrario el resultado de la instrucción será indefinido.

### Pregunta #3

- ¿Qué relación hay entre el tamaño de un segmento y el máximo de llamadas recursivas a una función?

Una rutina recursiva tiene asociado necesariamente un consumo de stack determinado; esto nos indica que tiene que haber un límite de llamadas recursivas a una función, ya que de no existir dicho límite nos "desbordaríamos" del segmento de stack con el que estamos trabajando.

Para entenderlo mejor:

$$
\frac{\text{tamaño stack}}{\text{consumo por llamada}}=\text{cantidad maxima de llamadas}
$$
