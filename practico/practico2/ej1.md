# Asignaciones en C

**Fecha:** 24-09-2026

## Consigna

Dados los siguientes programas en C, indique el valor binario que se almacena en cada variable luego de cada asignación:

1.

```c
short a = 0xA87B;
short b = 0x771C;
short c = a | b;
char d = c >> 4;
```

2.

```c
char a = 7;
char b = 0xF3;
char c = 5;
short d = a << 12 | b << 4 | c;
```

3.

```c
char a = -3;
char b = 0x80;
short res = a + b;
```

## Resolución

### Parte 1

```c
short a = 0xA87B; // 1010 1000 0111 1011
short b = 0x771C; // 0111 0111 0001 1100
short c = a | b;  // 1111 1111 0111 1111
char d = c >> 4;  // 1111 0111
```

Hacemos un comentario al lado de cada variable para mostrar el valor almacenado.

### Parte 2

```c
char a = 7;                     // 0x00000007
char b = 0xF3;                  // 0xFFFFFFF3
char c = 5;                     // 0x00000005

                                // a << 12 -> 0x00007000
                                // b << 4  -> 0xFFFFFF30
                                // c       -> 0x00000005
short d = a << 12 | b << 4 | c; // 0xFFFFFF35
```

Hacemos un comentario al lado de cada variable para mostrar el valor almacenado.

1. `char` no se opera directamente en C, cualquier operando de este tipo se convierte a `int` antes de aplicar cualquier operador, esto es importante para esta parte.
2. Para `b`, al realizar la conversión a 32 bits, notamos que es un número negativo. En estos casos siempre se "extiende" el signo agregando unos en vez de ceros. Se puede verificar que esto mantiene el número correcto.

### Parte 3

```c
// 1. Convertimos a complemento a dos: 0000 0011 -> 1111 1101
// 2. Binario a Hexadecimal: 1111 1101 -> 0xFD
// 3. Extender el signo
char a = -3;       // 0xFFFFFFFD

char b = 0x80;     // 0xFFFFFF80
short res = a + b; // 0xFFFFFFFD + 0xFFFFFF80 = 0xFFFFFF7D
```

Hacemos un comentario al lado de cada variable para mostrar el valor almacenado.
La dificultad de esta parte es realizar la suma de 32 bits. Después es extender el signo de los números negativos como ya vimos antes.
