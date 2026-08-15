# TryHackMe — OSI Model | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** OSI Model  
> **Çətinlik:** Easy (Info)  
> **Müddət:** 30 dəqiqə  
> **Link:** https://tryhackme.com/room/osimodelzi  

---

## Ümumi Baxış

Bu otaqda şəbəkələşmənin (networking) ən fundamental konseptlərindən biri olan **OSI Modeli** öyrənilir. OSI modeli — fərqli cihazların bir-biri ilə necə ünsiyyət qurduğunu izah edən 7 qatlı bir çərçivədir. Kibertəhlükəsizlik mütəxəssisləri üçün bu model mütləq bilinməlidir: hücumların harada baş verdiyini, paketlərin necə hərəkət etdiyini anlamaq üçün OSI modelini bilmək şərtdir.

---

## OSI Modeli — Ümumi Quruluş

```
┌─────────────────────────────────────┐
│  Layer 7 — Application (Tətbiq)     │
├─────────────────────────────────────┤
│  Layer 6 — Presentation (Təqdimat)  │
├─────────────────────────────────────┤
│  Layer 5 — Session (Sessiya)        │
├─────────────────────────────────────┤
│  Layer 4 — Transport (Nəqliyyat)    │
├─────────────────────────────────────┤
│  Layer 3 — Network (Şəbəkə)        │
├─────────────────────────────────────┤
│  Layer 2 — Data Link (Data Keçidi)  │
├─────────────────────────────────────┤
│  Layer 1 — Physical (Fiziki)        │
└─────────────────────────────────────┘
```

> **Əzbərləmə üçün: "All People Seem To Need Data Processing"**  
> (Application → Presentation → Session → Transport → Network → Data Link → Physical)

---

## Task 1 — OSI Modeli Nədir? (What is the OSI Model?)

### İzahat

**OSI** — **Open Systems Interconnection** (Açıq Sistemlər Qarşılıqlı Əlaqəsi) deməkdir.

Bu model müxtəlif istehsalçıların cihazlarının bir-biri ilə ünsiyyət qura bilməsi üçün standart bir çərçivə təyin edir. OSI modeli standartlaşdırılmadan əvvəl müxtəlif şirkətlərin cihazları bir-biri ilə "danışa" bilmirdi — sanki hər biri fərqli dil danışırdı.

OSI modeli bu problemi həll etdi: hər cihaz, fərqli dizayn və funksiyalara sahib olsa da, bu universal çərçivəyə uyğun danışdığı üçün bir-birini anlaya bilir.

**Encapsulation (İnkapsulyasiya) nədir?**

Məlumat göndərilərkən hər qatdan aşağıya keçdikcə o qata xas olan başlıq (header) məlumatına əlavə olunur. Bu proses zərfin içinə zərf qoymağa bənzəyir — hər qat öz "zərfini" əlavə edir. Bu prosesə **inkapsulyasiya** deyilir.

Məlumat ötürüldükdə əks tərəfdə bu prosess tərsinə işlənir — hər qat öz başlığını çıxarır. Bu prosesə **dekapsulasiya** deyilir.

---

### Suallar və Cavablar

> **Q: What does the "OSI" in "OSI Model" stand for?**  
> ✅ `Open Systems Interconnection`

> **Q: How many layers (in digits) does the OSI model have?**  
> ✅ `7`

> **Q: What is the key term for when pieces of information get added to data?**  
> ✅ `Encapsulation`

---

## Task 2 — Qat 1: Fiziki (Layer 1 — Physical)

### İzahat

Fiziki qat OSI modelinin ən aşağı qatıdır və ən sadə başa düşülən qatdır. Adından da göründüyü kimi, bu qat şəbəkələşmədə istifadə olunan **fiziki avadanlıqla** bağlıdır.

Bu qatda məlumat artıq fayl ya paket kimi deyil, **elektrik siqnalları**, işıq impulsu (fiber optik) və ya radio dalğaları kimi ötürülür. Bu siqnallar **0** və **1**-lərdən ibarət ikili (binary) say sistemini təmsil edir.

**Bu qata aid cihazlar və komponentlər:**
- Ethernet kabellər (Cat5e, Cat6 və s.)
- Fiber optik kabellər
- Hub-lar (ağa qoşulan hər şeyə siqnalı yayır)
- Şəbəkə Interfeys Kartları (NIC — Network Interface Card)

