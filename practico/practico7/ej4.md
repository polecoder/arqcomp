# Rutinas con MUL y DIV

**Fecha:** 05-11-2026

## Consigna

1. Implementar una rutina en assembler 8086 que dada una dirección D1 (recibida en SI), una dirección D2 (recibida en DI) y un natural N (recibido en BL), calcule el cuadrado de cada byte contenido a partir de D1 y escriba los resultados a partir de D2 (utilizando 2 bytes para cada resultado). Las direcciones D1 y D2 son desplazamientos con respecto al registro DS. Usar la instrucción MUL.
2. Implementar una rutina en assembler 8086 que dada una dirección D1 (recibida en SI), una dirección D2 (recibida en DI), un natural N (recibido en BL) y un número divisor (recibido en CX), tome las N palabras de memoria ubicadas a partir de D1, las divida entre el divisor y ubique el cociente en palabras de memoria ubicadas a partir de D2. Las direcciones D1 y D2 son desplazamientos con respecto al registro DS. Usar la instrucción DIV.

## Resolución

**Aclaración:** Como indica la letra del práctico, se asumirá que los datos pasados como parámetros se encuentran en el segmento de datos DS.

### Parte 1

- Implementar una rutina en assembler 8086 que dada una dirección D1 (recibida en SI), una dirección D2 (recibida en DI) y un natural N (recibido en BL), calcule el cuadrado de cada byte contenido a partir de D1 y escriba los resultados a partir de D2 (utilizando 2 bytes para cada resultado). Las direcciones D1 y D2 son desplazamientos con respecto al registro DS. Usar la instrucción MUL.

Primero resolveremos el problema en alto nivel.

```c
void arreglo_cuadrado(char* mem, short d1, short d2, int n) {
    int contador = 0;
    while (contador < n) {
        char actual = mem[d1];
        int cuadrado = actual * actual
        mem[d2] = cuadrado & 0x00FF;
        // guardo la parte alta del cuadrado en el segundo byte
        mem[d2+1] = (cuadrado & 0xFF00) >> 8;
        contador += 1;
        d1 += 1;
        d2 += 2;
    }
}  
```

**Nota:** El shift a la derecha de 8 lugares es porque `mem` es un arreglo de bytes, y se le intenta guardar un valor que solo tiene unos en los bits del byte más significativo; como C trunca el valor tomando el byte menos significativo, sin el shift tomaría el valor 0x00.

Ahora podemos traducirlo a Assembler 8086.

```
MOV CL, 0x00
inicioBucle:
    CMP CL, BL
    JZ finBucle
    MOV AL, DS:[SI] ; guardo el valor actual en AL
    MUL AL ; calculo el cuadrado

    MOV DS:[DI], AL ; guardo la parte baja del cuadrado
    INC DI

    MOV DS:[DI], AH ; guardo la parte alta del cuadrado
    INC DI

    INC SI ; paso al siguiente byte para reingresar al bucle

    INC CL ; incremento el contador
    JMP inicioBucle
finBucle:
```

### Parte 2

- Implementar una rutina en assembler 8086 que dada una dirección D1 (recibida en SI), una dirección D2 (recibida en DI), un natural N (recibido en BL) y un número divisor (recibido en CX), tome las N palabras de memoria ubicadas a partir de D1, las divida entre el divisor y ubique el cociente en palabras de memoria ubicadas a partir de D2. Las direcciones D1 y D2 son desplazamientos con respecto al registro DS. Usar la instrucción DIV.


Primero resolveremos el problema en alto nivel.

```c
void arreglo_dividido(char* mem, short d1, short d2,
                      int n, short divisor) {
    while (n > 0) {
        // guardo los bits más significativos en la parte alta
        int actual = mem[d1+1] << 8;
        actual = actual | mem[d1];

        short cociente = (short)(actual / divisor);
        mem[d2] = cociente & 0x00FF;
        mem[d2+1] = (cociente & 0xFF00) >> 8;
        d1 += 2;
        d2 += 2;
        n--;
    }
}  
```

**Nota:** Para la división "emulamos" el comportamiento de DIV haciendo que el dividendo sea de 32 bits.

Ahora podemos traducirlo a Assembler 8086.

```
inicioBucle:
    CMP BL, 0x0
    JZ finBucle
    MOV AL, DS:[SI] ; guardo la parte baja del dividendo de 4 bytes.
    INC SI
    MOV AH, DS:[SI] ; guardo la parte alta del dividendo de 4 bytes.
    INC SI

    MOV DX, 0x0000 ; seteo los 4 bytes más significativos del dividendo en 0
    DIV CX ; calculo el cociente

    MOV DS:[DI], AL ; guardo la parte baja del cociente
    INC DI
    MOV DS:[DI], AH ; guardo la parte alta del cociente
    INC DI

    DEC BL ; resto un byte a los N que faltan
    JMP inicioBucle
finBucle:
```

---

**Observación:** En ambos las rutinas que creé, hay algo que no tuve en cuenta: los MOV que hago no son los ideales. Actualmente seteo las partes altas y bajas por separado, pero este procesador es capaz de determinar por su cuenta el tamaño de lo que tiene que leer o escribir.