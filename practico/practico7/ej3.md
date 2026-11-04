# Búsqueda en arreglo con compilación a Assembler 8086

**Fecha:** 04-11-2026

## Consigna

Implementar una función que dados un arreglo de bytes y un elemento, indica si el elemento está o no en el arreglo. La función debe retornar la posición relativa en que se encuentra el elemento o -1 en caso de que el elemento no se encuentre en la tabla.

Compilar en assembler 8086 la rutina que recibe en AL el elemento a buscar, en BX la dirección de comienzo del arreglo y en DI la dirección final del arreglo (ambas direcciones son desplazamientos con respecto a ES). El valor a retornar se debe almacenar en el registro CX. Preservar el valor de todos los demás registros utilizados.

## Resolución

Para este ejercicio, primero resolvemos el problema en alto nivel, para luego hacer la compilación a Assembler.

```c
int busqueda_arreglo(char* mem, char elemento, short inicio,
                     short fin) {
    int contador = 0;
    short actual = inicio;
    while (actual <= fin) {
        if (mem[actual] == elemento) break;
        actual++;
        contador++;
    }
    if (actual <= fin) return contador;
    return -1;
}
```

Y ahora podemos traducir el programa a Assembler 8086 para finalizar el ejercicio.

> Compilar en assembler 8086 la rutina que recibe en AL el elemento a buscar, en BX la dirección de comienzo del arreglo y en DI la dirección final del arreglo (ambas direcciones son desplazamientos con respecto a ES). El valor a retornar se debe almacenar en el registro CX. Preservar el valor de todos los demás registros utilizados.

```
; guardar los valores iniciales de los registros usados
PUSH DX

MOV CX, 0x0000 ; contador
INC DI ; incremento la dirección final en 1 para terminar el bucle correctamente
inicioBucle:
CMP BX, DI ; condición while
JZ finBucle

MOV DL, ES:[BX] ; guardo en DL el valor de actual
CMP DL, AL; condición if
JZ finBusqueda
; post-chequeo de condiciones
INC BX
INC CX
JMP inicioBucle

finBucle:
MOV CX, 0xFFFF ; constante -1 ya que salió sin encontrar

finBusqueda:
POP DX ; devuelvo el valor inicial de DX
RET ; finaliza la función
```

**Nota:** Existe un "error" en esta rutina. Al ser las ciclicas las direcciones de los segmentos de la arquitectura Intel 8086, existe el riesgo de un bucle infinito si la dirección final del arreglo es 0xFFFF.
No lo corrijo porque entiendo que escapa del contexto del curso.