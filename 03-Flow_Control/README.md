# 03:Flow_Control

## 1. Label & Jump
### Label
 **Label**, bilgisayar için bir adresi işaret eder bu bizim için navigasyondur `j` ile atlanabilir ve oradan geri dönülebilir.
### Jump
Jump, assemblyde programın akışını bir yerden başka bir yere taşımak için kullanılır.
1. `j` (Jump)
	En basit atlama komutudur. Program akışını belirlenen labela gönderir ve geri dönme gibi derdi yoktur
```asm
main:
	#Kod Parçacığı
	j exit
exit:
	li $v0, 10
	syscall
```

2. `jal` & `jr` (Jump and Link & Jump Register)
	Genelde fonksiyon çağırmak için kullanılır. `j` komutundan farkı ise başka bir labela zıplamadan önce kendinden sonraki komutun adresini `$ra` adresine linkleyerek tutarak geri dönmemizi sağlama imkanı verir.
```asm
main:
	### Kod Parçacığı
	jal add
	j exit
add:
	### Kod Parçacığı
	jr $ra
exit:
	li $v0, 10
	syscall
```
3. `jalr` (Jump and Link Register)
	`jal` komutu gibi çalışır ama registera linklenen labelı kullanabilmemizi sağlar.
```asm
.data
    msg_fonk1: .asciiz "Fonksiyon 1 calisiyor!\n"
    msg_fonk2: .asciiz "Fonksiyon 2 calisiyor!\n"

.text
main:
    # Senaryo: Kullanıcıdan bir seçim aldığımızı varsayalım.
    # Burada doğrudan 'fonk2'nin adresini bir register'a yüklüyoruz.
    la $t0, fonk2       # $t0 register'ına fonk2'nin bellek adresini yükle

    # jalr Kullanımı:
    jalr $t0            # $t0 içindeki adrese git ve geri dönüş adresini $ra'ya yaz.
    
    # Program buradan devam edecek
    li $v0, 10
    syscall

# --- Fonksiyon Tanımlamaları ---

fonk1:
    li $v0, 4
    la $a0, msg_fonk1
    syscall
    jr $ra              # Ana programa geri dön

fonk2:
    li $v0, 4
    la $a0, msg_fonk2
    syscall
    jr $ra              # Ana programa geri dön
```

## 2. Temel Karşılaştırma İfadeleri

### beq & bne
`beq`, ifadesi iki registerda tutulan değerlerin birbirine eşitse bizi istenilen jumplar; `bne`, ifadesi bize eşit olmadığı durumda jumplar.
> **Sözdizimi:** `beq ${register1}, ${register2}, {label}`

### slt & slti
SLT, MIPS mimarisinde karşılaştırma yapmak ve karar mekanizmaları kurmak için kullanılır.
> **Sözdizimi:** slt {*Hedef*}, {*Kaynak1*}, {*Kaynak2*}

## Döngüler
Assembly'de C/C++ Java Python gibi kullandığımız şekilde While gibi döngüler içermez bunları  jump ve karşılaştırma ifadeleri gibi ifadelerle oluşturabiliriz. Örnek vermek gerekirse:
```asm
.text
main:
    li $t0, 0          # i = 0 (Sayaç)
    li $t1, 5          # Limit = 5

loop_control:
    # Eğer i == 5 ise döngüden çık
    beq $t0, $t1, loop_exit 
    
    # --- Döngü Gövdesi ---
    # (Burada yapmak istediğin işlemleri yap, örn: ekrana yazdır)
    
    addi $t0, $t0, 1   # i++ (Sayacı artır)
    j while_kontrol    # Başa dön ve koşulu tekrar kontrol et

loop_exit:
    # Döngü bitti, buradan devam et...
    li $v0, 10
    syscall
```
	