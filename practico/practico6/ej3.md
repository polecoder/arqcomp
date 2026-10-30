# Título

**Fecha:** 30-10-2026

## Consigna

La rutina BBinaria implementa el algoritmo de búsqueda binaria. Dada una palabra en la dirección de memoria 100h y una secuencia de 100 palabras ordenadas en memoria a partir de la dirección 101h hasta la dirección 165h, determina si la palabra pertenece a la secuencia, y en tal caso escribe en la posición 166h el valor 1, o en caso contrario escribe el valor 0.

```c
void BBinaria() {
    int inf, sup, medio, dato;
    char esta = 0;
    inf = 0x101;
    sup = 0x165;
    dato = memoria[0x100];
    do {
        medio = (inf + sup) / 2;
        if (dato == (memoria[medio])) {
            esta = 1;
        } else if (dato < memoria[medio]) {
            sup = medio - 1;
        } else {
            inf = medio + 1;
        }
    } while (!(esta || (inf > sup)));
    if (esta) {
        memoria[0x166] = 1;
    } else {
        memoria[0x166] = 0;
    }
}
```

Se pide: Compilar la rutina BBinaria en el lenguaje ensamblador del Ejercicio 2.

## Instrucciones

A continuación especificamos las instrucciones para el lenguaje ensamblador que se deben utilizar para este ejercicio.

| Instrucción | Descripción |
|-------------|-------------|
| LOAD REG1, REG2 | Guarda en REG2 el contenido de la dirección de memoria almacenada en REG1 |
| STORE REG1, REG2 | Escribe el contenido de REG1 en la dirección de memoria almacenada en REG2 |
| NOP | No realiza ninguna operación |
| SETLO INM, REG | Carga el valor constante INM de 8 bits en la parte baja de REG |
| SETHI INM, REG | Carga el valor constante INM de 8 bits en la parte alta de REG |
| ADD REG1, REG2, REG3 | Suma REG1 y REG2, guarda el resultado en REG3 |
| SUB REG1, REG2, REG3 | Resta REG2 a REG1, guarda el resultado en REG3 |
| AND REG1, REG2, REG3 | And bit a bit entre REG1 y REG2, guarda el resultado en REG3 |
| OR REG1, REG2, REG3 | Or bit a bit entre REG1 y REG2, guarda el resultado en REG3 |
| NOT REG1, REG2 | Guarda en REG2 el resultado de hacerle un NOT al REG1 |
| SL REG1, INM, REG2 | Shift a la izquierda de tantos bits de REG1 como indique el inmediato INM (de 4 bits) y guarda el resultado en REG2 |
| SR REG1, INM, REG2 | Shift a la derecha de tantos bits de REG1 como indique el inmediato INM (de 4 bits) y guarda el resultado en REG2. Expande el signo. |
| CMP REG1, REG2 | Instrucción de comparación que realiza la resta REG1-REG2 sin guardar el resultado y actualiza las banderas de condición |
| JZ INM | Si la bandera $Z$ está en 1 salta INM (INM es un número con signo de 12 bits) instrucciones desde la posición actual |
| JN INM | Si la bandera $N$ está en 1 salta INM (INM es un número con signo de 12 bits) instrucciones desde la posición actual |
| JMP INM | Salto incondicional de INM instrucciones (INM es un número con signo de 12 bits) desde la posición actual |

## Resolución

Encontrar la resolución de este ejercicio es bastante directo: debemos traducir línea por línea el código dado en C a ensamblador para este procesador.

```
// direccion inf en R00
SETLO 0x01 R00
SETHI 0x01 R00

// direccion sup en R01
SETLO 0x65 R01
SETHI 0x01 R01

// direccion dato en R02
SETLO 0x00 R02
SETHI 0x01 R02

// direccion resultado en R03
SETLO 0x66 R03
SETHI 0x01 R03

// valor dato en R04
LOAD R02 R04

// constante 1 positivo
SETLO 0x01 R07
SETHI 0x00 R07

// COMIENZO BUCLE
CMP R01 R00 // condición de salida si Z=1
JZ 0x00B // salida afuera del bucle sin haber encontrado el valor

ADD R00 R01 R05 // direccion medio en R05
SR R05 0x1 R05 // direccion medio en R05
LOAD R05 R06 // valor medio en R06

CMP R04 R06 // salto afuera del bucle si encontramos el valor
JZ 0x007 // salto afuera del bucle si encontramos el valor
JC 0x003 // el valor buscado está a la izquierda del medio

ADD R06 R07 R02 // CASO #1: valor buscado a la derecha del medio actual
JMP 0xFF7 // vuelvo al inicio del bucle

SUB R06 R07 R01 // CASO #2: valor buscado a la izquierda del medio actual
JMP 0xFF5 // vuelvo al inicio del bucle

// AFUERA DEL BUCLE
SUB R07 R07 R07 // VALOR NO ENCONTRADO: constante 0 en R07
STORE R07 R03 // salta acá directo si el valor si es encontrado
```

Esto concluye el ejercicio.

**TODO:** Resta corregirlo con Inteligencia Artificial para validar si existe algún error.