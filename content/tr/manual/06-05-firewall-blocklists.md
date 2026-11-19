---
title: "Güvenlik Duvarı > Engelleme Listeleri"
seo_title: "06.5 Güvenlik Duvarı > Engelleme Listeleri (Firewall > Blocklists) — Stratum FW Kullanım Kılavuzu"
description: "DNS engelleme listeleri (reklam, izleyici, zararlı yazılım, oltalama) ve IP engelleme listeleri (botnet, tarayıcılar, saldırganlar): liste seçimi, güncellemeler, izin/engelleme kayıtları, sorgulama ve Blocked raporu."
weight: 605
chapter_num: "06.5"
url: "/tr/manual/firewall-blocklists/"
aliases:
  - "/tr/manual/06-05-firewall-blocklists/"
translationKey: "manual-06-05-firewall-blocklists"
---

Engelleme listeleri (blocklists), tehdit istihbaratı projelerinin yayınladığı güncel listeleri kullanarak bilinen zararlı hedefleri ve saldırı kaynaklarını durdurur. Stratum FW **Firewall > Blocklists** altında iki ayrı sekmede iki tür liste sunar:

| Sekme | Neyi engeller | Nasıl çalışır | Katalog |
| :--- | :--- | :--- | :--- |
| **DNSBL** | Alan adları: reklamlar, izleyiciler, zararlı yazılımlar, oltalama (phishing), kripto madenciliği ... | Güvenlik duvarının DNS çözümleyicisi engellenen alan adı ve tüm alt alan adları için *mevcut değil* (`NXDOMAIN`) yanıtı döner. | 93 liste |
| **IP** | IP adresleri: botnet komuta sunucuları, port tarayıcılar, kaba kuvvet saldırganları, spam kaynakları, ele geçirilmiş sunucular ... | Paket filtresi (`pf`) listedeki adreslere giden ve bu adreslerden gelen trafiği doğrudan düşürür. | 63 liste |

Her iki koruma da ağlarınızdaki tüm cihazlar - telefonlar, televizyonlar, IoT cihazları - için uç cihazlara hiçbir yazılım kurmadan çalışır.

Üçüncü sekme olan **Blocked**, her iki katmanın neyi durdurduğunu gösteren ayrıntılı rapordur (yalnızca yöneticilere açıktır).

## DNS engelleme listeleri (DNSBL)

> **Not:** DNS engelleme listeleri **Unbound** içinde çalışır. Önce **Services > DNS Resolver** altından DNS çözümleyici olarak **Unbound** seçip uygulayın; o zamana kadar bu sekmedeki anahtar açılamaz.

![Firewall > Blocklists > DNSBL](/images/manual/bl-dnsbl.png)

### Etkinleştirme ve liste seçimi

1. **Block the domains in the selected lists** anahtarını açın.
2. Listeleri seçin. Listeler kategorilere göre gruplanmıştır (EasyList ve bölgesel reklam listeleri, zararlı alan adları, Firebog reklam/izleyici/şüpheli listeleri, oltalama, kripto madenciliği, DNS over HTTPS sunucuları, HaGeZi ...). **recommended** (önerilen) etiketli listeler başlangıç için idealdir: StevenBlack (reklam/zararlı), Disconnect.me malvertising, Spam404, OpenPhish, NoCoin, Dandelion Sprouts ve HaGeZi Multi Normal.
3. **Automatic updates** zamanlamasını ayarlayın (aşağıya bakınız) ve **Save** düğmesine tıklayın.
4. **Apply changes** ile etkinleştirin. Seçilen listeler indirilir ve belleğe yüklenir.

> **İpucu:** Daha fazla liste seçmek her zaman daha iyi sonuç vermez. Çok geniş reklam ve izleyici listeleri kullanıcıların açmak istediği bağlantıları da engelleyebilir - bülten e-postalarındaki bağlantılar, fiyat karşılaştırma siteleri veya bazı mobil uygulamalar gibi. Önerilen listelerle başlayın, bir hafta boyunca **Blocked** raporunu izleyin ve gerekirse ekleme yapın.

