# 🖥️ Inside a Computer System

> **TryHackMe Room:** [tryhackme.com/room/insideacomputer](https://tryhackme.com/room/insideacomputer)  
> **Modul:** Computer Fundamentals  
> **Müddət:** ~45 dəqiqə

---

## Task 1 — Giriş (Introduction)

Kibertəhlükəsizliyi öyrənməzdən əvvəl nəyi qoruyacağını anlamaq lazımdır.

> *"Qaladan xəbərsiz keşikçi — onu qoruya bilməz. Xəzinə otağı haradadır? Kim içəriyə girir? Bu suallar olmadan müdafiə mümkün deyil."*

Kompüter sistemini öyrənməyin məqsədi budur: sistemin "xəritəsini" başa düşmək. Bu room kompüterin nədən ibarət olduğunu, hissələrinin bir-biri ilə necə əlaqəli olduğunu və sistemin necə işə başladığını izah edir.

**Öyrənmə hədəfi:** Kompüterin əsas komponentlərini tanımaq və onların funksiyalarını başa düşmək.

---

## Task 2 — Kompüterin Daxili (Inside a Computer System)

Hər kompüter sistemi eyni əsas hissələrdən ibarətdir. Bu hissələrin hər birinin öz vəzifəsi var və birlikdə kompüteri işlək saxlayırlar.

Lab boyunca hər komponenti **insan bədəni** ilə müqayisə edərək izah edilir — bu analogy anlayışı çox asanlaşdırır.

---

### 🦴 Motherboard (Ana Platası)
**Bədəndəki qarşılığı:** Skelet + Sinir sistemi

Motherboard bütün komponentləri bir yerdə saxlayır və onları bir-birinə bağlayır. Tipik bir desktop motherboard-da CPU soketi, RAM slotları, genişləndirmə slotları və müxtəlif portlar olur. Hər digər komponent birbaşa motherboard-a qoşulur və ya onun vasitəsilə əlaqə qurur.

---

### 🧠 CPU — Central Processing Unit
**Bədəndəki qarşılığı:** Beyin

CPU (Mərkəzi Prosessor), tez-tez sadəcə "prosessor" adlandırılır. Beynimiz necə ardıcıl olaraq əmrləri yerinə yetirirsə (rəqəm topla, süd tökürdü və s.), CPU da kompüter üçün eyni şeyi edir. Müasir CPU-lar paralel işləyən bir neçə nüvəyə (core) malikdir. CPU, motherboard-dakı CPU sокetinə qoşulur.

---

### 🧠 RAM — Random Access Memory
**Bədəndəki qarşılığı:** Qısamüddətli / İş yaddaşı

RAM bir tapşırıq üzərində işləyərkən lazım olan məlumatı müvəqqəti saxlayır — beynimizin "hazırda yadda saxladıqları" kimidir. RAM **volatil**dir: elektrik kəsilən kimi içindəki hər şey silinir. Müasir RAM modulları DDR5 və ya DDR6 kimi texnologiyalardan istifadə edir.

---

### 💾 Storage — SSD / HDD
**Bədəndəki qarşılığı:** Uzunmüddətli yaddaş

Yaddaş cihazları məlumatı daimi saxlayır — xoş xatirələr kimi həmişəlik qalır. Fərqlər:

- **HDD (Hard Disk Drive):** Köhnə texnologiya, hərəkətli hissələri var, yavaş, lakin böyük tutum və aşağı qiymətə görə populyar olaraq qalır.
- **SSD (Solid State Drive):** Hərəkətli hissəsi yoxdur, yaddaş çipləri istifadə edir, çox daha sürətlidir.

Yaddaş cihazları SATA kabeli və ya PCI Express slotu vasitəsilə qoşulur.

---

### 🌐 Network Adapter (Şəbəkə Adapteri)
**Bədəndəki qarşılığı:** Səs telləri (ünsiyyət orqanı)

Danışmaq üçün səs tellərindən istifadə etdiyimiz kimi, şəbəkə adapteri kompüterin digər sistemlərlə ünsiyyət qurmasına imkan verir. Simsiz (Wi-Fi) və naqilli (Ethernet) variantları mövcuddur. Çox vaxt motherboard-a inteqrasiya olunur, lakin genişləndirmə kartı olaraq da əlavə edilə bilər. Şəbəkə kartları adətən PCI Express portları vasitəsilə qoşulur.

---

### 🔌 PSU — Power Supply Unit
**Bədəndəki qarşılığı:** Ürək

Hər sistemə enerji lazımdır. Ürəyimiz qanı orqanlara necə ötürürsə, PSU da bütün komponentlərə enerji verir. PSU çox vacibdir və diqqətlə seçilməlidir — əgər komponentlər PSU-nun verə biləcəyindən çox enerji tələb edərsə, sistem işləməyəcək. PSU elektrik şəbəkəsindən enerji alır və müxtəlif konnektor növləri (əsas motherboard konnektoru, Molex konnektorları və s.) vasitəsilə paylaşdırır.

---

### 🎮 Graphics Card (Qrafik Kart / GPU)
**Bədəndəki qarşılığı:** Görmə qabığı (visual cortex)

Gözlərimiz məlumat toplayan kimi, görmə qabığı onu şəkillərə çevirir. Qrafik kart da eyni şeyi edir: əməliyyat sistemi və proqramlardan məlumat alır, işlənmiş vizual məlumatı monitora çıxarır. Qrafik kartlar PCI Express slotlarına qoşulur.

---

### ⌨️ I/O — Input / Output Devices
**Bədəndəki qarşılığı:** Hisslər (məlumat almaq) + Hərəkət orqanları (cavab vermək)

Beynimiz hisslərimizdən məlumat alıb onlara görə hərəkət etdiyi kimi, kompüterin də giriş və çıxış cihazları var:

- **Input (Giriş):** Klaviatura, siçan, mikrofon, skaner
- **Output (Çıxış):** Monitor, printer, dinamik

Bu periferik cihazlar üçün ümumi konnektorlara USB, HDMI və DisplayPort daxildir.

---

## Task 3 — Güc Düyməsinə Bassaq Nə Olur? (Boot Prosesi)

Komponentlər quraşdırıldıqdan sonra sistemi işə salmaq vaxtıdır. Bu proses səhər oyandığımız zaman bədənimizin hər şeyin yaxşı olduğunu yoxlamasına bənzəyir — hər şey qaydasındadırsa, günə başlayırıq.

Kompüter işə düşməzdən əvvəl 5 addımdan keçir:

---

### Addım 1 — Güc Düyməsinə Bas (Press the Power Button)

Güc düyməsinə basdıqda PSU-ya elektrik axışına icazə vermək üçün signal göndərilir. Yuxuda olan bədənimizə bənzəyir — oyanıb oksigen alanda qanımız pompalanmağa başlayır.

---

### Addım 2 — Firmware Başlayır (UEFI / BIOS)

Bədən işə düşdükdən sonra əsas orqanlar işləyir, amma beyin hələ şüurlu deyil. Kompüterdə də belədir: bütün komponentlər işə düşür, amma hələ idarə mexanizmi aktiv deyil. Bu mərhələdə **firmware** devreye girir.

Bunu idarə edən sistem **UEFI** (Unified Extensible Firmware Interface) adlanır.

> **Qeyd:** BIOS termini də çox hallarda istifadə olunur. BIOS UEFI ilə eyni funksiyanı yerinə yetirir, lakin əsasən UEFI ilə əvəz edilmişdir.

---

### Addım 3 — POST (Power-On Self Test)

Bədən işə düşdükdən sonra hər şeyin düzgün işlədiyini yoxlamaq lazımdır. Bir şey xətalıdırsa, xəbərdarlıq siqnalları verilir.

UEFI-nin yüklədiyi rutinlərdən biri **POST**-dur — hər tələb olunan komponentin mövcud, düzgün konfiqurasiya edilmiş və işlək olduğunu yoxlayır. Problem varsa — bip səsləri və ya ekranda xəta mesajları görünür.

---

### Addım 4 — Boot Cihazı Seçilir (Select Boot Device)

Bədən tam işlək vəziyyətə gəldikdə, sistem şüurun harada olduğunu axtarmağa başlayır. Kompüterdə UEFI, Əməliyyat Sisteminin yükləmə rutinini axtarmaq üçün hansı cihaza əvvəlcə baxacağını müəyyən edən **sıralı siyahı** saxlayır (Boot Order).

---

### Addım 5 — Bootloader İşə Düşür (Initiate Bootloader)

Sistem artıq şüurun hansı hissədə olduğunu bilir — onu yükləmə vaxtıdır. Kompüterdə də belədir: seçilmiş boot cihazında **bootloader** işə düşür. Bu bootloader, Əməliyyat Sistemini seçilmiş yaddaş cihazından RAM-a köçürür. OS köçürüldükdən sonra UEFI müxtəlif komponentlər üzərindəki nəzarəti OS-ə verir.

---

## Task 4 — Nəticə (Conclusion)

Kompüterin əsas komponentlərini və açılış prosesini öyrəndik. İndi bunlar az əhəmiyyətli görünə bilər, lakin kibertəhlükəsizlik konsepsiyalarını öyrənərkən tez-tez bu komponentlərin funksiyalarını və bir-biri ilə qarşılıqlı əlaqəsini xatırlamaq lazım gələcək.

**Boot prosesi xüsusilə vacibdir** — çünki bu, hacker-ların tez-tez hədəf aldığı sahədir.

Növbəti addım: kompüterin müxtəlif kombinasiya və ixtisaslaşmaları müxtəlif tipli kompüter sistemlərini necə yaradır — **Computer Types** roomunda davam edir.

---

## 📋 Qısa Xülasə

| Komponent | Funksiya | Bədən Analogy |
|---|---|---|
| Motherboard | Hər şeyi bağlayır | Skelet + Sinir sistemi |
| CPU | Əmrləri icra edir | Beyin |
| RAM | Müvəqqəti yaddaş | Qısamüddətli yaddaş |
| SSD/HDD | Daimi yaddaş | Uzunmüddətli yaddaş |
| PSU | Enerji paylayır | Ürək |
| Network Adapter | Digər sistemlərlə ünsiyyət | Səs telləri |
| Graphics Card | Vizual məlumat emal edir | Görmə qabığı |
| I/O Devices | İnsan-kompüter əlaqəsi | Hisslər + Hərəkət |

**Boot Sequence:**
```
Güc düyməsi → UEFI/BIOS → POST → Boot Device seçimi → Bootloader → OS yüklənir
```
