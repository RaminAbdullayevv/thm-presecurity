# TryHackMe — Extending Your Network | Writeup (Azərbaycanca)

> **Platforma:** TryHackMe  
> **Room:** Extending Your Network  
> **Çətinlik:** Easy (Info)  
> **Müddət:** 20 dəqiqə  
> **Link:** https://tryhackme.com/room/extendingyournetwork  

---

## Ümumi Baxış

Bu otaqda şəbəkəni internetə necə açdığımızı, onu necə qoruduğumuzu, və şəbəkə cihazlarının necə işlədiyini öyrənirik. Mövzular: **Port Yönləndirmə**, **Firewall**, **VPN** və **LAN cihazları (Router, Switch)**.

---

## Task 1 — Port Yönləndirməyə Giriş (Introduction to Port Forwarding)

### İzahat

**Port Forwarding (Port Yönləndirmə)** — daxili şəbəkədəki xidməti internetə açmaq üçün istifadə olunan bir texnologiyadır.

**Problem:** Daxili şəbəkədəki (intranet) server yalnız eyni şəbəkədəki cihazlar tərəfindən görünür. İnternet istifadəçiləri onu tapa bilmir.

```
   PORTSUZ (Yalnız daxili şəbəkə):
   ┌──────────────────────────────┐
   │  Şəbəkə #1 (192.168.1.x)    │
   │                              │
   │  [PC1] ─────[Server:80]      │
   │  [PC2] ─────┘                │
   │                              │
   │  ⚠ İnternet buraya çata     │
   │    bilmir!                   │
   └──────────────────────────────┘

   PORT FORWARDING İLƏ:
   ┌──────────────────────────────┐
   │  Şəbəkə #1 (192.168.1.10)   │
   │             ↕                │
   │         [Router]             │
   │     82.62.51.70:80           │
   │             ↕                │
   └───────── İNTERNET ───────────┘
         ↕
   [Şəbəkə #2 — xarici istifadəçi]
   "82.62.51.70" ünvanına daxil olur
```

**Necə işləyir?**

Router xarici IP ünvanına (məs. `82.62.51.70`) gələn trafiki daxili serverə (məs. `192.168.1.10:80`) yönləndirir. Beləliklə, xarici şəbəkədən gələn istifadəçi `82.62.51.70`-ə qoşularaq daxili veb serveri görür.

**Port Forwarding ilə Firewall arasındakı fərq:**
- **Port Forwarding** — portu açır (trafiki yönləndirir)
- **Firewall** — həmin portdan trafiki keçirip-keçirməməyi qərara alır

Port Forwarding **routerda** konfiqurasiya edilir.

---

### Sual və Cavab

> **Q: What is the name of the device that is used to configure port forwarding?**  
> ✅ `Router`  
> *İzah: Port yönləndirmə şəbəkənin routerında tənzimlənir.*

---

## Task 2 — Firewalllar 101 (Firewalls 101)

### İzahat

**Firewall (Güvənlik Duvarı)** — şəbəkəyə daxil olan və çıxan trafikin hansısının icazəli, hansısının qadağan olduğuna qərar verən bir cihaz (və ya proqram) təminatıdır. Firewall-u şəbəkənin sərhəd mühafizəçisi kimi düşün.

**Administrator firewall-a bu meyarları əsasında qaydalar qura bilər:**

| Meyar | Nümunə |
|-------|--------|
| **Trafikin mənbəyi** | Yalnız 192.168.1.0/24 şəbəkəsindən gələn trafiki qəbul et |
| **Trafikin hədəfi** | Yalnız 10.0.0.5 ünvanına gedən trafiki qəbul et |
| **Port nömrəsi** | Yalnız 80 (HTTP) portuna gələn trafiki qəbul et |
| **Protokol növü** | Yalnız TCP trafikini qəbul et, UDP-ni blok et |

Firewall bu suallara cavab vermək üçün **paket müayinəsi** (packet inspection) aparır.

---

### Firewall Növləri

#### 1. Stateful (Vəziyyətli) Firewall

