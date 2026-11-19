---
title: "Kontrol Paneli (Dashboard)"
seo_title: "04. Kontrol Paneli (Dashboard) — Stratum FW Kullanım Kılavuzu"
description: "Kontrol panelindeki bilgi kartları, panelleri taşıma ve boyutlandırma, panel ekleme ve kaldırma, güncelleme denetimi ve sağ üstteki bildirimler."
weight: 4
chapter_num: "04"
url: "/tr/manual/dashboard/"
aliases:
  - "/tr/manual/04-dashboard/"
translationKey: "manual-04-dashboard"
---

Kontrol paneli (**Overview > Dashboard**, oturum açtıktan sonra karşılaştığınız ilk sayfa) güvenlik duvarının durumunu bir bakışta gösterir. Izgara düzenindeki panellerden oluşur; her kullanıcı panelleri kendisi için dilediği gibi sıralar, boyutlandırır, ekler veya kaldırır ve bu yerleşim kullanıcının hesabına kaydedilerek tüm tarayıcılarda korunur.

![Kontrol paneli](/images/manual/dashboard.png)

## Paneller

Sayfa açık kaldığı sürece paneller kendiliğinden yenilenir; yenilenme sıklığı anlık durum panellerinde 5 saniye ile rapor panellerinde 30 saniye arasında değişir. Çoğu panel doğrudan ilgili özelliğin yönetildiği sayfaya bağlanır: bir satıra veya panel başlık çubuğundaki bağlantılara tıklayarak yönetim sayfasını açabilirsiniz.

| Panel | Gösterdiği bilgiler |
| :--- | :--- |
| **System** | Cihaz adı (hostname), Stratum FW sürümü, çalışma süresi (uptime), internete gecikme süresi (`1.1.1.1`), son yapılandırma değişikliği ve paket filtresinin (`pf`) durumu. |
| **WAN** | Her internet bağlantısı için: IP adresi, ağ geçidi, gecikme süresi, ne kadar süredir bağlı olduğu, bugünkü toplam veri hacmi ve anlık hız. |
| **CPU** | Son 5 dakikalık grafikle işlemci kullanımı ve yük ortalaması (load average). |
| **Memory** | Kullanımdaki bellek miktarı. |
| **Disk** | Kullanımdaki disk alanı. |
| **Temperature** | En sıcak işlemci çekirdeği veya sensör değeri (sıcaklık raporlayan donanımlarda). |
| **Connections** | Üst sınıra kıyasla kullanımda olan güvenlik duvarı bağlantı durumları (states). |
| **Blocked** | Son bir saat içinde engellenen ve günlüğe kaydedilen paket sayısı. |
| **Traffic** | Panelin başlık çubuğundan seçtiğiniz arayüzün son bir saatlik trafik grafiği - **Diagnostics > Traffic Graphs** sayfasındaki verinin aynısıdır. |
| **Protection** | Engelleme listeleri, engellenen giriş denemeleri, saldırı tespit sistemi ve VPN durumunun tek bakışta özeti. |
| **Interfaces** | Her arayüzün durumu, IP adresi ve anlık gelen/giden trafik hızı. |
| **Services** | Çalışan ve durmuş servisler; başlıktaki sayı ilgilenilmesi gereken servis sayısını gösterir. |
| **DHCP** | Ağ başına bağlı cihaz sayısı ve adres havuzu doluluk oranı. |
| **VPN connections** | Şu anda bağlı olan OpenVPN ve WireGuard kullanıcıları (nereden bağlandığı, aldığı VPN adresi, ne zamandan beri bağlı olduğu, trafik miktarı) ile noktadan noktaya WireGuard ve IPsec tünelleri. |
| **Intrusion detection** | Son 24 saatin uyarıları, engellenen paketler ve en çok tetiklenen kurallar. |
| **Top blocked sources** | Son bir saatte en çok engellenen kaynak IP adresleri. |
| **Top blocked ports** | Son bir saatte en çok engellenen hedef portlar. |
| **Recent changes** | Son uygulanan, kalıcı yapılan ve geri alınan yapılandırma değişiklikleri. |
| **Gateways** | Ağ geçidi izleyicisi: her ağ geçidinin gecikme ve paket kaybı değerleri ile yedeklilik (failover) kurulduğunda varsayılan rotayı hangi hattın taşıdığı. |
| **Bandwidth limits** | Ağ başına indirme ve yükleme sınırları (**Firewall > Shaper**) ve anlık kullanım hızı. |
| **Blocklists** | Liste bazında son 24 saatin DNS ve IP engellemeleri; yöneticiler ayrıca en çok engellenen alan adlarını ve IP adreslerini de görür. |
| **Captive portal** | Bölge (zone) başına oturum açmış cihazlar, son girişler ve servis durumu; RADIUS sunucusu muhasebe (accounting) onayı vermediğinde uyarı gösterir. |
| **Backup** | Kendi sunucunuza yapılan son otomatik yedeklemenin durumu. *Yalnızca yöneticiler.* |
| **Web Activity** | Bugün en çok ziyaret edilen siteler ve etkin cihazlar. *Yalnızca yöneticiler.* |

