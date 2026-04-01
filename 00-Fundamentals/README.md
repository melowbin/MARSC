# Modül 00: Fundementals

MIPS mimarisinde kod yazmaya başlamadan önce, işlemcinin veriyi nasıl işlediğini ve RAM'i nasıl kullandığını anlamak gerekir.

---

## 1. Registers
Registers, işlemcinin (CPU) içinde bulunan çok hızlı veri depolama birimleridir. İşlemci tüm hesaplamaları bunlar üzerinde yapar.

* **`$v0 - $v1`:** **Syscall** kodlarını göndermek ve sonuçları almak için kullanılır.
* **`$a0 - $a3`:** **Syscall** veya fonksiyonlara gönderilecek verileri (argümanları) tutar.
* **`$t0 - $t9` (Temporary):** Geçici hesaplamalar için kullanılır.
* **`$s0 - $s7` (Saved):** Kalıcı olması gereken önemli verileri tutar.
* **`$sp` (Stack Pointer):** RAM'deki **Stack** bölgesinin yerini gösterir.

---

## 2. Syscalls (Sistem Çağrıları)
**Syscall**, hazırladığınız kodun işletim sisteminden bir işlem (ekrana yazı yazdırma, sayı okuma vb.) talep etmesidir.

| İşlem | $v0 Kodu | $a0 İçeriği | Sonuç |
| :--- | :--- | :--- | :--- |
| **Print Integer** | 1 | Yazdırılacak sayı | Sayıyı ekrana basar |
| **Print String** | 4 | Metnin adresi | Metni ekrana basar |
| **Read Integer** | 5 | Yok | Girilen sayı `$v0`'a döner |
| **Exit** | 10 | Yok | Programı durdurur |

---

## 3. Bellek Segmentleri (Memory Segments)

### .text (Code)
* **Nedir:** Yazdığınız Assembly komutlarının saklandığı yerdir.
* **Özellik:** **Read-Only** (Salt Okunur). İşlemci buradan komutları okur ama kod çalışırken burayı değiştiremez.

### .data (Initialized Data)
* **Nedir:** Program başlamadan önce değeri belli olan değişkenler (metinler, sabit sayılar) içindir.
* **Örnek:** `mesaj: .asciiz "Merhaba"`

### .bss / .space (Uninitialized Data)
* **Nedir:** Henüz değeri olmayan, sadece RAM'de yer ayrılan alanlardır (Boş tamponlar/buffer).
* **Örnek:** `input: .space 20` (20 byte yer ayırır).

---

## Temel Kod Şablonu (MARS)

```mips
.data
    selam: .asciiz "Sistem Hazir.\n"  # .data kısmında metin tanımladık

.text
main:
    # Ekrana yazı yazdırma (Syscall 4)
    li $v0, 4            
    la $a0, selam        
    syscall              

    # Programı kapatma (Syscall 10)
    li $v0, 10           
    syscall