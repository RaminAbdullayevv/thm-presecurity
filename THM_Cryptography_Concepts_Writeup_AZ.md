# TryHackMe — Cryptography Concepts | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** Cryptography Concepts  
> **Çətinlik:** Easy  
> **Müddət:** 60 dəqiqə  
> **Link:** https://tryhackme.com/room/cryptographyconcepts  

---

## Ümumi Baxış

Bu otaqda **kriptoqrafiya** (cryptography) — yəni məlumatları gizlətmə elmi öyrənilir. CIA Triadının "Məxfilik" (Confidentiality) dirəyini praktikada necə qorumaq olar? Cavab: kriptoqrafiya ilə. Simmetrik şifrələmə (Caesar Cipher nümunəsi), asimmetrik şifrələmə (açıq/gizli açar), sertifikatlar və HTTPS-in işləmə prinsipi ətraflı izah edilir.

---

## Task 1 — Giriş (Introduction)

### İzahat

**Kriptoqrafiya nədir?**

Kriptoqrafiya — məlumatları yalnız icazəli şəxslərin oxuya biləcəyi şəkildə şifrələmək üçün riyazi qaydalar və gizli açarlar istifadə edən elmdir. Brauzerin ünvan çubuğundakı o kiçik **kilid işarəsi** — arxasında kriptoqrafiyanın gücü dayanır.

**Niyə lazımdır?**

Göndərdiyiniz məlumat birbaşa hədəfə getmir. O, yüzlərlə kompüter və router-dən "sıçrayır". Bu yol boyunca bu sistemlərin birinə çıxışı olan hər kəs — qoruma olmasa — məlumatı oxuya, dəyişdirə və ya blok edə bilər.

```
   SİZ                İNTERNET              SAYT
    │                    │                    │
    │──── məlumat ──────►│                    │
    │              [Router A]                 │
    │              [Router B]  ← Hücumçu     │
    │              [Router C]    burada       │
    │              [Router D]    gözləyir!    │
    │                    │──── məlumat ──────►│
    
    Kriptoqrafiya olmadan: məlumat açıq mətndə ötürülür
    Kriptoqrafiya ilə:     məlumat şifrəli "saçmalığa" çevrilir
```

**Əsas terminologiya:**

| Termin | İzahı | Nümunə |
|--------|-------|--------|
| **Plaintext** | Oxuna bilən açıq mətn | `HELLO` |
| **Ciphertext** | Şifrələnmiş, mənasız görünən mətn | `KHOOR` |
| **Key (Açar)** | Şifrələməni idarə edən gizli dəyər | `3` |
| **Algorithm (Alqoritm)** | Açarın məlumatla istifadəsinin ictimai "resepti" | Caesar Cipher |

> **Vacib prinsip:** Alqoritm gizli saxlanılmır — bütün dünya onu bilə bilər. Təhlükəsizlik yalnız **açarı** gizli saxlamaqdan gəlir.

```
Şifrələmə:  açıq mətn + alqoritm + açar → şifrəli mətn
Deşifrələmə: şifrəli mətn + alqoritm + açar → açıq mətn
```

---

### Sual və Cavab

> **Q: Let's get started.**  
> ✅ No Answer Needed

---

## Task 2 — Məlumatı Gizlətmək: Simmetrik Şifrələmə (Symmetric Encryption)

### İzahat

#### Kilit Qutusu Analoji

Simmetrik şifrələməni anlamaq üçün fiziki kilit qutusu düşünün:

```
Alice                                        Bob
  │                                           │
  │ 1. Məktubu (plaintext) yazır              │
  │ 2. Kilit qutusuna qoyur                  │
  │ 3. ÖZ AÇARI ilə kilidləyir               │
  │    (şifrəli mətn = ciphertext)           │
  │                                           │
  │─────── Kilidli qutu (şəbəkə) ──────────►│
  │                                           │ 4. EYNİ AÇARI ilə açır
  │                                           │ 5. Məktubu oxuyur
```

- **Alqoritm** = kilid mexanizmi (hamı görə bilir, gizli deyil)
- **Açar** = metal açar (yalnız Alice və Bob-da var)
- **Plaintext** = qutunun içindəki məktub
- **Ciphertext** = postal sistemdən keçən kilidli qutu

**Simmetrik şifrələmə:** Bir açar həm kilidləyir, həm açır.