Yeni bir kullanıcı hesabı varsayılan yerleşimle başlar: System, WAN, altı küçük gösterge (CPU, Memory, Disk, Temperature, Connections, Blocked), Traffic, Protection, Interfaces, Services, DHCP, VPN connections, Intrusion detection, Top blocked sources, Top blocked ports ve Recent changes. Diğer panelleri ihtiyaç duyduğunuzda ekleyebilirsiniz.

Henüz kullanılmayan bir özelliğe ait panel bunu açıkça belirtir ve kurulum sayfasına doğrudan bağlantı verir - örneğin *No bandwidth limit. Add one*.

## Kontrol panelini düzenleme

Panellerin üzerindeki **Customize** düğmesine tıklayın. Kontrol paneli düzenleme kipine geçer:

- Bir paneli başlık çubuğundan sürükleyerek **taşıyın**.
- Kenarlarından veya alt köşelerinden sürükleyerek **boyutlandırın**. Her panelin bir en küçük boyut sınırı vardır.
- Başlık çubuğundaki **×** düğmesiyle bir paneli **kaldırın**.
- **Add panels** düğmesi kullanabileceğiniz tüm panellerin listesini açar; **Add** seçilen paneli ilk boş alana yerleştirir, **Remove** ise kaldırır. Ardından kontrol paneline dönüp yerini ve boyutunu ayarlayabilirsiniz.
- **Reset layout** varsayılan yerleşime geri döner.

İşiniz bittiğinde **Done** düğmesine tıklayın. Yapılan her değişiklik hesabınız için anında kaydedilir; diğer kullanıcıların kendi yerleşimleri etkilenmez.

![Kontrol panelini özelleştirme kipi](/images/manual/dashboard-customize.png)

> **Not:** Telefon ekranında veya dar bir tarayıcı penceresinde paneller kendi yerleşim sıranıza göre alt alta dizilir ve yerleşim oradan düzenlenemez (*Arrange the panels on a wider screen*). Telefonda gördüğünüz görünüm masaüstü yerleşiminizi değiştirmez.

## Güncellemeleri kontrol etme

Kontrol panelinin üst kısmındaki yuvarlak ok düğmesi güncelleme sunucusuna yeni bir sürüm olup olmadığını sorar ve yanıtı gösterir: *Stratum FW is up to date* veya *Version ... is available.* Güncellemelerin kurulumu **System > Update** sayfasından yapılır. Güvenlik duvarı ayrıca günde bir kez kendiliğinden kontrol yapar ve her yeni sürümü bir kez bildirim olarak raporlar.

## Bildirimler

Her sayfanın sağ üst köşesindeki zil simgesi güvenlik duvarındaki önemli olayları toplar. Zilin üzerindeki sayı henüz okumadığınız bildirim adedini gösterir.

![Bildirim açılır penceresi](/images/manual/dashboard-notifications.png)

Buradaki olaylar Telegram veya e-posta ile de gönderilebilen olayların aynısıdır (**System > Settings > Notifications**): bir WAN bağlantısının kopması ve geri gelmesi, başarısız giriş denemelerinden sonra bir IP adresinin engellenmesi, uygulanamayan veya otomatik olarak geri alınan bir yapılandırma, yeni bir sürümün yayınlanması, başarısız engelleme listesi indirmeleri, başarısız yedekleme, diskin dolması, yüksek sıcaklık, panel sertifikasının süresinin dolmasına 21 günden az kalması ve saldırı tespit uyarıları. Sorunlar kırmızıyla, düzelmeler yeşille işaretlenir.

- Son olayların listesini açmak için zil simgesine tıklayın.
- **✓** tek bir olayı okundu olarak işaretler; **×** listenizden kaldırır.
- **Mark all read** ve **Clear** tüm liste üzerinde işlem yapar.
- **Settings** bağlantısı **System > Settings > Notifications** sayfasını açar.

Okundu işaretleri ve silme işlemleri kullanıcı bazlıdır: kendi listenizi temizlemek başka bir kullanıcının listesini silmez. Sayfa açıkken yeni bir olay gerçekleştiğinde ekranın köşesinde birkaç saniyeliğine kısa bir bildirim kutusu olarak da belirir.

Panel içi bildirimler her zaman açıktır; Telegram ve e-posta bildirimleri ile hangi olayların gönderileceği bildirim ayarları sayfasından seçilir.
