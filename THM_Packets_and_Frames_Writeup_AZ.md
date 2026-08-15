# TryHackMe — Packets & Frames | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** Packets & Frames  
> **Çətinlik:** Easy  
> **Müddət:** 30 dəqiqə  
> **Link:** https://tryhackme.com/room/packetsframes  

---

## Ümumi Baxış

Bu otaqda məlumatın şəbəkə üzərindən ötürülməsinin "atom" səviyyəsinə — **paket** (packet) və **freym** (frame) anlayışlarına baxırıq. OSI modelinin davamı kimi bu otaqda TCP-nin 3-addımlı əl sıxışması (Three-Way Handshake), UDP-nin sadə quruluşu və port sisteminin necə işlədiyini öyrənirik.

---

## Task 1 — Paket və Freym Nədir? (What are Packets and Frames?)

### İzahat

Şəbəkə üzərindən böyük bir məlumat göndərildikdə, o məlumat bütövlükdə göndərilmir — **kiçik hissələrə** bölünür. Bu hissələr birləşərək tam məlumatı əmələ gətirir. Bu kiçik hissələr **paket** və ya **freym** adlanır.

**Bəs paket ilə freym arasındakı fərq nədir?**

Bu iki anlayış OSI modelinin fərqli qatlarına aiddir:

```
┌─────────────────────────────────────────────────────────┐
│  PAKET (Packet)                                         │
│  • Layer 3 (Network qatı) ilə bağlıdır                 │
│  • IP ünvanı məlumatı ehtiva edir                       │
│  • Şəbəkələr arasında marşrutlaşdırma üçün istifadə    │
│    olunur                                               │
├─────────────────────────────────────────────────────────┤
│  FREYM (Frame)                                          │
│  • Layer 2 (Data Link qatı) ilə bağlıdır               │
│  • IP ünvanı məlumatı YOXDUR                            │
│  • Yalnız MAC ünvanları ehtiva edir                     │
│  • Yerli şəbəkə (LAN) daxilindəki ötürmə üçün         │
└─────────────────────────────────────────────────────────┘
```

**Zərf analoji:** Bunu iki zərfə bənzətmək olar. Xarici zərf — paketdir (üstündə IP ünvanı var, poçtdan keçir). Xarici zərfi açdıqda içindəki zərf — freymdir (IP məlumatı yoxdur, yalnız yerli çatdırılma üçündür).

**Niyə məlumat hissələrə bölünür?**

- Böyük məlumatı bütövlükdə göndərmək şəbəkədə tıxac (bottleneck) yaradır.
- Kiçik paketlər paralel ötürülə bilər — hər biri fərqli yoldan getdikdən sonra alıcı tərəfdə yenidən birləşir.
- Məsələn: veb saytdan şəkil yüklədikdə, şəkil bütöv deyil, onlarla kiçik paket kimi gəlir və brauzer onları birləşdirib tam şəkli göstərir.

**IP paketinin başlıqları (headers):**

| Başlıq | Məzmunu |
|--------|---------|
| **Time to Live (TTL)** | Paketin nə qədər "router"-dən keçə biləcəyini göstərir; 0-a çatdıqda paket atılır |
| **Checksum** | Məlumatın korlanıb-korlanmadığını yoxlamaq üçün |
| **Source Address** | Göndərənin IP ünvanı |
| **Destination Address** | Alıcının IP ünvanı |

---

### Suallar və Cavablar

> **Q: What is the name for a piece of data when it does have IP addressing information?**  
> ✅ `Packet`  
> *İzah: IP ünvanı ehtiva edən məlumat hissəsi paket adlanır (Layer 3-də işlənir).*

> **Q: What is the name for a piece of data when it does not have IP addressing information?**  
> ✅ `Frame`  
> *İzah: IP ünvanı olmayan məlumat hissəsi freym adlanır (Layer 2-də işlənir).*

---

## Task 2 — TCP/IP (Üç Addımlı Əl Sıxışma)

### İzahat

**TCP (Transmission Control Protocol)** — OSI modelinin Transport qatında işləyən, **etibarlı əlaqə** təmin edən bir protokoldur.

TCP-nin əsas xüsusiyyəti: məlumat göndərilməzdən əvvəl iki cihaz arasında **sabit əlaqə** qurulur. Bu əlaqə qurulma prosesi **Three-Way Handshake** (Üç Addımlı Əl Sıxışma) adlanır.

#### TCP Paketinin Başlıqları

