# Cloud Computing Fundamentals — Lab İzahı

---

## 1. Bulud Hesablama (Cloud Computing) nədir?

**Cloud Computing** — kompüter resurslarını (server, yaddaş, proqram, verilənlər bazası) internet üzərindən icarəyə götürmək modelidir.

> Misal: Öz serverini almaq əvəzinə, AWS-dən aylıq icarəyə götürürsən — istifadə etdiyin qədər ödəyirsən.

**Sadə izah:**
- Əvvəl: Şirkət öz serverini alır, qurur, saxlayır
- İndi: Şirkət interneti olan hər yerdən bulud resurslarından istifadə edir

---

## 2. Bulud Hesablamanın Üstünlükləri

- **Sürət:** Dəqiqələr içində yeni server, yaddaş, xidmət əlavə et
- **Miqyaslanma (Scalability):** İstifadəçi artanda resursları artır, azalanda azalt
- **Qənaət:** Fiziki server almaq, quraşdırmaq, saxlamaq xərci yoxdur
- **Etibarlılıq:** Məlumatlar bir neçə mərkəzdə saxlanılır — bir yer çöksə digəri işləyir
- **Əlçatımlılıq:** İnternetin olduğu hər yerdən iş görmək mümkündür
- **Avtomatik yeniləmə:** Proqram yeniləmələrini provayderlar özü idarə edir

---

## 3. Bulud Xidmət Modelləri

### IaaS — Infrastructure as a Service (Xidmət kimi infrastruktur)
- Fiziki avadanlıq əvəzinə virtual avadanlıq icarəyə verilir
- Server, şəbəkə, yaddaş — hamısı virtual
- İstifadəçi əməliyyat sistemi və proqramları özü qurur
- **Misal:** AWS EC2, Google Compute Engine, Microsoft Azure VMs
- **Kimin üçün:** IT şirkətləri, sistem administratorları

---

### PaaS — Platform as a Service (Xidmət kimi platforma)
- İnfrastruktur + əməliyyat sistemi artıq hazırdır
- Proqramçı yalnız kod yazmaqla məşğul olur
- Server idarəsi, yeniləmə — provayderın işidir
- **Misal:** Google App Engine, Heroku, AWS Elastic Beanstalk
- **Kimin üçün:** Proqram təminatı işləyənlər

---

### SaaS — Software as a Service (Xidmət kimi proqram)
- Hazır proqram internet üzərindən istifadəyə verilir
- Quraşdırma yoxdur, sadəcə brauzerdən açılır
- **Misal:** Gmail, Google Docs, Microsoft 365, Zoom, Slack, Dropbox
- **Kimin üçün:** Son istifadəçilər, hər kəs

---

### Müqayisə cədvəli

```
                    Sən idarə edirsən:
                IaaS        PaaS        SaaS
Tətbiq           ✅          ✅          ❌
Data             ✅          ✅          ❌
Runtime          ✅          ❌          ❌
Middleware       ✅          ❌          ❌
OS               ✅          ❌          ❌
Virtualizasiya   ❌          ❌          ❌
Server           ❌          ❌          ❌
Yaddaş           ❌          ❌          ❌
Şəbəkə           ❌          ❌          ❌
```

> ✅ = Sən idarə edirsən | ❌ = Provayderın işidir

---

## 4. Bulud Yerləşdirmə Modelləri

### Public Cloud (İctimai Bulud)
- Resurslar internet üzərindən hamıya açıqdır
- Provayderə məxsusdur, idarə edilir
- **Misal:** AWS, Google Cloud, Microsoft Azure
- **Üstünlük:** Ucuz, asan, sürətli
- **Çatışmazlıq:** Məlumatlar üçüncü tərəfdə saxlanılır

---

### Private Cloud (Şəxsi Bulud)
- Yalnız bir şirkət üçün olan ayrıca bulud infrastrukturu
- Şirkətin özündə və ya xüsusi mərkəzdə saxlanılır
- **Misal:** Bankların öz bulud sistemi, hərbi sistemlər
- **Üstünlük:** Tam nəzarət, yüksək təhlükəsizlik
- **Çatışmazlıq:** Baha, mürəkkəb

---

### Hybrid Cloud (Hibrid Bulud)
- Public + Private bulud birlikdə istifadə olunur
- Həssas məlumatlar private-də, digər xidmətlər public-də
- **Misal:** Bank müştəri məlumatlarını öz serverində saxlayır, veb saytı AWS-də işlədir
- **Üstünlük:** Çeviklik + təhlükəsizlik

---

### Multi-Cloud (Çox Bulud)
- Bir neçə bulud provayderindən eyni anda istifadə
- **Misal:** Həm AWS, həm də Google Cloud istifadə etmək
- **Üstünlük:** Bir provayderə bağlı qalmamaq

---

## 5. Əsas Bulud Anlayışları

### Scalability (Miqyaslanma)
- Tələbata görə resursları artırmaq və ya azaltmaq
- **Vertical Scaling (Şaquli):** Mövcud serverə daha çox CPU/RAM əlavə etmək
- **Horizontal Scaling (Üfüqi):** Daha çox server əlavə etmək

