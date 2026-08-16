# TryHackMe — The CIA Triad | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** The CIA Triad  
> **Çətinlik:** Easy  
> **Müddət:** 30 dəqiqə  
> **Link:** https://tryhackme.com/room/theciatriad  

---

## Ümumi Baxış

Bu otaq kibertəhlükəsizlik yolunun **ilk addımıdır** — artıq şəbəkə, veb və digər texniki anlayışları öyrəndikdən sonra "biz nəyi qoruyuruq?" sualına cavab verilir. Cavab: **CIA Triadı** — Confidentiality (Məxfilik), Integrity (Bütövlük) və Availability (Əlçatanlıq). Bu üç prinsip kibertəhlükəsizliyin bütün düşüncə tərzinin əsasıdır.

---

## Task 1 — Giriş (Introduction)

### İzahat

Keçmiş dövrlərdə məlumatlar kağız üzərindəki sənədlərdə saxlanılırdı. Bu gün isə eyni məlumatlar rəqəmsal şəkildə sistemlərdə saxlanılır, şəbəkələr üzərindən ötürülür. Düzgün təhlükəsizlik tədbirləri olmadan bu rəqəmsal məlumatlar ciddi nəticələrlə üzləşə bilər:

- Yanlış insanların əlinə keçə bilər
- İcazəsiz dəyişdirilə bilər
- Ən lazımlı anda əlçatmaz ola bilər

Buna görə rəqəmsal məlumatların qorunması dövlət, təşkilat və fərdlər üçün əsas tələbə çevrilmişdir. CIA Triadı bu qorumanın çərçivəsini müəyyən edir.

---

### Sual və Cavab

> **Q: I am ready to start!**  
> ✅ No Answer Needed — "Completed" düyməsinə bas.

---

## Task 2 — CIA Triadını Anlamaq (Understanding the CIA Triad)

### İzahat

**CIA Triadı** kibertəhlükəsizliyin üç əsas dirəyidir. Bu üç prinsip birlikdə informasiya təhlükəsizliyinin bütün mənzərəsini əhatə edir:

```
         ╔══════════════════╗
         ║   CIA  TRİADI    ║
         ╚══════════════════╝
                  │
    ┌─────────────┼─────────────┐
    │             │             │
    ▼             ▼             ▼
┌────────┐  ┌──────────┐  ┌──────────────┐
│  C     │  │    I     │  │     A        │
│Məxfilik│  │ Bütövlük │  │ Əlçatanlıq  │
│Confid- │  │Integrity │  │Availability  │
│entialy │  │          │  │              │
└────────┘  └──────────┘  └──────────────┘
```

---

### Confidentiality — Məxfilik (C)

**Məxfilik** — həssas məlumatlara yalnız icazəli şəxslərin çıxış əldə etməsini təmin edir. Məxfilik pozulduqda icazəsiz şəxslər məlumatlara çata bilər — bu maliyyə itkisinə, məxfilik pozuntusuna və ya hüquqi nəticələrə gətirib çıxara bilər.

**Gündəlik həyat nümunəsi:**

Dostunuzla xüsusi bir mövzuda söhbət edərkən yad bir şəxs qulaq asır və həmin məlumatı sizi manipulyasiya etmək üçün istifadə edir. Bu, məxfiliyə bilavasitə zərər vurur. Gələcəkdə belə söhbətləri etibarlı mühitdə etmək məxfiliyi təmin edir.

**Rəqəmsal dünya nümunəsi:**

İctimai WiFi-da sosial media hesabınıza daxil olursunuz. Kimin isə şəbəkəni dinləməsi nəticəsində parolu ələ keçirilir, siz hesabınızdan çıxarılırsınız. Bu, məxfiliyə zərər verən real bir hücumdur. **Şifrələmə** (encryption) və **giriş nəzarəti** (access control) məxfiliyi qoruyur.

**Nümunə cədvəli:**

| Vəziyyət | Məxfilik Qorunubmu? |
|----------|---------------------|
| Gmail parolu iş masasındakı stikerdə yazılıb | ❌ Xeyr |
| Şirkətin daxili sənədlərinə yalnız ehtiyacı olan işçilər çıxış əldə edir | ✅ Bəli |
| Şəxsi sənədiniz internetdə açıq şəkildə mövcuddur | ❌ Xeyr |

> **Kibertəhlükəsizlik baxımından məxfiliyi pozan hücumlar:** Man-in-the-Middle (MitM), Sniffing (Şəbəkə dinləmə), Phishing (Fişinq), Data Breach (Məlumat sızması)

