# 🖥️ TryHackMe — Data Representation | Azərbaycanca Oxuma Materialı

> **Room:** https://tryhackme.com/room/datarepresentation
> **Path:** Pre-Security
> **Mövzu:** Binary, Hexadecimal, Rənglər, Say Sistemləri

---

## 📖 Bu Room Nə Öyrədir?

Kompüterlər yalnız **0** və **1** ilə işləyir. Bəs biz gördüyümüz rənglər, rəqəmlər, hətta şifrə açarları — bunlar hamısı kompüterdə necə saxlanılır? Bu room məhz bunu izah edir. Kibertəhlükəsizlikdə binary və hexadecimal oxumağı bacarmaq **zəruridir** — çünki şəbəkə paketlərindən tutmuş exploit kodlarına qədər hər şey bu formatda görünür.

---

## 📚 Task 1 — Giriş: Bit və Byte nədir?

### Bit nədir?
**Bit** — "binary digit" sözlərinin qısaltmasıdır. Yalnız 2 dəyər ala bilər: **0** və ya **1**. Kompüterin anladığı ən kiçik məlumat vahididir. Düşün ki, bit bir elektrik düyməsidir — ya açıqdır (1), ya da bağlıdır (0).

### Byte nədir?
**Byte** — 8 bitin bir araya gəlməsindən ibarətdir. Buna **octet** də deyilir. Məsələn, `01000001` — bu 8 bit = 1 byte-dır. Bir byte ilə 0-dan 255-ə qədər istənilən ədədi ifadə etmək mümkündür.

```
1 bit  = 0 ya 1
8 bit  = 1 byte
         (256 fərqli dəyər: 0-dan 255-ə qədər)
```

---

## 🎨 Task 2 — Kompüterdə Rənglər (RGB və Hex)

### Rənglər necə işləyir?

Kompüter ekranı üç rəng işığından istifadə edir: **Qırmızı (Red)**, **Yaşıl (Green)**, **Mavi (Blue)** — buna **RGB** deyilir. Bu üç rəngi müxtəlif intensivlikdə qarışdırmaqla istənilən rəngi almaq olur. Məsələn TV ekranına yaxından baxsaydın, hər piksel bu üç rəngin kiçik nöqtələrindən ibarət olduğunu görərdin.

### Sadə izah: 3-bit sistem

Əgər hər rəng yalnız **açıq (1)** ya **bağlı (0)** ola bilsəydi:

```
R G B  →  Rəng
0 0 0  →  Qara (heç biri yoxdur)
1 0 0  →  Qırmızı
0 1 0  →  Yaşıl
0 0 1  →  Mavi
1 1 0  →  Sarı (qırmızı + yaşıl)
1 0 1  →  Magenta (qırmızı + mavi)
0 1 1  →  Cyan (yaşıl + mavi)
1 1 1  →  Ağ (hamısı var)
```

3 bit = 2³ = **8 rəng** — çox azdır.

### Real sistem: 24-bit rəng

Hər rəngə 1 byte (8 bit) verilərsə:
- Qırmızı: 0–255 arası intensivlik
- Yaşıl: 0–255 arası intensivlik
- Mavi: 0–255 arası intensivlik

```
3 rəng × 8 bit = 24 bit
24 bit = 2²⁴ = 16,777,216 rəng  →  16 milyondan çox!
```

### Hex rəng kodu nədir?

`#3BC81E` kimi rəng kodlarını görmüsən? Bu **hexadecimal (hex)** formatdır. Hər iki hərf 1 byte-ı ifadə edir:

```
#  3B   C8   1E
   ↑    ↑    ↑
   Red  Green Blue

3B = decimal 59   (qırmızı intensivliyi)
C8 = decimal 200  (yaşıl intensivliyi)  ← ən yüksək!
1E = decimal 30   (mavi intensivliyi)

Yaşıl dominant olduğuna görə bu rəng → yaşıldır 🟢
```

Niyə hex istifadə edilir? Çünki `3BC81E` yazmaq `00111011 11001000 00011110` yazmaqdan çox asandır — eyni məlumatı daha qısa ifadə edir.

---

## 🔢 Task 3 — Say Sistemləri

### İnsanlar niyə 10-lu sistem (Decimal) işlədər?

