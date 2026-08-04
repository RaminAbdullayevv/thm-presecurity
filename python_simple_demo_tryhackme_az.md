# 🐍 TryHackMe — Python: Simple Demo | Azərbaycanca Oxuma Materialı

> **Room:** https://tryhackme.com/room/pythonsimpledemo
> **Path:** Pre-Security
> **Əvvəlki roomlar:** Data Representation, Data Encoding

---

## Python nədir və niyə öyrənirik?

Python — **high-level, general-purpose** proqramlaşdırma dilidir. "High-level" dedikdə, dilin kompüterin daxili işlərini bizdən gizlətdiyi nəzərdə tutulur — yəni biz yaddaş idarəsi, prosessor əməliyyatları kimi detaylarla məşğul olmaq əvəzinə, sadəcə məntiqimizi yazırıq. "General-purpose" isə o deməkdir ki, Python-u veb tətbiqlər yazmaqdan tutmuş, məlumat analizi, maşın öyrənməsi, avtomatlaşdırma skriptləri kimi çox fərqli sahələrdə istifadə etmək olar.

Kibertəhlükəsizlikdə Python xüsusilə vacibdir — exploit skriptləri, avtomatlaşdırma alətləri, şəbəkə skanerlər, CTF çağırışları... bunların böyük əksəriyyəti Python ilə yazılır. Bunu bilmək sənə həm hücum, həm müdafiə tərəfindən çox güc verir.

Bu roomda bir **"Rəqəmi Tap"** oyunu yazacağıq. Plan belə:
- Kompüter 1 ilə 20 arasında gizli bir rəqəm seçir
- İstifadəçi düzgün rəqəmi tapana qədər təxmin edir
- Kompüter hər dəfə "çox böyük" ya "çox kiçik" deyir

---

## Dəyişənlər (Variables)

Hər proqramın məlumat saxlamağa ehtiyacı var. **Dəyişən** — bir məlumatı saxlayan adlı qutu kimidir. Python-da dəyişən yaratmaq çox sadədir: sadəcə ad yazıb `=` işarəsi ilə dəyər verirsən.

Oyunumuz üçün 3 dəyişən lazımdır:

```python
secret  →  kompüterin gizli seçdiyi rəqəm
guess   →  istifadəçinin verdiyi cavab
tries   →  neçə dəfə cəhd etdiyini sayan sayğac
```

İlk olaraq `random` kitabxanasını yükləyirik. **Kitabxana (library)** — başqalarının yazdığı hazır funksiyalar toplusudur. `random` kitabxanası bizə təsadüfi rəqəm yaratmağa kömək edir. `import` əmri ilə kitabxananı proqrama daxil edirik.

```python
import random  # random kitabxanasını yüklə

secret = random.randint(1, 20)  # 1 ilə 20 arasında təsadüfi tam ədəd seç
tries = 0                        # hələ heç bir cəhd yoxdur
guess = 0                        # istifadəçi hələ heç nə deməyib
```

`random.randint(1, 20)` — bu funksiya 1 ilə 20 arasında (hər ikisi daxil olmaqla) təsadüfi bir tam ədəd qaytarır. Nəticəni `secret` dəyişəninə veririk.

`tries = 0` və `guess = 0` — hər ikisini 0 ilə başladırıq. `guess`-i 0 etməyin xüsusi səbəbi var: gizli rəqəm heç vaxt 0 olmayacaq (çünki 1-20 arasındadır), buna görə başlanğıc dəyər kimi 0 təhlükəsizdir.

Sonra istifadəçiyə mesaj göstəririk:

```python
print("I'm thinking of a number between 1 and 20")
```

`print()` — ekrana mətn yazan funksiyadadır. Dırnaq içindəki hər şey ekranda görsənir.

Sonra istifadəçidən cavab alırıq:

```python
text = input("Take a guess: ")  # input() mətni string kimi qaytarır
guess = int(text)               # mətni tam ədədə çevir
tries = tries + 1               # cəhd sayğacını 1 artır
```

`input()` funksiyası ekranda sual göstərir və istifadəçinin yazdığını qaytarır — amma **həmişə mətn (string) kimi** qaytarır. Yəni istifadəçi `10` yazsа, `input()` bunu `"10"` kimi, yəni rəqəm kimi yox, mətn kimi qaytarır. Buna görə `int()` funksiyası ilə mətni tam ədədə çeviririk.

`tries = tries + 1` — `tries`-in köhnə dəyərini götür, üstünə 1 əlavə et, nəticəni yenidən `tries`-ə yaz. Bu cür sayğac artırma proqramlaşdırmada çox geniş istifadə olunur.

