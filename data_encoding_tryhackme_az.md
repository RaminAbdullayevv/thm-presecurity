# 🖥️ TryHackMe — Data Encoding | Azərbaycanca Oxuma Materialı

> **Room:** https://tryhackme.com/room/dataencoding
> **Path:** Pre-Security
> **Əvvəlki room:** Data Representation (binary, hex, rənglər)

---

## Başlanğıc: Hərflər kompüterdə necə saxlanılır?

Əvvəlki roomda öyrəndik ki, kompüter yalnız **0** və **1** ilə işləyir. Rəqəmləri binary-ə çevirdik, rəngləri hex ilə ifadə etdik. Amma bir sual qaldı: bəs **hərflər, nöqtələr, emojilər** — bunlar kompüterdə necə saxlanılır?

Cavab sadədir: **hər hərf bir rəqəmə uyğun gəlir.** Kompüter `01000001` görəndə bunu `A` kimi göstərir — çünki ikisi arasında bir **razılaşma (encoding)** mövcuddur. Bu razılaşma olmasaydı, bir kompüterin yazdığı mətn başqasında saçma-sapan simvollar kimi görünərdi. (Bəlkə bir faylı açanda belə bir şey görmüsən — məhz encoding fərqliliyi buna səbəb olur.)

---

## ASCII — İlk Standart

**ASCII** — *American Standard Code for Information Interchange* — 1963-cü ildə yaradılmış ilk geniş yayılmış encoding standartıdır. İngilis dilindəki hərflər, rəqəmlər, durğu işarələri və bəzi xüsusi simvolları rəqəmlərlə uyğunlaşdırır.

ASCII **7-bit** standartdır — yəni hər simvol üçün 7 bit istifadə edir. 7 bit ilə `2⁷ = 128` fərqli dəyər ifadə etmək mümkündür. Buna görə ASCII cədvəlində **0-dan 127-ə qədər** 128 simvol var.

Bu 128 simvol iki qrupa bölünür:

**1. İdarəetmə simvolları (Control Characters) — 0-dan 31-ə qədər:**
Bunlar ekranda görünmür, amma sistemə əmr verir. Məsələn:
- `7` → BEL (Zəng səsi — köhnə terminallar zəng çalardı)
- `8` → Backspace (bir geri sil)
- `10` → Line Feed / `\n` (yeni sətir)
- `13` → Carriage Return / `\r` (sətrə qayıt)
- `27` → ESC (Escape)

**2. Çap olunan simvollar (Printable Characters) — 32-dən 126-ya qədər:**
Bunlar klaviaturada gördüyün bütün simvollardır:

```
32       → Boşluq (space)
48–57    → '0' – '9'  (rəqəmlər)
65–90    → 'A' – 'Z'  (böyük hərflər)
97–122   → 'a' – 'z'  (kiçik hərflər)
33–47    → !, ", #, $, %, &, ...  (xüsusi simvollar)
```

Vacib bir nüans: böyük `A` = 65, kiçik `a` = 97. Fərq **32**-dir. Bu qanunauyğunluq bütün hərflər üçün eynidir. Yəni `B`=66, `b`=98; `C`=67, `c`=99 və s. Bunu bilmək CTF-lərdə çox kömək edir.

### "TryHackMe" faylda necə saxlanılır?

Bir mətn faylı yaradıb "TryHackMe" yazsaydın, disk üzərində bu bitlər saxlanılardı:

```
T        r        y        H        a        c        k        M        e
01010100 01110010 01111001 01001000 01100001 01100011 01101011 01001101 01100101
```

Bu bitləri hex ilə yazmaq daha rahatdır:
```
54  72  79  48  61  63  6B  4D  65
```

Faylı açanda text editor bu hex dəyərlərini oxuyur, ASCII cədvəlinə baxır və ekranda `TryHackMe` göstərir.

---

## ASCII-nin Problemi — Yalnız İngilis Dili

ASCII 1963-cü ildə **Amerika**da yaradılıb. O dövrün məqsədi İngilis dilini kompüterdə ifadə etmək idi — buna da nail oldu. Amma dünya yalnız İngilis dilindən ibarət deyil.

İspancada `ñ`, `¿` var. Almancada `ß`, `ü`, `ö` var. Rusca kiril hərfləri, ərəbcə, yaponcа, çincə — bunların heç biri ASCII-də yoxdur. 128 simvol belə dilləri əhatə etmək üçün çox azdır.

