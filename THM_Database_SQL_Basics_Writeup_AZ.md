# TryHackMe — Database SQL Basics | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** Database SQL Basics  
> **Çətinlik:** Easy  
> **Müddət:** 30 dəqiqə  
> **Link:** https://tryhackme.com/room/databasesqlbasics  

---

## Ümumi Baxış

Bu otaqda verilənlər bazasının (database) nə olduğunu, içindəki məlumatların necə təşkil edildiyini, və SQL dilindən istifadə edərək bazadan məlumat necə çəkiləcəyini öyrənirik. Mövzu bir kafe nümunəsi üzərindən izah edilir — bu isə hər şeyi çox asan başa düşməyə imkan verir.

---

## Task 1 — Giriş (Introduction)

### İzahat

**Data (Məlumat) nədir?**

Data — hər hansı faktdır: bir sifarişin qiyməti, içkinin adı, vaxtı. Bu məlumatlar kompüter işləyərkən yaddaşda saxlanılır. Lakin kompüter söndürüldükdə o yaddaş silinir. Bəs məlumatları kalıcı saxlamaq üçün nə etmək lazımdır?

Cavab: **Verilənlər bazası (Database).**

**Niyə database lazımdır?**

Bir kafe sahibini düşün. Başlanğıcda hər sifarişi kağız dəftərə yazır — içkinin adı, qiyməti, vaxtı. Kafe kiçik olduqda bu üsul işləyir. Amma günlər keçdikcə dəftər dolur. "Bu gün neçə qəhvə satıldı?" sualına cavab tapmaq üçün bütün səhifələri oxuyub əllə saymaq lazım gəlir. Bu çox yavaş və çətin bir prosesdir.

Verilənlər bazası bu problemi həll edir: məlumatları strukturlaşdırılmış şəkildə saxlayır, minlərlə qeyd içindən saniyələr ərzində axtarış edir.

**SQL nədir?**

SQL (Structured Query Language — Strukturlaşdırılmış Sorğu Dili) — verilənlər bazasına sual vermək üçün istifadə olunan xüsusi bir dildir. Siz SQL yazırsınız, verilənlər bazası axtarışı özü edir və nəticəni sizə göstərir. Sorğu məlumatları **dəyişdirmir**, yalnız **oxuyur**.

### Sual

> **I am ready to dive into the database!**  
> ✅ Cavab tələb olunmur — "Correct Answer" düyməsinə bas.

---

## Task 2 — Cədvəllər, Sətrlər və Sütunlar (Understanding Tables, Rows, and Columns)

### İzahat

Verilənlər bazası içindəki məlumatlar **cədvəllər** (tables) şəklində saxlanılır. Cədvəl görünüşcə Excel cədvəlinə bənzəyir.

#### Cədvəlin quruluşu

```
+----+----------+-------+----------+
| id |  drink   | price |   time   |
+----+----------+-------+----------+
|  1 | Coffee   |  2.50 | 09:15:00 |
|  2 | Tea      |  1.80 | 09:22:00 |
|  3 | Latte    |  3.20 | 09:30:00 |
|  4 | Coffee   |  2.50 | 09:45:00 |
+----+----------+-------+----------+
```

**Sütun (Column)** — cədvəlin üst hissəsindəki başlıqlar. Hər sütun bir məlumat növünü saxlayır.
- `id` → sifariş nömrəsi
- `drink` → içki adı
- `price` → qiyməti
- `time` → sifariş vaxtı

**Sətir (Row)** — tam bir qeyd. Yuxarıdakı misalda hər sətir bir müştərinin sifarişini əks etdirir.

#### Niyə bu quruluş?

- 10 sifariş olsa → 10 sətir
- Yeni sifariş gəlsə → yeni sətir əlavə olunur
- Bir sifariş silinse → yalnız o sətir silinir, digərləri heç dəyişmir

Bu struktur verilənlər bazasına sürətli axtarış etmək imkanı verir. Minlərlə sifariş olsa belə, SQL sorğusu cavabı anında tapır.

---

### Sual və Cavab