> **Vacib fərq:** Hub fiziki qatda işləyir — gələn siqnalı bütün portlara yayır, heç bir "ağıl" işlətmir. Switch isə Layer 2-də işləyir.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Physical`

> **Q: What is the name of the numbering system that is both 0's and 1's?**  
> ✅ `Binary`

> **Q: What is the name of the cables that are used to connect devices?**  
> ✅ `Ethernet Cables`

---

## Task 3 — Qat 2: Data Keçidi (Layer 2 — Data Link)

### İzahat

Data Keçidi qatı **fiziki ünvanlama** ilə məşğul olur. Yuxarıdakı Şəbəkə qatından (Layer 3) gələn məlumatı götürür və ona **alıcının MAC ünvanını** əlavə edir.

**MAC (Media Access Control) ünvanı nədir?**

Hər şəbəkə cihazında istehsalçı tərəfindən "yandırılmış" unikal bir fiziki ünvan var — buna MAC ünvanı deyilir. MAC ünvanı 6 baytlıq hex dəyəridir: `AA:BB:CC:DD:EE:FF`

Bu ünvan hardware-ə yazılıdır — dəyişdirilə bilməz (baxmayaraq ki, proqram vasitəsilə saxtalaşdırmaq mümkündür). Hər NIC (Şəbəkə Interfeys Kartı) öz unikal MAC ünvanına malikdir.

**Bu qatın vəzifəsi:**
1. Yuxarıdan gələn məlumatı **Frame** formatına çevirir
2. Alıcının MAC ünvanını əlavə edir
3. Məlumatın ötürülməyə hazır olmasını təmin edir
4. Məlumatın korlanıb-korlanmadığını yoxlamaq üçün sona trailer əlavə edir

> **IP ünvanı ilə MAC ünvanının fərqi:** IP ünvanı şəbəkələr arasında marşrutlaşdırma (routing) üçün istifadə olunur. MAC ünvanı isə yerli şəbəkə (LAN) daxilindəki hədəfi müəyyənləşdirmək üçün.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Data Link`

> **Q: What is the name of the piece of hardware that all networked devices come with?**  
> ✅ `Network Interface Card`

---

## Task 4 — Qat 3: Şəbəkə (Layer 3 — Network)

### İzahat

Şəbəkə qatı **marşrutlaşdırma** (routing) ilə məşğul olur — yəni məlumatın bir şəbəkədən digərinə ən optimal yolu tapıb çatdırılması.

Bu qatda hər şey **IP ünvanları** vasitəsilə idarə olunur. Router-lər məhz bu qatda işləyir — onlar IP ünvanlarını oxuyub məlumatın hansı istiqamətə göndərilməli olduğunu müəyyənləşdirir.

**Ən optimal yolu tapmaq üçün routing protokolları:**

| Protokol | Tam adı | Necə işləyir |
|----------|---------|--------------|
| **OSPF** | Open Shortest Path First | Şəbəkənin tam xəritəsini çıxarır, ən qısa yolu hesablayır |
| **RIP** | Routing Information Protocol | Hop sayına görə yolu seçir (maks. 15 hop) |

**Router ən yaxşı yolu seçərkən nəyə baxır?**
- **Yolun uzunluğu:** Neçə router-dən keçmək lazımdır?
- **Etibarlılıq:** Bu yolda əvvəlcədən paket itkisi olubmu?
- **Sürət:** Mis kabel mi, fiber optik mi?