---

### Integrity — Bütövlük (I)

**Bütövlük** — icazəsiz şəxslərin məlumatları dəyişdirə bilməməsini təmin edir. Bütövlük pozulduqda məlumatlara etibar edilmir. Məlumatın icazəsiz dəyişdirilməsi bəzən təhlükəli nəticələrə yol aça bilər.

**Gündəlik həyat nümunəsi:**

Müəllim imtahan qiymətinizi yazır — ancaq sənəd müvafiq orqana göndərilmədən əvvəl kimsə qiyməti dəyişdirir. Bu, qiymətləndirmənin bütövlüyünü pozur. Müəllim gələcəkdə qiymətləri ayrıca vərəqdə qeyd edib yekun göndərmədən əvvəl yoxlaya bilər.

**Rəqəmsal dünya nümunəsi:**

Bank köçürməsi edirsiniz. Əməliyyat tamamlanmadan kimsə araya girir və alıcı hesab məlumatını dəyişdirir. Pul başqa hesaba keçir. Bu, bütövlüyün pozulmasının ən ciddi nümunəsidir.

**Nümunə cədvəli:**

| Vəziyyət | Bütövlük Qorunubmu? |
|----------|---------------------|
| Məlumat icazəli təsdiq ilə dəyişdirildi | ✅ Bəli |
| Müəllim davamiyyət siyahısını kilidlədikdən sonra dəyişdirildi | ❌ Xeyr |
| Ödəniş məbləği ödənişdən əvvəl dəyişdirildi | ❌ Xeyr |

> **Kibertəhlükəsizlik baxımından bütövlüyü pozan hücumlar:** MitM ilə məlumat dəyişikliyi, SQL Injection ilə məlumat bazasındakı qeydləri dəyişdirmək, Malware ilə fayl dəyişikliyi

---

### Availability — Əlçatanlıq (A)

**Əlçatanlıq** — məlumat və xidmətlərin icazəli istifadəçilər tərəfindən lazım olduqda əlçatan olmasını təmin edir. Əlçatanlıq pozulduqda heç bir məlumat sızmasa belə, iş dayanır və böyük ziyan yaranır. Qısa müddətli dayanma belə ciddi nəticələr doğura bilər.

**Gündəlik həyat nümunəsi:**

Pulunuzu bankda saxlayırsınız — çox etibarlıdır. Amma elektrik kəsintisi səbəbindən bank bağlıdır. Pul orada olsa da, sizinlə gün lazım olanda çata bilmirsiniz. Buna görə banklar yedək generator qoyur — xidmət fasiləsiz davam edir.

**Rəqəmsal dünya nümunəsi:**

Hücumçular bir vebsayta həddindən artıq sorğu göndərir (DoS — Denial of Service hücumu). Sayt çöküb xidmət verə bilmir. Heç bir məlumat sızmır, dəyişdirilmir — amma əlçatanlıq pozulduğundan biznes daxil ola bilmir. Saytlar bu halı önləmək üçün rate limiting (sorğu məhdudlaşdırması) tətbiq edir.

**Nümunə cədvəli:**

| Vəziyyət | Əlçatanlıq Qorunubmu? |
|----------|-----------------------|
| Proqram quraşdırılması kritik xidmətləri pozdu | ❌ Xeyr |
| İş saatlarında şirkətin saytı çökdü | ❌ Xeyr |
| Bütün sistemlər iş saatlarında işçilər üçün əlçatandır | ✅ Bəli |

> **Kibertəhlükəsizlik baxımından əlçatanlığı pozan hücumlar:** DoS/DDoS hücumları, Ransomware (məlumatı şifrələyib girilməz edir), Hardware xəraları, Güc kəsilməsi

---

### Task 2 Sualları və Cavabları

> **Q: Which pillar of the CIA focuses on preventing unauthorized modification of data?**  
> ✅ `Integrity`  
> *İzah: Bütövlük (Integrity) məlumatın icazəsiz dəyişdirilməsinin qarşısını alır — yalnız icazəli dəyişikliklər qəbul edilir.*

> **Q: Which pillar of the CIA focuses on preventing unauthorized access to data?**  
> ✅ `Confidentiality`  
> *İzah: Məxfilik (Confidentiality) həssas məlumatlara yalnız icazəli şəxslərin çıxışını təmin edir.*

> **Q: Which CIA pillar ensures data is available to users when needed?**  
> ✅ `Availability`  
> *İzah: Əlçatanlıq (Availability) məlumat və xidmətlərin lazım olduqda əlçatan olmasını zəmanət altına alır.*

