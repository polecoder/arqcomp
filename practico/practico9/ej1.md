# E/S para un teclado

**Fecha:** 13-11-2026

## Consigna

Se considera el teclado de una terminal de caracteres controlado por un microprocesador. El dispositivo es accesible mediante dos registros de un byte, ESTADO_TECLADO y DATO_TECLADO, accesibles en direcciones de E/S de solo lectura. El hardware coloca el valor 1 en el bit más significativo de ESTADO_TECLADO cuando hay un carácter disponible. Cuando se lee el carácter disponible en DATO_TECLADO, el hardware coloca el valor 0 en el bit más significativo de ESTADO_TECLADO.
Escribir un procedimiento que lea los caracteres y los almacene en una lista de largo variable.

## Resolución

Este ejercicio es muy sencillo pero vale la pena hacer algunas observaciones:

1. Hasta no ver la parte de interrupciones, se trabaja con bucles `while (true)` que claramente no son los más eficientes, pues el CPU estaría permanentemente corriendo el fragmento de código.
2. No es necesario implementar las estructuras auxiliares que utilicemos, lo importante es que se entienda bien que queremos hacer (por ejemplo la lista de este ejercicio)

A continuación, el fragmento de código que resuelve el problema.

```c
Lista lista = crearLista();

void leerCaracteres(nodo* lista) {
  while (true) {
    char estado = in(ESTADO_TECLADO);
    if (estado & 0x80) {
      char dato = in(DATO_TECLADO);
      lista->insertar(dato);
    }
  }
}
```