---

#### Caesar Cipher (Sezar Şifrəsi)

Sezar Şifrəsi — simmetrik şifrələmənin ən sadə nümunəsidir. Yülius Sezarın 2000 il əvvəl hərbi mesajlar göndərərkən istifadə etdiyi texnikadır.

**Necə işləyir?**

Hər hərfi əlifbada müəyyən sayda mövqe irəli sürüşdür. Həmin sayı — **açar**dır.

```
Açar = 3 ilə şifrələmə:

A → D    F → I    K → N    P → S    U → X
B → E    G → J    L → O    Q → T    V → Y
C → F    H → K    M → P    R → U    W → Z
D → G    I → L    N → Q    S → V    X → A  (döngü!)
E → H    J → M    O → R    T → W    Y → B
                                    Z → C
```

**Nümunə — `HELLO` açar 3 ilə şifrələmək:**

```
H → K
E → H
L → O
L → O
O → R

HELLO → KHOOR  ✅
```

**Deşifrələmə — `KHOOR` açar 3 ilə:**

```
K → H  (3 geri)
H → E
O → L
O → L
R → O

KHOOR → HELLO  ✅
```

---

#### Niyə Caesar Cipher Təhlükəsiz Deyil?

```
Yalnız 25 mümkün açar var (1-dən 25-ə qədər)
↓
İnsan üçün bütün variantları yoxlamaq darıxdırıcıdır
↓
Kompüter üçün: ~1 millisaniyə!
```

Bu baxımdan Caesar Cipher **əsl sistemlərdə heç vaxt istifadə edilmir** — yalnız öyrənmə məqsədi üçündür. Praktikada **AES** (Advanced Encryption Standard) kimi çox daha mürəkkəb alqoritmlər istifadə olunur.

---

#### Simmetrik Şifrələmənin Üstünlükləri və Çatışmazlıqları

| | Simmetrik Şifrələmə |
|-|---------------------|
| ✅ **Üstünlük** | Çox sürətlidir |
| ✅ **Üstünlük** | Böyük həcmli datanı effektiv şifrələyir |
| ❌ **Çatışmazlıq** | Açarı necə təhlükəsiz paylaşmaq? |

**Açar paylama problemi (Key Distribution Problem):**

```
Alice: "Bob-a açar göndərəcəm"
Hücumçu: "Açarı internet üzərindən görürəm! Alıram!"

Alice: "Onda açarı şifrələyim!"
→ Amma bunun üçün başqa açar lazımdır
→ O açarı da şifrələmək üçün yenə başqa açar...
→ Sonsuz regress!
```

Bu problem **Task 3-də** asimmetrik şifrələmə ilə həll edilir.

---

### Praktiki Oyun: "Secret Message Rescue"

"View Site" düyməsinə basın. Ofis Wi-Fi-ını dinləyən hücumçular var. Komanda Caesar Cipher ilə ünsiyyət saxlayır. Siz:
- Ələ keçirilmiş xəbərdarlıqları deşifrə etməlisiniz
- Yeni mesajları şifrələyib göndərməlisiniz

Oyundakı bütün levelləri tamamladıqdan sonra flag ekrana çıxır.

---

### Suallar və Cavablar

> **Q: What's the flag you received after completing all levels of the Secret Message Rescue game?**  
> ✅ `THM{CAESAR_CIPHER_MASTER_2026}`  
> *İzah: Bütün levelləri uğurla tamamladıqdan sonra oyun bu flag-ı verir.*

---

> **Q: Using the Caesar cipher with a key of 5, what does `CYBER` become when encoded?**

**Manual hesablama (Açar = 5):**

```
Əlifba: A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
        0 1 2 3 4 5 6 7 8 9 ...

C (2)  + 5 = 7  → H
Y (24) + 5 = 29 mod 26 = 3 → D
B (1)  + 5 = 6  → G
E (4)  + 5 = 9  → J
R (17) + 5 = 22 → W

CYBER → HDGJW
```

> ✅ `HDGJW`

---

> **Q: Using the Caesar cipher, find the correct key and decode the following secret message: `FVZCYR PNRFNE PVCURE`**

**Brute-force yanaşması — bütün açarları sına:**

Key 13 (ROT13) ilə sınandıqda:

```
F → S   (F=5, 5+13=18 → S)
V → I   (V=21, 21+13=34 mod 26=8 → I)
Z → M   (Z=25, 25+13=38 mod 26=12 → M)
C → P   (C=2, 2+13=15 → P)
Y → L   (Y=24, 24+13=37 mod 26=11 → L)
R → E   (R=17, 17+13=30 mod 26=4 → E)

FVZCYR → SIMPLE

P → C   N → A   R → E   F → S   N → A   R → E   → CAESAR
P → C   V → I   C → P   H → U   E → R   R → E   → CIPHER

FVZCYR PNRFNE PVCURE → SIMPLE CAESAR CIPHER
```

> ✅ `SIMPLE CAESAR CIPHER`  
> *İzah: Açar 13 (ROT13) ilə deşifrə edilir. Bu mətnin özü isə nə öyrəndiyimizi simvolik olaraq təsvir edir!*

---

## Task 3 — Açarları Təhlükəsiz Paylaşmaq: Asimmetrik Şifrələmə (Asymmetric Encryption)

### İzahat

#### İki Açar Əvəzinə Bir

Asimmetrik şifrələmə açar paylama problemini həll edir. Bir açar yerinə **riyazi bağlantılı iki açar** istifadə edir:

```
┌──────────────────────┐    ┌──────────────────────┐
│    PUBLIC KEY         │    │    PRIVATE KEY        │
│    (Açıq Açar)       │    │    (Gizli Açar)      │
│                       │    │                       │
│  Hər kəsə bəlidir    │    │  Yalnız sahibdə var  │
│  Paylaşıla bilər     │    │  Heç vaxt paylaşılmır│
└──────────────────────┘    └──────────────────────┘

Açıq açarla şifrələnən → Yalnız GİZLİ açarla açıla bilər
```

**Riyazi reallıq:** İki açar çox güclü riyaziyyatla bağlıdır. Açıq açardan gizli açarı tapmaq — adi kompüter üçün **yüzlərlə, hətta minlərlə il** sürgün edir.

---

#### Poçt Qutusu Analoji

```
BOB-UN POÇT QUTUSU:

  [Üstdəki yuva]          [Öndəki qapı]
  ───────────────          ─────────────
  AÇIQ AÇAR               GİZLİ AÇAR
  
  Hər kəs məktub          Yalnız Bob aça bilir
  ata bilər               (yalnız Bob-da açar var)
```

- **Açıq Açar** = üstdəki yuva (hər kəs istifadə edə bilər)
- **Gizli Açar** = öndəki qapının açarı (yalnız Bob-da)

---

#### Açar Paylama Probleminin Həlli

Asimmetrik şifrələmə ilə Alice və Bob əvvəlcədən gizli açar paylaşmağa ehtiyac duymur:

```
1. Bob öz kompüterində açıq + gizli açar cütü yaradır
   Gizli açarı özündə saxlayır
   Açıq açarı dünyaya paylaşır (vebsaytına qoyur)

2. Alice Bob-un açıq açarını tapır

3. Alice mesajını Bob-un AÇIQ AÇARI ilə şifrələyir

4. Bob öz GİZLİ AÇARI ilə şifrəni açır

→ Şəbəkədən yalnız Bob-un AÇIQ AÇARI ötürüldü
→ Açıq açar gizli deyil — bu problemsizdir!
→ Gizli açar heç vaxt şəbəkəyə çıxmadı! ✅
```

---

#### HTTPS-də Asimmetrik Şifrələmə

`https://google.com` açdığınızda arxada nə baş verir:

```
Brauzer                                    Server
  │                                           │
  │──── "Açıq açarını ver" ─────────────────►│
  │◄─── Sertifikat (içində açıq açar) ───────│
  │                                           │
  │──── Asimmetrik ilə paylaşılan ──────────►│
  │     simmetrik açar                        │
  │                                           │
  │═══════ SİMMETRİK ŞİFRƏLƏMƏ ════════════│
  │         (tez, səmərəli)                  │
```

Bu **hibrid yanaşma** adlanır:
- **Asimmetrik** → Açar paylama problemini həll edir (yavaş, amma təhlükəsiz)
- **Simmetrik** → Faktiki datanı şifrələyir (sürətli, səmərəli)

---

#### Sertifikatlar (Certificates) nədir?

