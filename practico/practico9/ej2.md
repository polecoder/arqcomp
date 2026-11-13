# E/S para una bomba de agua

**Fecha:** 13-11-2026

## Consigna

Se desea controlar una bomba de agua mediante un microprocesador. La bomba se enciende colocando el valor 1 en el bit 2 del puerto de E/S BOMBA, y se apaga colocando el valor 0 en el mismo bit. Dado que el encendido no es inmediato, existe un sensor externo que coloca el valor 1 en el bit 0 del puerto BOMBA cuando se activa efectivamente la bomba y el valor 0 cuando la bomba está desactivada. Cuando la bomba se activa se debe activar un LED conectado al bit 4 del puerto BOMBA; en otro caso se debe desactivar el LED.

Escribir los procedimientos activarBomba() y desactivarBomba(), sabiendo que el puerto de E/S BOMBA es de lectura y escritura

## Resolución

El único punto a aclarar para este ejercicio, es que el sensor externo es controlado por hardware, nosotros simplemente lo utilizaremos para determinar si la bomba está efectivamente activa. Esto implica que haremos `out` sobre un bit del puerto que "no controlamos", por lo que se asume que los `out` que hacemos no tienen efecto sobre el valor del bit del sensor.

A continuación, se escriben los códigos para cada función solicitada:

```c
void activarBomba() {
  char bomba = in(BOMBA); // se asume char porque no dice tamaño
  // bomba & 0x04 -> encender
  // bomba & 0x01 -> sensor externo
  // bomba & 0x10 -> LED
  bomba = bomba | 0x04; // solicito encendido
  out(BOMBA, bomba);
  while (true) { // espero la activación para encender el led
    bomba = in(BOMBA);
    if (bomba & 0x01) {
      bomba = bomba | 0x10; // activo el led
      out(BOMBA, bomba);
      break;
    }
  }
}
```

```c
void desactivarBomba() {
  char bomba = in(BOMBA);
  // bomba & 0x04 -> encender
  // bomba & 0x01 -> sensor externo
  // bomba & 0x10 -> LED
  bomba = bomba & ~0x04; // solicito apagado
  out(BOMBA, bomba);
  while (true) { // espero la desactivación para encender el led
    bomba = in(BOMBA);
    if (!(bomba & 0x01)) {
      bomba = bomba & ~0x10; // desactivo el led
      out(BOMBA, bomba);
      break;
    }
  }
}
```
