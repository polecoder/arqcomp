# Construir un cronómetro: calidad examen

**Fecha:** 16-10-2026

## Consigna

Se desea construir un circuito cronómetro que cuenta minutos y segundos. El mismo debe mostrar en todo momento el tiempo en un display de cuatro dígitos de 7 segmentos (dos para los minutos, dos para los segundos), es decir de (**00:00 a 59:59**).
El cronómetro tiene un botón de reset pone el sistema en **00:00**. En todo momento que el boton reset no este presionado, el cronometro debe actualizarse una vez por segundo.
Una imagen del cronómetro se puede ver en la figura 1 y un detalle de las entradas de cada módulo de display de 7 segmentos en la figura 2:

![Figura 1](./img/ej7fig1.png)

Para construir el sistema se cuenta con una señal de reloj de 1Hz.

1. Implementar un contador del 0-9 y un contador de 0-6 con el reset que considere correspondiente, utilizando flip-flops tipo D.
2. Resolver el circuito planteado a partir de los contadores diseñados en la parte a, módulos display 7 segmentos (1 dígito cada módulo) y compuertas básicas.
3. Modifique el circuito anterior agregando la siguiente funcionalidad: el circuito ahora cuenta con dos botones:
   - El botón reset previamente descripto
   - Un botón incrementar que incrementa el tiempo a razón de 1 minuto por cada segundo que el botón esté presionado. Mientras que se está pulsando este botón los segundos no deben incrementarse
     Mientras el botón incrementar no esté presionado, el circuito debe funcionar normalmente.

## Resolución

Este ejercicio es una consolidación de todos los conceptos que aprendimos a lo largo de este práctico.

### Parte 1

- Implementar un contador del 0-9 y un contador de 0-6 con el reset que considere correspondiente, utilizando flip-flops tipo D.

Para esta parte usaremos la estrategia vista en el teórico para construir contadores con flip-flops tipo D. Construiremos primero el contador de 0-9 que es el más complejo (requiere 1 bit más), el contador de 0-6 se construye análogamente de forma más simple.

#### Contador 0-9

El primer paso es crear una tabla con los "nuevos" valores a partir de los "actuales".

$$
\begin{array}{ccc}
&\text{Actuales}&\text{Nuevos}\\
&\text{Q3 Q2 Q1 Q0}&\text{D3 D2 D1 D0}\\
0&0\quad0\quad0\quad0&0\quad0\quad0\quad1\\
1&0\quad0\quad0\quad1&0\quad0\quad1\quad0\\
2&0\quad0\quad1\quad0&0\quad0\quad1\quad1\\
3&0\quad0\quad1\quad1&0\quad1\quad0\quad0\\
4&0\quad1\quad0\quad0&0\quad1\quad0\quad1\\
5&0\quad1\quad0\quad1&0\quad1\quad1\quad0\\
6&0\quad1\quad1\quad0&0\quad1\quad1\quad1\\
7&0\quad1\quad1\quad1&1\quad0\quad0\quad0\\
8&1\quad0\quad0\quad0&1\quad0\quad0\quad1\\
9&1\quad0\quad0\quad1&0\quad0\quad0\quad0\\
\end{array}
$$

A continuación hacemos el mapa de Karnaugh para cada uno de los bits nuevos, utilizando comodines para los valores que no nos interesan (10-15):

**D3**:

![Figura 2](./img/ej7fig2.png)

Y por lo tanto $D3=Q3Q0'+Q2Q1Q0$

**D2**:

![Figura 3](./img/ej7fig3.png)

Y por lo tanto $D2=Q2Q1'+Q2'Q1Q0+Q2Q0'$

**D1**:

![Figura 4](./img/ej7fig4.png)

Y por lo tanto $D1=Q3'Q1'Q0+Q1Q0'$

**D0**:

![Figura 5](./img/ej7fig5.png)

Y por lo tanto $D0=Q0'$

Resumimos todo en las siguientes expresiones:

- $D3=Q3Q0'+Q2Q1Q0$
- $D2=Q2Q1'+Q2'Q1Q0+Q2Q0'$
- $D1=Q3'Q1'Q0+Q1Q0'$
- $D0=Q0'$

Esto nos permite construir el circuito que es el siguiente:

![Figura 6](./img/ej7fig6.png)

#### Contador 0-6 (0-5 en realidad)

Como anticipa el título, mientras la letra solicita un contador 0-6, esto no es necesario para cumplir con los requerimientos. Por lo tanto se implementará un contador 0-5; pues el cronómetro va desde **00:00** hasta **59:59**.

En este caso, el primer paso ya está hecho pues usamos la misma tabla hasta el 5:

$$
\begin{array}{ccc}
&\text{Actuales}&\text{Nuevos}\\
&\text{Q2 Q1 Q0}&\text{D2 D1 D0}\\
0&0\quad0\quad0&0\quad0\quad1\\
1&0\quad0\quad1&0\quad1\quad0\\
2&0\quad1\quad0&0\quad1\quad1\\
3&0\quad1\quad1&1\quad0\quad0\\
4&1\quad0\quad0&1\quad0\quad1\\
5&1\quad0\quad1&0\quad0\quad0\\
\end{array}
$$

A continuación hacemos el mapa de Karnaugh para cada uno de los bits nuevos, utilizando comodines para los valores que no nos interesan (6, 7):

**D2**:

![Figura 7](./img/ej7fig7.png)

Y por lo tanto $D2=Q2Q0'+Q1Q0$

**D1**:

![Figura 8](./img/ej7fig8.png)

Y por lo tanto $D1=Q2'Q1'Q0+Q1Q0'$

**D0**:

![Figura 9](./img/ej7fig9.png)

Y por lo tanto $D0=Q0'$

Resumimos todo en las siguientes expresiones:

- $D2=Q2Q0'+Q1Q0$
- $D1=Q2'Q1'Q0+Q1Q0'$
- $D0=Q0'$

Esto nos permite construir el circuito que es el siguiente:

![Figura 10](./img/ej7fig10.png)

---

**Aclaración:** Si bien uno intenta no equivocarse en ninguna de estas tareas... Es bastante sencillo hacerlo; un 1 en un lugar incorrecto resulta en un circuito totalmente distinto que no cumple la función que queremos que cumpla. Tener esto en cuenta para no tomar el circuito como válido automáticamente

### Parte 2

- Resolver el circuito planteado a partir de los contadores diseñados en la parte a, módulos display 7 segmentos (1 dígito cada módulo) y compuertas básicas.

Para esta parte el desafío es entender cuando se activa qué contador. Empecemos por lo más sencillo: claramente necesitamos 2 contadores 0-5 (para las decenas) y 2 contadores 0-9 (para las unidades).
Estos contadores se separan en dos grupos: 1 contador 0-5 + 1 contador 0-9 conforman el contador de segundos; mientras que los restantes conforman el contador de minutos.

El siguiente paso es entender cuando "activar" (con la señal CLK) el contador de las decenas. Esto lo querremos hacer cada vez que el contador de las unidades llegue a 9, por lo que nuestro circuito debe reflejar este comportamiento con compuertas básicas.

El último paso es entender cuando "activar" el contador de minutos. De forma similar al caso anterior queremos hacerlo cuando el contador de segundos llega a 59.

A continuación se muestra el circuito diseñado:

![Figura 11](./img/ej7fig11.png)

**Nota:** La construcción se basa principalmente en las observaciones "teóricas" que hicimos antes. El único detalle que puede confundir es: por qué usamos las compuertas NAND en vez de las AND?
La respuesta es sencilla, y es que CLK conecta con los flip-flops, y estos no se fijan cuando la entrada CLK **es** 1, sino en cuando la entrada CLK **pasa** de 0 a 1 (flanco ascendente).
En este caso (miremos el primer NAND), cuando el contador de 0-9 llegue a 9, el NAND valdrá 0, para pasar a 1 cuando el contador vuelva a 0: esto genera el flanco ascendente activando el contador 0-5.