Asimmetrik şifrələmədə yeni sual yaranır: Bob-un açıq açarı doğrudanmı Bob-a aiddir? Bəlkə hücumçu saxta açıq açar göndərir?

**Sertifikat** bu problemi həll edir:

```
Sertifikat içindəkilər:
┌─────────────────────────────┐
│ example.com-un açıq açarı  │
│ Kimin açarı olduğu          │
│ Etibarlı CA-nın imzası      │
│ Etibarlılıq müddəti         │
└─────────────────────────────┘
         ↑
  CA (Certificate Authority) = İnandırıcı üçüncü tərəf
  (DigiCert, Let's Encrypt, GlobalSign və s.)
```

Brauzer öncədən yüklənmiş etibarlı CA siyahısına sahib olur. Sayt sertifikatını göndərdikdə brauzer yoxlayır:
- Bu CA etibarlıdır?
- Sertifikat vaxtı keçibmi?
- Sertifikat bu domenə aiddir?

Hər şey qaydasındadırsa → Yaşıl kilid görünür ✅  
Bir şey yanlışdırsa → Brauzer xəbərdarlıq göstərir ⚠️

---

#### Simmetrik vs Asimmetrik — Müqayisə

| Xüsusiyyət | Simmetrik | Asimmetrik |
|-----------|-----------|------------|
| **Açar sayı** | 1 açar (həm şifrələr, həm açar) | 2 açar (açıq + gizli) |
| **Açar paylaşımı** | Hər iki tərəf eyni gizli açara ehtiyac duyur | Açıq açar açıq paylaşıla bilər |
| **Sürət** | Çox sürətli | Yavaş |
| **Əsas istifadə** | Böyük həcmli data şifrələmə | Açar mübadiləsi, sertifikatlar |
| **Analoji** | Bir açar qutuyu həm kilidləyir, həm açır | Poçt qutusu: hər kəs atar, yalnız sahib açar |

---

### Suallar və Cavablar

> **Q: In asymmetric encryption, which key stays secret?**  
> ✅ `Private key`  
> *İzah: Asimmetrik şifrələmədə gizli açar (private key) heç vaxt paylaşılmır — yalnız sahibdə qalır. Açıq açar (public key) isə dünyaya açıq paylaşılır.*

> **Q: With asymmetric encryption, Alice can encrypt a message using Bob's public key, and only Bob's private key can decrypt it. Yay or Nay?**  
> ✅ `Yay`  
> *İzah: Bu, asimmetrik şifrələmənin əsas prinsipidir. Bob-un açıq açarı ilə şifrələnmiş məlumatı yalnız Bob-un gizli açarı aça bilər.*

> **Q: What problem does asymmetric encryption solve that symmetric cannot?**  
> ✅ `Key distribution`  
> *İzah: Simmetrik şifrələmənin əsas problemi — açarı necə təhlükəsiz paylaşmaq. Asimmetrik şifrələmə bu "açar paylama problemini" (key distribution problem) həll edir — gizli açar heç vaxt şəbəkəyə çıxmır.*

> **Q: After initial asymmetric exchange in HTTPS, what encryption type handles bulk data?**  
> ✅ `Symmetric`  
> *İzah: HTTPS-də asimmetrik şifrələmə yalnız ilkin açar mübadiləsi üçün istifadə olunur. Sonra simmetrik şifrələmə devreye girer — çünki o çox daha sürətlidir və böyük həcmli datanı səmərəli emal edir.*

---

## Task 4 — Nəticə (Conclusion)

### İzahat

Bu otaqda kriptoqrafiyanın əsasları öyrənildi — CIA Triadının **Məxfilik** (Confidentiality) dirəyini praktikada qorumağın əsas alətləri.

**Öyrəndiklərimizin xülasəsi:**

```
PLAINTEXT  → oxuna bilən məlumat
CIPHERTEXT → şifrəli, mənasız görünən məlumat
AÇAR       → şifrələməni idarə edən gizli dəyər
ALQORİTM   → açarın istifadəsinin ictimai üsulu

SİMMETRİK ŞİFRƏLƏMƏ:
  • 1 açar həm şifrələyir, həm açır
  • Sürətli, səmərəli
  • Problem: açarı necə paylaşmaq?
  • Nümunə: Caesar Cipher (öyrənmə üçün), AES (real)

ASİMMETRİK ŞİFRƏLƏMƏ:
  • 2 açar: açıq (public) + gizli (private)
  • Açar paylama problemini həll edir
  • Yavaş — yalnız kiçik data üçün
  • Nümunə: RSA

HİBRİD YANAŞMA (HTTPS):
  • Asimmetrik → açar mübadiləsi
  • Simmetrik → datanın şifrələnməsi
```

