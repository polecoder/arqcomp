# Preguntas teóricas

**Fecha:** 04-11-2026

## Consigna

1. Explique cómo se obtiene la siguiente instrucción a ejecutar al comienzo de un ciclo en la arquitectura 8086.
2. ¿La arquitectura 8086 es RISC o CISC? Justifique su respuesta.
3. ¿Qué es una dirección segmentada? ¿Cómo es transformada por el 8086 para generar una dirección física?
4. ¿Qué cuidado se debe tener si existen segmentos que se solapan en el rango de direcciones que abarcan?
5. Describa detalladamente qué hacen las instrucciones PUSH y POP. ¿Qué registros están implícitamente relacionados a estas operaciones?
6. Indique al menos otras dos instrucciones que también hagan uso de registros implícitos.
7. ¿Qué banderas aritméticas son manejadas por el procesador?

## Resolución

### Pregunta #1

- Explique cómo se obtiene la siguiente instrucción a ejecutar al comienzo de un ciclo en la arquitectura 8086.

La siguiente instrucción a ejecutar al comienzo de un ciclo en esta arquitectura se obtiene utilizando dos registros: CS:[IP]. 

La explicación es sencilla: CS (Code Segment) nos lleva al segmento dedicado al código del programa, mientras que IP (Instruction Pointer) nos indica el desplazamiento dentro del Code Segment para llegar a la primera instrucción.

### Pregunta #2

- ¿La arquitectura 8086 es RISC o CISC? Justifique su respuesta.

La arquitectura 8086 es CISC por los siguientes puntos:
- cuenta con un set de instrucciones extenso y las mismas son de largo variable (1 a 6 bytes)
- algunas instrucciones ejecutan tareas en más de un solo ciclo de reloj
- tiene muy pocos registros de uso general, la mayoría de ellos son nomenclados y tienen un propósito preestablecido.

### Pregunta #3

- ¿Qué es una dirección segmentada? ¿Cómo es transformada por el 8086 para generar una dirección física?

La arquitectura Intel 8086 divide su memoria en "segmentos" con el propósito de aumentar la cantidad de memoria direccionable que puede manejar el sistema (1 MB).
Esto significa que la memoria está dividida en partes, y cada una de estas partes contiene ciertos tipos específicos de datos.

Sea la dirección de memoria 0xF100:0xE000, su dirección física calculada por el CPU es:

```
0xFF000 = 0xF100 * 2^4 + 0xE000
```

Es decir: primero se multiplica el segmento por $2^4$ y luego se le suma el desplazamiento.

### Pregunta #4

- ¿Qué cuidado se debe tener si existen segmentos que se solapan en el rango de direcciones que abarcan?

Si existen segmentos que se solapan, el riesgo es claro: podemos modificar la misma dirección de memoria desde dos segmentos distintos.
Imaginemos que los segmentos DS y CS se solapen, y modificamos un dato en DS. Este dato podría contener una instrucción de CS (pues se solapan) y sería sobreescrita por una instrucción inocente sobre DS.

### Pregunta #5

- Describa detalladamente qué hacen las instrucciones PUSH y POP. ¿Qué registros están implícitamente relacionados a estas operaciones?

Describamos el funcionamiento de estas instrucciones sobre la pila de la arquitectura:

- PUSH: Inserta un elemento especificado por parámetro mediante su dirección (o registro) en el tope de la pila.
- POP: Elimina el elemento del tope de la pila, guardandolo en la dirección (o registro) específicada por parámetro.

Los registros que están implicitamente relacionados a las operaciones son SS (Stack Segment) y SP (Stack Pointer). Éstos registros son los que nos indican la dirección exacta del tope de la pila.

### Pregunta #6

- Indique al menos otras dos instrucciones que también hagan uso de registros implícitos.

Otras dos instrucciones que hacen uso de registros implícitos podrían ser MUL o DIV (utilizan AX), además todas las instrucciones de "saltos" como JMP utilizan el registro IP de forma implícita.

### Pregunta #7

- ¿Qué banderas aritméticas son manejadas por el procesador?

Las banderas aritméticas manejadas por el procesador son: CF (Carry), AF (Auxiliary carry), ZF (Zero), OF (Overflow), SF (Sign), PF (Parity)