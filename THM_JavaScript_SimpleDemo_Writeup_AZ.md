# TryHackMe — JavaScript: Simple Demo | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** JavaScript: Simple Demo  
> **Çətinlik:** Medium  
> **Link:** https://tryhackme.com/room/javascriptsimpledemo  

---

## Ümumi Baxış

Bu otaqda JavaScript proqramlaşdırma dilinin əsaslarını "Rəqəmi tap" oyunu üzərindən öyrənirik. Oyunun məntiqi belədir:

1. Kompüter 1 ilə 20 arasında gizli bir rəqəm seçir.
2. İstifadəçi düzgün cavabı tapana qədər təxmin edir.
3. Hər təxmindən sonra proqram "çox yüksək" və ya "çox aşağı" deyir.

Bu otaqdakı 3 əsas mövzu: **Dəyişənlər**, **Şərti ifadələr** və **Döngülər**.

---

## Task 1 — Giriş (Introduction)

### İzahat

JavaScript bu gün istifadə etdiyimiz demək olar ki, hər veb səhifədə işlənir. Başlanğıcda yalnız brauzer tərəfində (client-side) işlənən bu dil, **Node.js**-in meydana çıxması ilə server tərəfində (server-side) də istifadə olunmağa başladı.

Bu otaqda biz JavaScript kodunu brauzerdə yox, **Node.js** vasitəsilə terminal üzərindən işlədəcəyik.

```
ubuntu@tryhackme:~/JavaScript-Demo$ node guess_v3.js
I'm thinking of a number between 1 and 20
Take a guess: 10
Too high, try again.
Take a guess: 5
Too low, try again.
Take a guess: 7
You got it in 3 tries!
```

### Sual

> **I am ready to build this game in JavaScript!**  
> ✅ Cavab tələb olunmur — "Correct Answer" düyməsinə bas.

---

## Task 2 — Dəyişənlər (Variables)

### İzahat

#### Dəyişən nədir?

Dəyişən (variable) — yaddaşda bir yer tutub, dəyəri saxlayan və sonradan həmin dəyəri dəyişə biləcəyimiz strukturdur. Məsələn, istifadəçinin neçə dəfə cəhd etdiyini (`tries`) və ən son verdiyini (`guess`) yadda saxlamaq üçün dəyişən açırıq.

#### `let` — Dəyişən elan etmək

```javascript
let tries = 0;
let guess = 0;
```

- `let` açar sözü dəyişən elan etmək üçün istifadə olunur.
- `tries = 0` — oyun başlayanda istifadəçi hələ heç bir cəhd etməyib, ona görə 0-dan başlayırıq.
- `guess = 0` — gizli rəqəm 1-dən 20-yə qədər olduğundan, 0 ilkin dəyər olaraq düzgündür (yanlış uyğunluq yaranmaz).

#### `const` — Sabit elan etmək

Dəyəri dəyişməyəcək bir şey üçün `const` açar sözü istifadə edirik. Gizli rəqəm oyun boyu sabit qalacaq, ona görə `const` ilə elan edirik:

```javascript
const secret = Math.floor(Math.random() * 20) + 1;
```

Bu ifadənin izahı:
- `Math.random()` → 0 ilə 1 arasında ondalıklı rəqəm verir (məs. `0.372`)
- `* 20` → həmin rəqəmi 20 ilə vururuq (məs. `7.44`)
- `Math.floor()` → ondalığı kəsib tam ədəd alırıq (məs. `7`)
- `+ 1` → aralığı 0–19-dan 1–20-yə çeviririk

#### Ekrana çıxarmaq — `console.log()`

```javascript
console.log("I'm thinking of a number between 1 and 20");
```

`console.log()` funksiyası terminalda (ekranda) mətn göstərmək üçün istifadə olunur. Python-dakı `print()` funksiyasının JavaScript qarşılığıdır.

---

### Suallar və Cavablar

> **Q: What word is used to declare a variable?**  
> ✅ `let`

> **Q: What word is used to declare a constant?**  
> ✅ `const`

> **Q: What is the method that we call to display text on the screen?**  
> ✅ `console.log()`

---

## Task 3 — İstifadəçidən Giriş Almaq (Prompting the User for Input)

### İzahat

İstifadəçinin klaviaturadan daxil etdiyi dəyəri oxumaq üçün `readline` modulundan istifadə edirik.

