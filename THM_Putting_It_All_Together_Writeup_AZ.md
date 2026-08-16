# TryHackMe — Putting It All Together | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** Putting It All Together  
> **Çətinlik:** Easy  
> **Müddət:** 15 dəqiqə  
> **Link:** https://tryhackme.com/room/puttingitalltogether  

---

## Ümumi Baxış

Bu otaq **Pre-Security** yolunun yekun otağıdır. Əvvəlki bütün mövzular — DNS, HTTP, OSI modeli, paketlər, şəbəkə cihazları, HTML, JavaScript — bir araya gəlir və tam bir veb sorğusunun başdan-sona necə işlədiyini görürük. Bundan əlavə yeni komponentlər — **Load Balancer**, **CDN**, **Verilənlər Bazası**, **WAF** və **Veb Server** — ətraflı izah edilir.

---

## Task 1 — Hamısını Bir Yerə Yığmaq (Putting It All Together)

### İzahat

Bu task əvvəlki bütün mövzuların xülasəsidir. Bir vebsayta daxil olduğunuzda arxada bir çox proses baş verir:

```
1. DNS  → "tryhackme.com" = 104.26.10.229
2. TCP  → Serverə qoşulma (3-addımlı əl sıxışma)
3. HTTP → Sorğu göndərilir (GET / HTTP/1.1)
4. Server → HTML, CSS, JS, şəkillər qaytarır
5. Brauzer → Alınan məlumatı render edib ekranda göstərir
```

Bütün bu addımları koordinasiya edən bir neçə əlavə komponent də mövcuddur — bunları növbəti tasklarda öyrənəcəyik.

---

### Sual və Cavab

> **Q: I've read this...**  
> ✅ No Answer Needed — "Completed" düyməsinə bas.

---

## Task 2 — Digər Komponentlər (Other Components)

### İzahat

Böyük vebsaytlar yalnız bir serverlə idarə oluna bilmir. Milyonlarla istifadəçiyə xidmət etmək, həm sürəti artırmaq, həm də sistemin daima aktiv olmasını təmin etmək üçün əlavə komponentlər lazımdır.

---

### Load Balancer (Yük Balanslaşdırıcısı)

Böyük trafikli saytlar üçün tək server yetərli olmur. Load Balancer gələn sorğuları **birdən çox server arasında paylaşdırır**.

```
              İSTİFADƏÇİ SORĞUSU
                     │
                     ▼
              [LOAD BALANCER]
               /      |      \
              /       |       \
        [Server 1] [Server 2] [Server 3]
        (30%)       (40%)      (30%)
```

**İki əsas funksiyası:**

| Funksiya | İzahı |
|----------|-------|
| **Yük paylaşdırma** | Gələn sorğuları serverlərin arasında bölür ki, heç biri həddindən artıq yüklənməsin |
| **Failover (Uğursuzluq keçişi)** | Bir server çöksə, gələn sorğular avtomatik digər serverlərə yönləndirilir |

**Load Balancing Alqoritmləri:**

- **Round Robin** — növbə ilə hər serverə göndərir (1→2→3→1→2→3...)
- **Weighted** — daha güclü serverlərə daha çox sorğu göndərir
- **Least Connections** — ən az aktiv əlaqəsi olan serverə göndərir

**Health Check (Sağlamlıq Yoxlaması):**

Load Balancer arxasındakı serverlərin işlək olub-olmadığını mütəmadi yoxlayır. Əgər bir server cavab vermirsə, o server siyahıdan çıxarılır; düzəlincə yenidən əlavə edilir.

```
Load Balancer → "Salam Server 1, işlirsinmi?" → [Server 1]
Load Balancer ← "Bəli, hər şey yaxşıdır!" ← [Server 1]   ✅

Load Balancer → "Salam Server 2, işlirsinmi?" → [Server 2]
(10 saniyə cavab yoxdur)                                   ❌
Load Balancer Server 2-ni siyahıdan çıxarır → Sorğuları Server 1 və 3-ə yönləndirir
```

---

### CDN (Content Delivery Network — Məzmun Çatdırılma Şəbəkəsi)

CDN — dünya üzrə coğrafi cəhətdən yayılmış serverlərdən ibarət bir şəbəkədir. Statik məlumatları (JavaScript, CSS, şəkillər, videolar) istifadəçiyə **ən yaxın serverdən** çatdırır.

```
    Londondakı istifadəçi       Tokiodakı istifadəçi
           │                           │
           ▼                           ▼
    [CDN Server London]        [CDN Server Tokyo]
           │                           │
           └───────── Ana Server ──────┘
                    (Nyu-York)
```

