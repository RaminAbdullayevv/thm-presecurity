# TryHackMe — Database SQL Basics Lab İzahı

**Çətinlik:** Easy  
**Platforma:** TryHackMe  
**Link:** https://tryhackme.com/room/databasesqlbasics  
**Mövzu:** Verilənlər bazası, Cədvəllər, SQL sorğuları

---

## Task 1 — Giriş

### Problem: Məlumat hara gedir?

Kompüter işləyərkən məlumatları RAM-da saxlayır. Amma kompüter söndürüldükdə RAM-dakı məlumatlar silinir. Bəs uzun müddət saxlanılmalı məlumatlar harada saxlanılır?

**Cavab: Verilənlər bazası (Database)**

### Ssenariya — Kiçik Kafe

Kiçik bir kafenin sahibi sifarişləri kağız dəftərçədə yazır:
- İçki adı
- Qiymət
- Sifariş vaxtı

**Problem:** Kafe böyüdükcə dəftərçə dolur. Suallar çətinləşir:
- "Bu gün neçə qəhvə satıldı?"
- "Bu səhər ən ucuz içki nə idi?"

Hər suala cavab vermək üçün bütün səhifələri oxumaq lazımdır — bu çox yavaşdır.

**Həll: Verilənlər bazası**

---

### Öyrəniləcəklər

- Məlumat nədir və niyə vacibdir
- Verilənlər bazası nədir və niyə istifadə edilir
- SQL nədir
- Cədvəl, sətir və sütun anlayışları
- Sadə SQL sorğuları yazmaq

---

## Task 2 — Cədvəllər, Sətirlər və Sütunlar

### Verilənlər Bazası nədir?

**Verilənlər bazası (Database)** — məlumatları strukturlu, axtarıla bilən şəkildə saxlayan rəqəmsal sistemdir.

> Kağız dəftərçə kimi, amma heç vaxt dolmayan, saniyələrdə minlərlə sətri axtara bilən rəqəmsal versiya.

---

### Cədvəl (Table) nədir?

Verilənlər bazasında məlumatlar **cədvəllərdə** saxlanılır.

Cədvəl — elektron cədvəl (spreadsheet) kimidir:

```
+----+----------+-------+----------+
| id | drink    | price | time     |
+----+----------+-------+----------+
|  1 | Coffee   | 3.50  | 09:15    |
|  2 | Tea      | 2.00  | 09:22    |
|  3 | Latte    | 4.50  | 09:45    |
|  4 | Coffee   | 3.50  | 10:01    |
+----+----------+-------+----------+
```

---

### Sütun (Column) nədir?

- Cədvəlin **yuxarısındakı başlıqlar**
- Bir tip məlumatı saxlayır
- Nümunə: `id`, `drink`, `price`, `time`

**Kafedə sütunlar:**
- `id` — sifariş nömrəsi
- `drink` — içki adı
- `price` — qiymət
- `time` — sifariş vaxtı

---

### Sətir (Row) nədir?

- Cədvəlin **üfüqi sətirləri**
- Bir tam qeyd saxlayır
- Hər sifariş = bir sətir

**Qaydalar:**
- Yeni sifariş gəldi → yeni sətir əlavə olunur
- Sifariş silindi → yalnız o sətir silinir, qalan cədvəl toxunulmaz qalır
- 10 sifariş = 10 sətir

---

### Verilənlər Bazası vs Kağız Dəftərçə

| Xüsusiyyət | Kağız Dəftərçə | Verilənlər Bazası |
|-----------|---------------|-------------------|
| Axtarış | Manual, yavaş | Anında, avtomatik |
| Məlumat həcmi | Məhdud | Milyonlarla sətir |
| Sıralama | Mümkün deyil | Saniyələrdə |
| Paylaşım | Çətin | Çox istifadəçi eyni anda |
| Yedəkləmə | Çətin | Avtomatik |

---

### SQL nədir?

**SQL** — Structured Query Language (Strukturlu Sorğu Dili)

- Verilənlər bazasına sual vermək üçün istifadə edilən dildir
- Proqramlaşdırma dili deyil — **sorğu dili**dir
- Cədvəldəki məlumatları oxumaq, süzmək, sıralamaq üçün

**SQL suallar (queries) necə görünür:**
- "Bütün sifarişləri göstər"
- "Yalnız qəhvə sifarişlərini göstər"
- "Ən ucuz içkini göstər"

> Sorğu məlumatı **dəyişdirmir** — yalnız göstərir.

---

## Task 3 — İlk SQL Sorğunu Yaz

### Kafe Cədvəlləri

Bu labda 2 cədvəl var:

**Orders cədvəli:**
```
(id, drink, price, time)
```

**Menu cədvəli:**
```
(drink, price)
```

---

### SQL-in 4 Əsas Açar Sözü

#### `SELECT` — Nəyi seçirsən?

```sql
SELECT *        -- bütün sütunları seç
SELECT drink    -- yalnız içki adını seç
SELECT drink, price  -- iki sütun seç
```

- `*` — "hamısı" deməkdir (wildcard)

---

#### `FROM` — Haradan alırsın?

```sql
FROM Orders    -- Orders cədvəlindən
FROM Menu      -- Menu cədvəlindən
```

---

#### `WHERE` — Hansı şərtlə?

```sql
WHERE drink = 'Coffee'     -- yalnız qəhvə sifarişləri
WHERE price < 3.00         -- 3-dən ucuz içkilər
WHERE time = '09:15'       -- müəyyən vaxtdakı sifarişlər
```

---

#### `ORDER BY` — Necə sıralanır?

