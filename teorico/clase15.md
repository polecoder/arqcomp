# Funciones y datos en Intel 8086

**Fecha:** 09-11-2026

## Introducción

Por definición, una variable es un espacio en memoria con un nombre asignado. En alto nivel, todos conocemos el concepto de variable. En assembler, esto se mapea a posiciones de memoria con una etiqueta.
El tipo de una variable de alto nivel se refleja con el tamaño que ocupan en memoria.
Tampoco hay *new* o *delete*, son funciones de alto nivel.

Las variables siempre tienen una posición de memoria asignada. En base a su alcance, decidiremos si almacenarla en memoria, stack o registro.
Por ejemplo: una variable con alcance (scope) corto, podremos ubicarla únicamente en un registro; mientras que por otra parte, una variable global si o si debe tener espacio en la memoria principal.

## Tipos de datos

Muchos de los conceptos vertidos en esta clase son de alcance general, pero nos restringiremos solamente a la arquitectura Intel 8086.

### Tipos básicos

Tenemos solo dos tipos básicos de acuerdo al tamaño de palabra de la arquitectura:

- char (1 byte)
- unsigned short o short (2 bytes)

### Punteros

Los punteros en Intel 8086 pueden ser:

- en el mismo segmento (2 bytes para el desplazamiento)
- en otro segmento (4 bytes: 2 desplazamiento y 2 segmento)

Usualmente se trabaja con punteros en el mismo segmento.

### Arreglos

Para compilar un arreglo en Intel, disponemos un elemento del arreglo a continuación del otro. Cada elemento del arreglo ocupa lo que ocupa el tipo de datos del arreglo.
Veamos por ejemplo, como sumar 10 elementos de un arreglo de enteros que comienza en [BX]:

```asm
MOV SI, 0
MOV AX, 0
MOV CX, 0
evaluar:
  CMP CX, 10
  JZ fin
  ADD AX, [BX+SI]
  ADD SI, 2
  INC CX
  JMP evaluar
```

### Structs

Para compilar un struct en Intel 8086, disponemos un elemento del struct a continuación del otro en el orden en el que fueron declarados. El struct ocupa la suma de lo que ocupan sus elementos.

**Ejemplo:** Acceso a los componentes de un struct ubicado en [BX]:

```c
struct ejemplo {
  short x,y;
  char a,b;
} // ocupa 6 bytes
```

```asm
; si BX apunta al inicio
mov AX, [BX] ; x
mov AX, [BX+2] ; y
mov AX, [BX+4] ; a
mov AX, [BX+5] ; b
```

### Compilar tipos compuestos

Nos referimos a tipos compuestos a aquellos que utilizan más de un tipo estructurado en su definición. Para compilarlos, seguiremos las pautas planteadas para cada uno de los tipos, teniendo en cuenta únicamente que sus componentes no son tipos básicos.

**Ejemplo:** Sumar 10 componentes enteros de un arreglo de structs que comienza en [BX]:

```c
typedef struct {
  char a;
  short x;
} ejemplo
ejemplo arreglo[10];
```

```asm
MOV SI, 0
MOV AX, 0
MOV CX, 0
evaluar:
  CMP CX, 10
  JZ fin
  ADD AX, [BX+SI+1]
  ADD SI, 3 ; tamaño del elemento
  INC CX
  JMP evaluar
fin:
```

## Funciones

Como la mayoría de las arquitecturas, Intel 8086 incluye un mecanismo para implementar funciones y llamados.
Difiere de un flujo condicional en cuanto a que se retorna luego de un llamado.

Veamos un ejemplo: ¿Cómo es la secuencia de pasos en este programa?

```c
int sumar(int a, int b) {
  return a+b;
}

int main() {
  ...
  int dato = sumar(4,5);
  ...
}
```

1. Pasar parámetros 4 y 5
2. Saltar a la función suma
3. Ejecutar la función suma
4. Pasar resultado a devolver
5. Volver al punto de invocación
6. Asignar a dato el resultado
7. Continuar con la ejecución del programa

### Pasaje de parámetros

Al llamar una función, es posible pasar los parámetros mediante dos formas:

1. Por REGISTRO. Se cargan los parámetros en registros antes de hacer el llamado.
2. Por STACK. Se hace PUSH de los parámetros antes hacer el llamado.

El retorno del resultado es análogo. Veremos esto más adelante.

---

El mecanismo para el llamado de funciones en Intel es naturalmente el **stack**.
Es la herramienta que provee la arquitectura para guardar el contexto del llamador (en particular, la posición actual).

Las dos instrucciones para implementar llamados a funciones son:

1. CALL: Realiza el llamado
2. RET: Devuelve el control al llamador

### CALL