Bütün əlaqəni bir bütün olaraq qiymətləndirir. Yalnız ayrı-ayrı paketlərə deyil, **əlaqənin tam davranışına** baxır.

- Daha güclü qoruma təmin edir
- Əlaqənin bütün kontekstini yaddaşda saxlayır
- Daha çox resurs tələb edir (bəzən yavaşlaya bilər)

> **Nümunə:** "Bu TCP əlaqəsi düzgün üç-addımlı əl sıxışma (handshake) ilə başladıldımı? Xeyr → blok et."

#### 2. Stateless (Vəziyyətsiz) Firewall

Hər paketi **ayrıca, müstəqil** şəkildə yoxlayır. Əvvəlki paketlərə baxmır.

- Daha sürətli işləyir (az resurs tələb edir)
- Qoruma daha azdır — əlaqənin kontekstini bilmir
- Böyük həcmli trafiki idarə etmək üçün uyğundur

> **Nümunə:** "Bu paketin hədəf portu 22-dirmi (SSH)? Bəli → blok et." — Paketin kim tərəfindən, hansı əlaqənin bir parçası kimi gəldiyinə baxmır.

**OSI modelindəki mövqeyi:**

Firewalllar Layer 3 (Şəbəkə qatı) və Layer 4 (Nəqliyyat qatı) üzərindən işləyir:
- Layer 3 → IP ünvanlarına görə filtrləmə
- Layer 4 → Port nömrəsi və protokola (TCP/UDP) görə filtrləmə

---

### Suallar və Cavablar

> **Q: What layers of the OSI model do firewalls operate at?**  
> ✅ `Layer 3,Layer 4`  
> *İzah: Firewall Layer 3-də IP ünvanlarını, Layer 4-də port nömrələrini və protokolları yoxlayır.*

> **Q: What category of firewall inspects the entire connection?**  
> ✅ `Stateful`  
> *İzah: Stateful firewall bütün əlaqənin davranışını izləyir.*

> **Q: What category of firewall inspects individual packets?**  
> ✅ `Stateless`  
> *İzah: Stateless firewall hər paketi ayrıca, müstəqil şəkildə yoxlayır.*

---

## Task 3 — Praktiki: Firewall (Practical — Firewall)

### İzahat

Bu task interaktiv bir simulyasiyadır. Cihazın çox yüklənməsinin qarşısını almaq üçün firewall qaydalarını düzgün konfiqurasiya etmək lazımdır.

**Ssenari:**

`198.57.100.34` IP ünvanlı cihaz `203.0.110.1` ünvanlı cihazı paket bombardımanına tutaraq çökdürmək istəyir (DoS hücumu). Bunu aşkar edib firewallda blok etmək lazımdır.

**Həll addımları:**

1. "View Site" düyməsinə bas — lab açılır
2. Şəbəkə loguna bax — hansı IP ünvanının anormal sayda paket göndərdiyini müəyyənlə
3. `198.57.100.34` ünvanlı cihazın bütün paketlərini bloklayan qayda əlavə et
4. Firewall qaydası aktiv olduqdan sonra paketlər dayanır → flag ekrana çıxır

**Bloklama məntiqi:**

```
Mənbə IP:     198.57.100.34
Hərəkət:      BLOCK (Blok et)
Protokol:     ALL
Port:         ALL
```

---

### Sual və Cavab

> **Q: What is the flag?**  
> ✅ `THM{FIREWALLS_RULE}`

---

## Task 4 — VPN Əsasları (VPN Basics)

### İzahat

**VPN (Virtual Private Network — Virtual Özəl Şəbəkə)** — ayrı-ayrı şəbəkələrdəki cihazların internet üzərindən **şifrəli tunel** vasitəsilə bir-biri ilə sanki eyni şəbəkədəymişlər kimi ünsiyyət qurmasına imkan verən texnologiyadır.

**Real həyat nümunəsi:**

Bir şirkətin İstanbul və Bakı ofisləri var. Hər ofis öz yerli şəbəkəsinə (LAN) malikdir. VPN bu iki ofisi bir virtual şəbəkədə birləşdirir:

```
┌─────────────────┐          ┌─────────────────┐
│   İSTANBUL      │          │      BAKI        │
│   Ofisi         │          │      Ofisi       │
│                 │          │                  │
│  [PC1][PC2]    │══════════│   [PC3][PC4]    │
│   Şəbəkə #1    │  İNTER-  │    Şəbəkə #2    │
│                 │  NET     │                  │
└─────────────────┘  (VPN   └─────────────────┘
                     Tunel)

         ↕ Hər iki ofis Şəbəkə #3-ü əmələ gətirir
         (yalnız VPN istifadəçiləri daxil ola bilir)
```

**VPN-in üstünlükləri:**

| Üstünlük | İzahı |
|----------|-------|
| **Şifrəli əlaqə** | Tunel daxilindəki məlumat şifrəlidir — üçüncü tərəf oxuya bilmir |
| **Anonimlik** | İnternet provayderə real fəaliyyət görünmür |
| **Uzaqdan giriş** | Evdən işyeri şəbəkəsinə sanki ofisdəymişsən kimi qoşulursan |
| **Coğrafi məhdudiyyətdən yayınma** | VPN serveri başqa ölkədədirsə, o ölkənin kontentinə çatmaq olar |

> TryHackMe özü də VPN-dən istifadə edir — həssas laboratoriya maşınları birbaşa internetə bağlı deyil, yalnız VPN tuneli vasitəsilə əlçatandır. Bu həm istifadəçiləri, həm platformanı qoruyur.

---

### VPN Texnologiyaları

| Texnologiya | Tam adı | Xüsusiyyəti |
|-------------|---------|-------------|
| **PPP** | Point-to-Point Protocol | Yalnız **autentifikasiya** və **şifrələmə** təmin edir. Öz başına routinq edə bilmir — PPTP tərəfindən istifadə olunur |
| **PPTP** | Point-to-Point Tunneling Protocol | PPP-ni internet üzərindən tunelləşdirir. Quraşdırması asandır amma **zəif şifrələmə** — köhnəlmiş sayılır |
| **IPSec** | Internet Protocol Security | Mövcud **IP çərçivəsindən** istifadə edərək məlumatı şifrələyir. Konfiqurasiya etmək çətindir, lakin güclü şifrələmə təmin edir |

---

### Suallar və Cavablar

> **Q: What VPN technology only encrypts & provides the authentication of data?**  
> ✅ `PPP`  
> *İzah: PPP yalnız şifrələmə və autentifikasiya təmin edir; özü tunel yaratmır.*

> **Q: What VPN technology uses the IP framework?**  
> ✅ `IPSec`  
> *İzah: IPSec (Internet Protocol Security) mövcud IP infrastrukturundan istifadə edərək şifrələmə həyata keçirir.*

---

## Task 5 — LAN Şəbəkə Cihazları (LAN Networking Devices)

### İzahat

Bu taskda iki əsas şəbəkə cihazını — **Router** və **Switch** — öyrənirik.

---

### Router (Marşrutlaşdırıcı)

Router-in vəzifəsi fərqli şəbəkələri bir-birinə bağlamaq və məlumatı onlar arasında ötürməkdir. Bu prosesə **routing (marşrutlaşdırma)** deyilir.

**OSI modelindəki mövqeyi:** Layer 3 (Şəbəkə qatı)

**Router ən yaxşı yolu hansı meyarlara görə seçir?**

| Meyar | İzahı |
|-------|-------|
| **Ən qısa yol** | Neçə router-dən keçmək lazımdır? |
| **Etibarlılıq** | Bu yolda əvvəlcədən paket itkisi olubmu? |
| **Sürət** | Mis kabel mi, fiber optik mi? |

**Router-in digər funksiyaları:**
- Firewall qaydalara konfiqurasiya etmək
- Port forwarding tənzimləmək
- DHCP — daxili cihazlara IP ünvanı vermək
- NAT — daxili IP ünvanlarını xarici IP-yə çevirmək

```
     [Kompüter A]          [Kompüter B]
          │                     │
     [Router 1] ────────── [Router 2]
          │                     │
     [Router 3] ─────────────────┘
     
  Məlumat ən optimal yolu seçir:
  A → Router1 → Router2 → B (ən qısa)
  Əgər Router2 çatışmazsa:
  A → Router1 → Router3 → Router2 → B
```

