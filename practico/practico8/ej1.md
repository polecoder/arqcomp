# Lista enlazada con recursión

**Fecha:** 11-11-2026

## Consigna

Se dispone de una lista enlazada de caracteres representada mediante un array. Cada elemento del array contiene un caracter y el índice dentro del array del siguiente elemento de la lista. La lista vacía se representa mediante un índice con valor negativo. La variable `Lista` se define mediante la siguiente declaración:

```c
struct Item {
    char c;
    short siguiente;
}
Item lista[N];
```

La función recursiva `ConstruirCadena` construye una cadena de caracteres representada de acuerdo a la convención del lenguaje C a partir del contenido de la variable `Lista`:

```c
void ConstruirCadena(short indiceLista, char *bufferDestino) {
    if (indiceLista < 0) {
        *bufferDestino = '\0';
    } else {
        *bufferDestino = lista[indiceLista].c;
        ConstruirCadena(lista[indiceLista].siguiente,
                        bufferDestino + 1);
    }
}
```

Compilar la función `ConstruirCadena` en assembler Intel 8086. El parámetro `indiceLista` se pasa en el registro `BX` y la variable `Lista` está apuntada por `DS:SI`. El parámetro `bufferDestino` se pasa en el stack como un desplazamiento con respecto al segmento apuntado por `ES` y la función no retira el parámetro del stack. Una llamada a la función se realiza mediante los siguientes pasos:

1. Se asigna el valor del parámetro `indiceLista` al registro `BX`.
2. Se coloca `bufferDestino` (desplazamiento con respecto a `ES`) en el tope de la pila.
3. Se llama a `ConstruirCadena`.
4. Se retira el parámetro `bufferDestino` de la pila.

## Resolución

Empezamos el ejercicio traduciendo los pasos de la llamada a la función a assembler:

```asm
MOV BX, indiceLista
PUSH ES:[bufferDestino]
CALL construirCadena
POP bufferDestino
```

La recursión puede ser dificil de entender en general o de construir en alto nivel. Por lo general estos ejercicios vienen con el alto nivel hecho; por lo tanto, es importante recordar que nuestra tarea es **traducir** el código de alto nivel a assembler, no necesariamente entender cada detalle del mismo.
Con esto en mente avanzamos hacia el detalle del funcionamiento de `construirCadena`.

```asm
construirCadena PROC
    ; guardar contexto
    PUSH BP
    MOV BP, SP                  ; [BP] = BP viejo, [BP+2] = retorno, [BP+4] = bufferDestino
    PUSH BX
    PUSH DX
    PUSH DI

    MOV DI, SS:[BP+4]           ; DI := bufferDestino
    CMP BX, 0                   ; BX == indiceLista
    JL casoBase
    ; ajusto el índice según el tamaño de cada nodo de la lista (3 bytes)
    MOV DX, BX                  ; DX := indiceLista
    SHL BX, 1
    ADD BX, DX

    ; guardar el char actual en bufferDestino
    MOV DL, DS:[SI+BX]          ; obtengo el char actual
    MOV ES:[DI], DL             ; guardo el char actual

    MOV BX, DS:[SI+BX+1]        ; actualizo BX para llamada recursiva
    
    INC DI
    PUSH DI                     ; bufferDestino + 1 en stack
    CALL construirCadena
    POP DI                      ; DI := bufferDestino post llamada recursiva
    JMP fin

    casoBase:
        MOV BYTE PTR ES:[DI], 0 ; '\0'
    fin:
        ; ajuste de contexto
        POP DI
        POP DX
        POP BX
        POP BP
        RET
construirCadena ENDP
```

Esto concluye el ejercicio, con el detalle de que la línea `MOV BYTE PTR ES:[DI], 0` se escribe pues en ASCII el código 0 justamente corresponde al final de línea '\0'.