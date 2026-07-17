# 🪟 Windows Basics

> **TryHackMe Room:** [tryhackme.com/room/windowsbasics](https://tryhackme.com/room/windowsbasics)
> **Modul:** Operating Systems
> **Səviyyə:** Başlanğıc

---

## Task 1 — Giriş (Introduction)

Hər əməliyyat sisteminin özünəməxsus "xarakteri" var. Məktəbdə, işdə və ya evdə kompüter istifadə etmisənsə, çox güman ki, artıq ən tanınan birini tanıyırsan: **Microsoft Windows**.

Əvvəlki roomda əməliyyat sisteminin nə olduğunu öyrəndin — cihazını rəvan işlək saxlayan pərdə arxası menecer. İndi isə real bir nümunəyə addım atacaq və əvvəl öyrənilmiş konsepsiyaların tanıdığın sistemdə necə göründüyünü müşahidə edəcəksən.

### Ssenari

Bu roomda **TryHatMe** şirkətinin yeni işçisi rolunu üstlənəcəksən — ilk iş günün. İş stansiyasına daxil olduqdan sonra Windows masaüstünü tanıyacaq, interfeysinin necə işlədiyini öyrənəcək, Start menyusundan alətlər açacaq, şirkətin qovluqlarında yolunu tapacaqsan. Müşfiq rəhbərin səni faylları yaratmaq və təşkil etmək, sistem parametrlərini dəyişmək, Task Manager-i yoxlamaq və təhlükəsizlik parametrlərini nəzərdən keçirməkdə istiqamətləndirəcək.

### Öyrənmə Hədəfləri

- Masaüstü, taskbar və Start menyusu da daxil olmaqla Windows qrafik interfeysini naviqasiya etmək
- Qovluqlara baxmaq, fayl yollarını anlamaq və faylları təşkil etmək üçün File Explorer-dən istifadə etmək
- Settings tətbiqi vasitəsilə sistem parametrlərini yoxlamaq və Windows mühitini fərdiləşdirmək
- Performansı izləmək və sistem qorumasını yoxlamaq üçün Task Manager və Windows Security kimi əsas sistem alətlərindən istifadə etmək

---

## Task 2 — Windows İş Mühitini Kəşf Etmək (Exploring the Windows Workspace)

Windows-un indiki cilalı görünüşünə qədər Microsoft-un əməliyyat sistemləri çox daha sadə idi. İlk kompüterlər MS-DOS işlədirdi — ikonlara klikləmək əvəzinə qara ekranda komandalar yazılırdı. 1985-ci ildə Microsoft, DOS üzərindən qurulmuş sadə bir GUI olan **Windows 1.0**-ı buraxdı; pəncərələri, menyuları və siçan idarəetməsini şəxsi kompüterlərə tanıtdı. Zamanla Windows daha çox funksiya əlavə etdi, orijinal qabığı tam əməliyyat sisteminə çevirdi. Bu gün müasir Windows OS həmin dəyişikliklərin nəticəsidir.

---

### 🔐 Daxil Olma və Autentifikasiya (Logging in and Authentication)

Windows Masaüstünə giriş əldə etməzdən əvvəl sistemə özünü **autentifikasiya** (kimliğini sübut etmək) etməlisən. Autentifikasiya prosesi kimliğini yoxlayır və daxil olduqdan sonra görməyinə icazə verilən əməliyyatları müəyyənləşdirir.

Windows sistemi açıldıqda, istifadəçi hesabının seçilməli və parol, PIN və ya başqa bir doğrulama metodu ilə autentifikasiya edilməli olduğu giriş ekranı göstərilir. Hər istifadəçiyə fayllara, parametrlərə və sistem funksionallığına girişini müəyyən edən icazə səviyyəsi verilir:

| Hesab Növü | İzah |
|---|---|
| **Guest (Qonaq)** | Müvəqqəti giriş üçün nəzərdə tutulmuş məhdud hesab. Sistem parametrlərini dəyişə bilmir |
| **Standard (Standart)** | Sistem miqyasında dəyişiklik etmədən proqramları işlətmək və şəxsi parametrləri dəyişmək kimi gündəlik tapşırıqlar üçün hesab |
| **Administrator (Administrator)** | Proqram quraşdırma, konfiqurasiya dəyişiklikləri və istifadəçi idarəetməsi daxil olmaqla sistem üzərində tam nəzarəti olan imtiyazlı hesab |

---

### 🖥️ Windows Masaüstü (The Windows Desktop)

Hava limanı analogiyasına davam etsək: əgər sən, istifadəçi, sərnişinsənsə və giriş ekranı hava limanı təhlükəsizlik nöqtəsini təmsil edirsə, **Masaüstünü** hava limanı terminalı hesab etmək olar. Daxil olduqdan sonra ilk giriş əldə etdiyin sahədir və bütün sonrakı sahələr buradan budaqlanır.

İlk daxil olduqda iki əsas sahə görünür:

- **Desktop (Masaüstü):** Faylların, qovluqların və qısayolların yaşadığı əsas iş sahəsi
- **Taskbar (Görev Çubuğu):** Proqramlara, sistem alətlərinə, parametrlərə və bildirişlərə giriş təmin edən nəzarət zolağı

#### Masaüstünün Əsas Elementləri

1. **Desktop icons (Masaüstü ikonları)** — Recycle Bin, qovluqlar və tez-tez istifadə olunan proqramlara qısayollar. Tam fərdiləşdirilə bilər
2. **Start menu (Başlat menyusu)** — Proqramlara, parametrlərə və güc seçimlərinə daxil olmağın əsas yolu. Buradan çıxış edə, yenidən başlada və ya maşını söndürə bilərsən
3. **Search (Axtarış)** — Açar sözlər istifadə edərək proqramları, faylları, qovluqları və sistem parametrlərini sürətlə tapmaq
4. **Task View** — Hal-hazırda açıq olan bütün pəncərələri görmək və aralarında sürətlə keçid etmək
5. **Pinned Applications and Folders (Sabitlənmiş Proqramlar və Qovluqlar)** — Ən çox istifadə olunan proqramlar və qovluqlar bura sabitlənə bilər
6. **Network and Audio settings (Şəbəkə və Səs parametrləri)** — Ehtiyaclarına uyğun fərdiləşdirilə bilər
7. **Date and Time (Tarix və Saat)** — Tam təqvimə açılır. Tarix və saat parametrlərinə buradan da daxil olmaq olar
8. **Notifications (Bildirişlər)** — Kompüter və ya proqram bildirişlərini göstərir. Şəbəkə və digər parametrlərə də buradan daxil olmaq olar

#### Start Menyusu

Windows istifadə edərkən Start menyusuna taskbar-ın sol altında yerləşən Windows ikonuna klikləməklə daxil olmaq olar. Windows Masaüstünü hava limanı terminalı kimi hesab etsək, Start menyusu uçuş lövhəsi və ya məlumat masasıdır. Mövcud olanları gördüyümüz sahədir: proqramlar, fayllar, qovluqlar, parametrlər və güc seçimləri. Bunu tez giriş menyusu kimi düşünmək olar.

---

### 🛠️ Daxili Alətlər və Proqramlar (Built-in Tools and Apps)

Windows gündəlik istifadə edəcəyin bir çox faydalı daxili alət və proqramla birlikdə gəlir. Sistemini idarə etmək üçün mövcud olan geniş parametrlər çeşidindən əlavə, Windows həmçinin mətn fayllarını redaktə etmək üçün **Notepad** və faylları naviqasiya edib idarə etmək üçün **File Explorer** kimi sadə, lakin güclü alətlər də ehtiva edir.

Bu alətlər dərhal mövcuddur və gündəlik Windows istifadəsinin təməlini təşkil edir. Hamısına Start menyusu və axtarış çubuğu vasitəsilə daxil olmaq olar.

---

### ℹ️ Sistem Məlumatlarını Əldə Etmək (Getting System Information)

Windows, sistemini konfiqurasiya etməyə və haqqında məlumat əldə etməyə imkan verən daxili **Settings** (Parametrlər) tətbiqini ehtiva edir. Settings tətbiqinin içindəki **About your PC** bölməsi sistemin əsas detallarını verir:

- Cihaz adı
- Quraşdırılmış RAM miqdarı
- Prosessor növü
- Windows versiyası və build nömrəsi
- Sistem tipi (32-bit / 64-bit)

---

### 📁 Fayl Kəşfi və İdarəetməsi (File Exploration and Management)

#### Windows-un Fayl Strukturu

Windows faylları və qovluqları **iyerarxik qovluq strukturu** ilə təşkil edir — qovluqlar özlərinin içərisində başqa qovluqlar və fayllar ehtiva edə bilər. Bu struktur məlumatların nizamlı saxlanmasına kömək edir.

Ümumi yerlər:
- **Desktop** — Qısayollar və aktiv fayllar üçün
- **Documents** — Şəxsi sənədlər üçün
- **Downloads** — İnternetdən endirilmiş fayllar üçün

Bu yerlərin daxilindəki alt qovluqlar əlaqəli faylları bir yerə qruplaşdırmaq üçün istifadə olunur.

#### File Explorer

File Explorer Windows-un daxili fayl idarəetmə alətidir. Hava limanı analogiyasında File Explorer terminalın kataloqu və mərtəbə xəritəsi kimi çıxış edir — müxtəlif sahələr arasında naviqasiya etməyə və lazım olanı tapmağa kömək edir.

**Fayl Yolu Nümunəsi:**
```
C:\Users\Administrator\Desktop\TryHatMe Onboarding
```

File Explorer-in əsas funksiyaları:
- Qovluqlara baxmaq və açmaq
- Faylları kopyalamaq, köçürmək, silmək
- Yeni qovluq yaratmaq
- Fayl adını dəyişmək
- Daxili axtarış funksiyası ilə qovluq içərisində axtarmaq

---

## Task 3 — Windows-u Konfiqurasiya Etmək və Qorumaq (Configuring and Securing Windows)

Proqramlar kompüterdə tapşırıqları yerinə yetirmək üçün istifadə etdiyin vasitələrdir. Windows-un verdiyi daxili proqramları artıq müzakirə etdik, lakin proqramları necə quraşdırmaq, yeniləmək və silmək — gündəlik Windows istifadəsinin əsas bacarığıdır.

Hava limanı analogiyasında: OS və ya proqramları yeniləmək uçuşu dəyişmək və ya yer rezerv etmək kimidir. Yeni proqram quraşdırmaq yeni uçuş planlaşdırmağa bənzəyir. Proqramları silmək isə artıq ehtiyac olmayan rezervi ləğv etmək kimidir.

---

### 🔄 Proqramları Yeniləmək (Updating Applications)

Əməliyyat sistemi və proqramları aktual saxlamaq təhlükəsiz və stabil sistem üçün vacibdir. Yeniləmələr tez-tez təhlükəsizlik yamaları, performans yenilikləri və xəta düzəltmələrini ehtiva edir.

#### Windows Update
Windows, OS-i və bəzi yerli proqramları və təhlükəsizlik funksiyalarını aktual saxlayan daxili yeniləmə aləti — **Windows Update**-i ehtiva edir. Settings tətbiqi vasitəsilə daxil olmaq olar. Konfiqurasiyadan asılı olaraq yeniləmələri avtomatik quraşdıra bilər.

#### Proqram Yeniləmələri
Proqram yeniləmələri proqramın necə quraşdırıldığından asılı olaraq fərqli işləyir:
- Daxili proqramlar arxa planda avtomatik yenilənə bilər
- Üçüncü tərəf proqramlar tez-tez öz yeniləmə mexanizmlərini ehtiva edir
- Bəzi proqramlar açılışda yeniləmə etməyi xəbərdar edir
- Bəziləri yeniləmə yoxlamağı və ya yeni quraşdırıcı əldə etməyi tələb edir

---

### ⬇️ Proqram Quraşdırma (Installing Applications)

Windows-da proqram quraşdırmanın iki əsas yolu var:

1. **Microsoft Store** — Etibarlı proqramlar üçün küratörlük edilmiş və təhlükəsiz seçim təqdim edir (lakin Windows Server-də default olaraq mövcud deyil)
2. **İnternetdən** — Çox mühitlərdə proqramlar etibarlı satıcının saytından birbaşa quraşdırıcı endirilərək quraşdırılır. Adətən `.exe` və ya `.msi` faylları şəklində gəlir və istifadəçini quraşdırma prosesindən keçirir

---

### 🗑️ Proqram Silmək (Uninstalling Applications)

Windows mühitindəki proqramları silməyin bir neçə yolu var:

- Quraşdırılmış proqramlar üçün Microsoft Store
- Sistem parametrlərindəki **Add or remove programs** (Proqram əlavə et və ya sil) funksiyası
- Control Panel-in **Uninstall a program** (Proqramı sil) bölməsi
- Proqramın öz daxili siləni (uninstaller)

---

### ⚙️ Parametrlərə Dalmaq (Diving Into Settings)

Windows istifadəçisinin mühitini dəyişdirə biləcəyi iki əsas yol var:

#### Windows Settings (Windows Parametrləri)
Sistemin, cihazın, fərdiləşdirmənin və təhlükəsizlik parametrlərini konfiqurasiya etmək üçün müasir, mərkəzləşdirilmiş yer. Buradan idarə edə biləcəyin əsas sahələr:
- Ekran və səs parametrləri
- İstifadəçi hesabları
- Proqramlar
- Şəbəkə seçimləri
- Əlçatanlıq funksiyaları
- Təhlükəsizlik konfiqurasiyaları
- **Tarix, Saat və Dil** parametrləri (hansı ölkə/bölgənin seçildiyi)

#### Control Panel (İdarə Paneli)
Xüsusi administrativ tapşırıqlar üçün hələ də tələb olunan köhnə sistem konfiqurasiya alətlərinə giriş təmin edən **köhnə idarəetmə interfeysidir**. Müasir Windows Settings ilə əvəz edilmişdir, lakin bəzi qabaqcıl parametrlər hələ də yalnız Control Panel vasitəsilə əlçatandır.

---

### 📊 Task Manager (Tapşırıq Meneceri)

**Task Manager** sisteminizdə real vaxt rejimində nə baş verdiyini izləməyə imkan verən daxili Windows alətidir. İşləyən proqramları və arxa plan proseslərini, həmçinin CPU və yaddaş istifadəsi də daxil olmaqla sistem performansını yoxlamağa imkan verir.

Task Manager-i açmağın yolları:
- Masaüstündə sağ klik → **Task Manager**
- `Ctrl + Shift + Esc` qısayolu
- `Ctrl + Alt + Delete` → Task Manager seç

#### Task Manager-in 5 Cədvəli

| Cədvəl | Nə Göstərir |
|---|---|
| **Processes (Proseslər)** | Hal-hazırda işləyən proqramlar və arxa plan prosesləri, onların resurs istifadəsi |
| **Performance (Performans)** | CPU, RAM, disk, şəbəkə üzrə qrafiklər və statistikalar |
| **Users (İstifadəçilər)** | Hal-hazırda daxil olmuş istifadəçilər və istifadə olunan resurslar |
| **Details (Detallar)** | Proses ID-ləri (PID-lər) daxil olmaqla işləyən proseslərin daha texniki görünüşü |
| **Services (Xidmətlər)** | Windows xidmətləri və onların cari statusu (işləyir və ya dayanıb) |

---

### 🛡️ Yerli Windows Təhlükəsizliyi (Native Windows Security)

Windows, sisteminizi zərərli proqramlar, etibarsız proqramlar və icazəsiz şəbəkə girişi kimi təhdidlərdən qorumağa kömək edən daxili təhlükəsizlik alətləri təklif edir. Bu alətlər default olaraq aktivdir.

#### Windows Security (Windows Təhlükəsizliyi)

**Windows Security** tətbiqi Windows-un daxili qoruma tədbirlərini idarə etmək üçün mərkəzi idarə panelidir. Dörd əsas bölməyə bölünür:

| Bölmə | Funksiya |
|---|---|
| **Virus & threat protection** | Real vaxt qoruması və fərdiləşdirilmiş skanlar istifadə edərək zərərli proqramları aşkar edir və silir |
| **Firewall & network protection** | İcazəsiz girişin qarşısını almaq üçün daxil olan və gedən şəbəkə trafikini idarə edir |
| **App & browser control** | İstifadəçiləri potensial təhlükəsiz olmayan proqramlardan, fayllardan və vebsaytlardan qoruyur |
| **Device security** | Sistemi qorumağa kömək edən hardware-əsaslı qorumalar təmin edir |

#### Fərdiləşdirilmiş Skan Necə Aparılır
1. Windows Security-ni açmaq
2. **Virus & Threat protection** bölməsini seçmək
3. **Scan options** seçmək
4. **Custom scan** → **Scan now** seçmək
5. Skanlanacaq qovluğu hədəf olaraq seçmək

---

#### 🔥 Windows Defender Firewall

**Windows Defender Firewall** kompüteri icazəsiz şəbəkə trafikindən qorumağa kömək etmək üçün nəzərdə tutulmuş daxili firewalldur. Şəbəkə bağlantılarını izləyir və əlaqələrin icazə verilib-verilməyəcəyini müəyyən edən qaydaları tətbiq edir.

Firewall üç şəbəkə profilini dəstəkləyir:

| Profil | İstifadə Yeri |
|---|---|
| **Domain** | Sistem təşkilatın domain şəbəkəsinə qoşulduqda istifadə olunur |
| **Private (Şəxsi)** | Ev və ya lab mühiti kimi etibarlı şəbəkələr üçün nəzərdə tutulub |
| **Public (İctimai)** | İctimai Wi-Fi kimi etibarsız şəbəkələr üçün istifadə olunur |

**Firewall Advanced Settings-də görülə biləcəklər:**
- Firewall-un giriş, çıxış və əlaqə qaydalarına ümumi baxış
- Ad, qrup, şəbəkə profili, status və hərəkət daxil olmaqla hər qaydanın ətraflı görünüşü
- Yeni qaydalar yaratmaq və ya mövcud görünüşü filtirləmək

---

## Task 4 — Nəticə (Conclusion)

Bu roomda Windows əməliyyat sisteminin əsaslarını araşdırdın:

- Windows interfeysini naviqasiya etdin
- Sistem xüsusiyyətlərini araşdırdın
- Proqram idarəetməsini öyrəndin (quraşdırma, yeniləmə, silmə)
- Fayllar və qovluqlarla işlədin
- Windows Server 2019 istifadə edərək **TryHatMe**-dəki ilk iş günündə onboarding tapşırıqlarını tamamladın

Bu bacarıqlar həm ümumi sistem istifadəsi, həm də öyrənmə yolunda daha dərin texniki anlayış üçün təməl yaradır.

---

## 📚 Əsas Terminlər

| Termin | İzah |
|---|---|
| **Desktop** | Faylların, qovluqların və qısayolların yaşadığı əsas iş sahəsi |
| **Taskbar** | Proqramlara, alətlərə, parametrlərə və bildirişlərə giriş təmin edən nəzarət zolağı |
| **Start Menu** | Windows loqosu ilə işarələnmiş, proqramlara, parametrlərə və güc seçimlərinə daxil olmağın əsas yolu |
| **Search** | Axtarış terminləri daxil edərək proqramları, parametrləri və faylları tapmağın tez giriş metodu |
| **File Explorer** | Faylları və qovluqları taramaq, idarə etmək və təşkil etmək üçün daxili Windows aləti |
| **Windows Update** | OS, yerli proqramlar və təhlükəsizlik funksiyalarını aktual saxlamağa kömək edən daxili yeniləmə aləti |
| **Microsoft Store** | Etibarlı proqramlar quraşdırmaq üçün yerli Windows tətbiqi |
| **Windows Settings** | Sistemin, cihazın, fərdiləşdirmənin və təhlükəsizlik parametrlərini konfiqurasiya etmək üçün mərkəzləşdirilmiş yer |
| **Control Panel** | Sistem konfiqurasiya seçimlərinə giriş təmin edən köhnə idarəetmə interfeysi |
| **Task Manager** | Sisteminizdə nə baş verdiyini real vaxt rejimində izləmək üçün Windows aləti |
| **Windows Security** | Windows-un daxili təhlükəsizlik alətlərini idarə etmək üçün mərkəzi idarə paneli |
| **Windows Defender Firewall** | Sistemi icazəsiz şəbəkə trafikindən qorumağa kömək etmək üçün nəzərdə tutulmuş firewall |
| **PID** | Hər prosesə verilən unikal nömrə (Process ID) |
| **Administrator** | Sistem üzərində tam nəzarəti olan imtiyazlı hesab növü |

---

## 🔗 Növbəti Addımlar

- Linux CLI Basics (tezliklə)
- Windows CLI Basics (tezliklə)

---

*📅 TryHackMe — Operating Systems Modulu | Windows Basics*
