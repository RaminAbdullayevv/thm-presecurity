# 🐧 Linux CLI Basics

> **TryHackMe Room:** [tryhackme.com/room/linuxclibasics](https://tryhackme.com/room/linuxclibasics)
> **Modul:** Operating Systems
> **Səviyyə:** Başlanğıc

---

## Task 1 — Giriş (Introduction)

Kibertəhlükəsizlik sahəsində Linux hər yerdədir — serverləri, təhlükəsizlik alətlərini, hətta hacking mühitlərini gücləndirən sistemdir. Lakin sistemləri müdafiə etməzdən və ya hadisələri araşdırmazddan əvvəl Linux OS-ini **command-line interface (CLI)** vasitəsilə naviqasiya etməyi bilmək lazımdır.

Bu room sənə kibertəhlükəsizlik komandası üzrə **yeni intern** rolunda bələdçilik edən missiyaya aparır.

### Ssenari

Cyber Operations Support Team-ə IT Support Engineer kimi işə başladığın ilk gündür. Müşfiq sənə lazımi tanışlıq turunu keçirməmiş, təcili bir hadisəni həll etmək üçün qaçmaq məcburiyyətində qalmışdı. Masanda bir qısa qeyd qoyub getmişdi:

> *"Xoş gəlmisən! Hər şeyi izah etməyə vaxtım olmadı, amma sən tez anlayacaqsan. Bizim işimizin çox hissəsi Linux terminalında baş verir. Sistemə bir neçə sadə tapşırıq qoymuşam ki, rahat hiss edəsən. Terminalı aç və bir az araşdır."*

İndi yalnız özünsən — terminal prompt-u və bir az sirr qarşında. Narahat olma, bu roomdakı hər addım sənə nə etməyin lazım olduğunu göstərəcək.

### Öyrənmə Hədəfləri

- Linux terminalının nə olduğunu və nə üçün istifadə edildiyini anlamaq
- Linux mühiti ilə rahat qarşılıqlı əlaqə qurmaq
- Əsas komandalarla Linux fayl sistemini naviqasiya etmək

---

## Task 2 — Naviqasiya Missiyası: "İtkin Qeydləri Tap" (Navigation Mission)

### Terminal Haqqında Qısa Məlumat

**Terminal** — Linux sistemini idarə etmək üçün mətn əsaslı interfeysdir. Qrafik interfeyslə qarşılıqlı əlaqə qurmaq əvəzinə, kompütərə nə etməsini tam olaraq izah edən komandalar yazırsan.

Kibertəhlükəsizlik mütəxəssisləri terminalı bu səbəblərdən istifadə edirlər:
- İkonlara klikləməkdən **daha sürətlidir**
- **Daha çox nəzarət** verir
- Bir çox təhlükəsizlik alətləri **yalnız terminalla** işləyir

Terminal hələ tanış deyilsə — bu tamamilə normaldır. Bu roomun sonunda o sənə çox rahat gəlməyə başlayacaq.

---

### Addım 1 — "Haradayam?" → `pwd`

```bash
ubuntu@tryhackme:~$ pwd
/home/ubuntu
```

`pwd` — **"print working directory"** deməkdir. Yəni: "hal-hazırda hansı qovluğun içindəyəm?" sualının cavabını verir.

Terminalı açdıqda sistemin hansı hissəsindəsən bilmirsənsə, bu komanda ilk istifadə etməli olduğun şeydir.

---

### Addım 2 — "Ətrafımda Nə Var?" → `ls`

```bash
ubuntu@tryhackme:~$ ls
Desktop    Downloads  Pictures  Templates  logs
Documents  Music      Public    Videos     projects
```

`ls` cari qovluğun məzmununu siyahıya salır. Daha ətraflı məlumat üçün:

```bash
ubuntu@tryhackme:~$ ls -l
total 44
drwxr-xr-x 2 ubuntu ubuntu 4096 Feb 27  2022 Desktop
drwxr-xr-x 6 ubuntu ubuntu 4096 Dec 11 12:45 Documents
...
```

`ls -l` hər faylın icazələrini, sahibini, ölçüsünü, tarixini göstərir.

#### Gizli Fayllar

Linuxda `.` nöqtə ilə başlayan fayllar **gizli fayl** hesab olunur. Standart `ls` onları göstərmir. Görmək üçün:

```bash
ubuntu@tryhackme:~$ ls -al
```

`-a` — bütün faylları (gizlinlər də daxil) göstərir.
`-l` — uzun format (ətraflı məlumat).

> 💡 Gizli fayllar əslində "gizli sirr" deyil — sadəcə ada nöqtə ilə başlayır, Linux onları default olaraq gizlədir.

---

### Addım 3 — Hərəkət Et → `cd`

```bash
ubuntu@tryhackme:~$ cd Documents
ubuntu@tryhackme:~/Documents$ pwd
/home/ubuntu/Documents
```

`cd` — **"change directory"** deməkdir. Bir qovluğa girməyin yoludur.

Bir qovluq geri qayıtmaq üçün:

```bash
ubuntu@tryhackme:~/Documents$ cd ..
ubuntu@tryhackme:~$
```

`..` — həmişə bir üst qovluğu ifadə edir.

| Komanda | Nə Edir |
|---|---|
| `cd Documents` | Documents qovluğuna girir |
| `cd ..` | Bir üst qovluğa qayıdır |
| `cd ~` | Ev qovluğuna (home) qayıdır |
| `cd /etc` | Sistemin `/etc` qovluğuna keçir |

---

### Addım 4 — "Find" — Güclü Axtarış Aləti → `find`

```bash
ubuntu@tryhackme:~$ find ~ -name mission_brief.txt
/home/ubuntu/Documents/.research/archive/mission_brief.txt
```

**Sintaksis:** `find <başlanğıc_nöqtəsi> -name <fayl_adı>`

- `~` — ev qovluğundan başla
- `-name` — faylın adı ilə axtar

`find` komandası hər alt qovluğu yoxlayır. Fayl tapılarsa, **tam yolu** çap edir. Bu, gizli qovluqlardakı faylları tapmaq üçün çox güclü bir vasitədir.

---

### Addım 5 — Faylı Oxu → `cat`

```bash
ubuntu@tryhackme:~$ cat mission_brief.txt
Növbəti tapşırığın sistem haqqında kiçik hesabat toplamaq:
- Kim kimi daxil oldun
- Kernel versiyası
- Ümumi disk sahəsi
- Bu Linux distribütivinin adı

FLAG: MISSION-FOUND
```

`cat` — faylın məzmununu terminal ekranında göstərir. Qısa faylları oxumağın ən sürətli yoludur.

---

## Task 3 — Sistemi Araşdır (Investigating the System)

Müşfiqin mission_brief.txt faylında yeni tapşırıq var idi: sistemin haqqında əsas məlumatları topla. Bu, kibertəhlükəsizlik komandalarının hansı mühitdə işlədiklərini anlamaları, Linux-un hansı versiyasından istifadə etdiklərini öyrənmələri üçün lazımdır. Bu, maşının sürətli "sağlamlıq yoxlaması" kimidir.

---

### Addım 1 — "Kim kimi daxil olmuşam?" → `whoami`

```bash
ubuntu@tryhackme:~$ whoami
ubuntu
```

`whoami` — ən sadə Linux komandalarından biridir, lakin ən faydalılarından biridir. Cari istifadəçi adını çap edir.

> 🔐 Kibertəhlükəsizlik baxımından: Bir sistemə girişdən sonra ilk icra etdiyin komandalardan biri budur — hansı hüquqlarla işlədiyini anlamaq üçün.

---

### Addım 2 — "Hansı sistemdəyəm?" → `uname -a`

```bash
ubuntu@tryhackme:~$ uname -a
Linux tryhackme 6.14.0-1018-aws #17-Ubuntu SMP Mon Sep 2 13:48:07 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

`uname -a` — əməliyyat sistemi, kernel versiyası və arxitektura haqqında ətraflı məlumat verir.

**Çıxışın izahı:**

| Hissə | Məna |
|---|---|
| `Linux` | Sistem Linux kernel işlədir |
| `tryhackme` | Hostname — kompüterin adı |
| `6.14.0-1018-aws` | Quraşdırılmış kernel versiyası |
| `x86_64` | Hardware platformu (64-bit) |
| `GNU/Linux` | ƏS növü (Linux kernel + GNU alətləri) |

Yalnız ƏS adını görmək istəyirsənsə:
```bash
ubuntu@tryhackme:~$ uname
Linux
```

---

### Addım 3 — Disk Sahəsini Yoxla → `df -h`

```bash
ubuntu@tryhackme:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        70G   12G   58G  17% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
tmpfs           774M  1.2M  773M   1% /run
```

`df -h` — disk istifadəsini **oxunaqlı formada** göstərir.

- `-h` — "human readable" deməkdir; `58G`, `1.9G` kimi göstərir, uzun bayt sayları əvəzinə
- `/dev/root` — sistemin əsas diski
- `tmpfs` — RAM-da saxlanılan müvəqqəti fayl sistemləri (fiziki diskdə deyil)

> 💡 Gerçək mühitlərdə alətləri işlətməzdən və ya logları analiz etməzdən əvvəl disk sahəsini yoxlamaq lazımdır.

---

### Addım 4 — Sistem Faylını Oxu → `/etc/os-release`

Linux, konfiqurasiya və məlumat fayllarını `/etc` qovluğunda saxlayır.

```bash
ubuntu@tryhackme:~$ cd /etc
ubuntu@tryhackme:/etc$ cat os-release
PRETTY_NAME="Ubuntu 24.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.1 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
```

`os-release` faylı demək olar hər Linux sistemdə mövcuddur. Linux distribütivi haqqında `uname` komandası ilə müqayisədə çox daha aydın məlumat verir.

---

### Mini Çağırış

Müşfiq son tapşırıq olaraq `day1_report.txt` adlı faylı tapıb oxumağı istədi. Addımlar:

```bash
# 1. Faylı tap
find ~ -name day1_report.txt

# 2. Tapılan qovluğa geç
cd /tapilan/yol/

# 3. Faylı oxu
cat day1_report.txt
```

Bu mini-çağırış sənin öyrəndiklərini müstəqil tətbiq etmənin sınanmasıdır.

---

## Task 4 — Nəticə (Conclusion)

Cyber Operations Support Team-ə ilk günün tamamlandı. Bu roomda öyrəndiklərin:

- Linux fayl sistemini naviqasiya etmək
- Faylları axtarmaq
- Sistem məlumatlarını toplamaq
- Vacib konfiqurasiya fayllarını oxumaq
- İpuclarını izləyib Linux mühitinin içindəki missiyaları tamamlamaq

Bu bacarıqlar sadə görünə bilər, amma bunlar sonra gələcək hər şeyin təməlini qurur: fayl icazələri, istifadəçilər və qruplar, proseslər, paket idarəetməsi, və nəhayət real təhlükəsizlik alətləri.

---

## 📋 Öyrənilən Komandalar — Xülasə Cədvəli

| Komanda | Tam Adı | Nə Edir | Nümunə |
|---|---|---|---|
| `pwd` | Print Working Directory | Cari qovluğun tam yolunu göstərir | `pwd` → `/home/ubuntu` |
| `ls` | List | Cari qovluğun məzmununu siyahıya salır | `ls` |
| `ls -l` | List Long | Ətraflı format (icazələr, sahibi, tarix) | `ls -l` |
| `ls -al` | List All Long | Gizli fayllar da daxil olmaqla ətraflı siyahı | `ls -al` |
| `cd` | Change Directory | Qovluq dəyişir | `cd Documents` |
| `cd ..` | — | Bir üst qovluğa qayıdır | `cd ..` |
| `cd ~` | — | Ev qovluğuna qayıdır | `cd ~` |
| `find` | — | Fayl sistemində fayl axtarır | `find ~ -name mission_brief.txt` |
| `cat` | Concatenate | Faylın məzmununu göstərir | `cat fayl.txt` |
| `whoami` | — | Cari istifadəçi adını göstərir | `whoami` → `ubuntu` |
| `uname -a` | Unix Name | Kernel və sistem məlumatlarını göstərir | `uname -a` |
| `df -h` | Disk Free | Disk sahəsini oxunaqlı formada göstərir | `df -h` |

---

## 🗂️ Linux Fayl Sistemi — Əsas Qovluqlar

| Qovluq | Məzmun |
|---|---|
| `/home/ubuntu` | İstifadəçinin ev qovluğu |
| `/etc` | Sistem konfiqurasiya faylları |
| `/var` | Dəyişkən məlumatlar (loglar, keşlər) |
| `/tmp` | Müvəqqəti fayllar |
| `/bin` | Əsas sistem komandaları |

---

## 💡 Faydalı Qısayollar

```bash
# Tab ilə avtomatik tamamlama
cd Doc<TAB>   # → cd Documents/

# Əvvəlki komandaya qayıt
↑ (yuxarı ox düyməsi)

# Ekranı təmizlə
clear

# Komandalara yardım al
man ls        # ls komandasının tam sənədləşməsi
```

---

## 🔗 Növbəti Addımlar

- Windows CLI Basics (tezliklə)
- Linux Users & Groups
- File Permissions

---

*📅 TryHackMe — Operating Systems Modulu | Linux CLI Basics*