---

### Elasticity (Elastiklik)
- Miqyaslanmanın avtomatik baş verməsi
- Misal: Məhsulun Black Friday-da trafikə görə avtomatik yeni serverlər qoşulur, traffic sakinləşəndə azalır

---

### Pay-as-you-go (İstifadə etdiyin qədər ödə)
- Aylıq sabit ödəniş yox
- Nə qədər CPU, RAM, trafik istifadə etdinisə, o qədər ödəyirsən
- Misal: Taksi kimidir — nə qədər getdinisə, o qədər ödəyirsən

---

### High Availability (Yüksək Əlçatımlılıq)
- Xidmətin daim işlək olması (99.9% uptime)
- Məlumatlar bir neçə mərkəzdə kopyalanır
- Bir mərkəz çöksə, digəri davam edir

---

### Data Center (Məlumat Mərkəzi)
- Serverlərin fiziki olaraq saxlandığı nəhəng binalar
- İçərisində minlərlə server, soyutma sistemi, generator var
- AWS-nin dünyanın hər yerində onlarca məlumat mərkəzi var

---

### Region və Availability Zone

| Termin | İzah |
|--------|------|
| **Region** | Coğrafi məkan (məs. Avropa, ABŞ-şərq) |
| **Availability Zone (AZ)** | Region daxilindəki ayrı məlumat mərkəzi |

> Misal: AWS Europe (Frankfurt) regionu → 3 ayrı AZ var → biri çöksə digərlər işləyir

---

## 6. Əsas Bulud Xidmətləri

| Xidmət kateqoriyası | Nə edir? | AWS nümunəsi |
|--------------------|---------|-------------|
| **Compute (Hesablama)** | Virtual serverlər | EC2 |
| **Storage (Yaddaş)** | Fayl və obyekt saxlama | S3 |
| **Database (Verilənlər bazası)** | İdarəolunan DB xidməti | RDS, DynamoDB |
| **Networking (Şəbəkə)** | Virtual şəbəkə, DNS | VPC, Route 53 |
| **Security (Təhlükəsizlik)** | Giriş idarəsi, şifrələmə | IAM, KMS |
| **AI/ML** | Süni intellekt xidmətləri | SageMaker |
| **Serverless** | Kod işlədmək, server idarəsi yoxdur | Lambda |

---

## 7. Serverless Computing

- Server qurmaq, idarə etmək yoxdur — yalnız kod yazırsan
- Kod işləyəndə ödəyirsən, işləməyəndə heç nə ödəmirsən
- Avtomatik miqyaslanır
- **Misal:** AWS Lambda, Google Cloud Functions, Azure Functions

---

## 8. Böyük Bulud Provayderləri

| Provayderler | Qısa adı | Payar |
|------------|---------|------|
| Amazon Web Services | AWS | ~32% |
| Microsoft Azure | Azure | ~23% |
| Google Cloud Platform | GCP | ~12% |
| Alibaba Cloud | — | ~4% |

---

## 9. Bulud Təhlükəsizliyi

- **Shared Responsibility Model (Paylaşılmış Məsuliyyət):**
  - Provayderın məsuliyyəti: Fiziki infrastruktur, şəbəkə, hardware
  - Müştərinin məsuliyyəti: Məlumatlar, giriş nəzarəti, proqramların təhlükəsizliyi

- **Encryption (Şifrələmə):** Məlumatlar ötürülən və saxlanılan zaman şifrələnir
- **IAM (Identity and Access Management):** Kim nəyə daxil ola bilər — nəzarət sistemi
- **Compliance:** GDPR, ISO 27001 kimi standartlara uyğunluq

---

## 10. Real həyat nümunələri

| Şirkət | Bulud istifadəsi |
|--------|----------------|
| **Netflix** | AWS-də işləyir — 200+ milyon istifadəçiyə video ötürür |
| **Spotify** | Google Cloud-da musiqi axını |
| **Airbnb** | AWS-də bütün platformasını idarə edir |
| **NASA** | AWS-də kosmik məlumatları saxlayır |
| **Zoom** | AWS + Oracle Cloud üzərindən video konferans |

---

## 11. Əsas terminlər lüğəti

| Termin | İzah |
|--------|------|
| **Cloud** | İnternet üzərindəki uzaq serverlər toplusu |
| **IaaS** | Virtual infrastruktur icarəsi |
| **PaaS** | Hazır platforma üzərində proqram yazmaq |
| **SaaS** | Hazır proqramı internet üzərindən istifadə |
| **Scalability** | Resursları tələbata görə artırmaq/azaltmaq |
| **Elasticity** | Avtomatik miqyaslanma |
| **Uptime** | Xidmətin işlək olduğu vaxt faizi |
| **Data Center** | Serverlərin saxlandığı nəhəng bina |
| **Region** | Coğrafi bulud məkani |
| **Serverless** | Server idarəsiz kod işlətmək |
| **CDN** | Məzmunu istifadəçiyə yaxın serverlərdən çatdırmaq |

---

*Mənbə: Brilliant.org — "Cloud Computing Fundamentals" Premium Lab (30 dəqiqə)*