**Niyə sürətlidir?**

Əgər Bakıdakı istifadəçi Nyu-Yorkdakı serverə sorğu göndərirsə, məlumat minlərlə kilometr yol gəlir — bu gecikmə (latency) yaradır. Amma CDN həmin məlumatı Avropadakı serverə kopyalamışsa, Bakıdan çox daha yaxın olan Avropa serverindən cavab gəlir — **sürət artır**.

> **CDN-in statik vs dinamik məzmuna münasibəti:** CDN yalnız **statik** məlumatları (dəyişməyən fayllar) keşləyir — şəkillər, CSS, JS faylları. Dinamik məlumatlar (istifadəçiyə xas, dəyişkən) ana serverə gedib gəlir.

---

### Databases (Verilənlər Bazası)

Vebsaytlar istifadəçi məlumatları, yazılar, məhsullar kimi bütün dinamik məlumatları verilənlər bazasında saxlayır. Veb server lazım olduqda bazadan sorğu edir, cavab alır, istifadəçiyə göstərir.

**Ümumi verilənlər bazası növləri:**

| Növ | Nümunələr | İstifadəsi |
|-----|-----------|-----------|
| **Relational (İlişkili)** | MySQL, PostgreSQL, MSSQL | Struktur məlumatlar, cədvəllər |
| **NoSQL** | MongoDB, CouchDB | Yarı strukturlu, elastik məlumatlar |

---

### WAF (Web Application Firewall — Veb Tətbiq Güvənlik Divarı)

WAF — veb sorğusu ilə veb server arasında yerləşən bir qoruyucudur. Hücum cəhdlərini aşkarlayıb blok edir.

```
İstifadəçi → [WAF] → Veb Server
                ↓
         Zərərli sorğu?
         → BLOK ET! ❌
         
         Normal sorğu?
         → KEÇİR! ✅
```

**WAF nəyə nəzarət edir?**

| Nəzarət | İzahı |
|---------|-------|
| **Hücum texnikaları** | SQL Injection, XSS, Directory Traversal kimi tanınmış hücum sxemlərini aşkarlayır |
| **Bot yoxlaması** | Sorğunun real brauzerdən mi, bot-dan mı gəldiyini müəyyənləşdirir |
| **Rate Limiting** | Bir IP-dən saniyədə icazə verilən maksimum sorğu sayını məhdudlaşdırır — DoS hücumlarına qarşı |

---

### Suallar və Cavablar

> **Q: What can be used to host static files and speed up a clients visit to a website?**  
> ✅ `CDN`  
> *İzah: CDN (Content Delivery Network) statik faylları dünya üzrə yayılmış serverlərdə keşləyərək istifadəçiyə ən yaxın serverdən çatdırır — bu saytın yüklənmə sürətini artırır.*

> **Q: What does a load balancer perform to make sure a host is still alive?**  
> ✅ `Health Check`  
> *İzah: Load Balancer arxasındakı serverlərin işlək olub-olmadığını mütəmadi Health Check (sağlamlıq yoxlaması) ilə nəzarətdə saxlayır.*

> **Q: What can be used to help against the hacking of a website?**  
> ✅ `WAF`  
> *İzah: WAF (Web Application Firewall) veb server ilə istifadəçi arasında durub zərərli sorğuları blok edir.*

---

## Task 3 — Veb Serverlərin İşləməsi (How Web Servers Work)

### İzahat

**Veb server** — gələn HTTP sorğularını dinləyib cavab verən proqram təminatıdır. Ən məşhur veb server proqramları: **Apache**, **Nginx**, **IIS** (Microsoft), **LiteSpeed**.

---

### Virtual Hosts (Virtual Hostlar)

Bir fiziki server **birdən çox vebsaytı** eyni anda host edə bilər. Bu Virtual Host mexanizmi ilə mümkündür.

**Necə işləyir?**

Brauzer HTTP sorğusunda `Host` başlığını göndərir:
```http
GET / HTTP/1.1
Host: blog.tryhackme.com
```

Veb server bu başlığa baxıb uyğun saytı müəyyənləşdirir:

```
Host başlığı: "one.com"    → /var/www/website_one/
Host başlığı: "two.com"    → /var/www/website_two/
Host başlığı: "blog.com"   → /var/www/website_blog/
```

Eyni IP ünvanında minlərlə fərqli domen saxlanıla bilər — Virtual Hosts bunu mümkün edir.

---

### Statik vs Dinamik Məzmun

