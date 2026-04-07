# 02:Memory_Control

*"Explore as One"*

---
**Kurs İçeriği:**
1. **Bellek Mimarisine Giriş**
2. **data Segmenti**
3. **Load/Store**
---
## 1. Bellek Mimarisine Giriş

Bir CPU, belleği iki yerde tutar:
1. Registerlar: CPU'nun hemen yanındaki çok hızlı ama sınırlı kapasiteli depolama alanlarıdır. MIPS mimarisinde /00-fundamentals kısmında bahsettiğimiz gibi yalnız 32 register vardır.
2. **RAM:** Çok daha geniş kapasitede yalnız registerlara göre daha yavaş olan alandır. Büyük veri yapıları burada tutulur.
3. CPU, RAM üzerinden doğrudan işlem yapamaz bu yüzden RAM'deki veri ilk Register'a *Load* işlemiyle yüklenir ve işlem burada yapıldıktan sonra RAM'e geri iade edilir.

## 2. data Segmenti

Genelde MIPS kodunun başında kullandığımız **.data** segmenti, yazılan direktifleri RAM'e yerleştirir. Bu direktifler:
1. **.word**: 4 byte yer ayırır (genelde integer değerler için)
2. **.byte**: 1 byte yer ayırır (genelde char değerler için)
3. **.asciiz**: Sonuna '\0' eklenmiş string dizileri saklar
4. **.space**: Belirli/istenilen miktarda yer ayırır.

```asm
.data
 age: .word 21        # int age = 25;
 name: .asciiz "Honda" # char name[] = "Honda"
 model: .byte 'U'  # char model = 'U'
```

---
## 3. Load/Store

Yukarıda **.data** kısmında bellekte oluşturduğumuz verileri işlemek için *register'a* **load**/yüklememiz gerekirken iade etmek için de **Store**/saklamalıyız.

- **la**: Bellekteki label'ı belirttiğimiz register'a kopyalar.
- **lw**: Bellekteki Store (4 Bytelık olan) veriyi türünden label'ı belirtilen register'a kopyalar.
- **lb**: Bellekten 1 Byte veri çeker.
- **sw**: 4 bytelık değeri değeri değiştirir.
- sb: 1 bytelık değeri değiştirir

- **Not:** `lw` ve `sw` kullanırken offset değerleri (0, 4, 8...) mutlaka 4'ün katı olmalıdır. Aksi halde MARS "Alignment Error" verir

```asm
.data
 veri: .word 10

.text
.globl _main
_main:
 # 1. Yazdırmak istediğimiz değişkeni $s0 registerına atayalım.
 li $s0, 100
 
 # 2. 'la' ile bellekteki 'val' isimli verinin adresini alır/işaretleriz.
 la $t0, veri
 
 # 3. $s0 içindeki 100 değerini $t0'ın işaret ettiği adrese yaz.
 # 0($t0): $t0 registerının işaretlediği yerin 0. offsete $s0 registerının
 # değişkenini yazdır. Aşağıda C++ örneğiyle açıklayacağım.
 sw $s0, 0($t0)
 
 # Programı Sonlandır.
 li $v0, 10
 syscall
```

**Örnek!** $t0 array olsaydı:
```C++
int arr[3] = {10, 20, 30}; 
int x = arr[1]; // C++ burada arka planda (Başlangıç + 1*4) işlemini yapar.
```

```asm
lw $s0, 4($t0) # $t0'daki adresten 4 byte ileri git ve veriyi oku.
```

**Özetle**
- `0($t0)` → Dizinin **0.** elemanı.
- `4($t0)` → Dizinin **1.** elemanı.
- `8($t0)` → Dizinin **2.** elemanı.

```asm
.data
    metin: .asciiz "Merhaba"  # M=0, e=1, r=2...

.text
main:
    la $t0, metin        # Metnin başlangıç adresini al
    li $t1, 'a'          # Yeni karakterimiz 'a'
    
    sb $t1, 1($t0)       # 1. offset'e (yani 2. karaktere) 'a' yaz
    
    # Ekranda "Marhaba" yazar
    li $v0, 4
    la $a0, metin
    syscall
	
	# Programı bitir.
	li $v0, 10
	syscall
```