> **Q: Which CIA pillar gets impacted if the data becomes untrustworthy?**  
> ✅ `Integrity`  
> *İzah: Məlumat etibar edilə bilməz hala gəldikdə — dəyişdirilmişdir deməkdir — bu, Bütövlüyün (Integrity) pozulmasıdır.*

> **Q: What is the term used collectively for all these pillars?**  
> ✅ `CIA Triad`  
> *İzah: Confidentiality, Integrity və Availability birlikdə CIA Triadı adlanır.*

---

## Task 3 — Təhlükəsizlik Düşüncə Tərzi (The Security Mindset)

### İzahat

CIA Triadı yalnız bir sıra təriflər deyil — bu, kibertəhlükəsizlik mütəxəssislərinin **düşüncə tərzidir** (mindset). Bir təhlükəsizlik hadisəsi baş verdikdə, mütəxəssislər dərhal bu sualları verirlər:

```
❓ Həssas məlumatlar icazəsiz şəxslərə ifşa olundu?
   → Confidentiality (Məxfilik) pozulub

❓ Məlumatlar icazəsiz dəyişdirildi?
   → Integrity (Bütövlük) pozulub

❓ Sistemlər/xidmətlər lazım olan anda əlçatmaz oldu?
   → Availability (Əlçatanlıq) pozulub
```

Bu suallar hadisənin təsirini qiymətləndirməyə və düzgün cavab tədbirləri seçməyə kömək edir.

---

### Praktiki Tapşırıq — Doqquz Hadisə (Drag & Drop)

"View Site" düyməsinə basın. Ekranda **doqquz fərqli təhlükəsizlik hadisəsi** görünür. Hər hadisəni oxuyub CIA Triadının düzgün bölməsinə sürükləyib buraxmaq lazımdır.

**Hadisələrin təhlili:**

**Confidentiality (Məxfilik) bölməsinə aid hadisələr:**

| Hadisə | Niyə Məxfilik? |
|--------|----------------|
| Müştəri məlumatları bazası sızdırıldı | Gizli məlumat icazəsiz şəxslərə ifşa olundu |
| İstifadəçi parolları açıq mətndə saxlanılıb | Parola icazəsiz çıxış mümkündür |
| Şifrəsiz WiFi üzərindən həssas məlumat ötürüldü | Şəbəkəni dinləyən hər kəs məlumatı əldə edə bilər |

**Integrity (Bütövlük) bölməsinə aid hadisələr:**

| Hadisə | Niyə Bütövlük? |
|--------|----------------|
| Bank köçürməsi zamanı məbləğ dəyişdirildi | Məlumat icazəsiz modifikasiya edildi |
| Tibbi qeydlər xəstənin xəbəri olmadan yeniləndi | İcazəsiz məlumat dəyişikliyi |
| Vebsaytın məzmunu hücumçular tərəfindən dəyişdirildi | İcazəsiz kontentin əvəzlənməsi |

**Availability (Əlçatanlıq) bölməsinə aid hadisələr:**

| Hadisə | Niyə Əlçatanlıq? |
|--------|------------------|
| DDoS hücumu nəticəsində sayt çökdü | Xidmət əlçatmaz oldu |
| Serverin hardware xərası nəticəsində sistem dayanıb | Məlumata çıxış mümkün deyil |
| Güc kəsilməsi şirkətin xidmətlərini dayandırdı | Sistem istifadəçilər üçün əlçatmaz oldu |

---

**Hadisələri düzgün bölmələrə yerləşdirdikdən sonra flag ekrana çıxır.**

---

### Suallar və Cavablar

> **Q: What is the flag received after solving the exercise?**  
> ✅ `THM{CIA_IS_ABOUT_BALANCE}`  
> *İzah: Doqquz hadisənin hamısını düzgün CIA bölməsinə yerləşdirdikdən sonra bu flag ekrana çıxır.*

> **Q: CIA Triad is not just a set of definitions; it's a mindset. What type of mindset is it?**  
> ✅ `security mindset`  
> *İzah: CIA Triadı kibertəhlükəsizlik mütəxəssislərinin hadisələrə yanaşma tərzi olan "security mindset" (təhlükəsizlik düşüncə tərzi)dir.*

---

## Task 4 — Nəticə (Conclusion)

### İzahat

Bu otaq kibertəhlükəsizlik yolunun **ilk böyük addımıdır**. Öyrənilən əsas məsələ: **Kibertəhlükəsizlikdə nəyi qoruyuruq?**

