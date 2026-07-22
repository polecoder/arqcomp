# Conversión a base 10 con decimales

**Fecha:** 22-07-2026

## Consigna

Convertir a base 10 los siguientes números:

1. $1001.11_2$
2. $\text{A7.A7}_{16}$
3. $\text{A7.B8}_{13}$

## Resolución

### Conversión #1

- $1001.11_2$

Para los números con decimales, separamos en parte entera y parte fraccionaria, para luego sumar lo obtenido y concluir el resultado final.

#### Parte entera

- $1001$

Como los ejercicios anteriores, desarrollamos con la forma polinómica.

$$
1001_2=1+2^3=9
$$

#### Parte fraccionaria

- $0.11_2$

Para esto recordamos el teórico, necesitamos sustituir $x=\frac{1}{b}$ donde $b$ es la base, en el polinomio característico.

$$
\begin{aligned}
&P(x)=x+x^2\\
&\iff\scriptstyle{(x=1/2)}\\
&P(1/2)=1/2+1/4\\
&\iff\scriptstyle{(\text{operatoria})}\\
&P(1/2)=\frac{3}{4}\\
&\iff\scriptstyle{(\text{operatoria})}\\
&P(1/2)=0.75\\
\end{aligned}
$$

#### Resultado

El resultado obtenido es sumar las dos partes anteriores, por lo tanto:

$$
1001_2=9.75
$$

Esto concluye la primer conversión.

---

### Conversión #2

- $\text{A7.A7}_{16}$

Separamos en parte entera y parte fraccionaria, para luego sumar lo obtenido y concluir el resultado final.

#### Parte entera

- $\text{A7}_{16}$

Como los ejercicios anteriores, desarrollamos con la forma polinómica.

$$
A7_{16}=7+10\cdot16=167
$$

#### Parte fraccionaria

- $\text{0.A7}_{16}$

Sustituimos $1/16$ en el polinomio característico:

$$
\begin{aligned}
&P(x)=10x+7x^2\\
&\iff\scriptstyle{(x=1/16)}\\
&P(1/16)=10(1/16)+7(1/16)^2\\
&\iff\scriptstyle{(\text{operatoria})}\\
&P(1/16)\approx0.65
\end{aligned}
$$

#### Resultado

El resultado obtenido es sumar las dos partes anteriores, por lo tanto:

$$
\text{A7.A7}_{16}\approx167.65
$$

Esto concluye la segunda conversión.

---

### Conversión #3

- $\text{A7.B8}_{13}$

Separamos en parte entera y parte fraccionaria, para luego sumar lo obtenido y concluir el resultado final.

#### Parte entera

- $\text{A7}_{13}$

Como los ejercicios anteriores, desarrollamos con la forma polinómica.

$$
A7_{13}=7+10\cdot13=137
$$

#### Parte fraccionaria

- $\text{0.B8}_{13}$

Sustituimos $1/13$ en el polinomio característico:

$$
\begin{aligned}
&P(x)=11x+8x^2\\
&\iff\scriptstyle{(x=1/13)}\\
&P(1/13)=11(1/13)+8(1/13)^2\\
&\iff\scriptstyle{(\text{operatoria})}\\
&P(1/13)\approx0.89
\end{aligned}
$$

#### Resultado

El resultado obtenido es sumar las dos partes anteriores, por lo tanto:

$$
\text{A7.B8}_{13}\approx137.89
$$

Esto concluye el ejercicio.