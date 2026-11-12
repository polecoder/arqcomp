# Recursión con árboles binarios

**Fecha:** 12-11-2026

## Consigna

Se considera un árbol binario definido con la misma estructura del ejercicio 4.

1. Escribir en un lenguaje de alto nivel una función recursiva que calcule la cantidad de nodos que tengan exactamente dos hijos. La firma de la función debe ser short cantidadBifurcaciones(Nodo* arbol);.
2. Compilar la rutina en assembler Intel 8086. El programa llamador hace la siguiente invocación:

    ```asm
    PUSH segmento árbol
    PUSH offset árbol
    CALL cantidad_bifurcaciones
    POP cantidad
    ```

    El resultado se devuelve en el stack y los argumentos deben retirarse del stack.

3. Calcular el consumo de stack en el peor caso para un árbol de $N$ nodos.

## Resolución

### Parte 1

- Escribir en un lenguaje de alto nivel una función recursiva que calcule la cantidad de nodos que tengan exactamente dos hijos. La firma de la función debe ser `short cantidadBifurcaciones(Nodo* arbol);`.

No hay muchas observaciones sobre el código de alto nivel.

```c
short cantidadBifurcaciones(Nodo* arbol) {
  if (arbol == NULL)
    return 0;
  short cant_izq = cantidadBifurcaciones(arbol->izquierdo);
  short cant_der = cantidadBifurcaciones(arbol->derecho);
  short resultado = cant_izq + cant_der;
  if (arbol->izquierdo != NULL && arbol->derecho != NULL)
    resultado++;
  return resultado;
}
```

### Parte 2

- Compilar la rutina en assembler Intel 8086. El programa llamador hace la siguiente invocación:

    ```asm
    PUSH segmento árbol
    PUSH offset árbol
    CALL cantidad_bifurcaciones
    POP cantidad
    ```

    El resultado se devuelve en el stack y los argumentos deben retirarse del stack.

```asm
cantidad_bifurcaciones PROC
  ; guardar contexto
  PUSH BP
  MOV BP, SP ; [BP] = BP viejo, [BP+2] = dir retorno
             ; [BP+4] = offset, [BP+6] = segmento
  PUSH ES
  PUSH SI
  PUSH AX
  PUSH CX
  PUSH DX

  ; extraer parámetros
  MOV SI, [BP+4] ; offset
  MOV ES, [BP+6] ; segmento

  ; evaluacion caso base
  MOV AX, ES     ; asignación temporal para CMP
  CMP AX, 0
  JNE pasoRecursivo
  CMP SI, 0
  JE casoBase

  pasoRecursivo:
    PUSH ES:[SI+2] ; segmento izq
    PUSH ES:[SI]   ; offset izq
    CALL cantidad_bifurcaciones
    POP CX         ; cant_izq
    PUSH ES:[SI+6] ; segmento der
    PUSH ES:[SI+4] ; offset der
    CALL cantidad_bifurcaciones
    POP AX         ; cant der
    ; cálculo de resultado
    MOV DX, CX
    ADD DX, AX     ; resultado
    ; evaluar si izq == NULL
    CMP WORD PTR ES:[SI+2], 0 ; segmento izq
    JNE evaluarDerecho
    CMP WORD PTR ES:[SI], 0   ; offset izq
    JE fin           ; no corresponde incremento
    ; evaluar si der == NULL
    evaluarDerecho:
      CMP WORD PTR ES:[SI+6], 0 ; segmento der
      JNE incrementar
      CMP WORD PTR ES:[SI+4], 0 ; offset der
      JE fin                    ; no corresponde incremento
    incrementar:
      INC DX
      JMP fin

  casoBase:
    MOV DX, 0
  fin:
    ; acomodar el stack
    MOV SS:[BP+6], DX ; resultado
    MOV AX, SS:[BP+2] ; dir retorno
    MOV SS:[BP+4], AX ; dir retorno
      ; [BP+2] = dir retorno
      ; [BP+4] = dir retorno, [BP+6] = resultado

    ; restaurar contexto
    POP DX
    POP CX
    POP AX
    POP SI
    POP ES
    POP BP
    ADD SP, 2 ; ajuste stack
    RET
cantidad_bifurcaciones ENDP
```

### Parte 3

- Calcular el consumo de stack en el peor caso para un árbol de $N$ nodos.

