# Conversiones con bases relacionadas entre si

**Fecha:** 22-07-2026

## Consigna

Realizar las siguientes conversiones:

1. $100110111011_2$ a hexadecimal
2. $\text{B2CD}_{16}$ a binario
3. $38726_9$ a base 3
4. $1101101001011_2$ a octal

## Resolución

En este ejercicio las bases con las que trabajamos son potencias una de otra. Esto facilita muchísimo las conversiones.

### Conversión #1

- $100110111011_2$ a hexadecimal

Recordando que $2^4=16$, tenemos que un dígito hexadecimal se corresponde a $4$ binarios, entonces:

$$
\underbrace{1001}_{9}
\underbrace{1011}_{B}
\underbrace{1011}_{B}
=
\text{0x9BB}
$$

Esto concluye la conversión 1.

### Conversión #2

- $\text{B2CD}_{16}$ a binario

Con la misma observación de la parte anterior, concluimos que:

$$
\underbrace{\text{B}}_{1011}
\underbrace{2}_{0010}
\underbrace{\text{C}}_{1100}
\underbrace{\text{D}}_{1101}
=
1011001011001101\text{b}
$$

Esto concluye la conversión 2.

### Conversión #3

- $38726_9$ a base 3

Observemos que $9=3^2$, por lo tanto un dígito en base 9, se corresponde con 2 dígitos en base 3, entonces:

$$
\underbrace{3}_{10}
\underbrace{8}_{22}
\underbrace{7}_{21}
\underbrace{2}_{02}
\underbrace{6}_{20}
=
1022210220_3
$$

Esto concluye la conversión 3.

### Conversión #4

- $1101101001011_2$ a octal

Recordando que $8=2^3$, tenemos que un dígito hexadecimal se corresponde a $3$ binarios, entonces:

$$
\underbrace{1}_{1}
\underbrace{101}_{5}
\underbrace{101}_{5}
\underbrace{001}_{1}
\underbrace{011}_{3}
=
15513_8
$$

Esto concluye el ejercicio.