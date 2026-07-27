# Preguntas teóricas

**Fecha:** 27-07-2026

## Consigna

1. Explique (y ejemplifique) por qué agregar un único bit de paridad a un sistema de codificación binario permite detectar errores pero no corregirlos.
2. Indique cuáles de las representaciones de enteros con signo tienen diferente cantidad de números positivos que negativos.

## Resolución

### Pregunta #1

- Explique (y ejemplifique) por qué agregar un único bit de paridad a un sistema de codificación binario permite detectar errores pero no corregirlos.

Agregar un bit de paridad implica que tenemos la capacidad de detectar errores. Supongamos que elegimos trabajar con el caso de **paridad par** y queremos transmitir la tira $101$; para hacerlo, seguimos el razonamiento a continuación

1. Calculamos el bit de paridad $P=1\oplus0\oplus1=0$
2. La tira que vamos a transmitir entonces es $0101$, que ya incluye el bit de paridad.
3. Imaginemos que al recibir la transmisión, tenemos el valor $1101$, al calcular la paridad de la tira, tenemos claro que hay un error, pues esta es **impar**.

Con el ejemplo, vimos claramente que se detecta errores con este método
También el ejemplo escalere el porque **no podemos** corregir errores, pues si no sabemos cual fue la tira inicial que se transmitió, este método **no nos dice donde** ocurrió el cambio de bit.

### Pregunta #2

- Indique cuáles de las representaciones de enteros con signo tienen diferente cantidad de números positivos que negativos.

Hay dos representaciones con cantidad distinta de positivos y negativos: complemento a dos y desplazamiento.
Vayamos uno a uno para dar un breve razonamiento sobre el porque de la diferencia (o no) de las cantidades.

- En la representación **valor absoluto y signo**, cada número negativo que existe se obtiene reflejando su positivo correspondiente (cambiando el bit del signo), por lo que hay tantos negativos como positivos.
- En la representación **complemento a uno** el razonamiento es análogo al anterior, solo que cambiamos todos los bits por sus complementos.
- En la representación **complemento a dos** el menor número representado no tiene una versión positiva, veamoslo con el siguiente ejemplo. Sea el número $-8$ con esta representación de $4$ bits, entonces:

    $$
    8=1000b
    $$

    Ahora para pasar a complemento a dos, complementamos los bits y sumamos uno:

    $$  
    1000b
    $$

    Esta es la representación de $-8$, que es igual a la que tendría el $8$. Esta representación "prioriza" la interpretación negativa, por lo que el $8$ entonces se queda sin representación.

- En la representación **desplazamiento** la respuesta es relativa según el valor de $d$, todos los casos son posibles en cuanto a las cantidades de positivos y negativos.