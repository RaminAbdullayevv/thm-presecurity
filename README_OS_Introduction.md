# 🖥️ Operating Systems: Introduction

> **TryHackMe Room:** [tryhackme.com/room/operatingsystemsintroduction](https://tryhackme.com/room/operatingsystemsintroduction)
> **Modul:** Operating Systems
> **Səviyyə:** Başlanğıc

---

## Task 1 — Giriş (Introduction)

Hər gün telefonu və ya noutbuku açanda hər şey öz-özünə işləyir: proqramlar açılır, fayllar görünür, musiqi ifa olunur. Bütün bunları mümkün edən görünməz bir qat var — **Əməliyyat Sistemi (OS)**.

### Ssenari

Dostun köhnə kompüterini sənə bağışlayıb. Üzərindəki əməliyyat sistemi haqqında çox az şey bilir — sadəcə "yaxşı işləyirdi" deyib. Kompüteri yüksəltmək, silmək, satmaq, yoxsa yeni layihəyə çevirmək qərarını vermədən əvvəl nə ilə işlədiyini anlamaq lazımdır.

### Öyrənmə Hədəfləri

- Əməliyyat sisteminin nə olduğunu və oynadığı rolu başa düşmək
- OS-in əsas vəzifələrini izah etmək
- Ümumi OS növlərini və onların istifadə sahələrini müəyyənləşdirmək
- OS ilə qarşılıqlı əlaqə quraraq sistem məlumatlarını toplamaq

---

## Task 2 — Görünməz Menecer (The Invisible Manager)

**Əməliyyat sistemi (OS)** kompüterdə baş verən hər şeyi koordinasiya edən əsas proqramdır. İstifadəçi, proqramlar və fiziki hardware arasında duraraq bütün maşını vahid, bütöv sistem kimi işlək saxlayan görünməz menecer rolunu oynayır.

### Hava Limanı Analogiyası

Kompüteri məşğul bir hava limanı kimi təsəvvür et:

- **Hardware** (CPU, RAM, yaddaş, cihazlar) → Uçuş-enmə zolaqları, təyyarələr, yanacaq sistemləri, radar və digər fiziki infrastruktur
- **Proqramlar** (brauzer, oyun) → Müxtəlif aviakompaniyalar və onların sərnişinləri — hamısı uçmaq, enmək, xidmət almaq istəyir
- **Əməliyyat sistemi** (Windows, Linux, macOS) → Bütün bu fəaliyyəti istiqamətləndirən tam hava trafikini idarəetmə sistemi; resursları planlaşdırır, trafiki idarə edir, münaqişələri həll edir

OS olmadan hər proqram CPU, yaddaş, fayllar, cihazlar və təhlükəsizlik üzərində birbaşa nəzarət etmək məcburiyyətindəqalardı. Bu isə dərhal münaqişələrə gətirib çıxarardı. OS mərkəzi təşkilatçı kimi çıxış edərək bunu önləyir.

---

### Sistem Privilege Qatları (System Privilege Layers)

Müasir kompüterin daxilində sistemin müxtəlif hissələri müxtəlif icazə səviyyələrində işləyir. Bəzi komponentlər hardware ilə birbaşa əlaqə qura bilir, adi proqramlar isə daha məhdud, daha təhlükəsiz mühitdə işləyir. Bu ayrılıq qəsdəndir — münaqişələrin və təhlükəsizlik problemlərinin qarşısını alır.

#### 🔒 Kernel Space (Nüvə Fəzası)
OS-in imtiyazlı, kilidlənmiş nüvəsidir. **Kernel** — hardware və sistem resurslarını birbaşa idarə edən OS hissəsidir — burada işləyir. CPU, RAM, yaddaş və bütün hardware komponentlərinə məhdudiyyətsiz çıxışı var.

Hava limanı analogiyasında: Kernel Space — idarəetmə qülləsidir. Yalnız etibarlı hava trafikini idarəedicilər (kernel) burada işləyir. Onlar birbaşa uçuş-enmə zolaqlarını, radarı və digər hardware-i idarə edə bilirlər.

#### 👤 User Space (İstifadəçi Fəzası)
Bütün standart proqramların işlədiyi yerdir. User space-dəki proqramlar hardware-ə birbaşa daxil olmaqdan qəsdən məhrum edilib. Fayl açmaq, səs çıxarmaq, Wi-Fi-ya qoşulmaq kimi hər şey üçün **sistem çağırışı (system call)** etməli, yəni kernel-dən öz adlarına hərəkət etmələrini xahiş etməlidirlər.

Analogiyada: Proqramlar yerüstü meydançadakı aviakompaniyalar və sərnişinlər kimidir. Qülliyə girə, avadanlığa toxuna bilmirlər. Bunun əvəzinə, qülliyə radio sorğusu göndərirlər (system call) — qüllə bunları təhlükəsiz şəkildə həll edir.

Bu ayrılıq OS-i etibarlı saxlayır: bir proqramın xətası bütün sistemi çökdürə bilmir.

---

### OS-in Əsas Vəzifələri (Operating System Duties)

OS pərdə arxasında bir neçə əsas vəzifəni yerinə yetirir:

| OS Vəzifəsi | OS Nə Edir | Nümunə |
|---|---|---|
| **Proses İdarəetməsi** | İşləyən proqramları yaradır, planlaşdırır, prioritetləşdirir, dayandırır | Brauzer, musiqi pleyeri, sosial media — hamısı eyni anda açıqdır, kompüter donmur |
| **Yaddaş İdarəetməsi** | Proses başına RAM ayırır, proqramların yaddaşını digərlərindən qoruyur, proqram bağlandıqda yaddaşı geri alır | RAM azalanda OS virtual yaddaşdan istifadə edərək sistemi stabil saxlayır |
| **Fayl Sistemi İdarəetməsi** | Faylları qovluqlara yerləşdirir, adlandırma, yollar, icazələr, metadata ilə məşğul olur | Yeni qovluq yaratmaq, foto saxlamaq, faylı "yalnız oxumaq" kimi təyin etmək |
| **İstifadəçi İdarəetməsi** | Bir neçə istifadəçi hesabını, autentifikasiyanı və icazələri idarə edir | Parolunla daxil olmaq, fayllarının başqalarına gözükməməsi |
| **Cihaz İdarəetməsi** | Drayverləri yükləyir, universal interfeys (hardware abstraction layer) təmin edir | Yeni siçan, printer, xarici HDD qoşduqda dərhal işləyir |

---

### OS Təhlükəsizliyi (Operating System Security)

Hər OS həm də təhlükəsizlik təməli kimi çıxış edir. Hər hansı antivirus, firewall və ya təhlükəsizlik aləti devreye girmədən əvvəl OS artıq pərdə arxasında qorumaları tətbiq edir:

- **Autentifikasiya (Authentication):** Giriş parolları və biometrika vasitəsilə kimliyin təsdiqləyir
- **İcazələr (Permissions):** Hər istifadəçinin və proqramın nəyi oxuya, yaza, icra edə biləcəyini idarə edir
- **İzolyasiya (Isolation):** Hər prosesi öz qorunan qutusunda saxlayır (kernel/user space ayrılığı)
- **Sistem Qoruması:** Kritik sistem fayllarını və parametrlərini icazəsiz dəyişikliklərdən qoruyur

---

## Task 3 — OS İlə Qarşılıqlı Əlaqə və Mənzərəsi (OS Interaction and Landscape)

### OS İnterfeysləri

OS ilə qarşılıqlı əlaqə iki əsas hissəyə bölünür: **GUI** və **CLI**.

#### 🖱️ GUI — Graphical User Interface (Qrafik İstifadəçi İnterfeysi)
Çox güman ki, ən çox istifadə etdiyin interfeys budur. Kompüterdə daxil olmaq istədiyin bütün məlumatların qrafik təsvirini verir — qovluq ikonları, proqram pəncərələri, parametr menyuları. Naviqasiya tətbiqini istifadə etmək kimidir: getmək istədiyin yerin ikonuna toxunursan, tətbiq sənin üçün marşrutu qurur.

#### ⌨️ CLI — Command-Line Interface (Komanda Sətri İnterfeysi)
Sistemi idarə etmək üçün mətn əsaslı xüsusi komandalar daxil etdiyin yerdir. İkonlara klikləmək əvəzinə, kompüterdən nə istədiyini sistemin başa düşdüyü söz və sintaksis ilə deyirsən. Bu sənə xüsusilə qabaqcıl tapşırıqlar üçün daha çox dəqiqlik, nəzarət və sürət verir — lakin komandaları bilməyi tələb edir.

GPS koordinatlarını birbaşa daxil etmək kimidir: düz və son dərəcə dəqiqdir, amma doğru məlumatı daxil etməyi bilirsənsə.

---

### OS Növləri (The Operating System Landscape)

Bütün əməliyyat sistemləri eyni deyil. Müxtəlif cihazlar və işlər müxtəlif dizaynlar tələb edir:

| OS Növü | Əsas İstifadə Sahəsi | Əsas Xüsusiyyətlər |
|---|---|---|
| **Desktop** | Şəxsi kompüterlər, gündəlik iş, oyun, məzmun yaratma | Zəngin qrafik interfeys, bir anda çox proqram, istifadəçi yönümlü |
| **Server** | Veb hosting, verilənlər bazası, bulud xidmətləri, arxa plan | GUI yoxdur (headless), maksimum işləmə müddəti, çox istifadəçili, uzaqdan giriş |
| **Mobile** | Smartfonlar və tabletlər | Toxunma əsaslı UI, enerji səmərəliliyi, həmişə qoşulu, tətbiq sandboxing |
| **Embedded** | Elektrik avadanlıqları, avtomobillər, IoT cihazları, ağıllı TV-lər, routerlər | Kiçik ölçülü, məhdud hardware-də işləyir |
| **Virtual/Cloud** | Virtual maşınlar, konteynerlər, bulud nümunələri | Yüngül, ölçəklənə bilən, sürətli yerləşdirmə |

---

### Real Dünya Əməliyyat Sistemləri

#### 🖥️ Desktop
- **Windows** — Şəxsi kompüterlərdə ən geniş yayılmış OS. (*Windows 10, Windows 11*)
- **macOS** — Apple-ın masa üstü OS-i, parıldayan GUI və digər Apple cihazları ilə inteqrasiyası ilə tanınır. (*Sonoma, Sequoia, Tahoe*)
- **Linux** — Tək OS deyil, **distributiv** adlanan açıq mənbəli OS-lər ailəsidir. (*Ubuntu, Debian, Fedora*)

#### 🗄️ Server
- **Windows Server** — Böyük şəbəkələrdə, məlumat mərkəzlərində və korporativ mühitlərdə istifadə olunur. (*Server 2016, 2019, 2022, 2025*)
- **Linux** — Veb serverlərin böyük əksəriyyəti, etibarlılığı və açıq mənbə xarakteri ilə seçilir. (*Ubuntu Server, Debian, CentOS, Red Hat*)
- **Unix** — Böyük müəssisələr, maliyyə, telekomunikasiya, dövlət sektoru. (*IBM AIX, Oracle Solaris*)

#### 📱 Mobile
- **Android** — Telefonlarda, tabletlərdə və ağıllı cihazlarda işləyən ən geniş yayılmış mobil OS. (*Android 14–16*)
- **iOS** — Apple-ın iPhone, iPad və digər cihazlarda işləyən mobil OS-i. (*iOS 17, 18, 26*)

#### ⚙️ Embedded və IoT
- **Embedded Linux** — Xüsusi funksiyaları olan cihazlara quraşdırılmış ixtisaslaşdırılmış OS. (*OpenWrt, Ubuntu Core, Yocto Project*)
- **Real-Time OS (RTOS)** — Tapşırıqların zəmanətli cavab müddəti tələb etdiyi proqramlar üçün (uçak idarəetmə sistemləri). (*FreeRTOS, VxWorks, QNX*)

#### ☁️ Virtual və Bulud
- **Cloud/VM** — Vebsaytları, proqramları və axın xidmətlərini hostinq edən nəhəng məlumat mərkəzləri. (*Ubuntu LTS, Amazon Linux, Rocky Linux*)
- **Konteyner-optimizasiyalı** — VM-lərin yüngül alternativləri, yalnız tətbiqi və onun asılılıqlarını paketləyir. (*Alpine Linux, Bottlerocket AWS, Flatcar Linux*)

---

### Niyə Bu Qədər Çox Əməliyyat Sistemi Var?

Müxtəlif cihazlar və mühitlər OS-dən müxtəlif imkanlar tələb edir:

- **Noutbuk** — İstifadəçi dostu olmalı, multitaskingi dəstəkləməlidir
- **Serverlər** — Sabitlik, təhlükəsizlik tələb edir, fasiləsiz işləməlidir
- **Mobil cihazlar** — Batareya ömrünü uzatmaq üçün enerji səmərəliliyi və hardware inteqrasiyası lazımdır
- **Embedded sistemlər** — Xüsusi məqsəd üçün nəzərdə tutulmuş yüngül OS-lər istifadə edir

Bu OS-ləri hazırlayan şirkətlər və icmalar da öz məqsədlərinə malikdir: bəziləri istifadə rahatlığına, performansa, təhlükəsizliyə, açıqlığa və ya fərdiləşdirməyə diqqət yetirir. Hər mühit müxtəlif imkanları qiymətləndirdiyi üçün heç bir OS hər vəziyyət üçün mükəmməl uyğun deyil. Bunun əvəzinə, OS-lərin bir ekosistemi inkişaf etmişdir.

---

## Task 4 — Nəticə (Conclusion)

Bu roomda OS-in pərdə arxasında əslində nə etdiyini öyrəndin:

- **Privilege** — Kernel space vs User space ayrılığı
- **Proses İdarəetməsi** — Multitasking necə mümkün olur
- **Yaddaş İdarəetməsi** — RAM necə bölüşdürülür
- **Fayl Sistemi** — Faylların necə təşkil edildiyi
- **İstifadəçi İdarəetməsi** — Hesablar və icazələr
- **Cihaz İdarəetməsi** — Hardware drayverləri
- **GUI vs CLI** — OS ilə iki cür qarşılıqlı əlaqə

---

## 📚 Əsas Terminlər

| Termin | İzah |
|---|---|
| **OS (Əməliyyat Sistemi)** | Hardware, proqramlar və bütün sistem resurslarını idarə edən əsas proqram |
| **Kernel Space** | OS-in hardware-ə birbaşa çıxışı olan yüksək imtiyazlı sahəsi |
| **User Space** | Adi proqramların təhlükəsizlik üçün məhdud icazələrlə işlədiyi sahə |
| **GUI** | Klik və toxunma vasitəsilə qarşılıqlı əlaqəyə imkan verən OS-in vizual hissəsi |
| **CLI** | Sistemi dəqiqlik və sürətlə idarə etmək üçün komanda yazdığın mətn əsaslı interfeys |
| **Kernel** | Hardware və sistem resurslarını birbaşa idarə edən OS-in nüvəsi |
| **System Call** | Proqramın kernel-dən öz adına hərəkət etməsini xahiş etdiyi mexanizm |
| **Driver** | OS-in hardware cihazı ilə ünsiyyət qurmasına imkan verən proqram |

---

## 🔗 Növbəti Addımlar

- [Windows Basics](https://tryhackme.com/room/windowsbasics)
- Linux CLI Basics (tezliklə)
- Windows CLI Basics (tezliklə)

---

*📅 TryHackMe — Operating Systems Modulu*
