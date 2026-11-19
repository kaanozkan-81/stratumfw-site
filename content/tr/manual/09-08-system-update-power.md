---
title: "Sistem > Güncelleme ve Güç Yönetimi"
seo_title: "09.8 Sistem > Güncelleme ve Güç (System > Update & Power) — Stratum FW Kullanım Kılavuzu"
description: "İmzalı depodan çevrimiçi ve çevrimdışı (.sfu) güncellemeler, güncelleme adımları, ZFS Önyükleme Ortamları (Boot Environments) ile önceki sürüme dönme; yeniden başlatma, kapatma ve fabrika ayarlarına sıfırlama."
weight: 908
chapter_num: "09.8"
url: "/tr/manual/system-update-power/"
aliases:
  - "/tr/manual/09-08-system-update-power/"
translationKey: "manual-09-08-system-update-power"
---

## Güncelleme (Update)

![System > Update](/images/manual/update.png)

Stratum FW doğrudan `stratumfw.com` üzerindeki kendi **kriptografik imzalı deposundan** güncellenir. Her güncelleme imzalıdır ve güvenlik duvarı imzası veya sağlama toplamı (checksum) eşleşmeyen hiçbir paketi kabul etmez - güncelleme paketlerine aktarım sırasında müdahale edilemez.

### Güncelleme kontrolü

Güvenlik duvarı **her gece** yeni güncelleme olup olmadığını kendiliğinden denetler. **Software update** kartı kurulu sürümü (**Installed**), mevcut yeni sürümü (**Available**), durumu (**Status**) ve son kontrol zamanını (**Last checked**) gösterir. **Check now** anında kontrol yapar.

Yeni bir güncelleme yayınlandığında **Release notes** (sürüm notları) ekranda gösterilir ve bildirim gönderilebilir ([**System > Settings > Notifications**](/tr/manual/system-settings-logging-notifications/), *A software update is available* olayı).

Güncellemeler hiçbir zaman kendiliğinden kurulmaz: ne zaman kurulacağına siz karar verirsiniz.

### Güncellemeyi kurma

**Update to ...** düğmesine tıklayın. Panel son bir onay ister - *Önce bir yapılandırma yedeği ve bir önyükleme ortamı oluşturulacaktır. Şimdi kurulsun mu?* - ardından her adımı canlı olarak gösterir:

1. **Configuration backup** - mevcut yapılandırmanın tam kopyası alınır.
2. **Boot environment (system snapshot)** - tüm işletim sisteminin anlık ZFS görüntüsü alınır; bir aksilik olursa geri dönüş yoludur.
3. **Install** - paketlerin indirilmesi, bütünlük/imza doğrulaması, yükseltme ve açma.
4. **Restart the web panel** - yönetim panelinin yeniden başlatılması.
5. **Verify the new version** - yeni sürümün doğrulanması.

Sayfa ilerlemeyi kendiliğinden takip eder - sayfayı kapatıp tekrar açsanız bile kaldığı yerden izlemeye devam eder. **Show the full log** tüm ayrıntılı çıktıyı gösterir.

**Güncelleme çalışırken güvenlik duvarını yeniden başlatmayın veya gücünü kesmeyin.** Güncelleme sırasında paket yönlendirme ve filtreleme kesintisiz çalışmaya devam eder; yalnızca yönetim paneli ve Stratum servisleri kısa süreliğine yeniden başlar. Suricata ise engelleme portlarında birkaç saniyelik duraksama yaratmamak için yalnızca Suricata paketinin kendisi değiştiğinde yeniden başlatılır.

> **Geliştirme kurulumu:** paket olarak kurulmak yerine doğrudan dosya kopyalamayla kurulmuş bir geliştirme ortamında sayfa bunu belirtir; çevrimiçi kurulum orada kapalıdır ancak sürüm kontrolü çalışır.

### Dosyadan çevrimdışı güncelleme (Update from a file)

İnternet erişimi olmayan kapalı ağ güvenlik duvarları için: güncelleme dosyasını (`.sfu`) başka bir bilgisayarda `stratumfw.com` üzerinden indirin, ardından **Choose file** ve **Upload and verify** düğmelerine tıklayın. Dosyanın kriptografik imzası ve her paketin sağlama toplamı tıpkı çevrimiçi güncellemedeki gibi doğrulanır ve aynı kurulum adımları çalıştırılır.