**DNS engellemelerinin aşılmasını önleme.** Kendi DNS sunucusunu kullanan bir cihaz (`8.8.8.8` veya tarayıcı içindeki şifreli DNS over HTTPS) güvenlik duvarının çözümleyicisini atlar. Bunu önlemek için:

- seçilen ağların DNS sorgularını güvenlik duvarına zorunlu yönlendirin (**Services > DNS Resolver** veya **Diagnostics > Web Activity** altındaki *Send all DNS to the firewall*) - böylece cihazlar port 53 üzerinden başka bir DNS sunucusuna çıkamaz;
- genel DNS-over-HTTPS sunucularının alan adlarını engelleyen **DoH** kategorisini etkinleştirin; böylece tarayıcılar otomatik olarak standart DNS'e geri döner.

### Kendi özel kayıtlarınız

| Alan | Anlamı |
| :--- | :--- |
| **Allow list** | Seçili listelerde bulunsa bile hiçbir zaman engellenmeyecek alan adları (her satıra bir alan adı). Bir alan adına izin verildiğinde alt alan adlarına da izin verilir. |
| **Block list** | Her zaman engellenecek alan adları (her satıra bir alan adı). |
| **Custom lists** | Başka kaynaklardan ek listeler; her satıra `isim \| https://adres` biçiminde yazılır. Hem `hosts` dosyası biçimi hem de düz alan adı listesi biçimi desteklenir. |

Güvenlik duvarının kendi güncelleme sunucuları ve liste indirme kaynakları hiçbir zaman engellenmez; dolayısıyla hiçbir liste güvenlik duvarının güncellemelerini kesemez.

### Bir alan adını sorgulama (Check a domain)

**Check a domain** aracı bir alan adının engellenip engellenmediğini ve hangi liste tarafından engellendiğini anında gösterir - *"bu site neden açılmıyor?"* sorusunun en hızlı yanıtıdır. Bir liste olmaması gereken bir adresi engelliyorsa alan adını **Allow list** içine ekleyin.

## IP engelleme listeleri

![Firewall > Blocklists > IP](/images/manual/bl-ip.png)

1. **Block the addresses in the selected lists** anahtarını açın.
2. Listeleri seçin. **recommended** (önerilen) listeler hiçbir ağın konuşmak istemeyeceği hedefleri engeller: abuse.ch Feodo Tracker (botnet komuta-kontrol sunucuları), Emerging Threats engelleme ve ele geçirilmiş sunucu listeleri ile Spamhaus DROP (siber suç amaçlı işletilen IP blokları). Daha sıkı politikalar için tarayıcılar, blocklist.de saldırı raporları, e-posta spam kaynakları, Tor çıkış düğümleri, açık vekil sunucular, VPN servisleri, DNS-over-HTTPS sunucuları ve torrent izleyicileri gibi ek kategoriler de mevcuttur.
3. **Direction** (Yön):
   - **Both directions (recommended)** - internetten gelen saldırganlar WAN kapısında düşürülür; içerideki bir cihazın listedeki zararlı bir adrese bağlanma girişimi de durdurulur (bu durum çoğu zaman ağdaki enfekte bir cihazın ilk belirtisidir).
   - **Incoming only (from the internet)** veya **Outgoing only (from local devices)**.
4. **Log blocked connections (Firewall Log)** - her engellemeyi **Diagnostics > Firewall Log** içine de yazar.
5. **Automatic updates** ayarını seçin, **Save** ve **Apply changes** düğmelerine tıklayın.

IP listeleri doğrudan paket filtresine (`pf` tablolarına) yüklenir: sonraki otomatik liste güncellemeleri servis yeniden başlatmaya veya Apply işlemine gerek kalmadan anında devreye girer.