Kriptoqrafiya güclü bir alətdir — amma yeganə alət deyil. Güclü parollar, açarların etibarlı saxlanması, istifadəçi maarifləndirməsi, müntəzəm yeniləmələr və hadisəyə cavab tədbirləri də lazımdır.

---

### Sual və Cavab

> **Q: I've completed the room!**  
> ✅ No Answer Needed

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Başlayaq | No Answer Needed |
| Task 2 | Secret Message Rescue oyununun flag-i? | `THM{CAESAR_CIPHER_MASTER_2026}` |
| Task 2 | Açar 5 ilə `CYBER` nə olur? | `HDGJW` |
| Task 2 | `FVZCYR PNRFNE PVCURE` deşifrə et | `SIMPLE CAESAR CIPHER` |
| Task 3 | Asimmetrik şifrələmədə hansı açar gizli qalır? | `Private key` |
| Task 3 | Alice Bob-un açıq açarı ilə şifrələyir, yalnız Bob-un gizli açarı açır — Yay or Nay? | `Yay` |
| Task 3 | Asimmetrik hansı problemi həll edir? | `Key distribution` |
| Task 3 | HTTPS-də asimmetrik mübadiledən sonra hansı şifrələmə tipli böyük datanı işləyir? | `Symmetric` |
| Task 4 | Otağı tamamladım | No Answer Needed |

---

## Bonus: Caesar Cipher — Sürətli Hesablama Cədvəli

```
Əlifba: A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
Dəyər: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25

Şifrələmə: (hərf dəyəri + açar) mod 26 → şifrəli hərf
Deşifrələmə: (şifrəli hərf dəyəri - açar + 26) mod 26 → açıq hərf
```

**CYBER + açar 5 = HDGJW yoxlaması:**
```
C = 2,  2+5 = 7  → H  ✅
Y = 24, 24+5 = 29, 29 mod 26 = 3 → D  ✅
B = 1,  1+5 = 6  → G  ✅
E = 4,  4+5 = 9  → J  ✅
R = 17, 17+5 = 22 → W  ✅
```

**FVZCYR PNRFNE PVCURE + açar 13 = SIMPLE CAESAR CIPHER yoxlaması:**
```
F=5,  5+13=18 → S     P=15, 15+13=28 mod 26=2 → C
V=21, 21+13=34 mod 26=8 → I  N=13, 13+13=26 mod 26=0 → A
Z=25, 25+13=38 mod 26=12 → M  R=17, 17+13=30 mod 26=4 → E
C=2,  2+13=15 → P     F=5,  5+13=18 → S
Y=24, 24+13=37 mod 26=11 → L N=13, 13+13=26 mod 26=0 → A
R=17, 17+13=30 mod 26=4 → E  R=17, 17+13=30 mod 26=4 → E

→ SIMPLE CAESAR CIPHER  ✅
```

---

## Bonus: Kriptoqrafiya Konseptləri — Qısa Xülasə

```
KRİPTOQRAFİYA
├── Simmetrik Şifrələmə
│   ├── 1 açar (həm şifrələyir, həm açır)
│   ├── Sürətli — böyük data üçün
│   ├── Problem: açar paylama
│   └── Nümunələr: Caesar (zəif), AES, 3DES
│
├── Asimmetrik Şifrələmə
│   ├── 2 açar (açıq + gizli)
│   ├── Açar paylama problemini həll edir
│   ├── Yavaş — kiçik data üçün
│   └── Nümunələr: RSA, ECC
│
└── Hibrid (HTTPS-də istifadə)
    ├── Asimmetrik → açar mübadiləsi (əl sıxışma)
    └── Simmetrik → data şifrələmə (sürətli)

SERTİFİKAT
├── Saytın açıq açarını ehtiva edir
├── CA (Certificate Authority) imzalayır
└── Brauzer CA-ya etibar edib sertifikatı yoxlayır
    → Kilid işarəsi ✅ / Xəbərdarlıq ⚠️
```