> **Layer 3 cihazı:** Router — IP ünvanlarını oxuyub marşrutlaşdırma qərarı verən ağıllı cihaz.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Network`

> **Q: Will packets take the most optimal route across a network? (Y/N)**  
> ✅ `Y`  
> *İzah: Routing protokolları ən optimal yolu seçmək üçün hazırlanmışdır.*

> **Q: What does the acronym "OSPF" stand for?**  
> ✅ `Open Shortest Path First`

> **Q: What does the acronym "RIP" stand for?**  
> ✅ `Routing Information Protocol`

> **Q: What type of addresses are dealt with at this layer?**  
> ✅ `IP Addresses`

---

## Task 5 — Qat 4: Nəqliyyat (Layer 4 — Transport)

### İzahat

Nəqliyyat qatı məlumatın necə göndəriləcəyini idarə edir. Bu qatda iki əsas protokoldan istifadə olunur: **TCP** və **UDP**. Hansını seçmək təlimatın tələbindən asılıdır — etibarlılıq mı, sürət mi?

---

### TCP — Transmission Control Protocol

TCP **etibarlılığı** ön plana çəkir. İki cihaz arasında sabit əlaqə qurulur və hər bir paketin düzgün çatıb-çatmadığı yoxlanılır.

TCP-ni puzzle kimi düşün: hər parçanın nömrəsi var. Alıcı tərəf bir parça əldə etmədikdə: "Dur, 4-cü parça gəlmədi, yenidən göndər!" deyir. Bütün parçalar gəlib düzgün yerləşdikdən sonra mənzərə tamamlanır.

| Üstünlüklər | Çatışmazlıqlar |
|-------------|----------------|
| Məlumatın düzgünlüyünə zəmanət verir | Sabit əlaqə tələb edir |
| Xəta yoxlaması var | Əlavə "əl sıxışma" (handshake) olduğundan yavaşdır |
| Cihazları sinxronlaşdırır | Bir paket itdikdə axın dayanır |

**TCP-nin istifadə sahələri:** Veb browsing (HTTP/HTTPS), E-poçt (SMTP), Fayl ötürmə (FTP) — bütün məlumatın eksiksiz çatması lazım olan yerlər.

---

### UDP — User Datagram Protocol

UDP **sürəti** ön plana çəkir. Heç bir əlaqə qurmadan məlumatı göndərir — çatan çatır, çatmayan çatmır.

UDP-ni fasiləsiz axan su kimi düşün: nə itir itir, növbəti gəlir. Video axını zamanı 1-2 frame-in itməsi problemi deyil — axın davam edir.

| Üstünlüklər | Çatışmazlıqlar |
|-------------|----------------|
| TCP-dən əhəmiyyətli dərəcədə sürətlidir | Məlumatın çatmasına zəmanət yoxdur |
| Cihaza az yük salır | Xəta yoxlaması yoxdur |
| Geliştiricilər üçün çevik | Qeyri-sabit əlaqədə keyfiyyət aşağı düşür |

**UDP-nin istifadə sahələri:** Video axını (Netflix, YouTube), VoIP (Zoom, zənglər), Onlayn oyunlar, DHCP, ARP — sürətin önəmli olduğu, kiçik itkinin qəbul edilə bildiyi yerlər.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Transport`

> **Q: What does TCP stand for?**  
> ✅ `Transmission Control Protocol`

> **Q: What does UDP stand for?**  
> ✅ `User Datagram Protocol`

> **Q: What protocol guarantees the accuracy of data?**  
> ✅ `TCP`

> **Q: What protocol doesn't care if data is received or not by the other device?**  
> ✅ `UDP`

> **Q: What protocol would an application such as an email client use?**  
> ✅ `TCP`  
> *İzah: E-poçtun hər hissəsi tam çatmalıdır — yarım e-poçt olmaz.*

> **Q: What protocol would an application that downloads files use?**  
> ✅ `TCP`  
> *İzah: Fayl yükləməsinin tam olması şərtdir, yarım fayl işləməz.*

> **Q: What protocol would an application that streams video use?**  
> ✅ `UDP`  
> *İzah: Video axınında sürət önəmlidir, 1-2 frame itməsi qəbul edilə bilər.*

---

## Task 6 — Qat 5: Sessiya (Layer 5 — Session)

### İzahat

Sessiya qatı məlumat formatlaşdırıldıqdan (Layer 6) sonra devreye girir. Bu qatın əsas vəzifəsi — iki kompüter arasında **əlaqə yaratmaq, saxlamaq və bitirmək**.

**Sessiya qatının funksiyaları:**

1. **Sessiya yaratmaq:** İki cihaz əlaqə qurduğunda, unikal bir "sessiya" açılır. Bu sessiya əlaqənin kimlik kartı kimi işləyir.

2. **Unikallıq:** Hər sessiya müstəqildir. Məlumat eyni anda bir neçə sessiyaya paylaşdırıla bilməz — hər məlumat öz sesiyasında qalır.

3. **Yoxlama nöqtələri (Checkpoints):** Böyük məlumat ötürülərkən sessiya qatı ara nöqtələr (checkpoint) qoyur. Əgər əlaqə kəsilsə, ötürülmə sıfırdan başlamır — sonuncu yoxlama nöqtəsindən davam edir. Bu, pul və vaxt qənaətidir.

4. **Sessiyanı bitirmək:** Məlumat tam ötürüldükdən sonra və ya əlaqə uzun müddət fəal olmadıqda sessiya bağlanır.