Bu mərhələdəki proqram (`guess_v1.py`) rəqəm seçir və istifadəçidən bir dəfə cavab alır — amma müqayisə etmir, heç bir cavab vermir. Bu bizim **natamam eskiz**imizdir.

---

## Şərti ifadələr — if / elif / else

İstifadəçinin cavabını gizli rəqəmlə müqayisə etməliyik. Bunun üçün **şərti ifadələr** lazımdır. Şərti ifadə — "əgər bu doğrudursa, bunu et; yox əgər o doğrudursa, onu et; heç biri deyilsə, bunu et" məntiqi ilə işləyir.

Pseudo-code (proqram dilinə yaxın Azərbaycan məntiqi) ilə yazaq:

```
Əgər guess 1-dən kiçik ya 20-dən böyükdürsə → "Aralıq xaricindədir"
Yox əgər guess, secret-dən kiçikdirsə        → "Çox kiçikdir"
Yox əgər guess, secret-dən böyükdürsə        → "Çox böyükdür"
Yox əgər heç biri deyilsə                   → "Tapdın!"
```

Bunu Python-da belə yazırıq:

```python
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

Hər sətri izah edək:

**`if guess < 1 or guess > 20:`** — Əgər `guess` 1-dən kiçik YA DA 20-dən böyükdürsə bu blok işləyir. `or` — "ya da" deməkdir. Bu şərt doğrudursa Python girintili (indented) sətri icra edir.

**`elif guess < secret:`** — `elif` = "else if" = "yox əgər". Əvvəlki `if` yanlış olduqda bura baxılır. Əgər `guess` `secret`-dən kiçikdirsə bu blok işləyir.

**`elif guess > secret:`** — Hər iki əvvəlki şərt yanlışdırsa bura baxılır. `guess` `secret`-dən böyükdürsə bu blok işləyir.

**`else:`** — Yuxarıdakıların heç biri doğru deyilsə bu blok işləyir. Məntiqi düşünsək: `guess` nə kiçik, nə böyük deyilsə — deməli `guess == secret`-dir, yəni tapdı!

**Nümunə 1:** `secret = 10`, istifadəçi `30` yazır.
```
30 < 1 → yalan    |    30 > 20 → doğru    →  "Out of range. Try again."
```

**Nümunə 2:** `secret = 10`, istifadəçi `5` yazır.
```
5 < 1 ya 5 > 20 → hər ikisi yalan  (birinci if keçilir)
5 < 10          → doğru            →  "Too low, try again."
```

**Nümunə 3:** `secret = 10`, istifadəçi `15` yazır.
```
15 < 1 ya 15 > 20 → yalan  (birinci if keçilir)
15 < 10           → yalan  (birinci elif keçilir)
15 > 10           → doğru  →  "Too high, try again."
```

**Nümunə 4:** `secret = 10`, istifadəçi `10` yazır.
```
10 < 1 ya 10 > 20 → yalan
10 < 10           → yalan
10 > 10           → yalan
else              → "You got it in 1 tries!"
```

**Vacib qeyd — Python-da girintilər (indentation):**
Python-da `{}` mötərizə yoxdur — onun əvəzinə **boşluqlar (spaces)** blokları müəyyən edir. `if`-dən sonrakı sətir mütləq içəri sürüşdürülmüş (indented) olmalıdır. Bu, Python-un ən xarakterik xüsusiyyətidir.

Bu mərhələdəki proqram (`guess_v2.py`) bir şərtlər sistemi qurur amma istifadəçiyə yalnız **bir cəhd** hüququ verir. Daha yaxşı oyun üçün sonsuz cəhd lazımdır.

---

## Dövrlər (Loops / Iterations)

İstifadəçiyə bir dəfə deyil, tapana qədər cəhd etmə imkanı vermək istəyirik. Bunun üçün **dövrə** (loop) lazımdır. Dövrə eyni kod blokunun müəyyən şərt doğru olduğu müddətcə təkrar-təkrar icra edilməsidir.

Gündəlik həyatdan nümunə: köynək almaq üçün mağazadan mağazaya girirsən — bəyəndiyini tapana qədər. Şərt: "bəyəndiyim köynəyi tapım". Şərt doğru olmadıqca (yəni hələ tapılmayıbsa) gəzməyə davam edirsən.

Python-da `while` dövrəsi belədir:

```python
while ŞƏRT:
    # Şərt doğru olduğu müddətcə bu blok icra olunur
