# Tipos compuestos

**Fecha:** 11-11-2026

## Consigna

Considere la siguiente definición de una lista de shorts y una función para incrementar todos los elementos de la lista:

```c
typedef struct {
  short dato;
  char sig; // índice en arreglo del siguiente elemento de la lista (0xFF indica NULL)
} nodo;
nodo[255] lista;
void incrementa(char indice) {
  while (indice != '0xFF') {
    lista[indice].dato++;
    indice = lista[indice].sig;
  }
}
```

**Se pide:** Compilar a 8086 la función incrementa sabiendo que la variable lista se encuentra a partir del desplazamiento cero con respecto a ES. El parámetro se recibe en BL.

## Resolución

Recordando el teórico, tenemos que el struct tiene el siguiente tamaño en memoria:

```c
typedef struct {
  short dato;
  char sig;
} nodo; // 3 bytes (2+1 bytes)
```

Restaría ahora compilar el fragmento en una rutina de assembler.

```
bucle:
  CMP BL, 0xFF
  JZ fin
  MOV BH, 0 ; para usar BX completo
  ; ahora multiplico por 3 el índice almacenado
  MOV AX, BX
  SHL BX, 1
  ADD BX, AX
  INC WORD PTR ES:[BX] ; incremento el dato
  
  MOV BL, ES:[BX+2] ; indice = lista[indice].sig
  JMP bucle
fin:
  RET
```