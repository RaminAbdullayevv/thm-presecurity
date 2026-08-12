# 🌐 TryHackMe — "What is Networking?" | Azərbaycan dilində öyrənmə yazısı

> **Məqsəd:** Bu lab-da kompüter şəbəkələrinin əsaslarını öyrənirik — şəbəkə nədir, İnternet necə işləyir, cihazlar bir-birini necə tanıyır, və ping aləti nədir.

---

## 📋 Mündəricat

1. [Task 1 — Şəbəkə nədir?](#task-1--şəbəkə-nədir)
2. [Task 2 — İnternet nədir?](#task-2--i̇nternet-nədir)
3. [Task 3 — Şəbəkədə cihazların identifikasiyası (IP və MAC)](#task-3--şəbəkədə-cihazların-identifikasiyası)
4. [Task 4 — Ping (ICMP)](#task-4--ping-icmp)
5. [Sual-Cavab Cədvəli](#sual-cavab-cədvəli)

---

## Task 1 — Şəbəkə nədir?

### 💡 Əsas anlayış

**Şəbəkə (Network)** — sadəcə bir-birinə bağlı şeylər toplusudur.

Bunu real həyatla müqayisə edək:

| Real həyat nümunəsi | Kompüter dünyasındakı qarşılığı |
|---|---|
| Dostlar dairəsi (hamı oxşar maraqlarla bağlıdır) | Kompüterlər bir-birinə kabel/Wi-Fi ilə bağlıdır |
| Şəhərin nəqliyyat sistemi | Lokal şəbəkə (LAN) |
| Milli elektrik şəbəkəsi | İnternet infrastrukturu |
| Poçt sistemi | Məlumat paketlərinin göndərilməsi |

### 🖥️ Kompüter şəbəkəsi nədir?

Kompüter şəbəkəsi **2 cihazdan milyardlarla cihaza** qədər ola bilər. Sadəcə noutbuklar deyil — bunlara daxildir:
- 📱 Telefonlar
- 📷 Təhlükəsizlik kameraları
- 🚦 Svetoforlar
- 🚜 Kənd təsərrüfatı avadanlıqları

### ❓ Niyə vacibdir?

Müasir dünyada şəbəkələr hər yerdədir: hava məlumatları toplamaq, evlərə elektrik çatdırmaq, yolda kimin keçmək hüququ olduğunu müəyyən etmək — hamısı şəbəkə üzərindən gedir. Buna görə **kibertəhlükəsizlik** üçün şəbəkə bilgisi vacibdir.

### 📝 Task 1 Cavabı

> **Sual:** Bir-birinə bağlı cihazlar üçün əsas termin nədir?
>
> **Cavab:** `Network`

---

## Task 2 — İnternet nədir?

### 💡 Əsas anlayış

**İnternet** — çoxlu kiçik şəbəkələrdən ibarət nəhəng bir şəbəkədir.

### 🕰️ İnternetin tarixi

```
1960-cı illər sonları → ARPANET layihəsi
    └── ABŞ Müdafiə Nazirliyi tərəfindən maliyyələşdirildi
    └── İlk sənədləşdirilmiş şəbəkə

1989-cu il → Tim Berners-Lee
    └── World Wide Web (WWW) icad edildi
    └── İnternet məlumat saxlama/paylaşma mühitinə çevrildi
```

### 🔒 Şəbəkə növləri

İnternet **iki əsas şəbəkə növündən** ibarətdir:

| Növ | İzah | Nümunə |
|---|---|---|
| **Özəl şəbəkə (Private Network)** | Məhdud sayda cihazı birləşdirir | Evin Wi-Fi-ı, ofis şəbəkəsi |
| **İctimai şəbəkə (Public Network)** | Kiçik şəbəkələri bir-birinə bağlayan böyük şəbəkə | İnternet özü |

### 🤝 Analoji — Dil vasitəçisi

Alice (həm ingilis, həm ərəb bilir) → Bob və Jim-i Zayn və Toby ilə tanış edir.
Alice ikisi arasında **vasitəçi** rolunu oynayır, eyni şəkildə kiçik şəbəkələr İnternet vasitəsilə bir-birinə bağlanır.

### 📝 Task 2 Cavabı

> **Sual:** World Wide Web-i kim icad etdi?
>
> **Cavab:** `Tim Berners-Lee`

---

## Task 3 — Şəbəkədə Cihazların İdentifikasiyası

### 💡 Əsas anlayış

Şəbəkədə cihazlar bir-birini tanımalıdır ki, ünsiyyət mümkün olsun. Cihazların **iki növ identifikasiyası** var — eynilə insanlar kimi:

| İnsan | Cihaz |
|---|---|
| Ad (dəyişdirilə bilər) | IP ünvanı (dəyişdirilə bilər) |
| Barmaq izi (dəyişdirilməz) | MAC ünvanı (zavoddan verilir) |

---

### 🔢 IP Ünvanı (IP Address)

**IP** = **I**nternet **P**rotocol

IP ünvanı cihazı şəbəkədə müəyyən müddət ərzində identifikasiya edir.

#### IPv4 quruluşu:

```
192  .  168  .   1  .  77
 │        │       │      │
Oktet1  Oktet2  Oktet3  Oktet4
```

- **4 oktetdən** (hissədən) ibarətdir
- Hər oktet **0–255** arasında bir ədəddir
- Nümunə: `192.168.1.1`

#### Özəl vs İctimai IP:

```
Şəbəkə daxilindəki 2 cihaz:
┌─────────────────────────────────────────────┐
│  DESKTOP-KJE57FD                            │
│  Özəl IP:   192.168.1.77   (şəbəkə daxili) │
│  İctimai IP: 86.157.52.21  (internet üçün) │
│                                             │
│  CMNatic-PC                                 │
│  Özəl IP:   192.168.1.74   (şəbəkə daxili) │
│  İctimai IP: 86.157.52.21  (internet üçün) │
└─────────────────────────────────────────────┘
         ↑
  Hər iki cihaz eyni ictimai IP-ni paylaşır!
  Bu IP-ni ISP (İnternet Xidməti Provayderi) verir.
```

> **Vacib:** Eyni şəbəkədə iki cihaz eyni özəl IP-yə sahib ola bilməz!

#### IPv4 vs IPv6:

| | IPv4 | IPv6 |
|---|---|---|
| Format | `192.168.1.1` | `2001:0db8:85a3:0000:0000:8a2e:0370:7334` |
| Maksimum ünvan sayı | 2³² = ~4.29 milyard | 2¹²⁸ = ~340 trilyon+ |
| Problem | Ünvanlar tükənir | Tükənmə problemi yoxdur |

Cisco-nun hesablamalarına görə 2021-ci ilin sonuna qədər internetə ~50 milyard cihaz qoşulmuşdur. Bu, IPv4-ün çatışmazlığını açıq göstərir!

---

### 🏷️ MAC Ünvanı (MAC Address)

**MAC** = **M**edia **A**ccess **C**ontrol

MAC ünvanı — cihazın anakartındakı şəbəkə interfeysinə (kartına) **zavod tərəfindən verilən unikal ünvan**.

#### Quruluşu:

```
a4 : c3 : f0 : 85 : ac : 2d
└────────────┘   └────────────┘
 İlk 6 simvol        Son 6 simvol
(istehsalçı şirkət)  (unikal nömrə)
```

- **12 simvollu onaltılıq (hexadecimal) ədəddir**
- İki-iki qruplara bölünür, aralarında `:` işarəsi var
- Nümunə: `a4:c3:f0:85:ac:2d`

#### ⚠️ MAC Spoofing (MAC saxtalaşdırma)

MAC ünvanı dəyişdirilə bilər — buna **spoofing** deyilir.

**Nümunə ssenarisi:**
```
Güvənsiz firewall konfiqrasiyası:
  ✅ Admin MAC: aa:bb:cc:dd:ee:ff → İcazə VAR
  ❌ Bob MAC:   11:22:33:44:55:66 → İcazə YOX

Hücum:
  Bob öz MAC-ini admin MAC-ı ilə əvəz edir
  Bob MAC: aa:bb:cc:dd:ee:ff (saxtalaşdırılmış)
  Firewall Bob-u admin kimi tanıyır → ✅ İcazə VERİLİR
```

**Real həyat nümunəsi:** Hotellər və kafeler öz Wi-Fi şəbəkələrini MAC ünvanına əsasən idarə edir. Pul ödəməyən istifadəçilərin MAC-ı bloklanır.

#### 🧪 Praktiki tapşırıq (Task 3):

Hotelin interaktiv laboratoriyasında:
1. Alice pul ödəyib → paketləri keçir ✅
2. Bob pul ödəməyib → paketləri bloklanır ❌
3. Hədəf: Bob-un MAC-ını Alice-in MAC-ı ilə dəyiş → Bob da daxil ola bilir ✅

### 📝 Task 3 Cavabları

| Sual | Cavab |
|---|---|
| "IP" nəyin abbreviaturasıdır? | `Internet Protocol` |
| IP ünvanının hər hissəsinə nə deyilir? | `Octet` |
| IPv4 neçə hissədən ibarətdir? | `4` |
| "MAC" nəyin abbreviaturasıdır? | `Media Access Control` |
| MAC spoofing ilə flag nədir? | *(interaktiv labdan alınır)* |

---

## Task 4 — Ping (ICMP)

### 💡 Əsas anlayış

**Ping** — iki cihaz arasındakı əlaqənin mövcudluğunu və keyfiyyətini yoxlayan ən fundamental şəbəkə alətidir.

**ICMP** = **I**nternet **C**ontrol **M**essage **P**rotocol

### ⚙️ Necə işləyir?

```
Siz                              Hədəf
 │                                 │
 │──── ICMP Echo Request ─────────→│
 │                                 │
 │←─── ICMP Echo Reply ────────────│
 │                                 │
 └── Vaxt ölçülür (ms ilə) ────────┘
```

1. Siz hədəf cihaza **ICMP Echo (sorğu) paketi** göndərirsiniz
2. Hədəf cihaz **ICMP Echo Reply (cavab paketi)** göndərir
3. Aralarındakı **vaxt (ms ilə)** ölçülür

### 💻 İstifadəsi

```bash
# Sintaksis:
ping <IP_ünvanı_və_ya_URL>

# Nümunələr:
ping 192.168.1.254       # Özəl şəbəkədəki cihaza
ping 8.8.8.8             # Google-un DNS serverinə
ping tryhackme.com       # Veb sayta
```

### 📊 Ping çıxışının oxunması

```
PING 192.168.1.254 (192.168.1.254) 56(84) bytes of data.
64 bytes from 192.168.1.254: icmp_seq=1 ttl=64 time=4.16 ms
64 bytes from 192.168.1.254: icmp_seq=2 ttl=64 time=3.82 ms

--- 192.168.1.254 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss
rtt min/avg/max = 3.82/4.16/5.20 ms
```

| Sahə | Məna |
|---|---|
| `bytes` | Göndərilən paketin ölçüsü |
| `icmp_seq` | Paketin sıra nömrəsi |
| `ttl` | Time To Live — paketin "ömrü" |
| `time` | Gedib-gəlmə vaxtı (ms) |
| `packet loss` | İtirilmiş paketlər (0% = mükəmməl əlaqə) |

### 🎯 Nə üçün istifadə edilir?

- ✅ Cihazın əlçatımlı olub-olmadığını yoxlamaq
- ✅ Şəbəkə gecikmə müddətini (latency) ölçmək
- ✅ Bağlantı problemlərini diaqnoz etmək
- ✅ Şəbəkə problemlərini troubleshoot etmək

### 📝 Task 4 Cavabları

| Sual | Cavab |
|---|---|
| Ping hansı protokolu istifadə edir? | `ICMP` |
| 10.10.10.10-u ping etmək üçün sintaksis? | `ping 10.10.10.10` |
| 8.8.8.8-i ping etdikdə flag? | *(interaktiv labdan alınır)* |

---

## 📚 Sual-Cavab Cədvəli (Ümumi)

| Task | Sual | Cavab |
|---|---|---|
| Task 1 | Bir-birinə bağlı cihazlar üçün termin? | `Network` |
| Task 2 | WWW-ni kim icad etdi? | `Tim Berners-Lee` |
| Task 3 | "IP" nəyin qısaltmasıdır? | `Internet Protocol` |
| Task 3 | IP ünvanının hər hissəsi necə adlanır? | `Octet` |
| Task 3 | IPv4 neçə hissədən ibarətdir? | `4` |
| Task 3 | "MAC" nəyin qısaltmasıdır? | `Media Access Control` |
| Task 4 | Ping hansı protokolu istifadə edir? | `ICMP` |
| Task 4 | 10.10.10.10-u ping etmək üçün sintaksis? | `ping 10.10.10.10` |

---

## 🧠 Öyrəndiklərimizin Xülasəsi

```
Şəbəkə (Network)
├── 2+ cihazın bir-birinə bağlanması
├── Özəl (Private) — ev/ofis şəbəkəsi
└── İctimai (Public) — İnternet

İnternet
├── Kiçik şəbəkələrin nəhəng şəbəkəsi
├── 1960-lar: ARPANET (ilk şəbəkə)
└── 1989: Tim Berners-Lee → WWW

Cihaz identifikasiyası
├── IP Ünvanı
│   ├── IPv4: 4 oktet, 2³² ünvan
│   └── IPv6: 2¹²⁸ ünvan (yeni nəsil)
└── MAC Ünvanı
    ├── 12 hex simvol, zavod tərəfindən verilir
    └── Spoofing ilə saxtalaşdırıla bilər ⚠️

Ping
├── ICMP protokolu istifadə edir
├── Echo Request → Echo Reply
└── Əlaqənin keyfiyyətini ölçür
```

---

## 🔗 Növbəti Addım

Bu labı tamamladıqdan sonra **"Intro to LAN"** labına keçin:
👉 [https://tryhackme.com/room/introtolan](https://tryhackme.com/room/introtolan)

Orada öyrənəcəksiniz:
- LAN topologiyaları (Ulduz, Şin, Halqa)
- Switch və Router nədir?
- DHCP və ARP protokolları

---

*Hazırladı: Azərbaycan dilində TryHackMe öyrənmə yazısı*
*Mənbə: [TryHackMe — What is Networking?](https://tryhackme.com/room/whatisnetworking)*
