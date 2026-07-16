# Virtualisation Basics — Lab İzahı

---

## 1. Virtualizasiya nədir?

**Virtualizasiya** — bir fiziki kompüterin içində bir neçə "virtual kompüter" yaratmaq texnologiyasıdır.

> Misal: Bir güclü server var. Virtualizasiya ilə onun üzərində eyni anda 10 ayrı kompüter işlədə bilərsən — hər biri sanki özünün ayrı kompüteri varmış kimi davranır.

---

## 2. Niyə lazımdır?

**Köhnə üsul (virtualizasiyasız):**
- Hər xidmət üçün ayrı fiziki server lazım idi
- Serverlərin çox hissəsi boş-boşuna dayanırdı (10-15% istifadə)
- Böyük xərc, böyük yer, böyük enerji

**Virtualizasiya ilə:**
- Bir fiziki server → bir neçə virtual server
- Resurslardan tam istifadə (80-90%)
- Az xərc, az yer, az enerji ✅

---

## 3. Əsas anlayışlar

### Hypervisor (Hipervizor)
- Virtual mühitləri yaradan və idarə edən proqram
- Fiziki resursları (CPU, RAM, disk) virtual maşınlara bölüşdürür
- İki növü var:

| Növ | Necə işləyir | Misal |
|-----|-------------|-------|
| **Type 1 (Bare-metal)** | Birbaşa hardware üzərində işləyir, OS lazım deyil | VMware ESXi, Microsoft Hyper-V |
| **Type 2 (Hosted)** | Əsas əməliyyat sistemi üzərində işləyir | VirtualBox, VMware Workstation |

---

### Virtual Machine — VM (Virtual Maşın)
- Fiziki kompüteri tam simulyasiya edən proqram mühiti
- Öz əməliyyat sistemi, öz CPU-su, öz RAM-ı var (hamısı virtual)
- Fiziki kompüterdən tamamilə təcrid edilmişdir (isolated)
- Bir VM çöksə, digərlərinə təsiri yoxdur

---

### Host və Guest

| Termin | Mənası |
|--------|--------|
| **Host** | Fiziki kompüter — virtual maşınları içində saxlayan |
| **Guest** | Virtual maşın — host-un içində işləyən |

> Misal: Windows kompüterin üzərində Linux VM işlədirsən → Windows = Host, Linux = Guest

---

## 4. Virtualizasiyanın üstünlükləri

- **Resurs səmərəliliyi:** Bir server bir neçə iş görür
- **Təcrid (Isolation):** VM-lər bir-birindən ayrıdır — biri viruslansa digəri təsirlənmir
- **Sürətli qurulum:** Yeni server lazımdır? Dəqiqələr içində yeni VM yarat
- **Snapshot (Anlıq görüntü):** VM-nin vəziyyətini qeyd et, xəta olsa geri qayıt
- **Portativlik:** VM-i bir serverdən digərinə köçür
- **Sınaq mühiti:** Proqramçılar canlı sistemə toxunmadan test edə bilər

---

## 5. Virtualizasiya növləri

### Server Virtualizasiyası
- Ən geniş yayılmış növ
- Bir fiziki serverdə bir neçə virtual server
- Misal: AWS, Azure, Google Cloud hamısı bu əsasda işləyir

### Desktop Virtualizasiyası (VDI)
- İstifadəçilər öz kompüterlərindən uzaq virtual desktop-a qoşulur
- Şirkətlər işçilərə virtual iş masası verir
- Misal: Citrix, VMware Horizon

### Network Virtualizasiyası
- Fiziki şəbəkə avadanlıqlarını virtual şəkildə idarə etmək
- Misal: Virtual LAN (VLAN), Software-Defined Networking (SDN)

### Storage Virtualizasiyası
- Bir neçə fiziki diski bir böyük virtual disk kimi göstərmək
- Misal: SAN (Storage Area Network)

