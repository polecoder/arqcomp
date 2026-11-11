# Algorítmo de compresión

**Fecha:** 11-11-2026

## Consigna

El algoritmo run length encoding es un algoritmo de compresión básico, el cual parte de un arreglo de caracteres y genera otro arreglo donde se indica, de forma ordenada cuántas veces aparece el carácter de forma consecutiva.
Por ejemplo: [A,A,A,D,D,D,C,C] -> [(3A),(3D),(2C)]

Considere el siguiente tipo de datos:

```c
typedef struct {
  char caracter;
  short cant;
} codChar; // 3 bytes (1+2 bytes)
```

Y la función `runLengthEncoding`:

```c
short runLengthEncoding(char* arreglo, short lengthIn,
                        codChar* codif) 
{
    short lengthOut = 0;
    for (short i = 0; i < lengthIn; i++) {
        char charActual = arreglo[i];
        short cantIguales = 0;
        while((i + cantIguales < lengthIn) &&
              (arreglo[i + cantIguales] == charActual)) 
        {
            cantIguales++;
        }
        codif[lengthOut].caracter = charActual;
        codif[lengthOut].cant = cantIguales;
        lengthOut++;
        i = i + cantIguales;
    }
    lengthOut--; // Corrijo: size = ultimaPosicion – 1
    return lengthOut;
}
```

**Se pide:** Compilar a assembler 8086. Los parámetros arreglo y codif se pasan en SI y DI respectivamente como desplazamientos con respecto al segmento ES y el parámetro lengthIn se pasa en el registro CX. El resultado se devuelve en el registro AX.

## Resolución

Este ejercicio consiste en ir paso a paso con la rutina. Cualquier observación importante para la resolución surgirá durante el desarrollo de la rutina.

```asm
; ES:[SI] -> arreglo
; ES:[DI] -> codif
; CX      -> lengthIn
; DX      -> lengthOut
MOV DX, 0
MOV BP, 0 ; BP := i
inicioFor:
  CMP BP, CX ; comparo con lengthIn
  JGE finFor ; termino si BP >= CX
  MOV AL, ES:[SI+BP] ; AL := charActual
  PUSH CX ; respaldo CX := lengthIn
  MOV BX, BP ; BX := i + cantIguales
  inicioWhile:
    CMP BX, CX ; (i + cantIguales < lengthIn)
    JZ finWhile
    CMP ES:[SI+BX], AL ; (arreglo[i + cantIguales] == charActual)
    JNE finWhile
    INC BX ; cantIguales++
    JMP inicioWhile
  finWhile:
  ; guardo cantIguales en CX
  MOV CX, BX ; CX := i + cantIguales
  SUB CX, BP ; CX := cantIguales
  ; obtengo el índice correcto multiplicando x3 a lengthOut
  MOV BX, DX ; BX := lengthOut
  SHL BX, 1
  ADD BX, DX
  ; seteo de codif
  MOV ES:[DI+BX], AL
  MOV ES:[DI+BX+1], CX
  ; preparación para reiniciar el bucle for
  ADD BP, CX ; i = i + cantIguales
  INC BP ; i++
  INC DX ; lengthOut++
  POP CX ; recupero CX := lengthIn
  JMP inicioFor
finFor:
DEC DX ; lengthOut--
MOV AX, DX ; guardo el resultado en AX como indica la letra
RET
```

**Nota #1:** Recordemos para direccionar solo podemos usar BX y BP.
**Nota #2:** JGE -> Jump Greater Equal
**Nota #3:** JNE -> Jump Not Equal

Quizás por el órden en el que aprendemos las cosas, pero es FUNDAMENTAL tener la solución en alto nivel hecha antes de intentar hacer la rutina. La forma más fácil de obtener una rutina que se adapte a lo que necesitamos es justamente "traducir" literalmente un programa de alto nivel.