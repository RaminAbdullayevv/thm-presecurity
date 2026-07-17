# TryHackMe — Operating System Security Lab İzahı

**Çətinlik:** Easy  
**Müddət:** 60 dəqiqə  
**Platforma:** TryHackMe  
**Link:** https://tryhackme.com/room/operatingsystemsecurity

---

## Task 1 — Əməliyyat Sistemi Təhlükəsizliyinə Giriş

### Əməliyyat Sistemi (OS) nədir?

**Hardware (Aparat təminatı)** — kompüterin fiziki hissələridir:
- Ekran, klaviatura, printer, USB yaddaş
- Anakart (motherboard), CPU (prosessor), RAM (yaddaş)
- HDD/SSD (saxlama)

**Əməliyyat Sistemi** — hardware ilə proqramlar arasında duran təbəqədir:

```
Proqramlar (Chrome, WhatsApp, Zoom)
            ↕
     Əməliyyat Sistemi
            ↕
         Hardware
```

Proqramlar birbaşa hardware-ə daxil ola bilmir — OS vasitəsilə işləyir.

---

### OS Növləri

| OS | İstifadə sahəsi |
|----|----------------|
| **Windows 11, macOS** | Laptop, masa üstü kompüter |
| **Android, iOS** | Smartfon |
| **Windows Server, IBM AIX, Oracle Solaris** | Server sistemləri |
| **Linux** | Həm personal, həm server |

---

### Niyə OS Təhlükəsizliyi Vacibdir?

Smartfonunda saxladıqların:
- Ailə və dostlarla şəxsi yazışmalar
- Şəxsi fotolar
- Email hesabları
- Brauzerdə saxlanılmış şifrələr
- E-bank tətbiqləri

Kompüterdə saxladıqların:
- İş/universitet sənədləri
- Şəxsi fayllar (pasport surəti və s.)
- Email proqramları (Outlook, Thunderbird)
- Brauzerdə şifrələr
- Kamera fotoları

---

### CIA Üçlüyü — Təhlükəsizliyin 3 Sütunu

#### Confidentiality (Gizlilik)
- Məxfi məlumatlar yalnız icazəli şəxslərə açıq olmalıdır
- Başqası sənin fayllarını oxuya bilməməlidir
- Misal: Şifrələnmiş fayl sistemi

#### Integrity (Bütövlük)
- Heç kim sənin fayllarını icazəsiz dəyişdirə bilməməlidir
- Həm saxlanılan, həm də ötürülən məlumatlar qorunmalıdır
- Misal: Fayl dəyişdirilib-dəyişdirilmədiyini yoxlayan hash

#### Availability (Əlçatımlılıq)
- Sistemin istifadəçiyə lazım olanda işlək olması
- Misal: DDoS hücumu əlçatımlılığa hücumdur

---

## Task 2 — OS Təhlükəsizliyinin Ümumi Nümunələri

Bu tapşırıqda 3 əsas zəiflik növü öyrədilir:

---

### 1. Authentication və Zəif Şifrələr

**Authentication (Kimlik doğrulama)** — sistemin sənin kim olduğunu yoxlamasıdır.

3 əsas üsul:
- **Bildiyiniz bir şey** — şifrə, PIN kod
- **Olduğunuz bir şey** — barmaq izi, üz tanıma
- **Sahib olduğunuz bir şey** — telefon nömrəsi (SMS kodu)

**Şifrələr ən çox hücuma məruz qalan üsuldur** — çünki insanlar zəif şifrə seçir.

**Ən çox istifadə edilən 20 şifrə (NCSC siyahısı):**

| Sıra | Şifrə |
|------|-------|
| 1 | 123456 |
| 2 | 123456789 |
| 3 | qwerty |
| 4 | password |
| 5 | 111111 |
| 6 | 12345678 |
| 7 | abc123 |
| 8 | 1234567 |
| 9 | password1 |
| 10 | 12345 |
| 11 | 1234567890 |
| 12 | 123123 |
| 13 | 000000 |
| 14 | iloveyou |
| 15 | 1234 |
| 16 | 1q2w3e4r5t |
| 17 | qwertyuiop |
| 18 | 123 |
| 19 | monkey |
| 20 | dragon |

**Niyə `qwerty`, `1q2w3e4r5t` zəifdir?**
- Klaviatura düzülüşünü izləyir — hücumçular bu nümunəni bilirlər

**Güclü şifrənin xüsusiyyətləri:**
- Ən az 12 simvol
- Böyük + kiçik hərf + rəqəm + xüsusi simvol
- Şəxsi məlumat (ad, doğum tarixi) yoxdur
- Hər hesab üçün fərqli şifrə
- Misal: `LearnM00r!` — güclü şifrədir

---

### 2. Zəif Fayl İcazələri (Weak File Permissions)

**Principle of Least Privilege (Ən Az İmtiyaz Prinsipi):**
> İstifadəçi yalnız işi üçün lazım olan resurslara giriş hüququna sahib olmalıdır — nə çox, nə az.

**Zəif fayl icazələrinin nəticələri:**

- **Gizlilik pozulur:** Hücumçu oxumamalı olduğu faylları oxuya bilir
- **Bütövlük pozulur:** Hücumçu dəyişdirməməli olduğu faylları dəyişdirə bilir

**Linux-da fayl icazələri:**
```
-rwxr-xr-- 1 root users file.txt
```
- `r` = oxumaq (read)
- `w` = yazmaq (write)
- `x` = işlətmək (execute)
- Birinci üçlük = sahibin icazəsi
- İkinci üçlük = qrupun icazəsi
- Üçüncü üçlük = digərlərin icazəsi

