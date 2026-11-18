# Atención de teclado con cambio de rutina

**Fecha:** 18-11-2026

## Consigna

Se dispone de tres rutinas utilitarias RUTINA1, RUTINA2 y RUTINA3. Un programa en ejecución es interrumpido en cualquier momento para ejecutar estas rutinas por acción del teclado. La ejecución comienza con la secuencia `<ESC><1>, <ESC><2> y <ESC><3>`, respectivamente. `<ESC>` es un carácter especial, conocido. Las rutinas no terminan en ningún momento y la forma de volver al programa original es presionando la tecla `<ESC>`. Cuando se ejecuta un programa $X$, presionando `<ESC><2>` se pasa a ejecutar RUTINA2. Al presionar `<ESC>` nuevamente, se vuelve a ejecutar el programa $X$ en la dirección donde fue interrumpido.

Escribir en un lenguaje de alto nivel un procedimiento que atienda la interrupción del teclado, lea el carácter contenido en el puerto de solo lectura de 1 byte `TECLA` y, en caso de que la sucesión de caracteres recibida sea válida, pase a ejecutar la rutina correspondiente. La interrupción debe retornar al programa adecuado luego de procesar una tecla.

Para obtener la dirección de una rutina se dispone de la función `rutina* radr(string nombre_rutina)`. Para pasar el control a una dirección determinada se usa la función `void jmp(rutina* dirección)`. Para obtener la dirección del programa interrumpido se usa la función `rutina* padr()`.

## Resolución

Para los ejercicios de interrupciones, conviene tener algunas observaciones en mente:

1. **Cada interrupción es un evento aislado:** La rutina se ejecuta, atiende un evento y termina. No hay bucles como pasaba con el polling.
2. **Si hay secuencia, se necesita memoria:** Si la consigna habla de "secuencia", "después de", etc. se necesitan variables `static` o globales que sobrevivan entre interrupciones.
3. **Pensarlo con máquinas de estados:** Esto suele ser una gran herramienta para determinar que se hace según cada caso antes de programar.

Considerando esto, veamos el código correspondiente a la función solicitada:

```c
static bool en_rutina = false;
static bool esperando_numero = false;
static rutina* programa = NULL; // guardo el programa interrumpido
void atender_interrupcion() {
  char t = in(TECLA);
  // 1. Caso en el que estamos ejecutando una rutina
  if (en_rutina) {
    if (t == ESC) {
      en_rutina = false;
      jmp(programa); // salgo de la rutina volviendo al programa principal
    }
    jmp(padr()) // sigo con la ejecución de la rutina interrumpida
  }

  // 2. Caso en el que la tecla previa fue un ESC
  if (esperando_numero) {
    esperando_numero = false;
    rutina* destino = NULL;
    if (t == '1') radr("RUTINA1");
    else if (t == '2') radr("RUTINA2");
    else if (t == '3') radr("RUTINA3");
    if (destino != NULL) {
      programa = padr();
      en_rutina = true;
      jmp(destino);
    }
    jmp(padr()); // secuencia invalida: vuelvo al programa X
  }

  // 3. La tecla es ESC y no estoy ejecutando una rutina
  if (t == ESC) esperando_numero = true;

  // 4. Si no pasa ninguno de los casos anteriores, vuelvo
  jmp(padr());
}
```