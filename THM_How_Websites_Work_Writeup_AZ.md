# TryHackMe — How Websites Work | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** How Websites Work  
> **Çətinlik:** Easy  
> **Müddət:** 25 dəqiqə  
> **Link:** https://tryhackme.com/room/howwebsiteswork  

---

## Ümumi Baxış

Bu otaqda vebsaytların necə qurulduğunu öyrənirik — **HTML** (quruluş), **JavaScript** (interaktivlik) və iki vacib təhlükəsizlik zəifliyi: **Sensitive Data Exposure** (Həssas Məlumatların İfşası) və **HTML Injection** (HTML İnyeksiyası). Bir veb hücumçusu kimi ilk addım — hədəfin necə qurulduğunu anlamaqdır.

---

## Task 1 — Vebsaytlar Necə İşləyir? (How Websites Work)

### İzahat

Bir vebsayta daxil olduğunuzda brauzeriniz veb serverə sorğu göndərir, server cavab verir — brauzer həmin cavabı ekranda göstərir.

```
SƏNİN BRAUZERIN          VEB SERVER
(İstanbul-dakı          (dünyanın hansısa
 kompüterindən)          bir yerindəki
                         server otağı)
      │                       │
      │── "tryhackme.com ──►  │
      │     ver mənə!"        │
      │                       │
      │◄─── HTML, CSS, JS ─── │
      │     göndərildi        │
      │                       │
   Brauzer                    │
   məlumatı                   │
   göstərir                   │
```

**Vebsaytın iki əsas komponenti:**

| Komponent | Adı | Nədir? |
|-----------|-----|--------|
| **Front End** | Client-Side (Müştəri Tərəfi) | Brauzerdə görünən və render edilən hissə |
| **Back End** | Server-Side (Server Tərəfi) | Serverdə işləyən, sorğuları emal edib cavab verən hissə |

> **Nümunə:** Facebook-un dizaynı, düymələri, şəkilləri — Front End. İstifadəçilərin məlumatlarını saxlayan verilənlər bazası, giriş yoxlaması — Back End.

---

### Sual və Cavab

> **Q: What term best describes the component of a web application rendered by your browser?**  
> ✅ `Front End`  
> *İzah: Brauzer vebsaytın Front End (Client-Side) hissəsini render edir — HTML, CSS, JavaScript-i oxuyub ekranda göstərir.*

---

## Task 2 — HTML

### İzahat

**HTML (HyperText Markup Language — HiperMətn İşarələmə Dili)** — vebsaytların quruluşunu və məzmununu müəyyən edən dildir. Brauzer HTML-i oxuyub ekranda göstərir.

HTML **teqlər** (tags) vasitəsilə işləyir. Hər teq brauzərə məzmunun necə görünəcəyini izah edir:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Mənim Saytım</title>
    </head>
    <body>
        <h1>Böyük Başlıq</h1>
        <p>Bu bir abzasdır.</p>
        <img src="img/shekil.jpg">
        <button onclick="alert('Salam!')">Düymə</button>
    </body>
</html>
```

**Ən çox istifadə olunan HTML teqləri:**

| Teq | Nümunə | Nə edir? |
|-----|--------|----------|
| `<h1>` – `<h6>` | `<h1>Başlıq</h1>` | Başlıq (h1 ən böyük, h6 ən kiçik) |
| `<p>` | `<p>Mətn</p>` | Abzas |
| `<img>` | `<img src='cat.jpg'>` | Şəkil göstərir |
| `<a>` | `<a href="url">Link</a>` | Keçid (link) |
| `<button>` | `<button>Klik</button>` | Düymə |
| `<div>` | `<div>Blok</div>` | Qrup elementi |

**Atributlar (Attributes):**

Teqlər içindəki əlavə məlumat atribut adlanır:
```html
<img src='img/cat-1.jpg'>
     ─┬─  ───────┬───────
      │           │
   Atribut adı  Atribut dəyəri
