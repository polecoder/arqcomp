# Prioridades de interrupciones

**Fecha:** 18-11-2026

## Consigna

Un procesador posee tres dispositivos conectados a una misma línea de interrupción. El procesador atiende por nivel y los dispositivos interrumpen también por nivel. Existen tres direcciones de E/S: ESTADO_1, ESTADO_2 y ESTADO_3. Cada una de estas entradas está asociada a un dispositivo y el hardware coloca el valor $1$ en su bit menos significativo cuando el dispositivo correspondiente solicita una interrupción. Para atender al dispositivo $n$ se debe invocar la función `atencionDispositivoN()`.

Escribir una rutina de interrupción en alto nivel capaz de atender a los tres dispositivos, en las siguientes situaciones:

1. Se considera un esquema de prioridades fijas $\text{pri}(\text{disp }1)>\text{pri}(\text{disp }2)>\text{pri}(\text{disp }3)$.
2. Se considera un esquema de prioridades variables. Existe la función `pri(dispositivo)` que devuelve el valor de la prioridad del dispositivo.

## Resolución

### Parte 1

- Se considera un esquema de prioridades fijas $\text{pri}(\text{disp }1)>\text{pri}(\text{disp }2)>\text{pri}(\text{disp }3)$.

Adjuntamos el código a continuación:

```c
void atender_interrupciones() {
  if (in(ESTADO_1) & 0x01) atencionDispositivo1();
  else if (in(ESTADO_2) & 0x01) atencionDispositivo2();
  else if (in(ESTADO_3) & 0x01) atencionDispositivo3();
}
```

Como vemos, es extremadamente sencillo. Conviene explicar porque podemos hacer que sea tan breve:

1. Por la naturaleza el `if-else`, se atiende una sola interrupción a la vez y se sale del bucle.
2. Mientras haya controladores solicitando interrupciones, se llamará a esta función hasta que ninguno más precise.
3. Por los puntos anteriores, si se reactiva por ejemplo mientras se atiende al dispositivo 3, y el 1 solicita una interrupción, se atenderá el 1.

### Parte 3

- Se considera un esquema de prioridades variables. Existe la función `pri(dispositivo)` que devuelve el valor de la prioridad del dispositivo.

Se asume que `pri(dispositivo)` toma un número del 1 al 3 y devuelve un número correspondiente a la prioridad del dispositivo. Cuánto más grande es este número, mayor es la prioridad del dispositivo.

```c
void atender_interrupciones() {
  char elegido = 0;
  // 1. Elijo el controlador de mayor prioridad
  if (in(ESTADO_1) & 0x1 && (elegido == 0 || pri(1) > pri(elegido)))
    elegido = 1;
  else if (in(ESTADO_2) & 0x1 && (elegido == 0 || pri(2) > pri(elegido)))
    elegido = 2;
  else if (in(ESTADO_3) & 0x1 && (elegido == 0 || pri(3) > pri(elegido)))
    elegido = 3;
  
  // 2. Atiendo al controlador de mayor prioridad elegido
  if (elegido == 1) atencionDispositivo1();
  else if (elegido == 2) atencionDispositivo2();
  else if (elegido == 3) atencionDispositivo3();
}
```

---

Esto concluye el ejercicio.