### Kendi özel IP kayıtlarınız

| Alan | Anlamı |
| :--- | :--- |
| **Allow list** | Hiçbir zaman engellenmeyecek IP adresleri ve ağlar. Uzaktan bağlandığınız kendi adreslerinizi buraya ekleyin - şubeleriniz, VPN kullanıcılarınızın bulunduğu yerler, dış izleme servisleriniz: bir liste günün birinde bu adresleri içerse bile erişiminiz kesilmez. |
| **Block list** | Her zaman engellenecek IP adresleri ve ağlar. |
| **Custom lists** | Her satıra `isim \| https://adres` biçiminde harici IP listeleri. |

**Listelerde bulunsa bile asla engellenmeyen aralıklar:** özel ve ayrılmış IP aralıkları (`10.x`, `172.16-31.x`, `192.168.x`, `127.x`, multicast ...) ve `/8`'den daha geniş bloklar. Hatalı hazırlanmış bir harici liste kendi yerel ağlarınızı veya internetin yarısını kesemez.

### Bir IP adresini sorgulama (Check an address)

**Check an address** aracı bir IP adresinin engellenip engellenmediğini ve hangi listede yer aldığını gösterir.

## Otomatik güncellemeler

Her iki sekmede de aynı güncelleme ayarları bulunur:

| Ayar | Anlamı |
| :--- | :--- |
| **Automatic updates** | Saatlik, 6 saatte bir, 12 saatte bir, günlük, haftalık veya **Off (manual updates only)**. |
| **On**, **At** | Güncellemenin yapılacağı gün (haftalık seçildiğinde) ve saat. |

Sekme üzerinde son güncellemenin durumu, bir sonraki otomatik güncelleme zamanı ve **Update log** gösterilir. **Update lists now** listeleri anında günceller.

**Başarısız bir indirme mevcut listeyi korur**; böylece harici bir kaynak geçici olarak erişilemez olduğunda koruma devre dışı kalmaz. Başarısız indirmeler bildirimlerde raporlanır.

Güncelleme zamanlaması kaydedildiği anda geçerli olur; engelleme listelerini açıp kapatmak veya liste seçimini değiştirmek ise **Apply changes** gerektirir.

## Engellenenler raporu (Blocked)

**Firewall > Blocklists > Blocked** sekmesi (yalnızca yöneticilere açıktır) DNS ve IP engelleme listelerinin durdurduğu trafiği özetler.

![Firewall > Blocklists > Blocked](/images/manual/bl-blocked.png)

- **Dönem (Period):** son bir saat, son 24 saat veya son 7 gün; **Refresh** görünümü yeniler.
- **Toplamlar:** engellenen DNS sorguları, engellenen gelen ve giden IP paketleri, son bir saatteki engellemeler.
- **Blocks per hour** - saatlik DNS ve IP engelleme grafiği.
- **By list** - hangi listelerin aktif olarak engelleme yaptığı. Hiçbir şey engellemeyen dev bir liste harcadığı belleğe değmeyebilir.
- **Top blocked domains**, **Top blocked addresses**, **Devices with the most blocks**. Çok sayıda giden IP engellemesi veya zararlı yazılım alan adı sorgusu üreten bir iç cihaz mutlaka incelenmelidir: zararlı yazılım bulaşmış olabilir.
- **Blocked queries and packets** - türe (DNS, IP) ve listeye göre süzülebilen tekil kayıtlar; zaman, alan adı veya IP adresi, cihaz ve eşleşen liste bilgisiyle gösterilir. **Load more** daha eski kayıtları getirir.

Kayıtlar 7 gün boyunca saklanır. Rapor hangi cihazın hangi alan adını sorguladığını gösterdiği için yalnızca yöneticiler tarafından görüntülenebilir ve her görüntüleme sistem günlüğüne kaydedilir. **Clear report** toplanan rapor kayıtlarını temizler.