| Başlıq | Vəzifəsi |
|--------|----------|
| **Source Port** | Göndərən cihazın port nömrəsi (0-65535 arası təsadüfi seçilir) |
| **Destination Port** | Alıcı cihazın port nömrəsi (məs. 80 = HTTP) |
| **Source IP** | Göndərənin IP ünvanı |
| **Destination IP** | Alıcının IP ünvanı |
| **Sequence Number** | Paketin sıra nömrəsi — məlumatı düzgün yığmaq üçün |
| **Acknowledgement Number** | Alınan paketin təsdiqi — `sıra nömrəsi + 1` |
| **Checksum** | Məlumatın bütövlüyünü (integrity) yoxlayan dəyər |
| **Data** | Faktiki ötürülən məlumat |
| **Flag** | Paketin növünü bildirir: SYN, ACK, FIN, RST, PSH, URG |

---

### Three-Way Handshake (Üç Addımlı Əl Sıxışma)

TCP əlaqəsi qurmaq üçün **3 addım** lazımdır. Bu prosesi Alice (müştəri) və Bob (server) arasında nümunə ilə izah edək:

```
Alice (Müştəri)                        Bob (Server)
      |                                     |
      |──── SYN (ISN=0) ──────────────────>|
      |     "Salam Bob, əlaqə quraq.        |
      |      Mənim başlangıç nömrəm 0-dır" |
      |                                     |
      |<─── SYN/ACK (ISN=5000, ACK=0) ────|
      |     "Salam Alice, mənim nömrəm      |
      |      5000-dir, sənin 0-ını          |
      |      aldım (ACK)"                   |
      |                                     |
      |──── ACK (ACK=5000+1=5001) ────────>|
      |     "Sənin 5000-ini aldım,          |
      |      əlaqə quruldu!"               |
      |                                     |
      |═════ MƏLUMAT ÖTÜRÜLƏBİLƏR ════════|
```

**Addımların izahı:**

**1. SYN (Synchronize — Sinxronizasiya):**
- Müştəri serverə "əlaqə quraq" siqnalı göndərir.
- Öz **İlk Sıra Nömrəsini** (ISN — Initial Sequence Number) bildirir.
- Niyə sıra nömrəsi lazımdır? — Məlumat fraqmentlərini düzgün ardıcıllıqla yığmaq üçün.

**2. SYN/ACK:**
- Server müştərinin SYN-ini alır, öz ISN-ini göndərir.
- Həm müştərinin nömrəsini təsdiqləyir (ACK), həm də öz nömrəsini sinxronizasiya edir (SYN).
- İkisi birlikdə: SYN/ACK.

**3. ACK (Acknowledge — Təsdiq):**
- Müştəri serverin ISN-ini aldığını bildirir.
- Rəsmi olaraq əlaqə qurulur — məlumat ötürmək başlaya bilər.

---

### TCP Əlaqəsini Bağlamaq (FIN)

TCP sistem resurslarını tutan bir protokol olduğundan, işi bitdikdə əlaqəni bağlamaq vacibdir. Bağlama prosesi:

```
Alice                          Bob
  |──── FIN ─────────────────>|   "Bob, işim bitdi, bağlayaq"
  |<─── FIN/ACK ──────────────|   "Aldım, mən də bağlamaq istəyirəm"
  |──── ACK ─────────────────>|   "Tamam, bağlandı"
```

**Digər əsas TCP flagları:**

| Flag | Mənası |
|------|--------|
| **SYN** | Əlaqə başlatmaq üçün |
| **ACK** | Paketin alındığını təsdiqləmək üçün |
| **FIN** | Əlaqəni düzgün şəkildə bağlamaq üçün |
| **RST** | Əlaqəni zorla kəsmək üçün (xəta vəziyyəti) |
| **PSH** | Məlumatı buferdən dərhal ötür |
| **URG** | Paketin təcili olduğunu bildir |

---

### Suallar və Cavablar

> **Q: What is the header in a TCP packet that ensures the integrity of data?**  
> ✅ `Checksum`  
> *İzah: Checksum başlığı məlumatın ötürülmə zamanı dəyişib-dəyişmədiyini yoxlayır — bu, TCP-nin bütövlük (integrity) zəmanətidir.*

> **Q: Provide the order of a normal Three-way handshake (with each step separated by a comma)**  
> ✅ `SYN,SYN/ACK,ACK`  
> *İzah: Müştəri SYN göndərir → Server SYN/ACK ilə cavab verir → Müştəri ACK ilə təsdiqləyir.*

---

## Task 3 — Praktiki: Əl Sıxışma (Practical — Handshake)

### İzahat

Bu task interaktiv bir simulyasiyadır. Alice və Bob arasındakı TCP əl sıxışmasını düzgün ardıcıllıqla yenidən qurmaq lazımdır.

**Simulyasiyanın gedişatı:**

Ekranda bir söhbət interfeysi görünür. Hər addımda doğru mesajı seçib göndərməlisən:

```
Addım 1: Alice → SYN göndər
Addım 2: Bob  → SYN/ACK ilə cavab ver
Addım 3: Alice → ACK ilə təsdiqlə
Addım 4: Alice → DATA göndər ("Cheesecake is on sale!")
Addım 5: Bob  → ACK (məlumatı aldım)
Addım 6: Alice → FIN/ACK (bitirdim, bağlayaq)
Addım 7: Bob  → FIN/ACK (mən də bağlamaq istəyirəm)
Addım 8: Alice → ACK (tamam, bağlandı)
```

Bütün addımları düzgün tamamladıqda flag ekrana çıxır.

---

### Sual və Cavab

> **Q: What is the value of the flag given at the end of the conversation?**  
> ✅ `THM{TCP_CHATTER}`

---

## Task 4 — UDP/IP

### İzahat

**UDP (User Datagram Protocol)** — TCP-nin "sürətli qardaşıdır." Əlaqə qurmadan birbaşa məlumat göndərir.

**UDP-nin əsas fərqi:** UDP **stateless** (vəziyyətsiz) protokoldur — iki cihaz arasında sabit əlaqə qurulmur, Three-Way Handshake baş vermir, sinxronizasiya yoxdur.

#### TCP ilə UDP-nin Müqayisəsi

| Xüsusiyyət | TCP | UDP |
|-----------|-----|-----|
| Əlaqə növü | Stateful (vəziyyətli) | Stateless (vəziyyətsiz) |
| Handshake | Var (SYN/SYN-ACK/ACK) | Yoxdur |
| Çatdırılma zəmanəti | Var | Yoxdur |
| Xəta yoxlaması | Var | Yoxdur |
| Sürət | Yavaş (əlavə overhead) | Sürətli |
| Paket itkisi | Yenidən göndərilir | İtir, davam edir |

#### UDP-nin üstünlükləri

- TCP-dən əhəmiyyətli dərəcədə **sürətlidir** (əlaqə qurmaq, təsdiq gözləmək yoxdur)
- Cihaza az yük salır
- Proqram tərtibatçıları üçün daha çevikdir

#### UDP-nin çatışmazlıqları

- Məlumatın çatmasına zəmanət yoxdur
- Xəta yoxlaması yoxdur
- Qeyri-sabit əlaqədə keyfiyyət aşağı düşür

#### UDP paketinin başlıqları (TCP-yə nisbətən çox sadədir)

| Başlıq | Vəzifəsi |
|--------|----------|
| **Time to Live (TTL)** | Paketin "ömrü" |
| **Source Address** | Göndərənin IP ünvanı |
| **Destination Address** | Alıcının IP ünvanı |
| **Source Port** | Göndərənin port nömrəsi |
| **Destination Port** | Alıcının port nömrəsi |
| **Data** | Ötürülən məlumat |

> UDP-də **Sequence Number**, **Acknowledgement Number** və **Checksum** (TCP mənasında) kimi başlıqlar yoxdur — bu, UDP-ni çox yüngül edir.

#### UDP-nin istifadə sahələri

UDP hər yerdə işlənmir — yalnız kiçik itki qəbul edilə bilən, sürətin önəmli olduğu hallarda:

- **Video axını** (Netflix, YouTube) — 1-2 frame itməsi problem deyil
- **VoIP / Video zənglər** (Zoom, Teams) — kiçik fasilə qəbul edilə bilər
- **Onlayn oyunlar** — gecikməni (latency) azaltmaq kritikdir
- **DNS** — qısa sorğulardır, sürət önəmlidir
- **DHCP** — ilk IP ünvanını almaq üçün

---

### Suallar və Cavablar

> **Q: What does the term "UDP" stand for?**  
> ✅ `User Datagram Protocol`

> **Q: What type of connection is "UDP"?**  
> ✅ `Stateless`  
> *İzah: UDP sabit əlaqə qurmur — hər paket müstəqil, ardıcıllıqsız göndərilir.*

> **Q: What protocol would you use to transfer a file?**  
> ✅ `TCP`  
> *İzah: Fayl ötürmədə hər bir bit önəmlidir — yarım fayl işləməz. TCP bütün hissələrin düzgün çatmasını zəmanət verir.*

> **Q: What protocol would you use to have a video call?**  
> ✅ `UDP`  
> *İzah: Video zəngdə sürət önəmlidir; kiçik gecikmə və ya itki qəbul edilə bilər — UDP burada idealdır.*

---

## Task 5 — Portlar 101 (Praktiki) — Ports 101 (Practical)

### İzahat

**Port nədir?**

Port — şəbəkə cihazında məlumatın daxil olub-çıxa biləcəyi rəqəmsal "qapı"dır. Liman analoji ilə düşün: limanda fərqli qamallı gəmilər fərqli rıhtımlara (port) bağlanır — balıq gəmisi kruiz gəmisinin yerinə yanaşa bilməz.