#### Kitabxanaları yükləmək (import)

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";
```

- `readline` — istifadəçidən mətn oxumaq üçün Node.js-in daxili modulu.
- `stdin` — klaviatura girişi (standart giriş).
- `stdout` — ekran çıxışı (standart çıxış).

#### Interface yaratmaq

```javascript
const rl = readline.createInterface({ input, output });
```

Bu sətir sanki bir "mikrofon" yaradır — istifadəçi ilə proqram arasında ünsiyyət kanalı.

#### İstifadəçidən sual sormaq

```javascript
const text = await rl.question("Take a guess: ");
guess = parseInt(text, 10);
```

- `rl.question()` — istifadəçidən cavab gözləyir.
- `await` — proqramı dayandırır, istifadəçi cavab yazana qədər gözləyir.
- `parseInt(text, 10)` — istifadəçinin yazdığı mətni rəqəmə çevirir (10 = onlu say sistemi).

> **Niyə `parseInt` lazımdır?** — İstifadəçinin yazdığı hər şey əvvəlcə mətn (string) kimi gəlir. Rəqəmlərlə müqayisə etmək üçün onu tam ədədə çevirmək lazımdır.

#### Interfeysi bağlamaq — `rl.close()`

```javascript
try {
    // ... proqram kodu ...
} finally {
    rl.close();
}
```

- `try` bloku — kodun düzgün işləməsini təmin edir; xəta olsa, proqram çökmür.
- `finally` bloku — nə olursa olsun, sonunda mütləq işlənir. Burada `rl.close()` ilə "mikrofonu" bağlayırıq.

#### İlk versiya — `guess_v1.js`

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * 20) + 1;
    let tries = 0;
    let guess = 0;

    console.log("I'm thinking of a number between 1 and 20");

    const text = await rl.question("Take a guess: ");
    guess = parseInt(text, 10);

    tries = tries + 1;
} finally {
    rl.close();
}
```

**Çatışmazlıq:** İstifadəçi cavab verdikdən sonra heç bir geri bildirim almır — proqram sadəcə bağlanır. Şərti ifadələr lazımdır.

---

### Sual və Cavab

> **Q: What method is used to convert user input into a number?**  
> ✅ `parseInt()`

---

## Task 4 — Şərti İfadələr (Conditional Statements)

### İzahat

İstifadəçinin verdiyini gizli rəqəmlə müqayisə etmək üçün `if / else if / else` konstruksiyasından istifadə edirik.

#### Məntiqi axış

| Şərt | Nəticə |
|------|--------|
| `guess < 1` və ya `guess > 20` | "That number is out of range. Try again." |
| `guess < secret` | "Too low, try again." |
| `guess > secret` | "Too high, try again." |
| heç biri deyilsə | "You got it in X tries!" |

#### Kod

```javascript
if (guess < 1 || guess > 20) {
    console.log("That number is out of range. Try again.");
} else if (guess < secret) {
    console.log("Too low, try again.");
} else if (guess > secret) {
    console.log("Too high, try again.");
} else {
    console.log("You got it in", tries, "tries!");
}
```

#### İzah

- `||` operatoru — "və ya" mənasını verir. `guess < 1 || guess > 20` — "1-dən kiçik YA DA 20-dən böyükdürsə".
- `else if` — əvvəlki şərt yanlışdırsa, bu şərtə bax.
- `else` (tək) — bütün yuxarıdakı şərtlər yanlışdırsa işlə. Buraya gəlmək üçün `guess` nə kiçik, nə böyük ola bilər — yəni `secret`-ə bərabərdir.

