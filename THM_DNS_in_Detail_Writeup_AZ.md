# TryHackMe — DNS in Detail | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** DNS in Detail  
> **Çətinlik:** Easy  
> **Müddət:** 45 dəqiqə  
> **Link:** https://tryhackme.com/room/dnsindetail  

---

## Ümumi Baxış

Bu otaqda internetin ən fundamental texnologiyalarından biri olan **DNS (Domain Name System)** öyrənilir. DNS-i "internetin telefon kitabçası" kimi düşünmək olar — domen adlarını IP ünvanlarına çevirir ki, insan dostlu ünvanları (məs. `tryhackme.com`) istifadə edə bilək.

---

## Task 1 — DNS Nədir? (What is DNS?)

### İzahat

İnternetdəki hər kompüterin unikal bir ünvanı var — **IP ünvanı**. Məsələn: `104.26.10.229`

Bu rəqəm silsiləsini yadda saxlamaq çətin olduğundan DNS köməyə gəlir. DNS sisteminin sayəsində mürəkkəb rəqəm kombinasiyaları əvəzinə sadəcə `tryhackme.com` yazırıq.

```
  SƏN                DNS               VEBSAYT
   │                  │                   │
   │──"tryhackme.com"─►│                   │
   │                  │──► 104.26.10.229  │
   │◄──104.26.10.229──│                   │
   │                  │                   │
   │─────────────────────────────────────►│
   │            Vebsayt açılır            │
```

DNS olmadan hər vebsayt üçün IP ünvanını əzbərləmək lazım olardı — bu praktiki olaraq mümkün deyil.

---

### Sual və Cavab

> **Q: What does DNS stand for?**  
> ✅ `Domain Name System`

---

## Task 2 — Domen İyerarxiyası (Domain Hierarchy)

### İzahat

Domen adları sadə görünsə də, içindən müəyyən bir iyerarxiyaya malikdir. `tryhackme.com` ünvanını nümunə götürək:

```
        jupiter.servers.tryhackme.com
        ────┬───  ──┬───  ────┬────  ──┬──
            │       │         │         │
        Alt      İkinci     İkinci   Üst Səviyyəli
       Subdomain  Subdomain  Domen    Domen (TLD)
```

---

### TLD (Top-Level Domain — Üst Səviyyəli Domen)

TLD — domen adının ən sağ hissəsidir. İki növü var:

| Növ | Tam adı | Nümunələr | Məqsədi |
|-----|---------|-----------|---------|
| **gTLD** | Generic Top-Level Domain | `.com`, `.org`, `.edu`, `.gov`, `.net` | Saytın məqsədini bildirir |
| **ccTLD** | Country Code Top-Level Domain | `.az`, `.uk`, `.ca`, `.de`, `.ru` | Ölkəni bildirir |

> **Yeni gTLD-lər:** Tələbatın artması ilə `.online`, `.club`, `.website`, `.biz`, `.app` kimi 2000-dən çox yeni TLD əlavə edilib.

---

### Second-Level Domain (İkinci Səviyyəli Domen)

`tryhackme.com`-da `.com` TLD-dir, `tryhackme` isə **Second-Level Domain**-dir.

Məhdudiyyətlər:
- Maksimum **63 simvol** (TLD daxil olmaqla)
- Yalnız `a-z`, `0-9` və tire (`-`) istifadə edilə bilər
- Tire ilə **başlaya və ya bitə bilməz**
- İki ardıcıl tire (`--`) istifadə edilə bilməz

---

### Subdomain (Alt Domen)

Subdomain — İkinci Səviyyəli Domenin **solunda** nöqtə ilə ayrılmış hissədir.

```
admin.tryhackme.com
──┬──  ────┬────  ───
  │         │
Subdomain  Second-Level Domain
```

Bir neçə subdomain birlikdə istifadə edilə bilər:
```
jupiter.servers.tryhackme.com
  ↑         ↑
subdomain  subdomain
```

**Subdomain məhdudiyyətləri:**
- Hər subdomain maksimum **63 simvol**
- Tam domen adı (bütün hissələr + nöqtələr) maksimum **253 simvol**
- `a-z`, `0-9` və tire — tire ilə başlaya/bitə bilməz
- `_` (alt xətt) subdomain-də **istifadə edilə bilməz**
- Subdomain sayı **limitsizdir**

---

### Suallar və Cavablar

> **Q: What is the maximum length of a subdomain?**  
> ✅ `63`  
> *İzah: Hər bir subdomain hissəsi maksimum 63 simvoldan ibarət ola bilər.*