```

---

### Praktiki Tapşırıqlar

**Tapşırıq 1 — Başlanğıc HTML kodu:**

```html
<!DOCTYPE html>
<html>
    <head>
        <title>TryHackMe HTML Editor</title>
    </head>
    <body>
        <h1>Cat Website!</h1>
        <p>See images of all my cats!</p>
        <img src='img/cat-1.jpg'>
        <img src='img/cat-2.'>      ← PROBLEM BURADADIR!
        <!-- Add dog image here -->
    </body>
</html>
```

**Sual 2 — Sınıq şəkili düzəlt:**

İkinci şəklin `src` atributunda fayl uzantısı çatışmır: `img/cat-2.` — `.jpg` əlavə edilməlidir.

```html
<!-- ƏVVƏL (sınıq): -->
<img src='img/cat-2.'>

<!-- SONRA (düzəldilmiş): -->
<img src='img/cat-2.jpg'>
```

"Render HTML Code" düyməsinə basıldıqda şəkil düzəlir və üzərindəki flag görünür.

> **Q: One of the images on the cat website is broken — fix it, and the image will reveal the hidden text answer!**  
> ✅ `HTMLHERO`

---

**Sual 3 — İt şəkli əlavə et:**

11-ci sətirə `<img>` teqi əlavə edilməlidir:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>TryHackMe HTML Editor</title>
    </head>
    <body>
        <h1>Cat Website!</h1>
        <p>See images of all my cats!</p>
        <img src='img/cat-1.jpg'>
        <img src='img/cat-2.jpg'>
        <img src='img/dog-1.png'>   ← 11-ci sətirə bu əlavə edilir
    </body>
</html>
```

"Render HTML Code" düyməsinə basıldıqda it şəkli görünür və üzərindəki flag aşkar olur.

> **Q: Add a dog image to the page by adding another img tag on line 11. The dog image location is img/dog-1.png. What is the text in the dog image?**  
> ✅ `DOGHTML`

---

## Task 3 — JavaScript

### İzahat

**JavaScript (JS)** — vebsaytlara **interaktivlik** qatan proqramlaşdırma dilidir. HTML quruluş yaradır, JavaScript isə həmin quruluğu canlandırır.

```
HTML      → İskelet (sümüklər)
CSS       → Geyim (görünüş)
JavaScript → Əzələlər (hərəkət, interaktivlik)
```

JavaScript HTML-ə `<script>` teqi vasitəsilə əlavə edilir:

```html
<script>
    // JavaScript kodu bura yazılır
    document.getElementById("demo").innerHTML = "Salam Dünya!";
</script>
```

**DOM (Document Object Model) nədir?**

DOM — brauzerin HTML sənədini bir ağac quruluşu kimi saxlaması üsuludur. JavaScript bu ağaca müraciət edərək elementləri dəyişdirə bilir.

```
document
   └── html
        ├── head
        │    └── title
        └── body
             ├── h1
             ├── p id="demo"  ← JavaScript bura müraciət edir
             └── button
```

**Əsas JavaScript funksiyaları:**

| Funksiya | Nümunə | Nə edir? |
|----------|--------|----------|
| `getElementById()` | `document.getElementById("demo")` | ID-yə görə element tapır |
| `.innerHTML` | `elem.innerHTML = "Yeni mətn"` | Elementin məzmununu dəyişir |
| `alert()` | `alert("Salam!")` | Popup qutu göstərir |

**onClick hadisəsi:**

```html
<button onclick='document.getElementById("demo").innerHTML = "Klik edildi!";'>
    Düymə
</button>
```

Düyməyə basıldıqda `demo` ID-li elementin məzmunu `"Klik edildi!"` olur.

---

### Praktiki Tapşırıqlar

**Tapşırıq 1 — JavaScript əlavə et:**

`demo` elementinin məzmununu `"Hack the Planet"` eləmək üçün JavaScript kodu əlavə edilir:

```javascript
document.getElementById("demo").innerHTML = "Hack the Planet";
```

Bu kodu `<script>` teqləri arasına yazıb "Render HTML+JS Code" düyməsinə basın.

> **Q: Click the "View Site" button on this task. On the right-hand side, add JavaScript that changes the demo element's content to "Hack the Planet"**  
> ✅ `JSISFUN`

---