### Tətbiq Virtualizasiyası
- Proqramı kompüterdə quraşdırmadan işlətmək
- Misal: Docker konteynerləri

---

## 6. Konteyner vs Virtual Maşın

| Xüsusiyyət | Virtual Maşın (VM) | Konteyner |
|-----------|-------------------|-----------|
| Öz OS-i | Var (tam OS) | Yoxdur (host OS paylaşır) |
| Ölçüsü | GB-larla | MB-larla |
| Başlanma vaxtı | Dəqiqələr | Saniyələr |
| Təcrid | Tam | Qismən |
| Misal | VMware, VirtualBox | Docker, Kubernetes |

> VM = Tam ayrı ev; Konteyner = Eyni evdə ayrı otaq

---

## 7. Snapshot (Anlıq görüntü)

- VM-nin müəyyən andakı vəziyyətini yadda saxlayır
- Faydası: Bir şey sınırsa, snapshot-a qayıt
- Misal: Sistem yeniləməsindən əvvəl snapshot al → yeniləmə problem çıxarsa geri qayıt

---

## 8. Live Migration (Canlı köçürmə)

- VM-i söndürmədən bir fiziki serverdən digərinə köçürmək
- İstifadəçi heç nə hiss etmir, xidmət kəsilmir
- Misal: Server texniki xidmətə gedəndə VM-lər avtomatik başqa serverə köçür

---

## 9. Cloud Computing ilə əlaqəsi

Bulud hesablama (Cloud) əsasən virtualizasiya üzərində qurulmuşdur:

```
Fiziki Serverlər
       ↓
  Hypervisor (Virtualizasiya)
       ↓
  Virtual Maşınlar
       ↓
  Cloud Xidmətləri (AWS, Azure, GCP)
       ↓
  İstifadəçilər
```

- **AWS EC2** — virtual maşın icarəsi
- **Google Cloud VMs** — virtual server xidməti
- **Microsoft Azure** — virtual infrastruktur

---

## 10. Təhlükəsizlik üstünlükləri

- **Sandbox mühiti:** Zərərli proqramı VM-də test et — host sisteminə zərər vermir
- **Təcrid:** Bir VM-ə hücum olunsa digərləri qorunur
- **Tez bərpa:** VM çöksə snapshot-dan saniyələrdə bərpa et
- **Mərkəzləşdirilmiş idarəetmə:** Bütün VM-lər bir yerdən idarə olunur

---

## 11. Real həyat nümunələri

- **Hosting şirkətləri:** Bir güclü serverdə yüzlərlə müştəriyə ayrı virtual server verir
- **Kibertəhlükəsizlik:** Zərərli faylları VM-də açıb təhlil edirlər
- **Proqram inkişafı:** Proqramçılar müxtəlif əməliyyat sistemlərini VM-də test edir
- **Oyun emulatorları:** PlayStation oyunlarını PC-də oynamaq — virtualizasiya prinsipi
- **Banklar:** Kritik sistemləri VM-lərdə idarə edir, backup asandır

---

## 12. Əsas terminlər lüğəti

| Termin | İzah |
|--------|------|
| **Hypervisor** | VM-ləri yaradan və idarə edən proqram |
| **VM (Virtual Machine)** | Virtual kompüter mühiti |
| **Host** | VM-ləri içində saxlayan fiziki kompüter |
| **Guest** | VM-in özü |
| **Snapshot** | VM-in anlıq vəziyyətinin surəti |
| **Live Migration** | VM-i söndürmədən köçürmək |
| **Sandbox** | Təcrid edilmiş test mühiti |
| **Bare-metal** | Birbaşa hardware üzərində işləmək |
| **Isolation** | VM-lərin bir-birindən ayrılması |
| **Provisioning** | Yeni VM qurmaq/hazırlamaq prosesi |

---

*Mənbə: Brilliant.org — "Virtualisation Basics" Premium Lab (30 dəqiqə)*
