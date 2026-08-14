# 🖧 TryHackMe — "Intro to LAN" | Azərbaycan dilində Öyrənmə Yazısı

> **Mənbə:** [TryHackMe | Intro to LAN](https://tryhackme.com/room/introtolan)
> **Çətinlik:** Info | **Müddət:** ~15 dəq
> **Məqsəd:** Özəl şəbəkələri idarə edən texnologiyaları və dizaynları öyrənmək.

---

## 📋 Mündəricat

1. [Task 1 — LAN Topologiyaları](#task-1--lan-topologiyaları)
2. [Task 2 — Subnetting (Alt Şəbəkəyə Bölmə)](#task-2--subnetting)
3. [Task 3 — ARP Protokolu](#task-3--arp-protokolu)
4. [Task 4 — DHCP Protokolu](#task-4--dhcp-protokolu)
5. [Sual-Cavab Cədvəli](#sual-cavab-cədvəli)

---

## Task 1 — LAN Topologiyaları

### LAN nədir?

**LAN** = **L**ocal **A**rea **N**etwork = Lokal Ərazi Şəbəkəsi

Bir bina, ev, ofis və ya kampus daxilindəki cihazları birləşdirən şəbəkədir.

### Topologiya nədir?

**Topologiya** — şəbəkənin fiziki və ya məntiqi **dizaynı, görünüşüdür.** Hansı cihazın nəyə necə qoşulduğunu göstərir.

3 əsas topologiya növü var:

---

### 🌟 1. Ulduz Topologiyası (Star Topology)

**Necə işləyir?**
Bütün cihazlar **mərkəzi bir avadanlığa** (switch və ya hub) ayrı-ayrı qoşulur. Məlumat ötürmək üçün hər şey bu mərkəzin üzərindən keçir.

```
     💻 PC1
      │
      │
💻 PC2──── [SWITCH] ────💻 PC3
      │
      │
     🖨️ Printer
```

**Üstünlükləri:**
- ✅ **Ən geniş yayılmış** topologiyadır — etibarlı və genişlənə bilən
- ✅ **Scalable** — yeni cihaz əlavə etmək çox asandır
- ✅ Bir cihaz sıradan çıxsa digərləri təsirlənmir

**Çatışmazlıqları:**
- ❌ **Baha başa gəlir** — çox kabel və xüsusi avadanlıq lazımdır
- ❌ Şəbəkə böyüdükcə texniki xidmət çətinləşir
- ❌ **Mərkəzi avadanlıq (switch) sıradan çıxarsa** — bütün şəbəkə dayanır

> **Qısa xülasə:** Ən etibarlı, amma ən baha topologiya.

---

### 🚌 2. Avtobus Topologiyası (Bus Topology)

**Necə işləyir?**
Bütün cihazlar **tək bir əsas kabelə** (backbone cable) qoşulur. Məlumat bu kabel boyunca hər iki istiqamətdə gəzir.

```
💻 PC1──💻 PC2──💻 PC3──💻 PC4──💻 PC5
         ════════════════════
              (backbone cable)
```

Ağac yarpağına bənzəyir — budaqlar (kabel) üzərindən yarpaqlar (cihazlar) çıxır.

**Üstünlükləri:**
- ✅ **Qurulması ucuz və asandır** — az kabel, xüsusi avadanlıq lazım deyil
- ✅ Kiçik şəbəkələr üçün əlverişlidir

**Çatışmazlıqları:**
- ❌ **Tıxac (bottleneck)** — hamı eyni kabeldən istifadə edir, trafik artanda yavaşlayır
- ❌ Xəta aşkarlamaq çətindir — hamı eyni xəttdən keçir
- ❌ **Tək uğursuzluq nöqtəsi** — əsas kabel qırılsa bütün şəbəkə çökür
- ❌ **Ehtiyat yoxdur (no redundancy)**

> **Qısa xülasə:** Ən ucuz, amma ən az etibarlı topologiya.

---

### 💍 3. Halqa Topologiyası (Ring Topology)

**Necə işləyir?**
Cihazlar bir-birinə birbaşa qoşularaq **qapalı halqa** əmələ gətirir. Məlumat yalnız bir istiqamətdə halqa boyunca gəzir, hədəfə çatana qədər cihazdan cihaza ötürülür.

```
    💻 PC1
   /        \
💻 PC4    💻 PC2
   \        /
    💻 PC3
```

**Xüsusi qayda:** Bir cihazın özünün göndərəcəyi məlumatı varsa, əvvəl onu göndərir, sonra başqasından gələni ötürür.

**Üstünlükləri:**
- ✅ Az kabel lazımdır
- ✅ Xüsusi avadanlığa (switch/hub) o qədər ehtiyac yoxdur
- ✅ Tıxac (bottleneck) az olur — eyni anda çox trafik olmur
- ✅ Xəta aşkarlamaq nisbətən asandır — məlumat yalnız bir istiqamətdə gedir

**Çatışmazlıqları:**
- ❌ **Məlumat yavaş gəzir** — hədəfə çatmaq üçün çox cihazı keçə bilər
- ❌ **Bir cihaz və ya kabel sıradan çıxarsa** — bütün şəbəkə dayanır

> **Qısa xülasə:** Orta həll — nə ən baha, nə ən ucuz; amma tam etibarsız da deyil.

---

### Topologiyaların Müqayisəsi

| Xüsusiyyət | Ulduz ⭐ | Avtobus 🚌 | Halqa 💍 |
|---|---|---|---|
| Qiymət | Baha | Ucuz | Orta |
| Qurulma asanlığı | Orta | Asan | Orta |
| Etibarlılıq | Yüksək | Aşağı | Orta |
| Genişlənmə | Asan | Çətin | Orta |
| Tıxac riski | Az | Çox | Az |
| Tək uğursuzluq nöqtəsi | Mərkəzi switch | Əsas kabel | İstənilən cihaz |

---

### Switch nədir?

**Switch** — çoxlu cihazı birləşdirən xüsusi şəbəkə avadanlığıdır. Ethernet kabelləri ilə cihazlar switch-in portlarına qoşulur.

Switch-in ağıllı cəhəti: **Hər porta hansı cihazın qoşulduğunu bilir.** Paketi yalnız hədəf cihaza göndərir, hamıya deyil.

```
Hub (köhnə):    Paketi BÜTÜN portlara göndərir → İsrafçı
Switch (yeni):  Paketi YALNIZ hədəf porta göndərir → Səmərəli
```

Switch-lər 4, 8, 16, 24, 32, 64 portlu ola bilər.

---

### Router nədir?

**Router** — şəbəkələri bir-birinə **bağlayan** avadanlıqdır.

- Switch eyni şəbəkə daxilindəki cihazları birləşdirir
- Router isə **fərqli şəbəkələri** bir-birinə bağlayır

```
Ev şəbəkəsi ──→ [ROUTER] ──→ İnternet (başqa şəbəkə)
```

**Routing** — məlumatın şəbəkələr arasında düzgün yol tapıb çatdırılması prosesinə deyilir.

Router-lər bir-birinə də qoşula bilər. Bu **redundancy (ehtiyat)** yaradır — bir yol bağlananda məlumat alternativ yolla gedir. Şəbəkə heç vaxt tamamilə dayanmır.

---

### Task 1 Sual-Cavabları

| Sual | Cavab |
|---|---|
| LAN nəyin abbreviaturasıdır? | `Local Area Network` |
| Router-lərin işinə verilən fel nədir? | `Routing` |
| Lokal şəbəkədə mərkəzi qoşulma üçün hansı avadanlıq işlədilir? | `Switch` |
| Hansı topologiya qurmaq üçün ucuzdur? | `Bus Topology` |
| Hansı topologiya baha başa gəlir? | `Star Topology` |
| İnteraktiv labın flag-ı | *(lab-dan alınır)* |

---

## Task 2 — Subnetting

### Subnetting nədir?

**Subnetting** — böyük bir şəbəkəni daha kiçik alt-şəbəkələrə (subnet) bölmək prosesidir.

**Tort analoji:** Dost yığıncağında tort var, hamıya pay düşməlidir. Subnetting — tortu kimin nə qədər alacağını qərarlaşdırmaq kimidir.

### Niyə lazımdır?

Şirkətdə fərqli departamentlər var:

```
Şirkət Şəbəkəsi (192.168.1.0/24)
        │
        ├── 💼 Mühasibatlıq    → 192.168.1.0/26   (64 cihaz)
        ├── 💰 Maliyyə         → 192.168.1.64/26  (64 cihaz)
        └── 👥 HR              → 192.168.1.128/26 (64 cihaz)
```

Şəbəkə administrator bu departamentlərə ayrı-ayrı alt-şəbəkə ayırır ki:
- **Təhlükəsizlik:** HR məlumatlarına Maliyyə çata bilməsin
- **Səmərəlilik:** Trafik azalsın
- **Nəzarət:** Hər departament ayrıca idarə olunsun

---

### Subnet Mask nədir?

IP ünvanı kimi **4 oktetdən** ibarətdir (32 bit). Hər oktet 0-255 arasındadır.

```
IP ünvanı:    192 . 168 .  1  . 100
Subnet mask:  255 . 255 . 255 .  0
```

Subnet mask şəbəkəyə neçə cihazın sığacağını müəyyən edir.

---

### Subnet-in 3 Vacib Ünvanı

| Növ | Nədir? | Nümunə |
|---|---|---|
| **Network Address** | Şəbəkənin başlanğıc ünvanı — şəbəkənin özünü identifikasiya edir | `192.168.1.0` |
| **Host Address** | Şəbəkədəki konkret cihazın ünvanı | `192.168.1.100` |
| **Default Gateway** | Başqa şəbəkəyə məlumat göndərə bilən cihazın ünvanı (adətən router) | `192.168.1.254` |

### Praktiki izah:

```
Şəbəkə: 192.168.1.0
         │
         ├── 192.168.1.1    → İlk host (router ola bilər)
         ├── 192.168.1.2    → Cihaz
         ├── 192.168.1.100  → Sənin kompüterin
         ├── ...
         ├── 192.168.1.253  → Son cihaz
         └── 192.168.1.254  → Default Gateway (adətən)
```

**Default Gateway niyə vacibdir?**
Sənin kompüterin `google.com`-a məlumat göndərmək istəyir. Google eyni şəbəkədə deyil. Buna görə məlumat əvvəlcə **Default Gateway**-ə (routerə) gedir, oradan İnternetə çıxır.

---

### Subnetting-in Faydaları

**1. Təhlükəsizlik (Security):**
```
Kafe nümunəsi:
├── İşçi şəbəkəsi  → Kassa, kompüterlər, kameralar
└── Müştəri Wi-Fi  → İctimai istifadə üçün hotspot

İkisi bir-birindən ayrıdır! Müştəri işçi sisteminə daxil ola bilməz.
```

**2. Səmərəlilik (Efficiency):** Trafik yalnız lazımi alt-şəbəkəyə gedir.

**3. Tam nəzarət (Full Control):** Administrator hər subneti ayrıca idarə edir.

---

### Task 2 Sual-Cavabları

| Sual | Cavab |
|---|---|
| Şəbəkəni kiçik hissələrə bölmənin texniki termini? | `Subnetting` |
| Subnet maskında neçə bit var? | `32` |
| Subnet maskının bir hissəsinin (oktet) diapazonu? | `0-255` |
| Şəbəkənin başlanğıcını identifikasiya edən ünvan? | `Network Address` |
| Şəbəkədəki cihazları identifikasiya edən ünvan? | `Host Address` |
| Başqa şəbəkəyə məlumat göndərən cihazın adı? | `Default Gateway` |

---

## Task 3 — ARP Protokolu

### ARP nədir?

**ARP** = **A**ddress **R**esolution **P**rotocol = Ünvan Həlli Protokolu

Öncəki labdan xatırlayırsan: cihazların **iki identifikatoru** var:
- **IP ünvanı** — məntiqi identifikator (dəyişə bilər)
- **MAC ünvanı** — fiziki identifikator (sabitdir)

**Problem:** Şəbəkədə məlumat göndərmək üçün hədəf cihazın **MAC ünvanı** lazımdır. Amma biz adətən yalnız IP ünvanını bilirik.

**ARP bu problemi həll edir** — IP ünvanından MAC ünvanını tapır.

---

### ARP Necə İşləyir?

Hər cihazda **ARP Cache** (yaddaş) var — orada digər cihazların IP↔MAC cütlükləri saxlanılır.

ARP 2 növ mesaj işlədir:

#### 1. ARP Request (Sorğu)
```
Cihaz A bütün şəbəkəyə yayımla (broadcast) göndərir:
"Hə, 192.168.1.100 IP-sinə sahib olan kimdir?
 Onun MAC ünvanı nədir?"
```

#### 2. ARP Reply (Cavab)
```
192.168.1.100 IP-li Cihaz B cavab verir:
"Mənəm! Mənim MAC ünvanım: aa:bb:cc:dd:ee:ff"
```

```
Tam axın:

Cihaz A                              Şəbəkə                    Cihaz B
   │                                    │                          │
   │──── ARP Request (broadcast) ──────→│──────────────────────────│→
   │     "192.168.1.100-un MAC-i nədir?"│                          │
   │                                    │                          │
   │←──────────────────────────────────────── ARP Reply ───────────│
   │              "MAC: aa:bb:cc:dd:ee:ff"                         │
   │                                    │                          │
   │ ARP Cache-ə saxla:                 │                          │
   │ 192.168.1.100 → aa:bb:cc:dd:ee:ff  │                          │
```

**ARP Cache:** Bir dəfə öyrənilən IP↔MAC cütlüyü yaddaşa yazılır ki, hər dəfə yenidən soruşulmasın.

---

### ARP-ın Təhlükəsizlik Əhəmiyyəti

ARP **güvənsiz** protokoldur — heç bir doğrulama yoxdur. Bu, **ARP Spoofing** (ARP Poisoning) hücumuna yol açır:

```
Hacker öz MAC ünvanını router-in IP-si ilə elan edir.
Şəbəkədəki cihazlar router əvəzinə hackerin kompüterinə məlumat göndərir.
Bu "Man-in-the-Middle" (MitM) hücumunun əsasıdır.
```

---

### Task 3 Sual-Cavabları

| Sual | Cavab |
|---|---|
| ARP nəyin abbreviaturasıdır? | `Address Resolution Protocol` |
| Hansı ARP paketi cihazın müəyyən IP-yə sahib olub-olmadığını soruşur? | `ARP Request` |
| Cihazın fiziki identifikatoru kimi hansı ünvan işlədilir? | `MAC Address` |
| Cihazın məntiqi identifikatoru kimi hansı ünvan işlədilir? | `IP Address` |

---

## Task 4 — DHCP Protokolu

### DHCP nədir?

**DHCP** = **D**ynamic **H**ost **C**onfiguration **P**rotocol = Dinamik Host Konfiqrasiya Protokolu

IP ünvanları cihazlara **iki yolla** verilə bilər:

| Üsul | Necə? | Nə vaxt? |
|---|---|---|
| **Manual (Əl ilə)** | Administrator IP-ni özü daxil edir | Server, printer kimi sabit cihazlar |
| **Avtomatik (DHCP)** | Server avtomatik IP verir | Kompüter, telefon, noutbuk |

Evdə İnternetə qoşulduqda IP ünvanını sən yazmırsan — **DHCP avtomatik verir.**

---

### DHCP Necə İşləyir? — 4 Addımlı Proses

```
Cihaz (yeni qoşulub)                          DHCP Server
       │                                            │
       │──── 1. DHCP Discover ──────────────────────│→
       │     "Şəbəkədə DHCP server varmı?"          │
       │                                            │
       │←─── 2. DHCP Offer ─────────────────────────│
       │     "Var! Sənə 192.168.1.50 verə bilərəm"  │
       │                                            │
       │──── 3. DHCP Request ───────────────────────│→
       │     "Yaxşı, o IP-ni istəyirəm!"            │
       │                                            │
       │←─── 4. DHCP ACK ───────────────────────────│
       │     "Təsdiqləndi! İstifadə edə bilərsən."  │
       │                                            │
    [192.168.1.50 IP-si ilə işləməyə başlayır]
```

### 4 Mərhələnin Adları

| Mərhələ | Ad | Kim göndərir? | Nə deyir? |
|---|---|---|---|
| 1 | **DHCP Discover** | Cihaz → Şəbəkə | "DHCP server varmı?" |
| 2 | **DHCP Offer** | Server → Cihaz | "Sənə bu IP-ni təklif edirəm" |
| 3 | **DHCP Request** | Cihaz → Server | "O IP-ni qəbul edirəm" |
| 4 | **DHCP ACK** | Server → Cihaz | "Təsdiqləndi, istifadə et" |

> **ACK** = Acknowledgement = Təsdiq

---

### DHCP-in Praktiki Nümunəsi

Kafeyə girib Wi-Fi-a qoşulursan:
1. Telefonun: *"Burada DHCP server varmı?"* → **Discover**
2. Router: *"Var! Sənə 192.168.0.47 verə bilərəm"* → **Offer**
3. Telefonun: *"Yaxşı, o IP-ni istəyirəm"* → **Request**
4. Router: *"Oldu, istifadə et"* → **ACK**

Bu proses **saniyələr ərzində** baş verir, sən heç fərk etmirsən.

---

### Task 4 Sual-Cavabları

| Sual | Cavab |
|---|---|
| Cihaz IP almaq üçün hansı DHCP paketini göndərir? | `DHCP Discover` |
| IP təklif edildikdən sonra cihaz hansı paketi göndərir? | `DHCP Request` |
| DHCP serverindən cihaza göndərilən son paket? | `DHCP ACK` |

---

## 📊 Tam Sual-Cavab Cədvəli

| Task | Sual | Cavab |
|---|---|---|
| Task 1 | LAN nəyin qısaltmasıdır? | `Local Area Network` |
| Task 1 | Router-lərin işinə verilən fel? | `Routing` |
| Task 1 | Mərkəzi qoşulma avadanlığı? | `Switch` |
| Task 1 | Ucuz qurulan topologiya? | `Bus Topology` |
| Task 1 | Baha başa gələn topologiya? | `Star Topology` |
| Task 1 | İnteraktiv lab flag-ı | *(labdan alınır)* |
| Task 2 | Şəbəkəni bölmənin texniki termini? | `Subnetting` |
| Task 2 | Subnet maskında neçə bit? | `32` |
| Task 2 | Oktetin diapazonu? | `0-255` |
| Task 2 | Şəbəkənin başlanğıc ünvanı? | `Network Address` |
| Task 2 | Cihazı identifikasiya edən ünvan? | `Host Address` |
| Task 2 | Başqa şəbəkəyə məlumat göndərən cihaz? | `Default Gateway` |
| Task 3 | ARP nəyin qısaltmasıdır? | `Address Resolution Protocol` |
| Task 3 | IP soruşan ARP paketi? | `ARP Request` |
| Task 3 | Fiziki identifikator? | `MAC Address` |
| Task 3 | Məntiqi identifikator? | `IP Address` |
| Task 4 | IP almaq üçün göndərilən paket? | `DHCP Discover` |
| Task 4 | IP qəbul edildikdə göndərilən paket? | `DHCP Request` |
| Task 4 | Serverdən gələn son paket? | `DHCP ACK` |

---

## 🧠 Öyrəndiklərimizin Xülasəsi

```
LAN (Lokal Şəbəkə)
│
├── Topologiyalar
│   ├── ⭐ Ulduz   → Mərkəzi switch, baha, etibarlı
│   ├── 🚌 Avtobus → Tək kabel, ucuz, az etibarlı
│   └── 💍 Halqa   → Qapalı dövr, orta etibarlılıq
│
├── Avadanlıqlar
│   ├── Switch → Eyni şəbəkədə cihazları birləşdirir (ağıllı)
│   └── Router → Fərqli şəbəkələri birləşdirir (routing)
│
├── Subnetting
│   ├── Böyük şəbəkəni kiçik hissələrə böl
│   ├── Network Address → Şəbəkənin ünvanı
│   ├── Host Address    → Cihazın ünvanı
│   └── Default Gateway → Xarici şəbəkəyə çıxış nöqtəsi
│
├── ARP
│   ├── IP → MAC ünvanına çevirir
│   ├── ARP Request → "Bu IP kimə aiddir?"
│   ├── ARP Reply   → "Mənəm, MAC-im budur"
│   └── ARP Cache   → Öyrənilən cütlükləri saxlayır
│
└── DHCP
    ├── Avtomatik IP verir
    ├── Discover → "Server varmı?"
    ├── Offer   → "Sənə bu IP-ni verirəm"
    ├── Request → "Qəbul edirəm"
    └── ACK     → "Təsdiqləndi"
```

---

## 🔗 Növbəti Addım

Bu labı bitirdikdən sonra:

👉 **[OSI Model](https://tryhackme.com/room/osimodelzi)** — Şəbəkə kommunikasiyasının 7 qatlı modelini öyrən

Orada öyrənəcəksən:
- Fiziki, Data Link, Network, Transport, Session, Presentation, Application qatları
- Hər qatın rolu və protokolları
- TCP/IP ilə OSI-nin fərqi

---

*Hazırladı: Azərbaycan dilində TryHackMe öyrənmə yazısı*
*Mənbə: [TryHackMe — Intro to LAN](https://tryhackme.com/room/introtolan)*
