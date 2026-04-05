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
#### AND Gate 
- **AND Gate**, matematikten aşina olduğumuz ve bağlacıdır. Bu bağlaç binary sistemde çarpma işlemi gibi sayılır eğer bu bağlaçta herhangi bir bağlam 0'sa sonuç da kesinlikle 0'dır.

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
0011 (t1)
1001 (t2) 
& ------ (AND)
0001
```

#### OR Gate 
- **OR Gate**, matematikten aşina olduğumuz veya bağlacıdır. Bu bağlaçta herhangi bir bağlam 1 ise sonuç kesinlikle 1'dir.

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
0001 (t1)
0110 (t2)
| ------ (OR)
0111 (t3)
```

#### XOR Gate
- **XOR Gate** ya da **eXclusive OR**, matematikten aşina olduğumuz 'ya da' bağlacıdır. S

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

```
0110 (t2) 
0111 (t3) 
^ ----- (XOR) 
0001 (t1)
```
#### NOR Gate
- **NOR Gate OR** Gate'nin değilidir yani NOR, Or gateinden oluşturulmuştur.

| p   | q   | ~(p \| q) |
| --- | --- | --------- |
| 1   | 1   | 0         |
| 1   | 0   | 0         |
| 0   | 1   | 0         |
| 0   | 0   | 1         |
```
0110 (t2)
0111 (t3)
~| ---- (NOR)
1000
```
###  Bit Kayırmak
#### SLL & SLA
- SLL (Shift Left Logical), register içerisindeki bit dizinini belirten miktarda sola kaydırır. Bunu yaparken: 
	1. En soldaki bitler "dışarı taşar" ve silinir.
	2. En sağda oluşan boşluklara ise **"0"** yerleşir.
```0111``` bitini 2 adım SLL yaparsak sonuç ```1100``` olur. 
- Bunu MARS'ta şöyle yapabiliriz:
```asm
sll $t0, $t1, 1
```
- SLA (Shift Left Arithmetic) ise Mips mimarisinde geçerli bir komt değildir. Her SLL adımı zaten bize bitlerin binary dilindeki karşılığından dolayı registerda tutulan verimizi 2 katına çıkarır.
### Comprasion
#### SLT (Set on Less Than)
- SLT, MIPS mimarisinde karşılaştırma yapmak ve karar mekanizmaları kurmak için kullanılır.

1. **Komutun Yazımı ve Mantığı**
```asm
slt {*Hedef*}, {*Kaynak1*}, {*Kaynak2*}
```

2. **Örnek Kullanım**
```asm
li $t1, 5
li $t2, 3

slt $s0, t1, t2 # $t1 registerındaki değer $t2 registerındakinden büyük mü?
# Eğer $t1 registerındaki değer $t2 registerındaki değerden büyük ise $s0 -> 1,
# Değilse $s0 değeri 0 olur.
```