> **Həqiqi həyat nümunəsi:** Böyük bir fayl yükləyərkən internet kəsildikdə bəzi brauserlər faylı sıfırdan başlatmadan davam etdirir — bu məhz sessiya qatının checkpoint mexanizmi sayəsindədir.

---

### Suallar və Cavablar

> **Q: What is the name of this layer?**  
> ✅ `Session`

> **Q: What is the technical term for when a connection is successfully established?**  
> ✅ `Session`

> **Q: What is the technical term for "small chunks of data"?**  
> ✅ `Packets`  
> *İzah: Sessiya qatı məlumatı kiçik hissələrə — paketlərə (packets) bölür.*

---

## Task 7 — Qat 6: Təqdimat (Layer 6 — Presentation)

### İzahat

Təqdimat qatı **standartlaşdırma** ilə məşğul olur. Müxtəlif proqramlar məlumatı fərqli formatlarda saxlayır. Bu qat "tərcüməçi" kimi işləyərək məlumatı hər iki tərəfin anlayacağı formata çevirir.

**Niyə lazımdır?** iPhone-dan Outlook istifadəçisinə e-poçt göndərəndə hər iki cihaz fərqli sistem işlədir. Təqdimat qatı məktubu, şəkilləri, formatlamanı hər iki tərəfdə eyni göstərilməsini təmin edir.

**Təqdimat qatının üç əsas funksiyası:**

| Funksiya | İzahat |
|----------|--------|
| **Tərcümə (Translation)** | Məlumatı bir formatdan digərinə çevirir (məs. ASCII → Unicode) |
| **Sıxışdırma (Compression)** | Şəbəkə üzərindən ötürülən məlumatın həcmini azaldır |
| **Şifrələmə (Encryption)** | Məlumatı şifrələyir (HTTPS/SSL-TLS buradadır) |