| | Statik Məzmun | Dinamik Məzmun |
|-|---------------|----------------|
| **Nədir?** | Hər kəsə eyni göstərilir | Hər istifadəçiyə fərqli olur |
| **Harada işlənir?** | Birbaşa fayl olaraq göndərilir | Backend kodla yaradılır |
| **Nümunə** | Şəkillər, CSS, JS faylları | Axtarış nəticələri, profil səhifəsi |
| **Dəyişirmi?** | Xeyr | Bəli — hər sorğuda fərqli ola bilər |

**Nümunə:**

- `logo.png` fayl kimi saxlanılır → **Statik** → CDN keşlənə bilər
- "Axırıncı 10 yazı" sorğusu → Verilənlər bazasından dinamik çəkilir → **Dinamik**

---

### Backend Proqramlaşdırma Dilləri

Dinamik məzmun yaratmaq üçün backend tərəfdə proqramlaşdırma dilləri işləyir. Bu kod **server tərəfindədir** — istifadəçi onu görə bilmir.

| Dil | Framework | İstifadə sahəsi |
|-----|-----------|----------------|
| **PHP** | Laravel | WordPress, məşhur CMS-lər |
| **Python** | Django, Flask | Müasir veb tətbiqləri |
| **Node.js** | Express | Real-time tətbiqlər |
| **Ruby** | Rails | Sürətli prototiplər |
| **Java** | Spring | Böyük müəssisə tətbiqləri |
| **C#** | ASP.NET | Microsoft ekosistemi |

> **Niyə istifadəçi backend kodu görə bilmir?**  
> Backend kodu serverdə işlənir. Server yalnız son **nəticəni** (HTML) göndərir. İstifadəçi "View Page Source" etsə, PHP/Python/Ruby kodunu yox, həmin kodun yaratdığı HTML-i görür.

---

### Suallar və Cavablar

> **Q: What does web server software use to host multiple sites?**  
> ✅ `Virtual Hosts`  
> *İzah: Virtual Hosts mexanizmi bir fiziki serverdə birdən çox vebsaytın host edilməsinə imkan verir. Server HTTP sorğusundakı `Host` başlığına baxaraq hansı saytın istənildiyini müəyyənləşdirir.*

> **Q: What is the name for the type of content that can change?**  
> ✅ `Dynamic`  
> *İzah: Dinamik məzmun hər sorğuda fərqli ola bilir — backend kodu verilənlər bazasından data çəkib hər dəfə yeni HTML yaradır.*

> **Q: Does the client see the backend code?**  
> ✅ `Nay`  
> *İzah: Xeyr — backend kodu server tərəfindədir. İstifadəçi yalnız backend kodun yaratdığı HTML/CSS/JS-i görür, kodun özünü yox.*

---

## Task 4 — Viktorina: Düzgün Sıra (Quiz)

### İzahat

Bu task interaktiv bir drag-and-drop (sürükləyib-buraxma) oyunudur. Bir vebsayta daxil olduqda baş verən bütün addımları **düzgün ardıcıllıqla** yerləşdirmək lazımdır.

Hər tile (plitə) düzgün yerə qoyulduqda **yaşıl** rəngə boyanır. Yanlış olduqda **qırmızı** olur. Bütün plitələr düzgün yerə qoyulduqda flag ekrana çıxır.

---

### Düzgün Ardıcıllıq

```
 1. Brauzerə "tryhackme.com" yazılır
          ↓
 2. DNS sorğusu göndərilir — IP ünvanı tapılır
          ↓
 3. IP ünvanı yerli DNS keşindən yoxlanır
          ↓
 4. Recursive DNS Serverə müraciət edilir
          ↓
 5. WAF sorğunu qəbul edir — zərərli mi yoxlayır
          ↓
 6. Load Balancer sorğunu alır — server seçir
          ↓
 7. Veb server HTTP GET sorğusunu qəbul edir
          ↓
 8. Tətbiq xidməti (Application) sorğunu emal edir
          ↓
 9. Verilənlər bazasına sorğu göndərilir
          ↓
10. Brauzer HTML-i render edib saytı göstərir
```

**Hər addımın rolu:**

| Addım | Komponent | Vəzifəsi |
|-------|-----------|----------|
| 1 | Brauzer | İstifadəçi domen adı yazır |
| 2-4 | DNS | Domen adını IP ünvanına çevirir |
| 5 | WAF | Zərərli sorğuları blok edir |
| 6 | Load Balancer | Sorğunu ən uyğun serverə yönləndirir |
| 7 | Veb Server | HTTP sorğusunu qəbul edib emal edir |
| 8 | Application | Backend kodu işlədilir |
| 9 | Database | Lazımi məlumatlar alınır |
| 10 | Brauzer | HTML render edilir, sayt görünür |

