# TryHackMe - HTTP in Detail | Azerbaycan dilinde Oyrenmə Yazisi

> **Mənbə:** https://tryhackme.com/room/httpindetail
> **Cetinlik:** Easy | **Muddet:** ~30 deq
> **Məqsəd:** Veb serverden məzmun sorğulamaq ucun HTTP protokolunu oyrənmək.

---

## Mundəricat

1. [Task 1 - HTTP ve HTTPS nədir?](#task-1)
2. [Task 2 - Sorğular ve Cavablar](#task-2)
3. [Task 3 - HTTP Metodlari](#task-3)
4. [Task 4 - HTTP Status Kodlari](#task-4)
5. [Task 5 - Başliqlar (Headers)](#task-5)
6. [Task 6 - Cookie-lər](#task-6)
7. [Task 7 - Praktiki: Sorğu Gondərmək](#task-7)
8. [Tam Sual-Cavab Cedvəli](#cedvel)

---

## Task 1 - HTTP ve HTTPS nədir?

### HTTP nədir?

**HTTP** = **H**yper**T**ext **T**ransfer **P**rotocol = Hipermətn Oturulmə Protokolu

HTTP - veb serverlər ilə rabitə ucun istifadə olunan qaydalar toplusudur. Bir veb sayta daxil olduqda brauzer HTTP vasitəsilə serverə müraciət edir və HTML, şəkillər, videolar kimi məlumatları alir.

HTTP-ni **1989-1991** illər arasinda **Tim Berners-Lee** və onun komandasi işləyib hazirlamişdir.

### HTTPS nədir?

**HTTPS** = **H**yper**T**ext **T**ransfer **P**rotocol **S**ecure = Təhlukəsiz HTTP

HTTPS - HTTP-nin **şifrəli versiyasidır.**

```
HTTP:
Sən --[şifrəsiz məlumat]--> Server
     Hər kəs göre bilər! (təhlükəli)

HTTPS:
Sən --[şifrəli məlumat]--> Server
     Yalnız sən və server gören bilər! (tərhlukəsiz)
```

HTTPS iki şey təmin edir:
- **Məlumatın şifrələnməsi** - kənar şəxslər görə bilməz
- **Kimlik doğrulaması** - həqiqətən düzgün serverə qoşulduğunu bilmək

### Task 1 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| HTTP nəyin qisaltmasidir? | `HyperText Transfer Protocol` |
| HTTPS-dəki "S" nəyi bildirir? | `Secure` |
| Mock veb səhifədəki problemi tapdin? (flag) | *(interaktiv labdan alinir)* |

---

## Task 2 - Sorğular ve Cavablar

### URL nədir?

**URL** = **U**niform **R**esource **L**ocator = Vahid Resurs Yeri Gosteri

URL - İnternetdəki bir resursa necə catmaq olar deyən **tam ünvan + tetlikmatdir.**

### URL-in Hissələri

Tam URL nümunəsi:
```
http://user:password@tryhackme.com:80/view-room?id=1#task3
```

Hər hissəni analiz edək:

```
http://          -> Scheme (Protokol)
user:password    -> User (İstifadəci adı ve şifrə)
tryhackme.com    -> Host (Domen adi)
:80              -> Port
/view-room       -> Path (Yol)
?id=1            -> Query String (Sorğu Sətri)
#task3           -> Fragment (Parça)
```

| Hissə | İzah | Nümunə |
|-------|------|--------|
| **Scheme** | Hansı protokol: HTTP, HTTPS, FTP | `https://` |
| **User** | Giris tələb edən servislər ucun ad:şifrə | `admin:pass123` |
| **Host** | Serverin domen adi ve ya IP ünvani | `tryhackme.com` |
| **Port** | Qoşulacaq port. HTTP=80, HTTPS=443 | `:8080` |
| **Path** | Resursa aparan yol | `/blog/post-1` |
| **Query String** | Yola gondərilən əlavə məlumat | `?id=1&page=2` |
| **Fragment** | Sayfanin icindəki konkret bənd | `#task3` |

---

### Sorğu (Request) Necə Görünür?

Ən sadə HTTP sorğusu belə görünür:

```
GET / HTTP/1.1
```

Lakin real həyatda sorğu daha cox məlumat ehtiva edir:

```http
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```

Hər sətiri izah edək:

| Sətir | Məna |
|-------|------|
| `GET / HTTP/1.1` | GET metodu ile "/" (ana səhifə) sor, HTTP 1.1 istifadə et |
| `Host: tryhackme.com` | Serverin harda axtaracağını bildir |
| `User-Agent: Mozilla/5.0 Firefox/87.0` | Brauzer növü ve versiyasi |
| `Referer: https://tryhackme.com/` | Seni bu səhifəyə gondərən səhifə |
| *(boş sətir)* | Sorğunun bitdiyini bildirir |

---

### Cavab (Response) Necə Görünür?

```http
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>TryHackMe</title>
</head>
<body>
    Welcome To TryHackMe.com
</body>
</html>
```

Hər sətiri izah edək:

| Sətir | Məna |
|-------|------|
| `HTTP/1.1 200 OK` | HTTP versiyasi + Status kodu (200 = uğurlu) |
| `Server: nginx/1.15.8` | Serverin proqram təminatı ve versiyasi |
| `Date:` | Serverin tarix, saat ve saat qurşağı |
| `Content-Type: text/html` | Qaytarilan məlumatın növü (HTML, PNG, PDF...) |
| `Content-Length: 98` | Cavabın uzunluğu (byte ilə) |
| *(boş sətir)* | HTTP cavabının başliq hissəsinin sonu |
| `<html>...` | Tələb olunan məlumat (HTML kodu) |

### Task 2 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| Yuxaridaki nümunədə hansı HTTP protokolu istifadə olunur? | `HTTP/1.1` |
| Hansı cavab başliği brauzərə nə qədər məlumat gələcəyini bildirir? | `Content-Length` |

---

## Task 3 - HTTP Metodlari

HTTP metodlari - müştərinin (brauzer) serverə **nə etmək istədiyini** bildirən metodlardır.

### 4 Əsas Metod

| Metod | CRUD | Nə edir? | Nə vaxt? |
|-------|------|----------|----------|
| **GET** | Read (Oxu) | Serverdən məlumat alır | Veb səhifəyə baxmaq |
| **POST** | Create (Yarat) | Serverə məlumat gondərir, yeni qeyd yarada bilər | Qeydiyyat formu, login |
| **PUT** | Update (Yenilə) | Serverə mövcud məlumatı yeniləmək ucun gondərir | Profili redaktə etmək |
| **DELETE** | Delete (Sil) | Serverdən məlumat/qeyd silir | Hesabı/şəkli silmək |

### Real Həyat Nümunələri

```
Instagram-a yeni şəkil yükləmək  -> POST
Dostunun profilini görmək        -> GET
Profil şəklini dəyişmək          -> PUT
Yüklədiyın şəkli silmək         -> DELETE
```

### Task 3 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| Yeni istifadəci hesabi yaratmaq ucun hansı metod? | `POST` |
| E-mail ünvanini yeniləmək ucun hansı metod? | `PUT` |
| Yuklədiyin şəkli silmək ucun hansı metod? | `DELETE` |
| Xəbər məqaləsini oxumaq ucun hansı metod? | `GET` |

---

## Task 4 - HTTP Status Kodlari

Hər HTTP cavabı **3 rəqəmli status kodu** ilə başlayır. Bu kod sorğunun nəticəsini bildirir.

### Status Kod Araliklari

| Aralik | Kateqoriya | Məna |
|--------|------------|------|
| **100-199** | Information Response | Sorğunun bir hissəsi alindi, gondərməyə davam et |
| **200-299** | Success | Sorğu uğurla tamamlandi |
| **300-399** | Redirection | Başqa resursa yonləndir |
| **400-499** | Client Errors | Müştəri tərəfindən xəta var |
| **500-599** | Server Errors | Server tərəfindən xəta var |

### Ən Çox Rast Gəlinən Kodlar

| Kod | Ad | İzah |
|-----|----|------|
| **200** | OK | Sorğu uğurla tamamlandi |
| **201** | Created | Yeni resurs yaradildi (yeni istifadəci, yeni post) |
| **301** | Moved Permanently | Resurs daimi olaraq başqa ünvana köçürüldü |
| **302** | Found | Müvəqqəti yönləndirmə |
| **400** | Bad Request | Sorğuda nəsə yanlış ve ya çatışmayan var |
| **401** | Not Authorised | Girish tələb olunur (login olmayibsan) |
| **403** | Forbidden | Girish etmisən, amma icazən yoxdur |
| **404** | Page Not Found | Tələb etdiyin resurs mövcud deyil |
| **405** | Method Not Allowed | Bu endpoint bu HTTP metodunu dəstəkləmir |
| **500** | Internal Server Error | Server sorğunu necə emal edəcəyini bilmir |
| **503** | Service Unavailable | Server həddən artiq yüklənib ve ya texniki işdədir |

### Vacib Fərqlər

```
401 vs 403:
401 -> Server deyir: "Kim olduğunu bilmirəm, əvvəlcə login ol"
403 -> Server deyir: "Kim olduğunu bilirəm, AMMa bu resursa icazən yoxdur"

404 vs 500:
404 -> Səhifə sadəcə mövcud deyil (müştəri xətası)
500 -> Səhifə var amma server sınib (server xətası)
```

### Task 4 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| Yeni istifadəci ve ya blog postu yaradildiginda hansı kod? | `201` |
| Mövcud olmayan səhifəyə daxil olmağa cəhd etsən? | `404` |
| Veb server veritabanina catxa bilmirs ve uculur? | `500` |
| Login olmadan profili redaktə etməyə cəhd etsən? | `401` |

---

## Task 5 - Başliqlar (Headers)

**Başliqlar (Headers)** - HTTP sorğusu ve cavabinda gondərilən **əlavə məlumat parcalaridır.**

### Sorğu Başliqları (Request Headers)

Bunlari brauzer servərə gondərir:

| Başliq | Nə edir? | Nümunə |
|--------|----------|--------|
| **Host** | Hansı veb saytı istədiyin | `Host: tryhackme.com` |
| **User-Agent** | Brauzer növü ve versiyasi | `User-Agent: Firefox/87.0` |
| **Content-Length** | Gondərilən məlumatın ölçüsü | `Content-Length: 256` |
| **Accept-Encoding** | Brauzer hansı sıxıştırma metodlarını dəstəkləyir | `Accept-Encoding: gzip` |
| **Cookie** | Serverin seni tanıması ucun saxlanılan məlumat | `Cookie: session=abc123` |

**Host başliği niyə vacibdir?**

Bir server cox sayda veb sayta "ev sahibliyi" edə bilər. `Host` başliği olmadan server hansı saytın istənildiyini bilməz.

```
Bir IP arxasında 3 sayt:
192.168.1.1 -> sayt-a.com
192.168.1.1 -> sayt-b.com
192.168.1.1 -> sayt-c.com

Host: sayt-b.com yazmasaydiq, server hangisini gondərəcəyini bilməzdi!
```

---

### Cavab Başliqları (Response Headers)

Bunlari server brauzərə gondərir:

| Başliq | Nə edir? | Nümunə |
|--------|----------|--------|
| **Set-Cookie** | Brauzerə cookie saxlatmaq ucun | `Set-Cookie: session=abc123` |
| **Cache-Control** | Məlumatın nə müddət cache-də saxlanılması | `Cache-Control: max-age=3600` |
| **Content-Type** | Gondərilən məlumatın növü | `Content-Type: text/html` |
| **Content-Encoding** | Məlumat necə sıxıştırılıb | `Content-Encoding: gzip` |

### Task 5 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| Hansı başliq servərə hansı brauzer istifadə olunduğunu bildirir? | `User-Agent` |
| Hansı başliq brauzərə hansı tip məlumat gondərildiyini bildirir? | `Content-Type` |
| Hansı başliq servərə hansı veb sayt istənildiyini bildirir? | `Host` |

---

## Task 6 - Cookie-lər

### Cookie nədir?

**Cookie** - kompüterinizdə saxlanılan kiçik məlumat parçasidır.

### Niyə lazimdir?

HTTP **stateless** (vəziyyətsiz) protokoldur — yəni hər sorğu öncəki sorğudan xəbərsizdir. Brauzer serverə her dəfə sanki ilk dəfə müraciət edir.

```
Problem:
Sən login oldun -> Server seni tanidi
Növbəti səhifəyə keçdin -> Server seni unutdu!

Cookie olmadan hər səhifədə login olmaq lazim gələrdi.
```

### Cookie Necə İşləyir?

```
1. Sən login olursan
        |
2. Server -> Set-Cookie: session=abc123xyz gondərir
        |
3. Brauzer bu cookie-ni saxlayir
        |
4. Növbəti hər sorğuda brauzer avtomatik gondərir:
   Cookie: session=abc123xyz
        |
5. Server cookie-ni oxuyur -> "Ah, bu Ali-dir!" -> Taniyir
```

### Cookie-lərin İstifadə Sahələri

- **Autentifikasiya** - login vəziyyətini saxlamaq (ən çox)
- **Şəxsi tənzimləmələr** - saytın dil, tema tercihləri
- **Tracking** - saytın seni əvvəl ziyarət edib-etmədiyini bilmək

**Vacib:** Cookie dəyəri adətən açıq mətn (şifrə) deyil, **token** (unikal gizli kod) şəklindədir.

```
Pis: Cookie: password=Sifrəm123    (təhlükəli!)
Yaxşi: Cookie: session=f7k2x9p1m3  (token - mənasız görünür)
```

### Cookie-ləri Necə Görmək?

Brauzerin Developer Tools-unu aç (F12) -> Network tabı -> İstənilən sorğuya klik -> Cookies tabı

### Task 6 Sual-Cavablari

| Sual | Cavab |
|------|-------|
| Cookie-ləri kompüterinizdə saxlamaq ucun hansı başliq istifadə olunur? | `Set-Cookie` |

---

## Task 7 - Praktiki: Sorğu Gondərmək

Bu task-da interaktiv HTTP emulator vasitəsilə real sorğular gondərirsən.

### Necə İşləyir?

"View Site" duymesine bas -> HTTP sorğu emulatorunu aç -> Metod ve yolu sec -> Gondər

### Tapşıriqlar ve Necə Etmək:

**1. GET /room**
```
Metod: GET
URL: /room
Gondər -> Flag alirsan
```

**2. GET /blog?id=1**
```
Metod: GET
URL: /blog
Query parameter: id = 1
(Gear iconuna bas -> URI Parameters -> id:1 əlavə et)
Gondər -> Flag alirsan
```

**3. DELETE /user/1**
```
Metod: DELETE
URL: /user/1
Gondər -> Flag alirsan
```

**4. PUT /user/2 (username=admin)**
```
Metod: PUT
URL: /user/2
Body parameter: username = admin
(Gear iconuna bas -> Body Parameters -> username:admin əlavə et)
Gondər -> Flag alirsan
```

**5. POST /login (username=thm, password=letmein)**
```
Metod: POST
URL: /login
Body parameters:
  username = thm
  password = letmein
Gondər -> Flag alirsan
```

### Task 7 Sual-Cavablari

| Tapşiriq | Cavab |
|----------|-------|
| GET /room | *(labdan alinir)* |
| GET /blog?id=1 | *(labdan alinir)* |
| DELETE /user/1 | *(labdan alinir)* |
| PUT /user/2 (username=admin) | *(labdan alinir)* |
| POST /login (thm/letmein) | *(labdan alinir)* |

---

## Tam Sual-Cavab Cedvəli

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | HTTP nəyin qisaltmasidir? | `HyperText Transfer Protocol` |
| Task 1 | HTTPS-dəki "S" nəyi bildirir? | `Secure` |
| Task 1 | Mock saytdaki flag? | *(interaktiv labdan)* |
| Task 2 | Yuxaridaki nümunədə hansı HTTP protokolu? | `HTTP/1.1` |
| Task 2 | Hansı cavab başliği nə qədər məlumat gələcəyini bildirir? | `Content-Length` |
| Task 3 | Yeni istifadəci yaratmaq ucun hansı metod? | `POST` |
| Task 3 | E-mail ünvanini yeniləmək ucun hansı metod? | `PUT` |
| Task 3 | Şəkli silmək ucun hansı metod? | `DELETE` |
| Task 3 | Xəbər məqaləsini oxumaq ucun hansı metod? | `GET` |
| Task 4 | Yeni resurs yaradildiginda hansı kod? | `201` |
| Task 4 | Mövcud olmayan səhifə ucun hansı kod? | `404` |
| Task 4 | Server xətasi ucun hansı kod? | `500` |
| Task 4 | Login olmadan resursa catmağa cəhd? | `401` |
| Task 5 | Hansı başliq brauzeri serversə bildirir? | `User-Agent` |
| Task 5 | Hansı başliq məlumat tipini bildirir? | `Content-Type` |
| Task 5 | Hansı başliq hansı saytın istənildiyini bildirir? | `Host` |
| Task 6 | Cookie saxlamaq ucun hansı başliq? | `Set-Cookie` |
| Task 7 | Bütün sorğular | *(interaktiv labdan alinir)* |

---

## Oyrəndiklərimizin Xulaəsi

```
HTTP (HyperText Transfer Protocol)
|
+-- URL quruluşu
|     Scheme://User@Host:Port/Path?Query#Fragment
|
+-- HTTP Metodları
|     GET    -> Məlumat al (oxu)
|     POST   -> Məlumat gondər (yarat)
|     PUT    -> Məlumatı yenilə
|     DELETE -> Məlumatı sil
|
+-- Status Kodlari
|     2xx -> Uğurlu
|     3xx -> Yönləndirmə
|     4xx -> Müştəri xətasi (401 tanınmadın, 403 icazən yox, 404 tapilmadi)
|     5xx -> Server xətasi
|
+-- Başliqlar (Headers)
|     Request:  Host, User-Agent, Cookie, Content-Length
|     Response: Set-Cookie, Content-Type, Content-Length, Cache-Control
|
+-- Cookie-lər
      HTTP stateless-dir -> Cookie kimliyi xatirlatmaq ucun
      Set-Cookie -> Saxla
      Cookie     -> Gondər
```

---

## Novbəti Addim

Bu labı bitirdikdən sonra:

**How Websites Work** - https://tryhackme.com/room/howwebsiteswork

Orada oyrənəcəksən:
- HTML, CSS, JavaScript-in rolu
- Veb saytların necə render edildiyini
- Client-side vs Server-side

---

*Mənbə: TryHackMe - HTTP in Detail - https://tryhackme.com/room/httpindetail*
