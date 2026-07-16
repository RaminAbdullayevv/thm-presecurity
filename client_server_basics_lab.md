# Client-Server Basics — Lab İzahı

---

## 1. Client-Server Modeli nədir?

**Client-Server** — kompüter şəbəkələrinin əsas iş prinsipidir.

- **Client (Müştəri):** Xidmət *istəyən* tərəf
- **Server (Server):** Xidmət *göstərən* tərəf

> Sadə misal: Sən brauzerə sayt ünvanı yazırsan (client) → server həmin saytı sənə göndərir.

---

## 2. Necə işləyir?

```
Client  ──── İstək (Request) ────►  Server
Client  ◄─── Cavab  (Response) ───  Server
```

**Addımlar:**
1. Client serverə **request (sorğu)** göndərir
2. Server sorğunu qəbul edir və emal edir
3. Server clientə **response (cavab)** qaytarır
4. Client cavabı göstərir (məs. veb səhifə açılır)

---

## 3. Gündəlik həyatdan nümunələr

| Nümunə | Client | Server |
|--------|--------|--------|
| Veb sayt açmaq | Brauzerin (Chrome, Firefox) | Veb server (Google, YouTube) |
| Email göndərmək | Gmail tətbiqi | Gmail serveri |
| Onlayn oyun oynamaq | Oyun proqramı | Oyun serveri |
| Bankomat istifadəsi | Bankomat cihazı | Bankın mərkəzi serveri |
| Netflix izləmək | TV / telefon | Netflix serveri |

---

## 4. Əsas anlayışlar

### Request (Sorğu)
- Clientin serverə göndərdiyi məlumat
- "Mənə bu məlumatı ver" demək kimidir
- Misal: `google.com` ünvanını brauzerdə açmaq

### Response (Cavab)
- Serverin clientə qaytardığı məlumat
- Müsbət cavab: 200 OK — uğurlu
- Mənfi cavab: 404 Not Found — tapılmadı, 500 Server Error — server xətası

### HTTP / HTTPS
- Client ilə server arasında danışıq qaydaları (protokol)
- **HTTP** — şifrsiz
- **HTTPS** — şifrəli, təhlükəsiz (kilid işarəsi 🔒)

### IP Ünvanı
- Hər cihazın şəbəkədəki unikal ünvanı
- Misal: `192.168.1.1` (ev şəbəkəsi), `8.8.8.8` (Google DNS)

### Port
- Serverdə müxtəlif xidmətlər fərqli portlardan işləyir
- Misal: HTTP → Port 80, HTTPS → Port 443, Email → Port 25

### DNS (Domain Name System)
- Sayt adını IP ünvanına çevirir
- `google.com` → `142.250.74.46`
- İnternetin "telefon kitabçası" kimidir

---

## 5. Server növləri

| Server növü | Nə edir? | Misal |
|-------------|----------|-------|
| **Web Server** | Veb səhifələri göndərir | Apache, Nginx |
| **File Server** | Faylları saxlayır və paylaşır | Google Drive backend |
| **Database Server** | Məlumat bazasını idarə edir | MySQL, PostgreSQL |
| **Mail Server** | Email göndərir / qəbul edir | Gmail serveri |
| **Game Server** | Onlayn oyunları idarə edir | Minecraft, Fortnite serverləri |
| **DNS Server** | Domen adlarını IP-yə çevirir | Google DNS (8.8.8.8) |

---

## 6. Client növləri

- **Thin Client (Nazik müştəri):** Özündə az güc var, işi serverə həvalə edir. Misal: Chromebook, brauzerlər
- **Thick/Fat Client (Qalın müştəri):** Özündə çox iş görür, serverə az ehtiyac duyur. Misal: Desktop oyunlar, ofis proqramları

---

## 7. Client-Server vs Peer-to-Peer (P2P)

| Xüsusiyyət | Client-Server | Peer-to-Peer (P2P) |
|-----------|--------------|-------------------|
| Mərkəzi server | Var | Yoxdur |
| Nəzarət | Asandır | Çətindir |
| Etibarlılıq | Yüksək | Orta |
| Misal | Veb saytlar | BitTorrent, Blockchain |

---

## 8. HTTP Status Kodları (Ən vacibləri)

| Kod | Mənası | Nə deməkdir? |
|-----|--------|--------------|
| **200** | OK | Hər şey yaxşıdır ✅ |
| **301** | Moved Permanently | Səhifə başqa ünvana köçürülüb |
| **400** | Bad Request | Sorğu yanlışdır |
| **401** | Unauthorized | Giriş icazəsi yoxdur |
| **403** | Forbidden | Qadağandır |
| **404** | Not Found | Səhifə tapılmadı ❌ |
| **500** | Internal Server Error | Server xətası 🔴 |

---

## 9. Şəbəkə anlayışları

### Bandwidth (Bant genişliyi)
- Şəbəkənin eyni anda nə qədər məlumat ötürə biləcəyi
- Nə qədər böyük olsa, internet o qədər sürətli

### Latency (Gecikmə)
- Sorğunun serverə çatıb geri qayıtması üçün keçən vaxt
- Onlayn oyunlarda "ping" deyilir — nə qədər aşağı olsa, o qədər yaxşı

### Firewall (Təhlükəsizlik divarı)
- Şəbəkəyə icazəsiz girişi bloklayan sistem
- Evdə: router-in təhlükəsizlik funksiyası
- Şirkətdə: xüsusi cihaz və ya proqram

### Load Balancer (Yük Balanslaşdırıcı)
- Çox istifadəçi olduqda sorğuları bir neçə serverə bölür
- Misal: YouTube-a milyonlar baxanda bir server yox, minlərlə server işləyir

---

## 10. Real həyat ssenarisi — Veb sayt açmaq

Brauzerə `www.example.com` yazanda nə baş verir:

1. **DNS Sorğusu:** Brauzerin `example.com`-un IP ünvanını soruşur
2. **Bağlantı:** Brauzerin server ilə TCP bağlantısı qurur
3. **Request:** HTTP GET sorğusu göndərilir
4. **Emal:** Server faylı tapır
5. **Response:** Server HTML faylı clientə göndərir
6. **Render:** Brauzerin HTML-i oxuyur, səhifə ekranda görünür

---

## 11. Təhlükəsizlik əsasları

- **SSL/TLS:** HTTPS-in arxasındakı şifrəlmə texnologiyası
- **Authentication:** İstifadəçinin kim olduğunu yoxlamaq (login/şifrə)
- **Authorization:** İstifadəçinin nəyə icazəsi olduğunu müəyyən etmək
- **DDoS hücumu:** Serverə eyni anda milyonlarla saxta sorğu göndərmək — serveri çökdürür

---

*Mənbə: Brilliant.org — "Client-Server Basics" Premium Lab (60 dəqiqə)*
