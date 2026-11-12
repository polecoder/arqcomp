# Recursión con árboles binarios

**Fecha:** 12-11-2026

## Consigna

Se considera un árbol binario cuyo nodo es definido de la siguiente manera (izquierdo y derecho son punteros a los dos subárboles del nodo):

```c
struct Nodo {
    Nodo *izquierdo;
    Nodo *derecho;
    short numero;
}; // 10 bytes
```

El árbol no tiene por qué estar balanceado. El valor NULL en cualquiera de los nodos (derecho o izquierdo) significa que el nodo no tiene un sucesor por la correspondiente rama del árbol.

1. Escribir en un lenguaje de alto nivel una función recursiva que calcula la profundidad del árbol (largo máximo de caminos entre la raíz y un nodo):

    ```c
    short profundidad(Nodo *arbol);
    ```

2. Compilar la rutina en assembler Intel 8086. El programa llamador hace la siguiente invocación:

    ```asm
    PUSH segmento árbol
    PUSH offset árbol
    CALL profundidad
    POP profundidad
    ```

    El resultado se devuelve en el stack y los argumentos deben retirarse del stack. Se deben conservar todos los registros. Los punteros en este ejercicio son far y se representan en la estructura Nodo como 2 palabras (offset y segmento del puntero).

3. Calcular el tamaño del stack necesario en el peor caso para un árbol con $N$ nodos.

## Resolución

### Parte 1

- Escribir en un lenguaje de alto nivel una función recursiva que calcula la profundidad del árbol (largo máximo de caminos entre la raíz y un nodo):

    ```c
    short profundidad(Nodo *arbol);
    ```

Detallamos a continuación el código para la función planteada:

```c
short profundidad(Nodo *arbol) {
  if (arbol == NULL) {
    return 0;
  }
  short prof_izq = profundidad(arbol->izquierdo);
  short prof_der = profundidad(arbol->derecho);
  if (prof_izq < prof_der)
    return 1 + prof_der;
  return 1 + prof_izq;
}
```

Esto concluye la parte 1.

### Parte 2

- Compilar la rutina en assembler Intel 8086. El programa llamador hace la siguiente invocación:

    ```asm
    PUSH segmento árbol
    PUSH offset árbol
    CALL profundidad
    POP profundidad
    ```

    El resultado se devuelve en el stack y los argumentos deben retirarse del stack. Se deben conservar todos los registros. Los punteros en este ejercicio son far y se representan en la estructura Nodo como 2 palabras (offset y segmento del puntero)

```asm
profundidad PROC
  ; guardar contexto
  PUSH BP
  MOV BP, SP           ; [BP] = viejo BP, [BP+2] = dir retorno,
                       ; [BP+4] = offset, [BP+6] = segmento,
  PUSH ES
  PUSH AX
  PUSH BX
  PUSH DX
  
  ; guardar parámetros del stack
  MOV BX, SS:[BP+4]    ; offset
  MOV ES, SS:[BP+6]    ; segmento
  MOV AX, ES           ; copia segmento para CMP

  ; evaluación de caso base
  CMP AX, 0
  JNE pasoRecursivo
  CMP BX, 0
  JE casoBase

  pasoRecursivo:
    PUSH ES:[BX+2]    ; segmento izq
    PUSH ES:[BX]      ; offset izq
    CALL profundidad
    POP AX            ; resultado llamada izq
    PUSH ES:[BX+6]    ; segmento der
    PUSH ES:[BX+4]    ; offset der
    CALL profundidad
    POP DX            ; resultado llamada der

    CMP AX, DX
    JL incrementar    ; en DX ya está la mayor profundidad
    MOV DX, AX
    incrementar:
      INC DX
      JMP fin
      
  casoBase:
    MOV DX, 0
  fin:
    MOV SS:[BP+6], DX ; resultado en stack
    MOV AX, SS:[BP+2] ; copio la dirección retorno
    MOV SS:[BP+4], AX ; [BP] = viejo BP, [BP+2] = dir retorno,
                      ; [BP+4] = dir retorno, [BP+6] = resultado,
    ; reestablecer contexto
    POP DX
    POP BX
    POP AX
    POP ES
    POP BP
    ADD SP, 2         ; ignoro dirección retorno en [BP+2]
    RET
profundidad ENDP
```

Sobre la rutina, es un gran ejemplo sobre como usar BP para manipular el stack correctamente y devolverlo en el estado que corresponde.

### Parte 3

- Calcular el tamaño del stack necesario en el peor caso para un árbol con $N$ nodos.

El peor escenario posible es aquel donde los $N$ nodos están distribuidos como una lista, en este caso el consumo está dado directamente por la cantidad de llamadas
Evaluemos el consumo para llegar a una recurrencia que nos ayude a calcular el tamaño mínimo del stack:

- $\text{consumo}(0)=12$, 5 registros + 1 dirección de retorno
- $\text{consumo}(n)=12+n\cdot16$

Donde el consumo está compuesto por:

- 12 del caso base que ya explicamos
- 16 -> 5 registros, 2 parámetros, 1 dirección de retorno

Esto concluye el ejercicio.