```

Bizim oyunda şərt: "istifadəçinin cavabı gizli rəqəmə bərabər deyil" — yəni `guess != secret`. `!=` — "bərabər deyil" mənasındadır.

```python
while guess != secret:
    text = input("Take a guess: ")
    guess = int(text)
    tries = tries + 1

    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

**`while guess != secret:`** — "guess, secret-ə bərabər deyilsə" dövrəni icra et. Hər dövr sonunda Python bu şərti yenidən yoxlayır. İstifadəçi düzgün cavabı tapanda şərt yalan olur (`guess == secret`) → dövrə dayanır → proqram biter.

**Dövrənin gedişi:**

Tutaq ki, `secret = 10`, proqram başlayanda `guess = 0`:
```
1. Yoxla: 0 != 10  → doğru → dövrəyə gir
   İstifadəçi 5 yazır → guess=5, tries=1
   5 < 10 → "Too low, try again."

2. Yoxla: 5 != 10  → doğru → dövrəyə gir
   İstifadəçi 15 yazır → guess=15, tries=2
   15 > 10 → "Too high, try again."

3. Yoxla: 15 != 10 → doğru → dövrəyə gir
   İstifadəçi 10 yazır → guess=10, tries=3
   else → "You got it in 3 tries!"

4. Yoxla: 10 != 10 → YANLIŞ → dövrə dayanır → proqram biter
```

---

## Tam Proqram — guess_v3.py

Bütün hissələri birləşdirdikdə final proqram belə görünür:

```python
import random  # random kitabxanasını yüklə

# Kompüter 1-20 arasında gizli rəqəm seçir
secret = random.randint(1, 20)
tries = 0
guess = 0  # başlanğıc dəyər (1-20 xaricindədir, beləliklə while işləyir)

print("I'm thinking of a number between 1 and 20")

# Düzgün tapana qədər dövr
while guess != secret:
    text = input("Take a guess: ")   # istifadəçidən mətn al
    guess = int(text)                # mətni ədədə çevir
    tries = tries + 1                # cəhd sayğacını artır

    # Nəticəni yoxla və istifadəçiyə dəstək ver
    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

Oyunun nümunəvi gedişi:
```
I'm thinking of a number between 1 and 20
Take a guess: 10
Too high, try again.
Take a guess: 5
Too low, try again.
Take a guess: 7
Too low, try again.
Take a guess: 8
You got it in 4 tries!
```

---

## Öyrəndiklərinin Xülasəsi

Bu roomda Python-un 3 əsas sütununu gördük:

**1. Dəyişənlər (Variables)**
Məlumat saxlamaq üçün istifadə olunur. `secret`, `guess`, `tries` — hər biri müəyyən məlumatı yadda saxlayır. `=` ilə dəyər verilir.

**2. Şərti ifadələr (Conditionals)**
`if / elif / else` — müxtəlif şərtlərə görə fərqli kod icra etmək üçün. Yalnız doğru olan şərtin bloku işləyir, digərləri keçilir. Python-da girintilər blokları müəyyən edir.

**3. Dövrlər (Loops)**
`while ŞƏRT:` — şərt doğru olduğu müddətcə eyni bloku təkrar icra edir. Şərt yalan olduqda dövrə dayanır.

---

## Proqramın İnkişaf Mərhələləri

| Versiya | Fayl | Nə edir? | Çatışmazlıq |
|---------|------|----------|-------------|
| v1 | `guess_v1.py` | Rəqəm seçir, bir dəfə cavab alır | Müqayisə yoxdur |
| v2 | `guess_v2.py` | Müqayisə edir, ipucu verir | Yalnız 1 cəhd |
| v3 | `guess_v3.py` | Tapana qədər dövr edir | Tam işləyir ✅ |

---

## Kibertəhlükəsizlikdə Python

Bu roomda öyrəndiklərinin real tətbiqləri:

| Konsept | Kibertəhlükəsizlikdə istifadə |
|---------|-------------------------------|
| `import` | Exploit kitabxanalarını yükləmək (`import socket`, `import requests`) |
| `while` dövrəsi | Brute force — şifrə tapana qədər dövr et |
| `if / elif` | Port skan nəticəsinə görə fərqli addım at |
| `input()` | İnteraktiv hücum skriptlərində istifadəçidən hədəf almaq |
| `int()` | Şəbəkə paketlərindəki byte-ları ədədə çevirmək |

---

*Hazırlandı: 2026 | Platforma: TryHackMe Pre-Security Path | Azərbaycanca*