#### İkinci versiya — `guess_v2.js`

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * 20) + 1;
    let tries = 0;
    let guess = 0;

    console.log("I'm thinking of a number between 1 and 20");

    const text = await rl.question("Take a guess: ");
    guess = parseInt(text, 10);

    tries = tries + 1;

    if (guess < 1 || guess > 20) {
        console.log("That number is out of range. Try again.");
    } else if (guess < secret) {
        console.log("Too low, try again.");
    } else if (guess > secret) {
        console.log("Too high, try again.");
    } else {
        console.log("You got it in", tries, "tries!");
    }
} finally {
    rl.close();
}
```

**Çatışmazlıq:** İstifadəçi yalnız bir şans alır. Yanlış cavab versə, proqram bağlanır. Birdən çox şans vermək üçün döngü lazımdır.

---

### Suallar və Cavablar

> **Q: The secret is 10. What will our program display on the screen if the user makes a guess of 15?**  
> ✅ `Too high, try again.`  
> *Səbəb: 15 > 10, yəni `guess > secret` şərti doğrudur.*

> **Q: The secret is 10. What will our program display on the screen if the user makes a guess of 35?**  
> ✅ `That number is out of range. Try again.`  
> *Səbəb: 35 > 20, yəni `guess > 20` şərti doğrudur — aralıq xaricindədir.*

---

## Task 5 — Döngülər (Iterations)

### İzahat

İstifadəçiyə düzgün cavabı tapana qədər dəfələrlə cəhd etmə imkanı vermək üçün `while` döngüsündən istifadə edirik.

#### `while` döngüsü necə işləyir?

```javascript
while (şərt) {
    // şərt doğru olduğu müddətcə bu blok işlər
}
```

Bizdə şərt budur: `guess !== secret` — yəni "istifadəçinin verdiyi cavab gizli rəqəmə bərabər deyilsə, döngüyə davam et."

- `!==` operatoru JavaScript-də "bərabər deyil" mənasını verir.

#### Döngünün içindəki əməliyyatlar

1. İstifadəçidən yeni bir təxmin al.
2. Daxil edilən mətni rəqəmə çevir.
3. Cəhd sayını (`tries`) bir artır.
4. Təxmini gizli rəqəmlə müqayisə et və geri bildirim ver.
5. Əgər düzgün cavabdırsa, döngü bitir.

#### Döngü kodu

```javascript
while (guess !== secret) {
    const text = await rl.question("Take a guess: ");
    guess = parseInt(text, 10);

    tries = tries + 1;

    if (guess < 1 || guess > 20) {
        console.log("That number is out of range. Try again.");
    } else if (guess < secret) {
        console.log("Too low, try again.");
    } else if (guess > secret) {
        console.log("Too high, try again.");
    } else {
        console.log("You got it in", tries, "tries!");
    }
}
```

#### Son versiya — `guess_v3.js` (tam kod)

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * 20) + 1; // 1 <= secret <= 20
    let tries = 0;
    let guess = 0;

    console.log("I'm thinking of a number between 1 and 20");

    while (guess !== secret) {
        const text = await rl.question("Take a guess: ");
        guess = parseInt(text, 10);

        tries = tries + 1;

        if (guess < 1 || guess > 20) {
            console.log("That number is out of range. Try again.");
        } else if (guess < secret) {
            console.log("Too low, try again.");
        } else if (guess > secret) {
            console.log("Too high, try again.");
        } else {
            console.log("You got it in", tries, "tries!");
        }
    }
} finally {
    rl.close();
}
```

#### Oyunu sınamaq üçün terminal əmri

```bash
cd /home/ubuntu/JavaScript-Demo
node guess_v3.js
```

---

### Suallar və Cavablar

> **Q: What is the name of the loop that we used in this task?**  
> ✅ `while`

> **Q: What is the name of the variable that is incremented by one when the user makes a new wrong guess?**  
> ✅ `tries`

> **Q: How is "not equal" written in JavaScript?**  
> ✅ `!==`

---

## Task 6 — Nəticə (Conclusion)

### İzahat

Bu otaqda imperativ proqramlaşdırma dillərinin 3 əsas dirəyini öyrəndik:

| Konsept | Açar söz / Metod |
|---------|------------------|
| Dəyişən elan etmək | `let` |
| Sabit elan etmək | `const` |
| Ekrana çıxarmaq | `console.log()` |
| İstifadəçidən giriş almaq | `rl.question()` + `parseInt()` |
| Şərti ifadə | `if / else if / else` |
| Döngü | `while` |
| Bərabər deyil operatoru | `!==` |

JavaScript öyrənmək üçün növbəti addım kimi Python: Simple Demo otağındakı eyni proqramı ilə bu kodu müqayisə etmək tövsiyə olunur — fərqli dillərin eyni problemi necə fərqli həll etdiyini görmək çox faydalıdır.

### Sual

> **I successfully ran my first game created in JavaScript.**  
> ✅ Cavab tələb olunmur — "Correct Answer" düyməsinə bas.

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Ready to build? | No answer needed |
| Task 2 | Dəyişən elan etmək üçün hansı söz? | `let` |
| Task 2 | Sabit elan etmək üçün hansı söz? | `const` |
| Task 2 | Ekrana mətn çıxarmaq üçün hansı metod? | `console.log()` |
| Task 3 | İstifadəçi girişini rəqəmə çevirmək üçün hansı metod? | `parseInt()` |
| Task 4 | secret=10, guess=15 olduqda nə çıxar? | `Too high, try again.` |
| Task 4 | secret=10, guess=35 olduqda nə çıxar? | `That number is out of range. Try again.` |
| Task 5 | Hansı döngüdən istifadə etdik? | `while` |
| Task 5 | Hər yanlış cəhddə artırılan dəyişənin adı? | `tries` |
| Task 5 | JavaScript-də "bərabər deyil" necə yazılır? | `!==` |
| Task 6 | Game completed? | No answer needed |
