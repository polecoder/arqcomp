# Recursión desde alto nivel hasta assembler

**Fecha:** 12-11-2026

## Consigna

Considere la secuencia de naturales $S(0),S(1),\ldots,S(N)$ definida por la relación $S(N)=S(N-1)+S(N-2)$, con $S(0)=0$ y $S(1)=1$.

1. Escribir en un lenguaje de alto nivel una función recursiva $F(N)$ tal que $F(N)=S(N)$.
2. Compilar la rutina en assembler Intel 8086. El argumento $N$ se pasa en el registro AX y el valor de la función se devuelve en el registro BX.
3. Calcular la cantidad de bytes mínima que debe tener el stack para que sea posible la ejecución de la función para el caso $N=5$

## Resolución

### Parte 1

- Escribir en un lenguaje de alto nivel una función recursiva $F(N)$ tal que $F(N)=S(N)$.

Esta es la sección más simple del ejercicio, tratamos de dejar todo lo más sencillo posible para facilitar la compilación.

```c
unsigned short relacion(unsigned short n) {
  unsigned short resultado = n;
  if (n == 0 || n == 1) {
    return resultado;
  }
  unsigned short n1 = relacion(n-1);
  unsigned short n2 = relacion(n-2);
  resultado = n1+n2;
  return resultado;
}
```

### Parte 2

- Compilar la rutina en assembler Intel 8086. El argumento $N$ se pasa en el registro AX y el valor de la función se devuelve en el registro BX.

```asm
relacion PROC
  ; guardar contexto
  PUSH AX
  PUSH CX

  CMP AX, 0
  JE casoBase0
  CMP AX, 1
  JE casoBase1
  ; llamadas recursivas
  DEC AX
  CALL relacion
  PUSH BX       ; [relacion(n-1),...]
  DEC AX        ; la rutina preserva AX
  CALL relacion
  PUSH BX       ; [relacion(n-2),relacion(n-1),...]
  ; cálculo de resultado
  POP CX
  POP BX
  ADD BX, CX    ; BX := relacion(n-1) + relacion(n-2)
  JMP fin

  casoBase0:
    MOV BX, 0
    JMP fin
  casoBase1:
    MOV BX, 1
  fin:
    ; reestablecer contexto
    POP CX
    POP AX
    RET
relacion ENDP
```

### Parte 3

- Calcular la cantidad de bytes mínima que debe tener el stack para que sea posible la ejecución de la función para el caso $N=5$

Para esta parte, debemos plantear la recurrencia del consumo, teniendo en cuenta que buscamos el máximo momento de consumo posible.

- $\text{consumo}(0)=\text{consumo}(1)=6$ pues: [AX, CX, DIR_RET]
- $\text{consumo}(n)=6+\max\{\text{consumo}(n-1), 2+\text{consumo}(n-2)\}$

Con esto simplemente tenemos que evaluar hasta llegar a $\text{consumo}(5)$:

- $\text{consumo}(2)=6+\max\{6,8\}=14$
- $\text{consumo}(3)=6+\max\{14,8\}=20$
- $\text{consumo}(4)=6+\max\{20,16\}=26$
- $\text{consumo}(5)=6+\max\{26,22\}=32$

Por lo que la cantidad de bytes mínima que debe tener el stack es 32 para que sea posible la ejecución de la función.