CIA Triadını başa düşməklə sizə kibertəhlükəsizlik mütəxəssislərinin istifadə etdiyi əsas düşüncə tərzi verildi. Bu model gələcəkdə qarşılaşacağınız bir çox kibertəhlükəsizlik konseptinin özəyidir.

**Əsas terminologiya:**

| Termin | Tərifi |
|--------|--------|
| **Confidentiality** | Rəqəmsal məlumatın icazəsiz şəxslər tərəfindən əldə edilməməsini təmin edir |
| **Integrity** | Rəqəmsal məlumatın icazəsiz şəkildə dəyişdirilməməsini təmin edir |
| **Availability** | Rəqəmsal məlumat və xidmətlərin lazım olduqda əlçatan olmasını təmin edir |

---

### Sual və Cavab

> **Q: Complete this room.**  
> ✅ No Answer Needed — "Completed" düyməsinə bas.

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Başlamağa hazıram | No Answer Needed |
| Task 2 | Məlumatın icazəsiz dəyişdirilməsinin qarşısını alan dirək? | `Integrity` |
| Task 2 | Məlumata icazəsiz girişin qarşısını alan dirək? | `Confidentiality` |
| Task 2 | Məlumatın lazım olduqda əlçatan olmasını təmin edən dirək? | `Availability` |
| Task 2 | Məlumat etibarsız hala gəldikdə hansı dirək təsirlənir? | `Integrity` |
| Task 2 | Bu dirəklərin birlikdə adı nədir? | `CIA Triad` |
| Task 3 | Tapşırıq həll edildikdən sonra alınan flag? | `THM{CIA_IS_ABOUT_BALANCE}` |
| Task 3 | CIA Triadı hansı növ düşüncə tərzidir? | `security mindset` |
| Task 4 | Otağı tamamla | No Answer Needed |

---

## Bonus: CIA Triadını Daha Dərin Anlamaq

### Üç Dirəyin Bir-biri ilə Münasibəti

CIA Triadı bir üçbucaq kimidir — üç tərəf bir-birini tamamlayır. Birinin zəifliyi digərini mənasız edə bilər:

```
       Məxfilik
         /\
        /  \
       /    \
      /  CIA \
     /  Triad \
    /____________\
 Bütövlük    Əlçatanlıq
```

**Praktiki nümunə — Ransomware hücumu:**

Ransomware məlumatları şifrələyir:
- **Məxfilik:** Məlumat başqasına açılmır ✅ (pozulmur)
- **Bütövlük:** Məlumat dəyişdirilmir ✅ (pozulmur)
- **Əlçatanlıq:** Sahibi öz məlumatına çata bilmir ❌ **POZULUR**

Bu hücum yalnız əlçatanlığı hədəfləyir — amma nəticəsi olduqca dağıdıcıdır.

---

### DAD Triadı — CIA-nın əksi

Hücumçular CIA Triadını pozmağa çalışır — bu zaman **DAD Triadı** meydana çıxır:

| CIA | DAD | İzahı |
|-----|-----|-------|
| **C**onfidentiality | **D**isclosure (İfşa) | Gizli məlumatın icazəsiz şəxslərə açılması |
| **I**ntegrity | **A**lteration (Dəyişiklik) | Məlumatın icazəsiz modifikasiyası |
| **A**vailability | **D**estruction (Məhvetmə) | Məlumat və xidmətləri əlçatmaz etmək |

---

### Real Həyat Ssenarisi — Hücum Analizi

**Ssenari:** Bir hacker e-ticarət saytına daxil olur:

1. Müştərilərin kredit kartı məlumatlarını oğurlayır → **Məxfilik pozulur** (Disclosure)
2. Malların qiymətlərini dəyişdirir (500₼ olanı 5₼ edir) → **Bütövlük pozulur** (Alteration)
3. Serveri çöküdür — sayt işləmir → **Əlçatanlıq pozulur** (Destruction)

Bu hücum CIA Triadının **üç dirəyini birlikdə** hədəfləyir.

---

### CIA Triadını Qoruyan Tədbirlər

| Dirək | Qoruyucu Tədbirlər |
|-------|-------------------|
| **Məxfilik** | Şifrələmə (Encryption), Giriş nəzarəti (Access Control), VPN, MFA (çox faktorlu autentifikasiya) |
| **Bütövlük** | Hash yoxlaması, Rəqəmsal imzalar (Digital Signatures), Audit logları, Versiya nəzarəti |
| **Əlçatanlıq** | Yedəkləmə (Backup), Load Balancer, Redundant sistemlər, DDoS qoruması, Güc ehtiyat sistemləri |
