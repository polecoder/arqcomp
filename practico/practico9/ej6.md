# Control de iluminación

**Fecha:** 20-11-2026

## Consigna

Sea un sistema que controla las luces de iluminación de los pasillos de un edificio. El sistema cuenta con un botón de encendido en cada piso y se desea que, una vez transcurridos 45 segundos desde que fue presionado el botón de algún piso, estas se apaguen. 

Se sabe que:

- Cuando se aprieta un botón en cualquier piso, se invoca una rutina de interrupción llamada `boton()`.
- El encendido de todas las luces se realiza colocando el valor $1$ en el bit menos significativo del byte de la dirección `0x20` y las luces se apagan colocando el valor $0$ en el mismo bit.
- Se cuenta con un timer que genera una interrupción cada un segundo, la cual invoca a la rutina de interrupción `tiempo()`.

Escribir en lenguaje de alto nivel las rutinas necesarias para implementar el controlador de luces.

## Resolución

Para la resolución de este ejercicio vamos a saltar directamente al código, ya que el sistema descrito es bastante fácil de diseñar sin necesidad de detallar estados, eventos y acciones intermedias.

Quizás lo único que conviene aclarar es que guardaremos un valor importante entre las interrupciones: *el tiempo actual*

```c
// inicializar variables globales
#define TIEMPO_APAGADO 45
#define LUZ_APAGADA 0
#define LUZ_PRENDIDA 1
#define DIRECCION_PUERTO 0x20
static int segundos;
static char estado;

void inicializar() {
  // inicializo con la luz apagada
  out(DIRECCION_PUERTO, in(DIRECCION_PUERTO) & ~0x01);
  estado = LUZ_APAGADA;
  segundos = 0;
  enable();
  // si la máquina es dedicada: while (true);
}
```

```c
void interrupt tiempo() {
  if (estado == LUZ_APAGADA) return;
  segundos++;
  if (segundos >= TIEMPO_APAGADO) {
    out(DIRECCION_PUERTO, in(DIRECCION_PUERTO) & ~0x01);
    estado = LUZ_APAGADA;
  }
}
```

```c
void interrupt boton() {
  segundos = 0;
  if (estado == LUZ_APAGADA) {
    out(DIRECCION_PUERTO, in(DIRECCION_PUERTO) | 0x01);
    estado = LUZ_PRENDIDA;
  }
}
```

Las rutinas son adaptables al funcionamiento con una máquina dedicada o sin ella simplemente cambiando la función de `inicializar()` como indica el comentario.
Se asume que la dirección 0x20 es de lectura y escritura, y que las rutinas de interrupción no se interrumpen entre sí.