### Önyükleme ortamları - önceki sürüme geri dönme (Boot environments)

Bir **önyükleme ortamı (boot environment)** her güncellemeden hemen önce alınan tam sistem görüntüsüdür. Tabloda her ortam **Name**, **State**, **Next boot**, **Created** ve **Space** bilgileriyle listelenir.

Güncelleme öncesindeki sürüme geri dönmek için ilgili satırda **Boot this** düğmesine tıklayın ve *Reboot into this boot environment now?* sorusunu onaylayın. Güvenlik duvarı o andaki sistem sürümü **ve o andaki yapılandırmasıyla** yeniden başlar - güncellemeden sonra yapılmış değişiklikler o görüntüde yer almaz.

Yönetim panelinin hiç açılmadığı acil bir durumda önyükleme ortamı fiziksel konsoldaki **açılış menüsünden (boot menu)** de seçilebilir.

En yeni beş önyükleme ortamı korunur; daha eskiler güncelleme sonrasında otomatik temizlenir.

## Güç Yönetimi (Power)

![System > Power](/images/manual/power.png)

Bu sayfayı yalnızca yöneticiler kullanabilir.

### Yeniden başlatma veya kapatma

**Restart** yaklaşık bir dakika sürer; bu sırada ağ geçişi kesilir. **Power off** sonrasında ise cihazın fiziksel güç düğmesinden tekrar açılması gerekir. Her iki düğme de yanlış tıklamaya karşı ikinci kez onay ister.

Konsol menüsündeki karşılıkları **9) Reboot system** ve **10) Halt system** seçenekleridir.

### Fabrika ayarlarına sıfırlama (Reset to factory defaults)

Güvenlik duvarını yeni kurulmuş bir sistemin başlangıç ayarlarına döndürür - yeni bir göreve verilecek cihazlar veya onarılamayacak durumdaki yapılandırmalar için kullanılır. Mevcut yapılandırma ileride gerekebilecekse **önce mutlaka bir yedek indirin**.

| Seçenek | Anlamı |
| :--- | :--- |
| **Keep the panel users** | Açık: tüm kullanıcı hesapları korunur. Kapalı: `admin` dışındaki tüm hesaplar silinir ve `admin` için **yeni bir parola** (en az 12 karakter) belirlemeniz istenir. |
| **Erase certificates, keys and secrets** | OpenVPN, WireGuard, bildirimler, SNMP, MCP jetonları, yedekleme parolası ve SSH anahtarı, panel sertifikası, yapılandırma geçmişi ve yetkili SSH anahtarlarını siler. |
| **Erase logs and statistics** | Web Activity, saldırı tespit uyarıları, indirilmiş engelleme listeleri, trafik sayaçları, güvenlik duvarı ve DNS günlüklerini temizler. |
| **Type the host name to confirm** | Doğru cihazı sıfırladığınızdan emin olmak için güvenlik duvarının sistem adını (`hostname`) yazmanız gerekir. |

**Reset and restart** düğmesine tıklayın.

Fabrika varsayılanları **uzaktaki bir güvenlik duvarının sıfırlama sonrasında da erişilebilir geri gelmesi** için özel olarak şöyle tasarlanmıştır:

- sıfırlama işlemi **o anda kullanımda olan fiziksel portları** korur: WAN arayüzü DHCP ile adres alır; LAN arayüzü `192.168.1.1/24` adresini ve DHCP sunucusunu (`.100`-`.199`) alır ve internete çıkabilir;
- diğer tüm arayüzler, VLAN'lar, kurallar, NAT, VPN'ler ve ek servis ayarları kaldırılır;
- web paneli yalnızca iç ağlarda port `443` üzerinde dinler; SSH varsayılan ayarlarına döner;
- saat dilimi, kurulu yazılım sürümü ve güncelleme ayarları korunur.

Fabrika sıfırlaması olağan geri sayım süresi beklenmeden doğrudan uygulanır (eski adresten kimse onaylayamayacağı için) ve güvenlik duvarı yeniden başlatılır.