---

## Extended ASCII — Bandajlı Həll

Bəzi insanlar belə fikirləşdi: ASCII 7-bit işləyir, amma müasir kompüterlər 8-bit (1 byte) istifadə edir. O artıq bit ilə 128 əlavə simvol əlavə etmək olar — beləliklə 256 simvola çatmaq mümkündür.

Bu fikir **Extended ASCII**-ni doğurdu. Amma böyük bir problem yarandı: **hər ölkə 128-255 arasını özünə görə doldurdu.**

Beynəlxalq standartlar qurumu **ISO/IEC 8859** seriyası altında müxtəlif standartlar yaratdı:

```
ISO-8859-1  (Latin-1)  → Qərbi Avropa: Alman, Fransız, İspan, Portuqal...
ISO-8859-2  (Latin-2)  → Mərkəzi Avropa: Polyak, Çex, Macar...
ISO-8859-5             → Kiril (Rus, Bolqar...)
ISO-8859-6             → Ərəb
ISO-8859-7             → Yunan
...
```

**Problem:** Əgər Alman biri `Ø` hərfini ISO-8859-1 ilə saxlayıb, Polyak biri həmin faylı ISO-8859-2 ilə açarsa — `Ø` əvəzinə `Ř` görünür. Eyni bit ardıcıllığı fərqli encoding-lərdə fərqli simvol mənasına gəlir. Bu, tam bir xaos idi.

Aydın idi ki, köklü bir həll lazımdır.

---

## Unicode — Universal Həll

**Unicode** — dünyadakı bütün dillərin, simvolların, emojilerin vahid bir sistemdə ifadə edilməsi üçün yaradılmış universaul standartdır. Hər simvola unikal bir **kod nöqtəsi (code point)** verilir — bu kod nöqtəsi `U+` prefiksi ilə yazılır.

Məsələn:
```
U+0041  →  A  (böyük İngilis A)
U+0061  →  a  (kiçik İngilis a)
U+03A9  →  Ω  (Yunan Omega)
U+062A  →  ت  (Ərəb "taa" hərfi)
U+30C4  →  ツ  (Yapon "tsu" hərfi)
U+265E  →  ♞  (Şahmat qara at)
U+1F525 →  🔥  (Alov emoji)
U+1F60C →  😌  (Rahat üz emoji)
```

Unicode-un ən son versiyasında (v17.0) **157,000-dən çox** simvol var. Bütün dünya dillərini, tarixi yazıları, emojileri, riyazi işarələri əhatə edir. 0-127 arası ASCII ilə tam uyğundur — yəni köhnə ASCII mətnlər problemsiz işləyir.

---

## UTF — Unicode-u Faylda Necə Saxlayaq?

Unicode hər simvola bir kod nöqtəsi verir, amma bu kod nöqtəsini **faylda necə saxlayacağını** müəyyən etmir. Bunun üçün **UTF (Unicode Transformation Format)** encoding-ləri yaradılıb. Üç əsas variant var:

---

### UTF-32 — Ən Sadə, Ən İsrafçı

UTF-32 hər simvol üçün **sabit 4 byte (32 bit)** istifadə edir. Çox sadədir — hər simvol eyni ölçüdədir, hesablamaq asandır. Amma ciddi bir problemi var: İngilis mətni yazmaq üçün hər hərf üçün 4 byte lazımdır, yalnız 1 byte lazım olduğu halda.

```
A hərfi:
ASCII-də:  01000001                    → 1 byte
UTF-32-də: 00000000 00000000 00000000 01000001  → 4 byte (3 byte boşdur!)

🔥 emoji:
UTF-32-də: 00000001 11110101 00100101  → 4 byte (tam istifadə olunur)
```

UTF-32 adətən müəyyən daxili sistemlərdə istifadə olunur, internetdə nadir görünür.

---

### UTF-16 — Orta Yol

UTF-16 simvollara görə **2 ya 4 byte** istifadə edir:
- Adi simvollar (latın, kiril, çincə, yaponca kimi gündəlik istifadə olunanlar) → **2 byte**
- Nadir simvollar və emojilər → **4 byte** (surrogate pair adlanan cütlük)

```
A      → 2 byte
シ (ツ)→ 2 byte  (U+30C4, adi simvol)
🔥     → 4 byte  (nadir, surrogate pair lazımdır)
```