> **Q: Which of the following characters cannot be used in a subdomain ( 3 b _ - )?**  
> ✅ `_`  
> *İzah: Alt xətt (`_`) subdomain adlarında istifadə edilə bilməz; yalnız hərflər, rəqəmlər və tire icazəlidir.*

> **Q: What is the maximum length of a domain name?**  
> ✅ `253`  
> *İzah: Bütün hissələr (subdomain + second-level + TLD + nöqtələr) birlikdə maksimum 253 simvol ola bilər.*

> **Q: What type of TLD is .co.uk?**  
> ✅ `ccTLD`  
> *İzah: `.co.uk` Böyük Britaniyaya aid ölkə kodu TLD-dir (Country Code Top-Level Domain).*

---

## Task 3 — DNS Yazı Növləri (Record Types)

### İzahat

DNS yalnız vebsaytlar üçün deyil — müxtəlif məqsədlər üçün müxtəlif yazı (record) növləri mövcuddur:

---

### A Record (A Yazısı)

**IPv4** ünvanına həll edir.

```
tryhackme.com  →  A Record  →  104.26.10.229
```

IPv4 ünvanı 4 rəqəm qrupundan ibarətdir: `X.X.X.X` (hər biri 0-255 arasında).

---

### AAAA Record (AAAA Yazısı)

**IPv6** ünvanına həll edir.

```
tryhackme.com  →  AAAA Record  →  2606:4700:20::681a:be5
```

IPv6 daha uzun, hex formatındadır və IPv4-ün tükənməsinə həll kimi yaradılıb.

---

### CNAME Record (Kanonik Ad Yazısı)

Bir domeni **başqa bir domenə** yönləndirir (alias — ləqəb).

```
shop.tryhackme.com  →  CNAME  →  shops.shopify.com
                                       ↓
                                  (yenidən A record axtarışı)
                                  shops.shopify.com → 23.45.67.89
```

CNAME IP ünvanı deyil, başqa bir domen adı qaytarır. Sonra həmin domen üçün ayrıca DNS sorğusu edilir.

---

### MX Record (Mail Exchanger Yazısı)

Domene göndərilən e-poçtları **hansı mail serverinə** yönləndirilməli olduğunu bildirir.

```
tryhackme.com  →  MX Record  →  alt1.aspmx.l.google.com  (priority: 10)
                              →  alt2.aspmx.l.google.com  (priority: 20)
```

**Priority (Prioritet)** — rəqəm nə qədər kiçikdirsə, o server o qədər üstün tutulur. Əsas server çökərsə, növbəti prioritetli serverə keçir.

---

### TXT Record (Mətn Yazısı)

İstənilən mətn məlumatını saxlayan sərbəst sahədir. Bir neçə istifadə sahəsi:

| İstifadə | Nümunə |
|----------|--------|
| **SPF** | Hansı serverlərin domen adından e-poçt göndərə biləcəyini bildirir (spam mübarizəsi) |
| **DMARC** | E-poçt autentifikasiya siyasətini müəyyənləşdirir |
| **Domenin doğrulanması** | Üçüncü tərəf xidmətlər (Google, Microsoft) domenin sahibliyini TXT record vasitəsilə yoxlayır |
| **CTF Flag-ları** | TryHackMe kimi platformalarda flag gizlədilir |

---

### Suallar və Cavablar

> **Q: What type of record would be used to advise where to send email?**  
> ✅ `MX`  
> *İzah: MX (Mail Exchanger) record e-poçt trafiki üçün hansı serverin istifadə ediləcəyini bildirir.*

> **Q: What type of record handles IPv6 addresses?**  
> ✅ `AAAA`  
> *İzah: A record IPv4, AAAA record isə IPv6 ünvanları üçündür.*

---

## Task 4 — DNS Sorğusu Necə Edilir? (Making A Request)

### İzahat

Brauzerdə `www.tryhackme.com` yazdığında arxada kompleks bir proses baş verir. Addım-addım izləyək:

```
SƏNİN KOMPÜTERİN
      │
      │ 1. Yerli keş yoxlanır
      │    (əvvəl bu sayta baxıbsan?)
      │
      ▼
RECURSIVE DNS SERVER (İSP)
      │
      │ 2. ISP-nin DNS serveri yoxlanır
      │    (məşhur saytlar burada keşlənib)
      │
      ▼
ROOT DNS SERVERS (Kök Serverlər)
      │
      │ 3. ".com" üçün TLD serverini tap
      │
      ▼
TLD DNS SERVER (.com Serveri)
      │
      │ 4. "tryhackme.com" üçün
      │    Authoritative server kim?
      │
      ▼
AUTHORITATIVE DNS SERVER
(kip.ns.cloudflare.com)
      │
      │ 5. "tryhackme.com = 104.26.10.229"
      │    cavabı qaytarılır + TTL əlavə edilir
      │
      ▼
RECURSIVE DNS SERVER
      │ Cavabı keşləyir (TTL qədər)
      │
      ▼
SƏNİN KOMPÜTERİN
      │ 104.26.10.229-a qoşulur
```

---

### Addımların Ətraflı İzahı

**1. Yerli Keş (Local Cache)**

Kompüterin özü əvvəlki sorğuları yadda saxlayır. Eyni sayta yenidən girsən, DNS sorğusu olmadan bilavasitə IP-yə qoşulursan.

**2. Recursive DNS Server (Rekursiv DNS Serveri)**

Adətən internet provayderiniz (ISP) tərəfindən verilir — amma Google-un `8.8.8.8` və ya Cloudflare-in `1.1.1.1` serverini seçmək də mümkündür. Bu server öz keşini yoxlayır; tapmasa sorğunu kök serverə ötürür.

**3. Root DNS Servers (Kök Serverlər)**

İnternetin DNS "onurğa sütunu"dur. `.com`, `.org`, `.az` kimi TLD-lərə cavabdeh TLD serverini bildirir. Dünyada 13 kök server cluster-i var (A-dan M-yə qədər), amma hər biri yüzlərlə fiziki maşından ibarətdir.

**4. TLD DNS Server**

Həmin TLD-yə aid (`.com`, `.org` və s.) bütün domenlərin **Authoritative serverinin** yerini bilir.

**5. Authoritative DNS Server (Yetkili DNS Serveri)**

Domenin faktiki DNS yazılarını saxlayan serverdir. Bura domen qeydiyyatı zamanı göstərdiyiniz **nameserver**-lərdir (məs. `kip.ns.cloudflare.com`). Hər bir domenin adətən ehtiyat üçün birdən çox nameserver-i olur.

---

### TTL (Time To Live — Yaşama Müddəti)

DNS yazılarının hər birinin **TTL** dəyəri var — saniyə cinsindən ifadə olunur. Bu dəyər cavabın neçə saniyə keşlənəcəyini göstərir.

```
TTL = 3600  →  1 saatlıq keşlənəcək
TTL = 86400 →  24 saatlıq keşlənəcək
```

TTL bitdikdən sonra növbəti sorğuda DNS server yenidən Authoritative serverə müraciət edir.

---

### Suallar və Cavablar

> **Q: What field specifies how long a DNS record should be cached for?**  
> ✅ `TTL`  
> *İzah: TTL (Time To Live) DNS cavabının neçə saniyə keşlənəcəyini müəyyənləşdirir.*

> **Q: What type of DNS Server is usually provided by your ISP?**  
> ✅ `Recursive`  
> *İzah: ISP tərəfindən verilən DNS server Recursive (Rekursiv) DNS serveridir — o özü kök və TLD serverlərlə əlaqə qurub cavab tapır.*

> **Q: What type of server holds all the records for a domain?**  
> ✅ `Authoritative`  
> *İzah: Authoritative DNS server domenin bütün DNS yazılarını saxlayan yeganə yetkin mənbədir.*

---

## Task 5 — Praktiki (Practical)

### İzahat

Bu taskda brauzer daxilindəki interaktiv DNS sorğu aləti istifadə olunur. Fərqli DNS yazı növlərini (`CNAME`, `TXT`, `MX`, `A`) sorğulayaraq cavablar əldə edilir.

Eyni əməliyyatları terminal vasitəsilə `nslookup` əmri ilə də etmək mümkündür:

```bash
# CNAME sorğusu
nslookup --type=CNAME shop.website.thm

# TXT sorğusu
nslookup --type=TXT website.thm

# MX sorğusu
nslookup --type=MX website.thm

# A record sorğusu
nslookup --type=A www.website.thm
```

---

### Sual 1: `shop.website.thm`-in CNAME-i nədir?

**Nə etmək lazımdır:**
- Sorğu növünü `CNAME` seç
- Domen: `shop.website.thm`
- "Send DNS Request" düyməsinə bas