**Tapşırıq 2 — Düymə əlavə et:**

```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>
    Click Me!
</button>
```

Bu HTML-i koda əlavə edib "Render HTML+JS Code" düyməsinə, sonra "Click Me!" düyməsinə basın.

> **Q: Add the button HTML from this task that changes the element's text to "Button Clicked" on the editor on the right, update the code by clicking the "Render HTML+JS Code" button and then click the button.**  
> ✅ No Answer Needed

---

## Task 4 — Həssas Məlumatların İfşası (Sensitive Data Exposure)

### İzahat

**Sensitive Data Exposure** — vebsaytın həssas məlumatları (parollar, gizli linklər, API açarları) frontend mənbə kodunda açıq şəkildə saxlamasıdır. İstənilən istifadəçi brauzerə sağ klik edib **"View Page Source"** seçərək bu kodu görə bilər.

**Niyə bu baş verir?**

Proqramçılar tez-tez:
- Test üçün müvəqqəti parollar yazır amma sonra silməyi unutur
- Şərh blokları (`<!-- ... -->`) içinə gizli məlumat yazır
- Gizli idarəetmə panellərinin linklərini kodda saxlayır
- API açarlarını (şifrəli kodları) JavaScript-in içinə yazır

**Bunun nə qədər sadə olduğuna baxaq:**

```html
<body>
    <h1>Sayta xoş gəldiniz</h1>
    
    <!-- Müvəqqəti admin girişi - UNUTMA SİLMƏYİ! -->
    <!-- username: admin, password: testpasswd -->
    
    <p>Normal məzmun...</p>
</body>
```

Bu kodu brauzer ekranda göstərmir — amma "View Page Source" ilə görünür!

---

### Praktiki Tapşırıq

**"View Site" düyməsinə bas** → açılan saytda sağ klik et → **"Open in New Tab"** → yeni tabda sağ klik → **"View Page Source"** seç.

Mənbə kodunda `<body>` elementinin sonuna yaxın bir çox sətirli şərh bloku görünür:

```html
<!-- 
    Müvəqqəti giriş məlumatları:
    username: admin
    password: testpasswd
-->
```

> **Q: View the website on this task. What is the password hidden in the source code?**  
> ✅ `testpasswd`  
> *İzah: Mənbə kodundaki şərh blokunda (`<!-- -->`) developer tərəfindən unutulmuş parol gizlənib.*

> **Kibertəhlükəsizlik dərsi:** Heç vaxt həssas məlumatları HTML şərhlərində, JavaScript dəyişkənlərində və ya frontend kodunda saxlamayın. Müvəqqəti məlumatlar belə kod idarəetmə sisteminə (git) gedərək tarixdə qala bilər.

---

## Task 5 — HTML İnyeksiyası (HTML Injection)

### İzahat

**HTML Injection** — istifadəçinin daxil etdiyi məlumatın filtrdən keçirilmədən birbaşa saytda göstərilməsidir. Əgər sayt istifadəçinin girişini **sanitize** (təmizləmə) etmirsə, zərərli HTML kodu sayta inyeksiya edilə bilər.

**Necə işləyir?**

```
Normal istifadəçi:
Ad sahəsinə → "Əli" yazır
Sayt göstərir → "Salam, Əli!"

Hücumçu:
Ad sahəsinə → <a href="http://hacker.com">Bura klik et!</a> yazır
Sayt göstərir → "Salam, [Bura klik et!]" (klikləyən zərərli sayta gedir!)
```

**Niyə təhlükəlidir?**

- İstifadəçiləri aldatmaq üçün saxta linklər yerləşdirilə bilər (**Phishing**)
- Saytın görünüşü dəyişdirilə bilər (**Defacement**)
- JavaScript inyeksiyasına qədər genişlənə bilər (**XSS — Cross-Site Scripting**)

**Həll yolu — Input Sanitization (Giriş Təmizləmə):**

```
İstifadəçi yazır: <a href="hacker.com">Klik</a>
Sanitizasiya sonra: &lt;a href="hacker.com"&gt;Klik&lt;/a&gt;
Saytda görünür: <a href="hacker.com">Klik</a>  (sadə mətn kimi, link kimi deyil)
```