> **Q: Inside databases, what is the term for the "spreadsheets" that store the information?**  
> ✅ `Table`  
> *İzah: Verilənlər bazasında məlumatlar cədvəllərdə (table) saxlanılır — Excel-dəki "vərəq" (sheet) kimi düşün.*

---

## Task 3 — İlk SQL Sorğunu Yaz (Writing Your First SQL Query)

### İzahat

Bu task praktiki hissədir. Brauzer içindəki bir SQL terminalında real sorğular yazacaqsınız. **Heç nə sınan bilməz** — yanlış yazsanız "Reset Data" düyməsinə basıb yenidən başlaya bilərsiniz.

İki cədvəl mövcuddur:

**`Orders` cədvəli** — sifarişlər:
| Sütun | Məzmun |
|-------|--------|
| `id` | Sifariş nömrəsi |
| `drink` | İçki adı |
| `price` | Qiyməti |
| `time` | Sifariş vaxtı |

**`Menu` cədvəli** — menyu:
| Sütun | Məzmun |
|-------|--------|
| `drink` | İçki adı |
| `price` | Qiyməti |

---

### 4 Əsas SQL Açar Sözü

| Açar söz | Nə edir? |
|----------|----------|
| `SELECT` | Hansı sütunları göstərəcəyini seçir |
| `FROM` | Hansı cədvəldən oxuyacağını bildirir |
| `WHERE` | Şərtə görə sətirləri filtrləyir |
| `ORDER BY` | Nəticələri sıralayır |

---

### Addım 1 — Bütün məlumatları göstər (`SELECT *`)

`*` işarəsi "bütün sütunlar" deməkdir.

```sql
SELECT * FROM Orders;
```

Bu sorğu `Orders` cədvəlindəki **bütün** sütunları və **bütün** sətirləri göstərir. Nəticədə **50 sətir** gəlir.

```sql
SELECT * FROM Menu;
```

Menyu cədvəlini göstərir.

---

### Addım 2 — Yalnız bəzi sütunları göstər

Hər dəfə bütün sütunlar lazım olmur. Yalnız lazım olanları `SELECT`-dən sonra sıralayırıq:

```sql
SELECT drink, price FROM Orders;
```

Nəticədə yalnız içki adı və qiyməti görünür — digər sütunlar ekrana gəlmir. Bu sorğu oxunuşu asanlaşdırır.

---

### Addım 3 — Filtrləmə (`WHERE`)

`WHERE` ilə yalnız müəyyən şərtə uyğun sətirləri göstərə bilirik:

```sql
SELECT * FROM Orders WHERE drink = 'Coffee';
```

Bu sorğu yalnız qəhvə sifarişlərini göstərir. Digər içkilər filtrdən keçmir.

> **Məsləhət:** Verilənlər bazasındakı içki adlarını bilmirsənsə, əvvəlcə menyu cədvəlini yoxla:
> ```sql
> SELECT * FROM Menu;
> ```

Mətn dəyərləri həmişə tək dırnaqlarda yazılır: `'Coffee'`, `'Tea'`, `'Latte'`.

---

### Addım 4 — Sıralama (`ORDER BY`)

`ORDER BY` nəticələri müəyyən sütuna görə sıralayır. Standart olaraq **artan sıra** (kiçikdən böyüyə) istifadə olunur:

```sql
SELECT * FROM Orders ORDER BY price;
```

Ən ucuz içkilər üstdə görünür.

**Azalan sıra üçün `DESC` əlavə et:**

```sql
SELECT * FROM Orders ORDER BY price DESC;
```

Ən bahalı içkilər üstdə görünür.

**Menyunu bahalıdan ucuza sırala:**

```sql
SELECT * FROM Menu ORDER BY price DESC;
```

Bu sorğu ilk sətirdə ən bahalı içkini göstərir — **Latte**.

---

### Addım 5 — Filtri və Sıralamanı Birləşdir

`WHERE` və `ORDER BY` birlikdə istifadə oluna bilər — əvvəlcə filtr, sonra sıralama:

```sql
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;
```

Bu sorğu:
1. Yalnız qəhvə sifarişlərini filtrləyir
2. Onları qiymətə görə azalan sırada sıralayır