Windows daxili olaraq UTF-16 istifadə edir. Java da UTF-16 əsaslıdır.

---

### UTF-8 — Ən Geniş Yayılmış

UTF-8 simvollara görə **1-dən 4 byte-a qədər** dinamik olaraq istifadə edir. Bu onu həm yüngül, həm universal edir.

```
A (U+0041)      → 1 byte   (ASCII ilə eyni!)
é (U+00E9)      → 2 byte   (Fransız aksanlı e)
Ω (U+03A9)      → 2 byte   (Yunan Omega)
ت (U+062A)      → 2 byte   (Ərəb hərfi)
あ (U+3042)     → 3 byte   (Yapon hiragana)
🔥 (U+1F525)    → 4 byte   (emoji)
```

**Niyə UTF-8 bu qədər populyardır?**

Birincisi, ASCII ilə **tam geriyə uyğundur** — 0-127 arası bütün ASCII simvolları UTF-8-də eyni 1 byte ilə ifadə olunur. Yəni köhnə ASCII ilə yazılmış bütün mətnlər UTF-8-də problemsiz oxunur.

İkincisi, İngilis dili ağırlıqlı mətnlər üçün **çox yüngüldür** — hər hərf 1 byte. UTF-32 ilə müqayisədə 4 dəfə az yer tutur.

Üçüncüsü, bütün Unicode simvollarını əhatə edir — yəni istənilən dildə istənilən simvolu ifadə edə bilər.

Bu üstünlüklər sayəsində **internetdəki veb səhifələrin 98%-dən çoxu UTF-8 istifadə edir.**

---

## Müqayisə Cədvəli

| | ASCII | Extended ASCII | UTF-8 | UTF-16 | UTF-32 |
|--|-------|---------------|-------|--------|--------|
| **Bit sayı** | 7-bit | 8-bit | 1-4 byte | 2-4 byte | 4 byte (sabit) |
| **Simvol sayı** | 128 | 256 | 1,000,000+ | 1,000,000+ | 1,000,000+ |
| **Dil dəstəyi** | Yalnız İngilis | Məhdud | Bütün dillər | Bütün dillər | Bütün dillər |
| **ASCII uyğunluğu** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **Populyarlıq** | Köhnə | Köhnə | İnternetin standartı | Windows, Java | Daxili sistemlər |

---

## Kibertəhlükəsizlikdə Encoding Niyə Vacibdir?

Bu mövzu sadəcə nəzəri deyil — real hücumlarda encoding anlayışı çox işlədilir:

**Path Traversal hücumu:** Hücumçu `../` simvolunu URL encoding ilə gizlədir — `%2e%2e%2f` yazır. Zəif filtr `../` axtarır amma `%2e%2e%2f` görür, keçir buraxır. Server isə decode edib `../` kimi oxuyur.

**XSS hücumu:** HTML xüsusi simvolları encoding ilə gizlədib filter-ləri keçmək.

**Gibberish fayllar:** Müxtəlif encoding-lərlə saxlanmış faylların açılması — forensics-də hansı encoding istifadə olunub anlamaq lazım gəlir.

**CTF-lərdə:** Gizli məlumatlar çox vaxt Base64 (başqa bir encoding növü), hex, ya ASCII kodları şəklinde verilir.

---

## Xülasə

Kompüterdə hər şey 0 və 1-dir. Hərfləri ifadə etmək üçün **encoding** — rəqəm-simvol razılaşması — lazımdır.

**ASCII** 1963-ci ildə yaradıldı, 128 simvol, yalnız İngilis dili. **Extended ASCII** 256 simvola çatdı amma standartlaşmadı — hər ölkə öz versiyasını yaratdı, bu da xaos doğurdu.

**Unicode** bütün bu problemləri həll etdi — hər simvola unikal `U+XXXX` kodu verdi. Amma bu kodu faylda saxlamaq üçün **UTF** encoding-ləri yarandı: UTF-32 sabit 4 byte, UTF-16 dinamik 2-4 byte, **UTF-8** isə dinamik 1-4 byte — ASCII uyğun, yüngül, universal. İnternetin standartı UTF-8-dir.

---

*Hazırlandı: 2026 | Platforma: TryHackMe Pre-Security Path | Azərbaycanca*
EOF