---

### 3. Zərərli Proqramlara Giriş (Malicious Programs)

Zərərli proqramlar (malware) CIA üçlüyünün hər birinə hücum edə bilər:

#### Trojan Horse (Troya atı)
- Özünü faydalı proqram kimi göstərir
- Əslində hücumçuya sistemə giriş verir
- **Gizlilik** və **Bütövlüyü** pozur

#### Ransomware (Fidyə proqramı)
- İstifadəçinin fayllarını şifrələyir
- Fayllar oxunmaz hala gəlir
- Şifrəni açmaq üçün pul tələb edir
- **Əlçatımlılığa** hücum edir
- Misal: WannaCry, LockBit

#### Spyware (Casus proqram)
- Arxa planda işləyir, istifadəçi bilmir
- Şifrələri, klaviatura vuruşlarını, ekranı oğurlayır
- **Gizlilik** pozulur

---

## Task 3 — OS Təhlükəsizliyinin Praktiki Nümunəsi

Bu tapşırıqda real ssenari simulyasiya edilir — hücumçu kimi düşünmək lazımdır.

---

### Ssenari

Şirkətin ofisini ziyarət edəndə bir monitorda yapışqan kağız görünür:
```
sammie
dragon
```

Bu — istifadəçi adı və şifrədir!

---

### İstifadə Edilən Linux Əmrləri

#### `whoami` — Mən kimim?
```bash
whoami
# sammie
```
Hansı istifadəçi kimi daxil olduğunu göstərir.

---

#### `ssh USERNAME@IP` — Uzaq sistemə qoşulmaq
```bash
ssh sammie@<MACHINE_IP>
# Şifrə: dragon
```
- **SSH (Secure Shell)** — uzaq Linux sisteminə şifrəli bağlantı qurmaq üçün protokol
- Port 22-dən işləyir
- İstifadəçi adı + şifrə ilə giriş edilir

---

#### `ls` — Faylları siyahıla
```bash
ls
# user.txt  documents  downloads
```
Cari qovluqdakı faylları göstərir.

---

#### `cat FILENAME` — Faylın içini göstər
```bash
cat user.txt
# THM{flag_here}
```
Mətn faylının məzmununu ekranda göstərir.

---

#### `history` — Əvvəlki əmrlər
```bash
history
# 1  ls
# 2  cat file.txt
# 3  S3cr3tP4ssw0rd   ← istifadəçi şifrəni səhvən əmr kimi yazıb!
```
Terminalda yazılmış bütün əmrlərin tarixçəsini göstərir.

> 💡 Bu klassik zəiflikdir — istifadəçilər şifrəni səhvən terminal əmri kimi yazır. `history` əmri bunu saxlayır.

---

#### `su - root` — Root istifadəçisinə keçmək
```bash
su - root
# Password: S3cr3tP4ssw0rd
```
- `su` = switch user (istifadəçi dəyiş)
- `-` = root-un mühitini tam yüklə
- Root şifrəsini daxil et → tam sistem nəzarəti

---

### Hücumun Tam Axışı

```
1. Ofisdə yapışqan kağız tapıldı → sammie:dragon

2. SSH ilə giriş:
   ssh sammie@<IP>
   şifrə: dragon ✅

3. Sistemi kəşf et:
   whoami → sammie
   ls → faylları gör
   cat user.txt → user flag tap

4. /etc/passwd faylına bax:
   cat /etc/passwd
   → johnny, linda, sammie istifadəçiləri var

5. johnny-nin şifrəsini tap (top 7 şifrə siyahısından):
   ssh johnny@<IP>
   şifrə: abc123 ✅

6. johnny-nin tarixçəsinə bax:
   history
   → root şifrəsi səhvən yazılıb

7. Root ol:
   su - root
   şifrə: <tarixçədən tapılan şifrə> ✅

8. Root flag tap:
   cat /root/flag.txt
```

---

## Öyrənilən Dərslər

| Zəiflik | Nə baş verdi | Həll yolu |
|---------|-------------|-----------|
| **Zəif şifrə** | `dragon` — top 20 siyahısında | Güclü, unikal şifrə istifadə et |
| **Görünən məlumat** | Şifrə yapışqan kağızda yazılı idi | Şifrəni heç yerdə yazma |
| **Tarixçədə şifrə** | `history` əmrində root şifrəsi görünürdü | `history -c` ilə təmizlə |
| **Zəif istifadəçi şifrəsi** | johnny: abc123 — top 7-də | Güclü şifrə politikası tətbiq et |

---

## Əsas Terminlər Lüğəti

| Termin | İzah |
|--------|------|
| **OS** | Əməliyyat sistemi — hardware ilə proqramlar arasında təbəqə |
| **CIA** | Confidentiality, Integrity, Availability — təhlükəsizliyin 3 sütunu |
| **Authentication** | Kimlik doğrulama |
| **Least Privilege** | İstifadəçiyə yalnız lazım olan icazə verilir |
| **Malware** | Zərərli proqram |
| **Ransomware** | Faylları şifrələyib fidyə tələb edən proqram |
| **SSH** | Uzaq sistemə şifrəli bağlantı protokolu |
| **su** | Switch User — Linux-da istifadəçi dəyişdirmək |
| **history** | Terminalda yazılmış əmrlərin siyahısı |
| **root** | Linux-da tam administrator hesabı |

---

*Mənbə: TryHackMe — "Operating System Security" Lab (60 dəqiqə)*
