# Concentrador de comunicaciones

**Fecha:** 19-11-2026

## Consigna

Se desea implementar un concentrador de comunicaciones para recibir datos a través de cuatro líneas de entrada y enviarlos por una única línea de salida. Cada línea de entrada tiene un controlador asociado que se maneja por medio de dos registros de solo lectura, uno de control (EST_CONT_n) y otro de datos (DATO_n), de un byte cada uno, ubicados en las siguientes direcciones:

| Controlador | Puerto de control | Dirección | Puerto de datos | Dirección |
|-------------|-------------------|-----------|-----------------|-----------|
| $0$ | EST_CONT_0 | 0x10 | DATO_0 | 0x11 |
| $1$ | EST_CONT_1 | 0x12 | DATO_1 | 0x13 |
| $2$ | EST_CONT_2 | 0x14 | DATO_2 | 0x15 |
| $3$ | EST_CONT_3 | 0x16 | DATO_3 | 0x17 |

El bit 0 de EST_CONT_n en $1$ indica que hay un dato disponible en DATO_n. El bit se apaga cuando es leído el registro DATO_n. Los bits 1 al 7 no se usan. El registro DATO_n contiene el dato recibido por la línea $n$. Cuando algún controlador de entrada tiene un dato disponible, genera una interrupción que activa la rutina `intEntrada()`.

La línea de salida tiene su propio controlador con un registro de datos DATO_S, de un byte, ubicado en la dirección de E/S 0x20. Toda vez que se escribe un byte en DATO_S, el controlador transmite el byte por la línea de salida y, al terminar la transmisión, genera una interrupción que activa la rutina `intSalida()`.

Se debe cumplir que, al ingresar un dato (1 byte) por la línea $n$ ($0..3$), se transmite a la salida el número $n$ (1 byte) y luego el dato ingresado (1 byte).

Escribir en alto nivel las rutinas necesarias para implementar el concentrador.

*Observaciones:*
- El procesador está dedicado exclusivamente a la ejecución de las rutinas que implementan el concentrador.
- El concentrador deberá aceptar los bytes de entrada y guardarlos en un buffer en caso de que el canal de salida esté siendo utilizado, hasta un máximo de MAX_BUFFER bytes. En caso de que el buffer esté lleno se descartarán los caracteres recibidos.

## Resolución

Resolveremos este ejercicio paso a paso con un procedimiento estructurado antes de empezar a programar, la idea es que esta solución sea una guía para ejercicios generales de interrupciones.

1. **Identificar eventos:** tenemos dos eventos en el sistema que pueden disparar interrupciones, correspondientes a las rutinas:
    1. `intEntrada():` algún controlador de entrada tiene un dato disponible.
    2. `intSalida():` se trasmitió un byte mediante la línea de salida.
  
2. **¿Qué tiene que recordar el sistema tras las interrupciones?**
    1. Buffer de salida.
    2. Booleano para saber si se está usando actualmente la línea de salida

3. **Tabla de casos (opcional pero útil):** combinamos cada evento con el estado de la salida (usando_salida) y el estado de la cola, y decidimos qué hacer en cada caso.

| Evento | Salida | Cola | Acción | Estado siguiente |
|---|---|---|---|---|
| intEntrada | libre | vacía | leer dato, encolar n y dato, desencolar y escribir en DATO_S | salida ocupada |
| intEntrada | ocupada | con lugar (≥ 2 bytes) | leer dato, encolar n y dato | salida ocupada |
| intEntrada | ocupada | llena (< 2 bytes) | leer dato (para apagar el bit) y descartarlo | sin cambios |
| intSalida | ocupada | con datos | desencolar y escribir en DATO_S | salida ocupada |
| intSalida | ocupada | vacía | nada que enviar | salida libre |

Esto es lo mínimo que necesitamos para programar, aunque podríamos agregar un mapa de estados y que necesitamos hacer ante cada uno de ellos.

```c
// Contexto global
static bool usando_salida = false;
static Cola cola = Cola.crear(MAX_BUFFER); // crea una cola de MAX_BUFFER bytes
```

```c
void intEntrada() {
  for (char i = 0; i <= 3; i++) {
    char direccion = 0x10 + 2*i;
    if (in(direccion) & 0x01) {
      char dato = in(direccion+1);
      // si tengo más de dos bytes disponibles en la cola
      if (MAX_BUFFER - cola.cantidad() >= 2) {
        cola.encolar(i);
        cola.encolar(dato);
      }
    }
  }

  if (!usando_salida && !cola.vacia()) {
    char dato = cola.desencolar();
    out(dato, DATO_S);
    usando_salida = true;
  }
}
```

```c
void intSalida() {
  if (!cola.vacia()) {
    char dato = cola.desencolar();
    usando_salida = true;
    out(DATO_S, dato);
  } else { // si la cola está vacía
    usando_salida = false;
  }
}
```