# 03:Flow_Control
---
**Kurs İçeriği:**
1. Temel Karşılaştırma
2. Karar Yapıları
3. While Döngüsü
4. For Döngüsü
5. Kompleks Yapılar
---
## 1. Temel Karşılaştırma

### beq (Branch if Equal)

- **beq**, iki register'ı karşılaştırıp eşit ise belirlenen **label**'a götürür.
```asm
beq {karşılaştırılan register}, {karşılaştıran register}, {belirlenen label}
```

- Örnek vermek gerekirse:
```asm
.data
	esit: .asciiz "iki degisken birbirine esit"
	esit_degil: .asciiz "iki degisken birbirine esit degil"

.text
main:
	li $t0, 10
	li $t1, 10
	beq $t0, $t1, equal
	# Eğer $t0 registerı $t1 registerına
	# denk değilse bu label'ın gerisinde
	# kalan kod parçacığı çalışır.
	li $v0, 4
	la $a0, esit_degil
	
	j exit
	
equal:
	li $v0, 4
	la $a0, esit
	syscall
	
	j exit

exit:
	li $v0, 10
	syscall
```
### bne (Branch if Not Equal)

- **bne**, beq'in tersidir eğer iki register birbirine eşit değişse belirtilen **label**'a gider.
```asm
bne {karşılaştırılan register}, {karşılaştıran register}, {belirlenen label}
```

- Örnek verecek olursak:
```asm
.data
	esit: .asciiz "iki degisken birbirine esit"
	esit_degil: .asciiz "iki degisken birbirine esit degil"

.text
main:
	li $t0, 10
	li $t1, 10
	bne $t0, $t1, not eual
	# Eğer $t0 registerı $t1 registerına
	# eşitse bu label'ın gerisinde
	# kalan kod parçacığı çalışır.
	li $v0, 4
	la $a0, esit
	
	j exit  
	
not_equal:
	li $v0, 4
	la $a0, esit_degil
	syscall
	
	j exit

exit:
	li $v0, 10
	syscall
```
### slt/slti (Set on Less Than)
### j (Jump)