`<` → `&lt;` və `>` → `&gt;` çevrilməsi HTML teqlərini zərərsizləşdirir.

---

### Praktiki Tapşırıq

**"View Site"** düyməsinə bas. Bir ad sahəsi olan sadə forma görünür. Məqsəd — `http://hacker.com` ünvanına aparan zərərli link inyeksiya etmək.

**Ad sahəsinə bu HTML kodu daxil et:**

```html
<a href="http://hacker.com">malicious</a>
```

**"Say Hi"** düyməsinə basıldıqda sayt bu kodu filtrdən keçirmədən göstərir — ekranda kliklenebilir bir "malicious" linki peyda olur. Bu link hacker.com-a aparır. Bir neçə saniyə sonra flag görünür.

**Nə baş verir (arxada):**

```javascript
// Saytın zəif kodu:
function sayHi(name) {
    document.getElementById("output").innerHTML = "Salam, " + name + "!";
    // Burada name filtrdən keçirilmir!
}

// İstifadəçi <a href="http://hacker.com">malicious</a> yazırsa:
// Çıxış: Salam, <a href="http://hacker.com">malicious</a>!
// Brauzer bunu HTML kimi render edir — link aktivdir!
```

> **Q: View the website on this task and inject HTML so that a malicious link to http://hacker.com is shown.**  
> ✅ `HTML_INJ3CTI0N`  
> *Daxil edilən kod: `<a href="http://hacker.com">malicious</a>`*

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Brauzerin render etdiyi komponenti ən yaxşı hansı termin təsvir edir? | `Front End` |
| Task 2 | Pişik saytındakı sınıq şəkili düzəlt — gizli mətn nədir? | `HTMLHERO` |
| Task 2 | İt şəklini əlavə et — şəkildəki mətn nədir? | `DOGHTML` |
| Task 3 | Demo elementini "Hack the Planet" eləyən JS əlavə et | `JSISFUN` |
| Task 3 | Düymə əlavə et və klik et | No Answer Needed |
| Task 4 | Mənbə kodunda gizlənmiş parol nədir? | `testpasswd` |
| Task 5 | HTML inyeksiya edərək http://hacker.com linki göstər | `HTML_INJ3CTI0N` |

---

## Bonus: Bu Otaqdan Çıxarılan Dərslər

### Proqramçılar üçün:
```
✅ Heç vaxt həssas məlumatları frontend kodunda saxlama
✅ İstifadəçi girişini HƏMİŞƏ sanitize et (filtrdən keçir)
✅ HTML şərhlərini produksiya kodunda silməyi unutma
✅ API açarlarını JavaScript-də saxlama — backend-də saxla
```

### Hücumçular (Pentesters) üçün:
```
🔍 "View Page Source" → HTML şərhlərini axtar
🔍 JavaScript fayllarını yoxla — hardcoded credentials (sabit yazılmış məlumatlar) ola bilər
🔍 Forma sahələrini HTML teqləri ilə sına — injection zəifliyi varmı?
🔍 Browser DevTools-dan istifadə et (F12)
```

### HTML vs JavaScript — Fərq:
```
HTML       → Quruluş (nə var?)
           → Statikdir — brauzer oxuyub göstərir
           → <h1>, <p>, <img>, <a>, <button>

JavaScript → Davranış (nə edir?)
           → Dinamikdir — kod işlədilir
           → DOM-u dəyişdirir, hadisələrə reaksiya verir
           → getElementById(), innerHTML, onclick
```

### Zəifliklər:
```
Sensitive Data Exposure:
  → Mənbə kodunda gizlənmiş məlumatlar
  → "View Page Source" ilə tapılır
  → Həll: Həssas məlumatları frontend-dən uzaqlaşdır

HTML Injection:
  → Filtrdən keçirilməmiş istifadəçi girişi
  → <a href="zərərli.com">mətn</a> kimi inyeksiya
  → Həll: Input sanitization (HTML encodinq)
  → Genişlənmiş versiyası: XSS (Cross-Site Scripting)
```