> **Kibertəhlükəsizlik baxımından:** HTTPS əlaqələrindəki SSL/TLS şifrələməsi məhz bu qatda baş verir. Man-in-the-Middle hücumları bu qatı hədəf alır.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Presentation`

> **Q: What is the main purpose that this Layer acts as?**  
> ✅ `Translator`

---

## Task 8 — Qat 7: Tətbiq (Layer 7 — Application)

### İzahat

Tətbiq qatı OSI modelinin ən yuxarı qatıdır və istifadəçilərin gündəlik həyatda birbaşa işlədikləri qatdır. Bu qat — istifadəçinin şəbəkə xidmətlərinə çıxış nöqtəsidir.

İstifadəçi ilə məlumat arasında interfeys rolunu oynayır. Əksər proqramlar bu qatı istifadəçi üçün asanlaşdırmaq məqsədilə **GUI (Graphical User Interface — Qrafik İstifadəçi İnterfeysi)** — düymələr, pəncərələr, menyular — yaradır.

**Bu qatda işləyən ümumi protokollar:**

| Protokol | İstifadəsi |
|----------|-----------|
| **HTTP / HTTPS** | Veb browsing |
| **DNS** | Domen adını IP ünvanına çevirir (google.com → 142.250.x.x) |
| **FTP / SFTP** | Fayl ötürmə (FileZilla kimi proqramlar) |
| **SMTP / POP3 / IMAP** | E-poçt göndərmə/qəbul |
| **SSH** | Uzaqdan təhlükəsiz terminal əlaqəsi |

> **Kibertəhlükəsizlik baxımından:** SQL Injection, XSS (Cross-Site Scripting), Directory Traversal kimi hücumlar Layer 7-yi hədəf alır. WAF (Web Application Firewall) bu qatı qoruyur.

---

### Suallar və Cavablar

> **Q: What is the name of this Layer?**  
> ✅ `Application`

> **Q: What is the technical term that is given to the name of the software that users interact with?**  
> ✅ `Graphical User Interface`

---

## Task 9 — Praktiki: OSI Oyunu (Practical — OSI Game)

### İzahat

Bu task — öyrəndiklərini praktikada sınadığın interaktiv bir oyundur. Ekranda bir zindanın (dungeon) içindəsən, düzgün qapilardan keçərək çıxmaq lazımdır.

**Necə oynamalı:**
- Kişini **sol/sağ** ox düymələriylə hərəkət etdir
- Düzgün qapından keçmək üçün **Space** basın
- Qatları **1-dən 7-yə** (Physical → Application) doğru ardıcıl seç
- Yanlış qapıdan girsən oyun bitir, yenidən başlayırsan

**Düzgün ardıcıllıq (aşağıdan yuxarıya):**

```
1. Physical       (Fiziki)
2. Data Link      (Data Keçidi)
3. Network        (Şəbəkə)
4. Transport      (Nəqliyyat)
5. Session        (Sessiya)
6. Presentation   (Təqdimat)
7. Application    (Tətbiq)
```

Bütün qatlardan düzgün keçsən, flag ekrana çıxır.

---

### Sual və Cavab

> **Q: Escape the dungeon to retrieve the flag. What is the flag?**  
> ✅ `THM{OSI_DUNGEON_ESCAPED}`

---

## Task 10 — Davam et: Paketlər və Freymler (Continue Your Learning: Packets & Frames)

Bu son task yalnız sizi növbəti otağa yönləndirir.

> **Q: Join the "Packets and Frames" room.**  
> ✅ Cavab tələb olunmur — "Completed" düyməsinə bas.

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | "OSI" nəyin qısaltmasıdır? | `Open Systems Interconnection` |
| Task 1 | OSI neçə qatdan ibarətdir? | `7` |
| Task 1 | Məlumata hissələr əlavə olunmasının texniki adı? | `Encapsulation` |
| Task 2 | Bu qatın adı nədir? (Fiziki) | `Physical` |
| Task 2 | 0 və 1-lərdən ibarət say sistemi? | `Binary` |
| Task 2 | Cihazları birləşdirən kabellərin adı? | `Ethernet Cables` |
| Task 3 | Bu qatın adı nədir? (Data Keçidi) | `Data Link` |
| Task 3 | Bütün şəbəkə cihazlarında olan avadanlığın adı? | `Network Interface Card` |
| Task 4 | Bu qatın adı nədir? (Şəbəkə) | `Network` |
| Task 4 | Paketlər ən optimal yolu seçirmi? | `Y` |
| Task 4 | OSPF nəyin qısaltmasıdır? | `Open Shortest Path First` |
| Task 4 | RIP nəyin qısaltmasıdır? | `Routing Information Protocol` |
| Task 4 | Bu qatda hansı ünvanlar işlənir? | `IP Addresses` |
| Task 5 | Bu qatın adı nədir? (Nəqliyyat) | `Transport` |
| Task 5 | TCP nəyin qısaltmasıdır? | `Transmission Control Protocol` |
| Task 5 | UDP nəyin qısaltmasıdır? | `User Datagram Protocol` |
| Task 5 | Hansı protokol məlumatın düzgünlüyünə zəmanət verir? | `TCP` |
| Task 5 | Hansı protokol məlumatın çatıb-çatmadığına məhəl qoymur? | `UDP` |
| Task 5 | E-poçt müştərisi hansı protokoldan istifadə edər? | `TCP` |
| Task 5 | Fayl yükləyən tətbiq hansı protokoldan istifadə edər? | `TCP` |
| Task 5 | Video axını tətbiqi hansı protokoldan istifadə edər? | `UDP` |
| Task 6 | Bu qatın adı nədir? (Sessiya) | `Session` |
| Task 6 | Əlaqə uğurla qurulduqda texniki termin? | `Session` |
| Task 6 | "Kiçik məlumat parçaları" üçün texniki termin? | `Packets` |
| Task 7 | Bu qatın adı nədir? (Təqdimat) | `Presentation` |
| Task 7 | Bu qatın əsas rolu nədir? | `Translator` |
| Task 8 | Bu qatın adı nədir? (Tətbiq) | `Application` |
| Task 8 | İstifadəçilərin işlədiyi proqramların texniki adı? | `Graphical User Interface` |
| Task 9 | Zindandan çıxaraq flaqı tap. Flag nədir? | `THM{OSI_DUNGEON_ESCAPED}` |
| Task 10 | Növbəti otağa qoşul | No answer needed |

---

## Bonus: OSI Qatları — Kibertəhlükəsizlik Perspektivi

| Qat | Ad | Hücum növləri |
|-----|----|---------------|
| 7 | Application | SQL Injection, XSS, CSRF |
| 6 | Presentation | SSL Stripping, Şifrə hücumları |
| 5 | Session | Session Hijacking, Cookie Theft |
| 4 | Transport | SYN Flood (DoS), Port Scanning |
| 3 | Network | IP Spoofing, MITM, Routing Attack |
| 2 | Data Link | ARP Spoofing, MAC Flooding |
| 1 | Physical | Kabel kəsmə, Fiziki müdaxilə |
