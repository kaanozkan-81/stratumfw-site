---
title: "Stratum Firewall: HTTP/HTTPS İndirme Kontrolü, Gizlenmiş Program Tespiti ve Kapalı Başlayan Arayüzler"
date: 2026-11-26
author: "Stratum Security"
translationKey: "v0-40-0-download-control"
description: "Stratum Firewall ile gelen Firewall > Download Control (Squid :3128 vekil sunucu, ayrı Inspection CA, ilk baytlardan .pdf/.jpg kılıflı .exe yakalama, Monitor ve Block modları, tek tık Bypass listesi ve yarıda kalan Apply'ın anında geri alınması) ile kapalı başlayan arayüzler ve Internet hedefi yayında."
lead: "İndirilen dosyaların gerçek türünü ilk baytlarından tanıyarak .pdf veya .jpg kılıfına gizlenmiş .exe programlarını yakalayın; yeni kurulumlarda her VLAN'ı varsayılan olarak kapalı başlatıp tek tıkla yalnızca internete açın."
images: ["/images/download-control-hero.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "download control", "indirme kontrolü", "squid", "inspection ca", "vlan", "zero trust"]
---

![Stratum Firewall HTTP/HTTPS İndirme Kontrolü ve İçerikten Dosya Türü Tespiti](/images/download-control-hero.jpg)

[Stratum Firewall](https://stratumfw.com/tr/) için yayınlanan arka arkaya iki sürüm — (`25 Kasım 2026`) ve (`26 Kasım 2026`) — hem iç ağların birbirinden yalıtımını hem de ağa indirilen dosyaların denetimini yeni bir seviyeye taşıyor:

1. **`Firewall > Download Control` + `Inspection CA` + Anında Geri Alma:** Seçtiğiniz iç ağlarda HTTP ve HTTPS üzerinden indirilen dosyaların **ilk baytlarına** bakarak gerçek türünü tespit eder, `.pdf` veya `.jpg` uzantısı arkasına gizlenmiş `.exe` programlarını yakalar; **Monitor** ve **Block** modları, tek tıkla eklenen **Bypass listesi** (`gov.tr`, bankalar, işletim sistemi güncellemeleri) ve aranabilir **Downloads** denetim sekmesi sunar.
2. (Kapalı Başlayan Arayüzler + `"Internet (not private networks)"`):** Yeni kurulumlarda ve fabrika sıfırlamalarında artık LAN ve VLAN'lar bölge tabanlı (*zone-based*) güvenlik duvarları gibi **kapalı başlar** (LAN'dan web paneline *anti-lockout* erişimi ve DHCP çalışmaya devam eder); kurallara eklenen **`Internet (not private networks)`** hedefi ve **`Allow internet access`** düğmesiyle bir ağı diğer VLAN'lara açmadan tek tıkla internete çıkarabilirsiniz.

Ayrıntılı dokümantasyon yeni [Bölüm 06.9 — Güvenlik Duvarı > İndirme Kontrolü](/tr/manual/firewall-download-control/), [Bölüm 09.6 — Sistem > Sertifikalar](/tr/manual/system-certificates/#inspection-ca), [Bölüm 06.1 — Güvenlik Duvarı > Kurallar](/tr/manual/firewall-rules/) ve [Sürüm Notları](/tr/download/#changelog) sayfalarında yayında.

![Stratum Firewall Download Control ve Kapalı Arayüz Mimarisi](/images/download-control-architecture.svg)

### 1. `Firewall > Download Control`: Dosyanın Gerçekte Ne Olduğu ve Gizlenmiş Programlar

![Firewall > Download Control > Settings](/images/manual/dlctl-settings.png)

 Zararlı yazılımlar çoğu zaman `efatura_kasim.pdf` veya `tarama_09.jpg` gibi masum görünen isimlerle gönderilir. **Download Control**, seçtiğiniz iç ağlarda güvenlik duvarının kendi IP adresinde (`:3128`) çalışan **Squid** vekil sunucusu üzerinden (örneğin Windows etki alanında Group Policy ile yönlendirilen cihazlarda; adımlar panelde yazılıdır) her indirmeyi inceler:

- **İlk baytlardan gerçek tür tespiti:** Yalnızca dosya adına ve sitenin söylediği türe değil, **dosyanın kendi ilk baytlarına** bakılır. Windows, Linux ve macOS programları, `MSI`, `APK`, `JAR`, `MSIX`, `ZIP`, `RAR`, `7z`, `CAB`, `PDF` ve Office dosyaları tanınır; web sayfalarının normal parçaları (sayfa betikleri, görseller, sıkıştırılmış sayfalar) indirme sayılmaz.
- **Gizlenmiş programlar her koşulda yakalanır:** Adı `.jpg` veya `.pdf` yapılmış bir `.exe` dosyası içeriğinden tanınır ve hangi dosya türleri seçili olursa olsun engellenebilir.
- **Önce `Monitor` ile izleyin, sonra `Block` ile engelleyin:** İlk birkaç gün hiçbir indirmeyi kesmeden nelerin yakalanacağını görmek için **Monitor** modunda başlayın; **Block** moduna geçtiğinizde engellenen dosyanın yerine [Engelleme Sayfası](/tr/manual/firewall-application-control/#engelleme-sayfası-block-page) tasarımınızı taşıyan bilgilendirme ekranı gösterilir.
- **Tek Tıkla Bypass Listesi, `Always allowed` ve Kesintisiz Geçiş (Fail-Open):** Bankalar, kamu kurumları (`gov.tr`), işletim sistemi ve güvenlik güncellemeleri ile sertifika sabitleme kullanan uygulamalar için varsayılan **Bypass listesini** tek tıkla ekleyin (bu siteler hiç açılmaz ve denetlenmez). Güvenilir iş ortağı sitelerini **Always allowed** listesine alın (denetlenir ve kaydedilir, asla engellenmez). Denetleyici durursa indirmeler kesilmeden geçer; bilgisayarlardaki antivirüs yazılımları kendi korumasını sürdürür.

![Firewall > Download Control > Downloads Sekmesi](/images/manual/dlctl-downloads.png)

Kaydedilen indirmeler **Downloads** sekmesinde saat, cihaz, site, dosya, gerçek tür, boyut ve sonuç bilgileriyle **1 ila 365 gün** (varsayılan **30 gün**) saklanır; yalnızca **Yöneticiler** görebilir ve kayıtlara yapılan her bakış sistem günlüğüne işlenir.

### 2. Ayrı `Inspection CA` Sertifikası ve Yarıda Kalan `Apply` İşleminin Anında Geri Alınması

![System > Certificates — Web Panel, Block Page CA ve Inspection CA](/images/manual/certs.png)

- **`System > Certificates > Inspection CA`:** **Block Page CA**'dan ayrı, yalnızca İndirme Kontrolü vekil sunucusuna özel sertifika otoritesi. Windows ve Group Policy için **`.cer`**, diğer sistemler için **`.crt`** (PEM) olarak indirilir; istemci üzerinde doğrulama için **SHA-256 parmak izi** gösterilir ve özel anahtarı güvenlik duvarından asla çıkmaz.
- **Yarıda Kalan `Apply` Anında Geri Alınır:** Mevcut durum kaydedildikten sonra beklenmedik bir hata Apply işlemini yarıda keserse, güvenlik duvarı artık 120 saniyelik onay süresini beklemeden **derhal** önceki duruma geri döner, ekrandaki mesajda bunu bildirir ve hatanın hangi adımda oluştuğunu günlüğe yazar.

### 3. Yeni Kurulumda Kapalı Başlayan Arayüzler ve `"Allow internet access"`

![Firewall > Rules — Kapalı Başlayan Arayüz Notu, Allow Internet Access ve Internet Hedefi](/images/manual/fw-rules-allow-internet.png)

| Yenilik | Güvenli VLAN Yalıtımını Nasıl Kolaylaştırır? |
| :--- | :--- |
| **Her Arayüz Kapalı Başlar** | Yeni kurulum ve fabrika sıfırlamasında LAN ve VLAN'lar artık birbirleri dahil her yere açık başlamaz; LAN panel erişimi (*anti-lockout*) ve DHCP çalışır, mevcut kurulumların kuralları korunur |
| **`Internet (not private networks)`** | `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` ve `100.64.0.0/10` dışındaki tüm genel internet adreslerini kapsar (*WAN net* yalnızca WAN hattının kendi alt ağıdır) |
| **`Allow internet access` Düğmesi** | Kuralı olmayan bir arayüzde tek tıklamayla iki kural ekler: **bu güvenlik duvarı** (DNS, NTP, ping) ve **internet** — diğer iç ağlara geçiş kapalı kalır |

### Stratum Firewall'ı Güncelleyin

Mevcut kurulumlarınızı **`System > Update`** (`stratum-update run`) üzerinden OpenZFS Önyükleme Ortamı güvencesiyle tek tıkla sürümüne yükseltebilir, yeni [Bölüm 06.9 İndirme Kontrolü Kılavuzunu](/tr/manual/firewall-download-control/) okuyabilir, kurulum imajlarını [İndir](/tr/download/) sayfasından edinebilir ve yönetim panelini [Canlı Demo](https://demo.stratumfw.com) üzerinde inceleyebilirsiniz.