---

### Sual və Cavab

> **Q: Flag**  
> ✅ `THM{YOU_GOT_THE_ORDER}`  
> *İzah: Plitələri yuxarıdakı ardıcıllığa uyğun düzgün yerlərə qoyduqda flag avtomatik ekrana çıxır.*

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Oxudum... | No Answer Needed |
| Task 2 | Statik faylları host edib sürəti artıran nədir? | `CDN` |
| Task 2 | Load Balancer serverin işlək olduğunu yoxlamaq üçün nə edir? | `Health Check` |
| Task 2 | Vebsaytı hakerlikdən qorumaq üçün nə istifadə olunur? | `WAF` |
| Task 3 | Veb server proqramı birdən çox saytı host etmək üçün nə istifadə edir? | `Virtual Hosts` |
| Task 3 | Dəyişə bilən məzmunun adı nədir? | `Dynamic` |
| Task 3 | Müştəri backend kodu görə bilirmi? | `Nay` |
| Task 4 | Düzgün sıranın flag-i? | `THM{YOU_GOT_THE_ORDER}` |

---

## Bonus: Tam Veb Sorğusu — Başdan Sona

Bu diaqram **bütün Pre-Security yolunda öyrəndiklərimizi** bir araya gətirir:

```
┌─────────────────────────────────────────────────────────────┐
│                    İSTİFADƏÇİ                               │
│           Brauzerdə "tryhackme.com" yazır                   │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                       DNS                                   │
│  1. Yerli keş yoxlanır                                      │
│  2. ISP Recursive DNS serverinə müraciət                    │
│  3. Kök DNS → TLD DNS → Authoritative DNS                   │
│  4. IP ünvanı tapılır: 104.26.10.229                        │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                  TCP ƏLAQƏ                                  │
│  SYN → SYN/ACK → ACK (Üç addımlı əl sıxışma)              │
│  Port 443 (HTTPS) → SSL/TLS şifrələmə aktiv edilir         │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                      WAF                                    │
│  Sorğu zərərlidir? SQL Injection, XSS var?                 │
│  Rate limit aşılıb?                                        │
│  ✅ Yoxdur → keçir  /  ❌ Var → blok et                    │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                 LOAD BALANCER                               │
│  Hansı server ən az yüklüdür?                              │
│  Health Check: Bütün serverlər işlirmi?                    │
│  → Server 2-yə yönləndir                                   │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                   VEB SERVER (Nginx)                        │
│  HTTP Sorğusu: GET / HTTP/1.1                              │
│  Host başlığı: tryhackme.com                               │
│  Virtual Host: /var/www/tryhackme/ → tap                   │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│               BACKEND TƏTBİQ (PHP/Python)                  │
│  Dinamik məzmun lazımdır → Verilənlər Bazasına sor         │
│  DB cavab verir → HTML generasiya edilir                   │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│              HTTP CAVABI (200 OK)                          │
│  HTML + CSS + JS göndərilir                               │
│  CDN statik faylları (şəkillər, CSS) ən yaxın             │
│  serverdən çatdırır                                        │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    BRAUZER                                  │
│  HTML parse edilir → DOM yaradılır                         │
│  CSS tətbiq edilir → görünüş formalaşır                    │
│  JavaScript işlənir → interaktivlik                        │
│  İstifadəçi saytı görür! ✅                                │
└─────────────────────────────────────────────────────────────┘
```

## Pre-Security Yolunun Xülasəsi

Bu otaqla **Pre-Security** yolunu tamamlamış olursunuz. Öyrəndikləriniz:

| Mövzu | Öyrənilənlər |
|-------|-------------|
| **Şəbəkə əsasları** | OSI modeli, IP, MAC, TCP/UDP, Paketlər, Freymler |
| **İnternet texnologiyaları** | DNS, HTTP, Port yönləndirmə, Firewall, VPN |
| **Şəbəkə cihazları** | Router, Switch, Hub, VLAN |
| **Veb texnologiyalar** | HTML, CSS, JavaScript, HTTP metodları, Status kodları |
| **Veb təhlükəsizliyi** | Sensitive Data Exposure, HTML Injection |
| **Veb infrastruktur** | CDN, Load Balancer, WAF, Virtual Hosts, Statik/Dinamik məzmun |