CALL permite invocar una rutina definida mediante la directiva PROC. Para esto, sencillamente realiza el PUSH de la posición actual en la ejecución y salta a la etiqueta.

1. PUSH IP
2. JMP etiqueta

**CALL FAR:** Si para invocar la rutina se debe cambiar el valor de CS, el call modifica su comportamiento: también hace push del CS.

1. PUSH CS
2. PUSH IP
3. JMP funcion (nuevo CS e IP)

### RET

RET nos permite volver al punto anterior de ejecución. Para esto, toma del tope del stack la dirección de retorno y vuelve hacia allí.

1. POP IP

Notar que el RET asume que lo que se encuentra en el tope del stack, es la dirección de retorno. Si es otra cosa, el sistema tendrá un comportamiento desconocido.
¿Qué pasa si hay que retornar de un CALL FAR?

---

Definimos el protocolo de recibir los parámetros en AX y BX respectivamente, devolviendo el resultado en AX.

```asm
PROC sumar:
  ADD AX, BX
  RET
ENDPROC

PROC main:
  ...
  MOV AX, 4
  MOV BX, 5
  CALL sumar
  ...
; AX es 9
```

### Contexto de ejecución

El contexto de ejecución consiste en el valor de los registros del CPU. Esto incluye los registros de datos así como los registros de estado.
Este concepto también existe en alto nivel y se relaciona con el alcance de una variable.

Por lo general, cuando llamamos a una función en alto nivel, al retornar, nuestras variables valen lo mismo antes y después del llamado.
Por otra parte, en bajo nivel y en particular con Intel 8086, al ejecutar una función el valor de los registros (contexto) no se preserva automáticamente como en alto nivel.

```asm
PROC sumar:
  XOR CX, CX
  ADD AX, BX
  RET
ENDPROC

PROC main:
  MOV CX, 30
  MOV AX, 4
  MOV BX, 5
  CALL sumar
; AX = 9 y CX = 0
```

En Intel 8086, la herramienta que se utiliza para preservar el contexto es el stack. Intel preserva el contexto mínimo de la función "padre", esto es: el punto de ejecución previo a la llamada (a dónde volver luego).
El programador debe preservar manualmente el contexto, pero es una tarea opcional.

En términos generales, el procedimiento es:

1. Al comenzar la función, almacenar en el stack el valor de todos los registros que se utilizan.
2. Ejecutar el cuerpo de la función.
3. Restaurar desde el stack en *orden inverso* los registros utilizados.
4. Retornar (el tope del stack debe contener el IP anterior)

Existen excepciones, como por ejemplo si un registro se utiliza para devolver el resultado (como AX en la suma).

Veamos dos ejemplos que preservan y no preservan el contexto respectivamente:

#### Preserva contexto

```asm
PROC sumar:
  PUSH CX
  PUSH DX
  XOR CX, CX
  ADD DX, DX
  ADD AX, BX
  POP DX
  POP CX
  RET
ENDPROC

PROC main:
  MOV AX, 4
  MOV BX, 5
  MOV CX, 30
  MOV DX, 40
  CALL sumar
; AX = 9, BX = 5, CX = 30, DX = 40
```

#### No preserva contexto

```asm
PROC sumar:
  XOR CX, CX
  ADD DX, DX
  ADD AX, BX
  RET
ENDPROC

PROC main:
  MOV AX, 4
  MOV BX, 5
  MOV CX, 30
  MOV DX, 40
  CALL sumar
; AX = 9, BX = 5, CX = 0, DX = 80
```

### Pasaje de parámetros

Como ya se mencionó anteriormente, los parámetros se pueden pasar tanto por stack como por registros. Las funciones pueden utilizar simultáneamente ambos mecanismos.

#### Pasaje por registros

Al definir una función, podemos especificar los registros utilizados para el intercambio de información. Para el caso de *sumar* del ejemplo anterior:

> SUMAR es una función que preserva el contexto, recibe en AX y BX los operandos a sumar y retorna en AX el valor de la suma.

El pasaje por registros de parámetros y resultados tiene dos inconvenientes principales:

1. **Pocos registros:** La cantidad de parámetros a pasar es muy limitada.
2. **Liberar registros para resultado:** Para retornar resultados, el llamador debe liberar registros.

#### Pasaje por stack

El mecanismo más flexible para el pasaje de parámetros y retorno de resultados es el stack.
El esquema básico para el llamado de una función que recibe y devuelve los parámetros por stack es:

1. **PUSH de parámetros:** Antes de invocar la función, se realiza el push de los parámetros al stack.
2. **Invocación:** La rutina invocada puede o no preservar el contexto.
3. **Retorno y POP de resultados:** Al retornar al llamador, en el tope del stack se encuentran los resultados.