İnsanların 10 barmağı var — bu səbəbdən gündəlik həyatda 0-dan 9-a qədər 10 rəqəm istifadə edirik. Buna **decimal** (onlu) sistem deyilir. Rəqəmlər tükənəndə sol tərəfə bir sütun əlavə edirik: 9 → 10, 99 → 100 və s.

### Kompüterlər niyə 2-li sistem (Binary) işlədər?

Kompüterin içindəki tranzistorlar yalnız iki vəziyyətdə ola bilər: cərəyan var (1) ya yoxdur (0). Ona görə kompüterlər **binary (ikili)** sistemlə işləyir. Hər şey — rənglər, hərflər, şəkillər, şifrələr — 0 və 1-lərin kombinasiyasıdır.

---

## 🔄 Binary → Decimal Çevirmə

Bu, bu roomun ən vacib hissəsidir. Mütləq anlamalısan.

### Prinsip: Hər bitin dəyəri var

Binary-də hər bit mövqeyinin bir **gücü** var. Sağdan sola doğru 2⁰, 2¹, 2², 2³... kimi artır.

```
Bit mövqeyi:   7    6    5    4    3    2    1    0
Dəyəri:       128   64   32   16    8    4    2    1
```

### Necə çeviririk?

`10110010` binary-ni decimal-a çevir:

```
Bit:    1    0    1    1    0    0    1    0
Dəyər: 128   64   32   16    8    4    2    1

1 × 128 = 128  ✅
0 × 64  = 0    ❌
1 × 32  = 32   ✅
1 × 16  = 16   ✅
0 × 8   = 0    ❌
0 × 4   = 0    ❌
1 × 2   = 2    ✅
0 × 1   = 0    ❌

Cəm: 128 + 32 + 16 + 2 = 178
```

Yəni `10110010` (binary) = `178` (decimal).

### Daha sadə nümunə — 4 bitlik:

```
1100 = ?
1 × 8 = 8
1 × 4 = 4
0 × 2 = 0
0 × 1 = 0
= 12

1101 = 8+4+0+1 = 13
1110 = 8+4+2+0 = 14
1111 = 8+4+2+1 = 15
```

---

## 🔢 Task 4 — Hexadecimal (16-lı Sistem)

### Hex nədir?

Hexadecimal — **16 simvoldan** ibarət say sistemidir. 0-dan 9-a qədər adi rəqəmlər, sonra A-dan F-ə qədər hərflər:

```
Decimal:  0  1  2  3  4  5  6  7  8  9  10  11  12  13  14  15
Hex:      0  1  2  3  4  5  6  7  8  9   A   B   C   D   E   F
```

### Niyə hex lazımdır?

Binary-dəki uzun 0 və 1 sıraları oxumaq çox çətindir. Hex bunu qısaldır. **4 bit = 1 hex rəqəmi.**

```
Binary:  1010  1011
Hex:       A    B
= AB

Bütün 8 biti yazmaq əvəzinə 2 hərf yazırıq — daha rahat!
```

### Hex → Decimal Çevirmə

`AB` hex-i decimal-a çevir:

```
A = 10 (decimal)
B = 11 (decimal)

AB = (A × 16¹) + (B × 16⁰)
   = (10 × 16) + (11 × 1)
   = 160 + 11
   = 171
```

Yəni `AB` (hex) = `171` (decimal).

### Praktiki nümunə: `FF FF FF` nədir?

```
FF = 15×16 + 15 = 255 (decimal)

FF FF FF = (255, 255, 255) RGB
= Maksimum qırmızı + Maksimum yaşıl + Maksimum mavi
= Ağ rəng 🤍

255 × 255 × 255 ≈ 16,581,375 ≈ 17 milyon (ən yaxın milyona yuvarlaqlaşdırsaq)
```

---

## 🔢 Task 5 — Octal (8-li Sistem)

Octal sistemi 0-dan 7-ə qədər 8 rəqəm işlədər. **3 bit = 1 octal rəqəmi.**

```
Binary:  000  001  010  011  100  101  110  111
Octal:    0    1    2    3    4    5    6    7
```

Octal günümüzdə o qədər geniş istifadə edilmir, amma Linux fayl icazələrini (`chmod 755`) octal ilə yazırıq — bu sistemdə görəcəksən.

```
chmod 755:
7 = 111 binary = oxu+yaz+icra (sahibi)
5 = 101 binary = oxu+icra     (qrup)
5 = 101 binary = oxu+icra     (digər)
```

---

## 📊 Bütün Sistemlər Bir Yerdə

