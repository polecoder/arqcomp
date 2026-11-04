# Rutinas en Assembler 8086

**Fecha:** 04-11-2026

## Consigna

Escribir una rutina en assembler 8086 que reciba dos números en los registros AX y BX, que los sume y en caso de que se produzca overflow, deje a partir de la dirección segmentada 0x800:0x100 (segmento 0x800, desplazamiento 0x100) el contenido de los registros AX, BX, CX, DX, SI y DI.

## Resolución

Para este ejercicio, no hay mucho más que sentarse a escribir la rutina paso por paso.
**Nota:** Se asume que lo pedido es el contenido del registro AX inicial

```
; especificar el segmento necesario en ES
PUSH AX             ; conservo el primer sumando en el stack
MOV AX, 0x800
MOV ES, AX          ; cambio el valor de Extra Segment para reflejar lo pedido
POP AX

PUSH AX             ; conservo primer sumando en el stack
ADD AX, BX
POP AX              ; devuelvo el primer sumando de AX a AX
JNO finIf           ; salto afuera si no hubo overflow en la operación
; copia
MOV ES:[0x100], AX
MOV ES:[0x102], BX
MOV ES:[0x104], CX
MOV ES:[0x106], DX
MOV ES:[0x108], SI
MOV ES:[0x10A], DI

finIf:
```

**Nota importante:** No se puede poner un valor inmediato directamente en un registro de segmentos. Los primeros 4 pasos corresponden a eso