**Sorğunun sırası həmişə belədir:**

```sql
SELECT [sütunlar]
FROM [cədvəl]
WHERE [şərt]
ORDER BY [sütun] [ASC/DESC];
```

---

### Suallar və Cavablar

> **Q: When you showed all orders, how many rows were returned?**  
> ```sql
> SELECT * FROM Orders;
> ```
> ✅ `50`  
> *İzah: `Orders` cədvəlində üst-üstə 50 sifariş qeydi var.*

> **Q: When you sorted orders by price from cheapest to most expensive, which drink appeared first?**  
> ```sql
> SELECT * FROM Orders ORDER BY price;
> ```
> ✅ `Tea`  
> *İzah: Çay (Tea) ən ucuz içki olduğundan artan sıralamada birinci gəlir.*

> **Q: When you sorted the menu by price from most expensive to cheapest, which drink appeared first?**  
> ```sql
> SELECT * FROM Menu ORDER BY price DESC;
> ```
> ✅ `Latte`  
> *İzah: Latte menyudakı ən bahalı içkidir, ona görə azalan sıralamada birinci görünür.*

---

## Task 4 — Nəticə (Conclusion)

### İzahat

Bu otaqda öyrəndiklərimizin xülasəsi:

| Anlayış | İzah |
|---------|------|
| **Database** | Məlumatları strukturlaşdırılmış şəkildə saxlayan rəqəmsal sistem |
| **Table** | Bazadakı cədvəl — məlumatlar burda saxlanılır |
| **Column (Sütun)** | Məlumat növünü təyin edir (ad, qiymət, vaxt...) |
| **Row (Sətir)** | Tam bir qeyd (bir sifariş, bir müştəri...) |
| **SQL** | Bazaya sual vermək üçün istifadə olunan dil |
| **SELECT** | Hansı sütunları göstərəcəyini seçir |
| **FROM** | Hansı cədvəldən oxuyacağını bildirir |
| **WHERE** | Sətirləri şərtə görə filtrləyir |
| **ORDER BY** | Nəticələri sıralayır (ASC = artan, DESC = azalan) |

Otaq bu sualı da gündəmə gətirir: "Kafe sifarişlərini icazəsiz kimsə dəyişə və ya silə bilsəydi nə baş verərdi?" Bu sual gələcək otaqlar üçün bir işarədir — **verilənlər bazası təhlükəsizliyi** (SQL Injection kimi hücumlar) mövzusuna körpü qurur.

### Sual

> **I have successfully completed this room and can write basic SQL queries.**  
> ✅ Cavab tələb olunmur — "Correct Answer" düyməsinə bas.

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Ready to dive in? | No answer needed |
| Task 2 | Bazada məlumatları saxlayan "elektron cədvəllərin" adı nədir? | `Table` |
| Task 3 | Bütün sifarişlər göstərildikdə neçə sətir gəldi? | `50` |
| Task 3 | Sifarişlər ucuzdan bahaya sıralandıqda hansı içki birinci gəldi? | `Tea` |
| Task 3 | Menyu bahalıdan ucuza sıralandıqda hansı içki birinci gəldi? | `Latte` |
| Task 4 | Room tamamlandı? | No answer needed |

---

## Bonus: SQL Sorğu Şablonları (Quick Reference)

```sql
-- Cədvəldəki hər şeyi göstər
SELECT * FROM Orders;

-- Yalnız müəyyən sütunları göstər
SELECT drink, price FROM Orders;

-- Menyunu göstər
SELECT * FROM Menu;

-- Şərtə görə filtrlə (yalnız qəhvə sifarişləri)
SELECT * FROM Orders WHERE drink = 'Coffee';

-- Artan sırada sırala (ucuzdan bahaya)
SELECT * FROM Orders ORDER BY price;

-- Azalan sırada sırala (bahadan ucuza)
SELECT * FROM Orders ORDER BY price DESC;

-- Menyuyu bahadan ucuza sırala
SELECT * FROM Menu ORDER BY price DESC;

-- Filtri və sıralamanı birləşdir
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;
```
