# Lab 1:

## 1. Odkaz na GitHub
https://github.com/xtomes07/Digital_elektronics_1/

## 2. Ověření De Morganových zákonů

### Výpis VHDL kódu:
```vhdl
f_o 	<= ((not b_i) and a_i) or ((not c_i) and (not b_i));
fnand_o	<= ((not b_i) nand a_i)nand((not c_i) nand (not b_i));
fnor_o  <= (b_i nor (not a_i)) or (c_i nor b_i);

```
### Tabulka logických hodnot

| **c** | **b** |**a** | **f(c,b,a)** | **fnand(c,b,a)** | **fnor(c,b,a)** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 1 | 1 | 1 |
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 |


### Časové průběhy
![grafy](https://github.com/xtomes07/Digital_elektronics_1/blob/main/Labs/01-gates/Obrazky/screenDeMorgan.PNG)

### EDA odkaz
https://www.edaplayground.com/x/ftZQ/

## 3. Ověření Distributivních zákonů

### Výpis VHDL kódu:
```vhdl
f1_o  <= x_i and (y_i or z_i);
f2_o  <=(x_i and y_i) or (x_i and z_i);
f3_o  <= x_i or (y_i and z_i);
f4_o  <= (x_i or y_i) and (x_i or z_i);
```

### Tabulka logických hodnot

| **z** | **y** |**x** | **f1(z,y,x)** | **f2(z,y,x)**  | **f3(z,y,x)**  | **f4(z,y,x)** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 |

### EDA odkaz
https://www.edaplayground.com/x/jqFR


