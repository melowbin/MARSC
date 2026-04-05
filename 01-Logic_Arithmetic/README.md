# Module:01 Logic Arithmetic 

## Basic Arithmetic
MARS, Mips assemblyde registerlar'da değerlerde aritmetik işlemler şöyledir

- **add;** add, iki registerı toplamak için kullanılan bir instructiondır.

```asm
.text
main:
	add $s0, $t1, $t2 # $s0 = $t1 + $t2
```

- **sub;** sub, iki registerı çıkarmak için kullanılan bir instructiondır.
```asm
.text
main:
	sub $s0, $t1, $t2 # $s0 = $t1 - $t2
```

- **addi;** addi, registera bir sabit sayı değeri eklemek için kullanılan bir instructiondır.
```asm
.text
main:
	addi $s0, $t1, 10 # $s0 = $t1 + 10
```

- **mull ve div;** bu işlemlere bu kursumda bakmak istemiyorum özellikle div **HI** ve **LO** içerdiği için Docs'taki yardımcı kaynaklardan bakmak daha iyi olacaktır.

## Basic Logic & Shifts
MIPS assemblyde, her assemblyde olduğu gibi mantıksal ve shift işlemleri bulunur

### Logic
### AND Gate 

| p   | q   | p & q |
| --- | --- | ----- |
| 1   | 1   | 1     |
| 1   | 0   | 0     |
| 0   | 1   | 0     |
| 0   | 0   | 0     |

```asm
.text
main:
	and $s0, $t1 $t2
```
Bu assembly kodu şuna tekabül eder: 
```C++
storage0 = temporary1 & temporary2
```
yani
```
00101011 (binary)
10010010 (binary) &
-------------------
00000010 (binary)
```

### OR Gate 

| p   | q   | p \| q |
| --- | --- | ------ |
| 1   | 1   | 1      |
| 1   | 0   | 1      |
| 0   | 1   | 1      |
| 0   | 0   | 0      |

```asm
.text
main:
	or $s0, $t1, $t2
```
Bu assembly kodu C++'ta şuna tekabüldür:
```C++
storage0 = temporary1 | temporary2
```
yani
```
t1 = 0001 (binary)
t2 = 0110 (binary) |
---------------------
t3 = 0111 (binary)
```

### XOR Gate

|  p  |  q  | p ^ q<br> |
| :-: | :-: | :-------: |
|  1  |  1  |     0     |
|  1  |  0  |     1     |
|  0  |  1  |     1     |
|  0  |  0  |     0     |
- Assembly'de XOR Gate kullanmak için
```asm
xor $s0 , $t1, $t2
``` 
- C++'ta karşılığı ise
```C++
storage0 = tempVal1 ^ tempVal2 
```
### NOR Gate
- NOR Gate OR Gate'nin değilidir yani NOR, Or gateinden oluşturulmuştur.

| p   | q   | ~(p \| q) |
| --- | --- | --------- |
| 1   | 1   | 0         |
| 1   | 0   | 0         |
| 0   | 1   | 0         |
| 0   | 0   | 1         |

### Shifts
- SLL & SLA
- SRL & SRA
### Comprasion
- SLT