**Nəticə:** `shop.website.thm` → CNAME → `shops.myshopify.com`

Bu o deməkdir ki, `shop.website.thm` öz IP ünvanı yoxdur — o, `shops.myshopify.com`-a yönləndirir. Sonra həmin ünvan üçün ayrıca A record sorğusu edilir.

> **Q: What is the CNAME of shop.website.thm?**  
> ✅ `shops.myshopify.com`

---

### Sual 2: `website.thm`-in TXT yazısının dəyəri nədir?

**Nə etmək lazımdır:**
- Sorğu növünü `TXT` seç
- Domen: `website.thm` (shop subdomain-siz)
- "Send DNS Request" düyməsinə bas

**Nəticə:** TXT yazısında flag gizlənib.

> **Q: What is the value of the TXT record of website.thm?**  
> ✅ `THM{7012BBA60997F35A9516C2E16D2944FF}`

---

### Sual 3: MX yazısının ədədi prioritet dəyəri nədir?

**Nə etmək lazımdır:**
- Sorğu növünü `MX` seç
- Domen: `website.thm`
- "Send DNS Request" düyməsinə bas

**Nəticə:** MX cavabında iki şey göstərilir: serverin ünvanı və prioritet rəqəmi.

```
website.thm  MX  30  mail.website.thm
                 ↑
              Prioritet
```

> **Q: What is the numerical priority value for the MX record?**  
> ✅ `30`  
> *İzah: MX yazısındakı prioritet rəqəmi — nə qədər kiçikdirsə o server o qədər üstün tutulur.*

---

### Sual 4: `www.website.thm`-in A yazısında IP ünvanı nədir?

**Nə etmək lazımdır:**
- Sorğu növünü `A` seç
- Domen: `www.website.thm`
- "Send DNS Request" düyməsinə bas

**Nəticə:** A yazısı IPv4 ünvanı qaytarır.

> **Q: What is the IP address for the A record of www.website.thm?**  
> ✅ `10.10.10.10`

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | DNS nəyin qısaltmasıdır? | `Domain Name System` |
| Task 2 | Subdomain-in maksimum uzunluğu? | `63` |
| Task 2 | Subdomain-də hansı simvol istifadə edilə bilməz? | `_` |
| Task 2 | Domen adının maksimum uzunluğu? | `253` |
| Task 2 | `.co.uk` hansı növ TLD-dir? | `ccTLD` |
| Task 3 | E-poçtun hayana göndərileceğini bildirən yazı növü? | `MX` |
| Task 3 | IPv6 ünvanlarını idarə edən yazı növü? | `AAAA` |
| Task 4 | DNS yazısının neçə müddət keşlənəcəyini bildirən sahə? | `TTL` |
| Task 4 | ISP tərəfindən verilən DNS server növü? | `Recursive` |
| Task 4 | Domenin bütün yazılarını saxlayan server növü? | `Authoritative` |
| Task 5 | `shop.website.thm`-in CNAME-i? | `shops.myshopify.com` |
| Task 5 | `website.thm`-in TXT yazısının dəyəri? | `THM{7012BBA60997F35A9516C2E16D2944FF}` |
| Task 5 | MX yazısının ədədi prioritet dəyəri? | `30` |
| Task 5 | `www.website.thm`-in A yazısında IP ünvanı? | `10.10.10.10` |

---

## Bonus: DNS Yazı Növləri — Qısa Xülasə

| Yazı Növü | Nə qaytarır? | İstifadəsi |
|-----------|-------------|-----------|
| **A** | IPv4 ünvanı (`192.168.1.1`) | Vebsayt ünvanını tapmaq |
| **AAAA** | IPv6 ünvanı (`2001:db8::1`) | Müasir IPv6 ünvanı tapmaq |
| **CNAME** | Başqa domen adı | Alias (ləqəb) — subdomain yönləndirmə |
| **MX** | Mail server + prioritet | E-poçtun hansı serverə göndərilməsi |
| **TXT** | Sərbəst mətn | SPF, DMARC, domen doğrulama, CTF flagları |

## Bonus: DNS Sorğu Prosesi — Qısa

```
Brauzer → Yerli Keş → Recursive DNS (ISP)
                              ↓
                      Kök DNS Server
                              ↓
                      TLD DNS Server (.com)
                              ↓
                      Authoritative DNS Server
                              ↓
                      IP ünvanı qaytarılır + TTL ilə keşlənir
                              ↓
                      Brauzer IP-yə qoşulur
```