| Decimal | Binary | Hex | Octal |
|---------|--------|-----|-------|
| 0 | 0000 | 0 | 0 |
| 1 | 0001 | 1 | 1 |
| 2 | 0010 | 2 | 2 |
| 5 | 0101 | 5 | 5 |
| 8 | 1000 | 8 | 10 |
| 10 | 1010 | A | 12 |
| 15 | 1111 | F | 17 |
| 16 | 00010000 | 10 | 20 |
| 171 | 10101011 | AB | 253 |
| 255 | 11111111 | FF | 377 |

---

## 🎯 Room Sualları və İzahları

### Task 2 — Rəng sualları:

**Sual: `#3BC81E` rəngi bir sözlə nədir?**
```
3B = 59  (az qırmızı)
C8 = 200 (çox yaşıl)  ← dominant
1E = 30  (az mavi)
→ Cavab: green (yaşıl)
```

**Sual: `#EB0037` rənginin binary təsviri nədir?**
```
EB = 1110 1011  (qırmızı)
00 = 0000 0000  (yaşıl)
37 = 0011 0111  (mavi)

→ 11101011 00000000 00110111
```

**Sual: `#D4D8DF` rənginin decimal təsviri nədir?**
```
D4 = 212  (qırmızı)
D8 = 216  (yaşıl)
DF = 223  (mavi)

→ (212, 216, 223)
```

### Task 3 — Decimal çevirmə:

**Binary `1100` = ?**
```
8+4+0+0 = 12
```

**Binary `1111` = ?**
```
8+4+2+1 = 15
```

### Task 4 — Hex çevirmə:

**`AB` hex = ? decimal**
```
(10×16) + (11×1) = 160+11 = 171
```

**`FF FF FF` → neçə milyon rəng?**
```
FF = 255
255×255×255 = 16,581,375 ≈ 17 milyon
```

---

## 🔐 Kibertəhlükəsizlikdə Niyə Vacibdir?

Bu bilikləri niyə öyrənirik? Çünki real kibertəhlükəsizlik işlərində hər yerdə rast gəlinir:

| Yer | Nümunə |
|-----|--------|
| **Şəbəkə trafiqi** | Wireshark-da paketlər hex formatında görünür |
| **Exploit kodları** | Shellcode: `\xeb\x1f\x5e\x89\x76\x08` — bunlar hex byte-lardır |
| **Faylların analizi** | Hex editor ilə fayl başlıqları (magic bytes) yoxlanılır |
| **Kriptografiya** | Şifrə açarları, hash-lər hex formatında göstərilir |
| **Steganography** | Gizli məlumatlar binary səviyyəsində faylın içinə yerləşdirilir |
| **CTF** | Əksər çağırışlarda binary/hex çevirmə lazım olur |

---

## ✅ Öyrəndiklərinin Siyahısı

Bu roomu bitirdikdən sonra bunları bacarmalısan:

- [ ] Bit və byte-ın nə olduğunu izah etmək
- [ ] RGB rəng sistemini anlamaq
- [ ] Hex rəng kodunu (`#RRGGBB`) oxumaq
- [ ] Binary → Decimal çevirmə (mövqe dəyərləri ilə)
- [ ] Hex rəqəmlərini (0-F) tanımaq
- [ ] Hex → Decimal çevirmə
- [ ] 4 bit = 1 hex rəqəmi əlaqəsini bilmək
- [ ] Octal-ın nə olduğunu bilmək (Linux icazələri)

---

## 🧮 Tez Çevirmə Qaydaları

```
Binary → Decimal:
  Sağdan sola: 1, 2, 4, 8, 16, 32, 64, 128
  1 olan bitlərin dəyərlərini cəmlə

Decimal → Binary:
  Ən böyük 2-nin gücündən başla, çıx, növbətisinə keç

Hex → Decimal:
  Hər rəqəmi decimal-a çevir (A=10...F=15)
  Sol rəqəm × 16 + Sağ rəqəm

Binary → Hex:
  4 bitlik qruplara böl, hər qrupu hex rəqəminə çevir
  1010 1111 → A F → AF

Hex → Binary:
  Hər hex rəqəmini 4 bitə çevir
  B = 1011, 3 = 0011 → B3 = 10110011
```

---

*Hazırlandı: 2026 | Platforma: TryHackMe Pre-Security Path | Azərbaycanca*