---

### Switch (Açar)

Switch — eyni şəbəkə daxilindəki çoxlu cihazı bir-birinə bağlayan avadanlıqdır. 3-dən 63-ə qədər cihazı Ethernet kabelləri vasitəsilə birləşdirə bilir.

Switch-lər OSI modelinin **həm Layer 2, həm də Layer 3**-də işləyə bilər — amma bu iki növ bir-birindən ayrıdır:

#### Layer 2 Switch

- Yalnız eyni şəbəkə daxilindəki cihazları birləşdirir
- **MAC ünvanlarına** görə freymləri (frames) ötürür
- Routinq edə bilmir (Layer 3 funksiyası yoxdur)

```
   Layer 2 Switch:
   [PC1]─┐
   [PC2]─┤─ [Switch] ─ hər cihazın MAC ünvanına göre
   [PC3]─┘              düzgün cihaza çatdırır
```

#### Layer 3 Switch

- Layer 2 funksiyalarını yerinə yetirməklə yanaşı, **əsas routinq** funksiyasını da həyata keçirə bilir
- Həm MAC, həm IP ünvanlarına baxır
- Böyük müəssisə şəbəkələrində istifadə olunur

---

### VLAN (Virtual Local Area Network)

VLAN — switch-ə qoşulmuş cihazları virtual olaraq **ayrı-ayrı şəbəkələrə** bölmək imkanı verir. Fiziki olaraq eyni switch-ə qoşulsalar da, VLAN-lar arası ünsiyyət yalnız müəyyən qaydalar daxilində mümkündür.

```
         [Layer 3 Switch]
               │
    ┌──────────┴──────────┐
    │                     │
[VLAN 10]            [VLAN 20]
Satış şöbəsi         Mühasibat
[PC1][PC2]           [PC3][PC4]
    │                     │
    └──────── İnternet ───┘
    ↑
    Hər iki VLAN internetə çıxa bilir,
    amma bir-biri ilə ƏLAQƏ QURA BİLMİR!
```

Bu şəbəkə seqmentasiyası **təhlükəsizlik** baxımından çox önəmlidir: həssas maliyyə məlumatları yalnız mühasibat VLAN-ında qalır.

---

### Suallar və Cavablar

> **Q: What is the verb for the action that a router does?**  
> ✅ `Routing`  
> *İzah: Router-in etdiyi əməl "routing" — şəbəkələr arası məlumatı yönləndirməkdir.*

> **Q: What are the two different layers of switches? Separate these by a comma i.e.: LayerX,LayerY**  
> ✅ `Layer2,Layer3`  
> *İzah: Switch-lər OSI modelinin Layer 2 (Data Link) və Layer 3 (Network) qatlarında işləyə bilir.*

---

## Task 6 — Praktiki: Şəbəkə Simulyatoru (Practical — Network Simulator)

### İzahat

Bu task interaktiv şəbəkə simulyatorudur. Paketin bir kompüterdən digərinə necə getdiyini addım-addım izləmək mümkündür.

**Tapşırıq:** Computer1-dən Computer3-ə **TCP paketi** göndər, flag-i al.

**Simulyatorda baş verənlər (Network Log):**

```
1. HANDSHAKE: Computer1 → Computer2 arasında TCP Handshake başladı
2. HANDSHAKE: Computer1 → Computer2-yə SYN paketi göndərildi
3. ARP REQUEST: Computer2-nin MAC ünvanı soruşuldu
4. ARP RESPONSE: Computer2 cavab verdi
5. HANDSHAKE: Computer2 → SYN/ACK göndərdi
6. HANDSHAKE: Computer1 → ACK göndərdi (Handshake tamamlandı)
7. TCP: Computer1 → Computer2-yə TCP paketi göndərildi
8. TCP: Computer2 → ACK ilə təsdiqlədi
9. HANDSHAKE: Computer1 → Computer3 arasında TCP Handshake başladı
   (Computer3 fərqli şəbəkədədir → Router-dən keçmək lazımdır)
10. ROUTING: Computer1 bildirir ki, Computer3 eyni şəbəkədə deyil
    → Paketi Gateway (Router)-ə göndər
... (router üzərindən keçib Computer3-ə çatır)
```