Portlar **0 ilə 65535** arasında rəqəmsal dəyərlərdir. Məlumat bu portlar vasitəsilə hansı xidmətə aiddir — müəyyənləşir.

**Niyə portlar lazımdır?**

Bir kompüterdə eyni anda brauzer, e-poçt, SSH terminali işləyə bilər. Gələn məlumatın hansı proqrama aid olduğunu **port nömrəsinə** görə fərqləndiririk:
- 80-ci porta gələn məlumat → brauzərə gedir (HTTP)
- 22-ci porta gələn → SSH terminalına gedir
- 25-ci porta gələn → e-poçt xidmətinə gedir

---

### Tanınmış Portlar (0–1024 arası)

| Port | Protokol | İstifadəsi |
|------|----------|-----------|
| **21** | FTP | Fayl Ötürmə Protokolu |
| **22** | SSH | Təhlükəsiz uzaqdan giriş |
| **23** | Telnet | Şifrəsiz uzaqdan giriş (köhnə, təhlükəli) |
| **25** | SMTP | E-poçt göndərmə |
| **53** | DNS | Domen adından IP ünvanına çevirmə |
| **80** | HTTP | Şifrəsiz veb trafiki |
| **443** | HTTPS | Şifrəli veb trafiki (SSL/TLS) |
| **3389** | RDP | Windows uzaqdan idarəetmə |

> **Qeyd:** Bu standart portlardır, lakin dəyişdirilə bilər. Məsələn, bir veb server 80 əvəzinə **8080** portunda da işləyə bilər — bu zaman URL-ə `:8080` əlavə etmək lazımdır: `http://example.com:8080`

---

### Praktiki Tapşırıq

Tapşırıq: Brauzerdə açılan saytda **IP ünvanı `8.8.8.8`** və **port `1234`** olaraq daxil et, flag alacaqsan.

```
IP:   8.8.8.8
Port: 1234
```

Bağlandıqdan sonra flag ekranda görünür.

---

### Sual və Cavab

> **Q: What is the flag received from the challenge?**  
> ✅ `THM{YOU_CONNECTED_TO_A_PORT}`

---

## Task 6 — Davam et: Şəbəkəni Genişlət (Continue Your Learning)

Bu son task yalnız sizi növbəti otağa yönləndirir.

> **Q: Terminate the static site lab deployed in tasks 3 and 5.**  
> ✅ No Answer Needed

> **Q: Join the "Extending Your Network" room to continue your learning.**  
> ✅ No Answer Needed

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | IP ünvanlı məlumat hissəsinin adı? | `Packet` |
| Task 1 | IP ünvansız məlumat hissəsinin adı? | `Frame` |
| Task 2 | TCP paketdə bütövlüyü təmin edən başlıq? | `Checksum` |
| Task 2 | Normal Three-Way Handshake-in ardıcıllığı? | `SYN,SYN/ACK,ACK` |
| Task 3 | Söhbətin sonunda verilən flaqın dəyəri? | `THM{TCP_CHATTER}` |
| Task 4 | "UDP" nəyin qısaltmasıdır? | `User Datagram Protocol` |
| Task 4 | UDP hansı növ əlaqədir? | `Stateless` |
| Task 4 | Fayl ötürmə üçün hansı protokol? | `TCP` |
| Task 4 | Video zəng üçün hansı protokol? | `UDP` |
| Task 5 | Challenge-dan alınan flag? | `THM{YOU_CONNECTED_TO_A_PORT}` |
| Task 6 | Lab-ı sonlandır | No Answer Needed |
| Task 6 | Növbəti otağa qoşul | No Answer Needed |

---

## Bonus: Paket, Freym, TCP, UDP — Qısa Xülasə

```
MƏLUMAT GÖNDƏRİLMƏ PROSESİ:

[Proqram]
    ↓ (Layer 7-5: Application/Presentation/Session)
[Segment] ← Layer 4 (Transport): TCP/UDP başlığı əlavə olunur
    ↓
[Paket]   ← Layer 3 (Network): IP ünvanları əlavə olunur
    ↓
[Freym]   ← Layer 2 (Data Link): MAC ünvanları əlavə olunur
    ↓
[Bit-lər] ← Layer 1 (Physical): 0 və 1 kimi kabel üzərindən ötürülür
```

**TCP Handshake xülasəsi:**
```
Müştəri  ──SYN──►  Server     (Salam, əlaqə quraq!)
Müştəri  ◄──SYN/ACK──  Server (Salam, hazıram!)
Müştəri  ──ACK──►  Server     (Tamam, başlayaq!)
         ══DATA══               (Məlumat axını)
Müştəri  ──FIN──►  Server     (Bitirdim, bağlayaq)
Müştəri  ◄──FIN/ACK──  Server (Mən də bağlayıram)
Müştəri  ──ACK──►  Server     (Bağlandı)
```