```sql
ORDER BY price ASC   -- ucuzdan bahaya (artan)
ORDER BY price DESC  -- bahadan ucuza (azalan)
ORDER BY drink ASC   -- əlifba sırası ilə
```

- `ASC` — Ascending (artan) — kiçikdən böyüyə
- `DESC` — Descending (azalan) — böyükdən kiçiyə

---

### Tam Sorğu Quruluşu

```sql
SELECT sütun(lar)
FROM cədvəl
WHERE şərt
ORDER BY sütun ASC/DESC;
```

---

### Praktiki Sorğu Nümunələri

**1. Bütün sifarişləri göstər:**
```sql
SELECT * FROM Orders;
```
→ 50 sətir qaytarır

---

**2. Yalnız içki adlarını göstər:**
```sql
SELECT drink FROM Orders;
```

---

**3. Sifarişləri ucuzdan bahaya sırala:**
```sql
SELECT * FROM Orders ORDER BY price ASC;
```
→ İlk sətirdə **Tea** (ən ucuz)

---

**4. Menyunu bahadan ucuza sırala:**
```sql
SELECT * FROM Orders ORDER BY price DESC;
```
→ İlk sətirdə **Latte** (ən baha)

---

**5. Yalnız qəhvə sifarişlərini göstər:**
```sql
SELECT * FROM Orders WHERE drink = 'Coffee';
```

---

**6. 3.00-dan ucuz içkilər:**
```sql
SELECT * FROM Orders WHERE price < 3.00;
```

---

**7. Qəhvə sifarişlərini vaxtla sırala:**
```sql
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY time ASC;
```

---

### SQL Yazılış Qaydaları

- SQL **böyük/kiçik hərfə həssas deyil** — `SELECT` = `select` = `Select`
- Lakin konvensiya olaraq açar sözlər BÖYÜK hərflə yazılır
- Mətn dəyərləri `'tək dırnaq'` içinə alınır: `WHERE drink = 'Coffee'`
- Sorğunun sonuna `;` qoyulur
- `--` ilə şərh yazılır:
```sql
SELECT * FROM Orders; -- bütün sifarişlər
```

---

### Müqayisə Operatorları

| Operator | Mənası | Nümunə |
|----------|--------|--------|
| `=` | Bərabərdir | `WHERE drink = 'Tea'` |
| `!=` | Bərabər deyil | `WHERE drink != 'Coffee'` |
| `<` | Kiçikdir | `WHERE price < 3` |
| `>` | Böyükdür | `WHERE price > 4` |
| `<=` | Kiçik ya bərabər | `WHERE price <= 3` |
| `>=` | Böyük ya bərabər | `WHERE price >= 3` |

---

## Task 4 — Nəticə və Xülasə

### Öyrənilənlər

**Anlayışlar:**

| Termin | İzah |
|--------|------|
| **Database** | Məlumatları strukturlu saxlayan sistem |
| **Table** | Cədvəl — məlumatlar burada saxlanılır |
| **Column (Sütun)** | Bir tip məlumat saxlayan şaquli bölmə |
| **Row (Sətir)** | Bir tam qeyd saxlayan üfüqi sətir |
| **SQL** | Verilənlər bazasına sorğu göndərmək üçün dil |
| **Query (Sorğu)** | Verilənlər bazasına verilən sual |

---

**SQL Açar Sözləri:**

| Açar söz | Funksiya | Nümunə |
|----------|---------|--------|
| `SELECT` | Hansı sütunları göstər | `SELECT drink, price` |
| `FROM` | Hansı cədvəldən al | `FROM Orders` |
| `WHERE` | Filtr şərti | `WHERE price < 3` |
| `ORDER BY` | Sırala | `ORDER BY price ASC` |
| `*` | Bütün sütunlar | `SELECT *` |
| `ASC` | Artan sıra | `ORDER BY price ASC` |
| `DESC` | Azalan sıra | `ORDER BY price DESC` |

---

### Tam SQL Sorğusu — Nümunə

```sql
-- Ən baha 5 sifariş — qəhvə istisna
SELECT drink, price, time
FROM Orders
WHERE drink != 'Coffee'
ORDER BY price DESC;
```

---

### Kibertəhlükəsizlikdə SQL-in Əhəmiyyəti

SQL bilikləri kibertəhlükəsizlikdə **çox vacibdir:**

**SQL Injection (SQLi)** — ən məşhur veb hücumlarından biri:
- Hücumçu giriş sahəsinə SQL kodu yerləşdirir
- Verilənlər bazasını manipulyasiya edir
- Misal: login formasına `' OR '1'='1` yazaraq şifrəsiz giriş
- OWASP Top 10-da hər zaman yer alır

**Nümunə:** Normal sorğu:
```sql
SELECT * FROM users WHERE username='admin' AND password='12345';
```

SQL Injection ilə:
```sql
SELECT * FROM users WHERE username='admin'-- ' AND password='anything';
```
`--` şərhə çevirir — şifrə yoxlanmır!

**Müdafiə üsulları:**
- Prepared Statements (Parametrik sorğular)
- Input validation (Giriş yoxlaması)
- WAF (Veb Tətbiq Firewallı)

---

### Məşhur Verilənlər Bazası Sistemləri

| Sistem | İstifadə sahəsi |
|--------|----------------|
| **MySQL** | Veb saytlar (WordPress, PHP) |
| **PostgreSQL** | Mürəkkəb tətbiqlər |
| **SQLite** | Mobil tətbiqlər, kiçik layihələr |
| **Microsoft SQL Server** | Korporativ Windows mühiti |
| **Oracle DB** | Böyük müəssisələr, banklar |
| **MariaDB** | MySQL-in açıq mənbəli versiyası |

---

*Mənbə: TryHackMe — "Database SQL Basics" Lab*