**Niyə 5 HANDSHAKE girişi var?**

Tam TCP əlaqəsi üçün bütün addımlar — SYN, SYN/ACK, ACK, DATA göndərməsi, əlaqənin bağlanması — şəbəkə log-unda qeydə alınır. Computer1→Computer2 və Computer1→Computer3 arasında ayrı-ayrı handshake-lər baş verir.

**Addımlar:**

1. Simulyatoru aç ("View Site")
2. "From: Computer1", "To: Computer3", "Packet Type: TCP", "Data: Hello" seç
3. "Send Packet" düyməsinə bas
4. Şəbəkə log-unu izlə — paketin hər addımını görürsən
5. Ötürmə tamamlandıqda flag ekrana çıxır

---

### Suallar və Cavablar

> **Q: What is the flag from the network simulator?**  
> ✅ `THM{YOU'VE_GOT_DATA}`

> **Q: How many HANDSHAKE entries are there in the Network Log?**  
> ✅ `5`  
> *İzah: Computer1→Computer2 və Computer1→Computer3 arası TCP əlaqə prosesinin bütün addımları birlikdə 5 HANDSHAKE girişi əmələ gətirir.*

---

## Bütün Cavabların Xülasəsi

| Task | Sual | Cavab |
|------|------|-------|
| Task 1 | Port forwarding konfiqurasiya üçün hansı cihaz? | `Router` |
| Task 2 | Firewalllar OSI-nin hansı qatlarında işləyir? | `Layer 3,Layer 4` |
| Task 2 | Bütün əlaqəni yoxlayan firewall kateqoriyası? | `Stateful` |
| Task 2 | Ayrı-ayrı paketləri yoxlayan firewall kateqoriyası? | `Stateless` |
| Task 3 | Firewall praktiki flag-i? | `THM{FIREWALLS_RULE}` |
| Task 4 | Yalnız şifrələmə və autentifikasiya verən VPN texnologiyası? | `PPP` |
| Task 4 | IP çərçivəsindən istifadə edən VPN texnologiyası? | `IPSec` |
| Task 5 | Router-in etdiyi əməlin adı? | `Routing` |
| Task 5 | Switch-lərin işlədiyi iki fərqli qat? | `Layer2,Layer3` |
| Task 6 | Şəbəkə simulyatoru flag-i? | `THM{YOU'VE_GOT_DATA}` |
| Task 6 | Network Log-da neçə HANDSHAKE girişi var? | `5` |

---

## Bonus: Bu Otaqda Öyrənilənlərin Xülasəsi

```
PORT FORWARDING
  → Router üzərindən xarici istifadəçilərə daxili xidmətləri açır
  → Konfiqurasiya: Router

FIREWALL
  → Trafiki filtrləyir: IP, Port, Protokol meyarlarına görə
  → Stateful → bütün əlaqəni izləyir (daha güclü)
  → Stateless → hər paketi ayrıca yoxlayır (daha sürətli)
  → Layer 3 + Layer 4-də işləyir

VPN
  → Şifrəli tunel vasitəsilə ayrı şəbəkələri birləşdirir
  → PPP → yalnız şifrələmə + autentifikasiya
  → PPTP → PPP-ni tunelləşdirir (köhnə, zəif)
  → IPSec → IP infrastrukturundan istifadə, güclü şifrələmə

ROUTER
  → Şəbəkələr arasında məlumat ötürür (Routing)
  → Layer 3-də işləyir
  → Ən optimal yolu taparaq paketi hədəfə çatdırır

SWITCH
  → Eyni şəbəkə daxilindəki cihazları birləşdirir
  → Layer 2 → MAC ünvanına görə freym ötürür
  → Layer 3 → həm freym, həm paket ötürür (əsas routing)
  → VLAN → switch-dəki cihazları virtual olaraq ayırır
```
