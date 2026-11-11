---
title: "Stratum Firewall v0.28.3: Engelleme Listesi Raporları, Politika Tabanlı Yönlendirme ve Servis Yönetimi"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-28-3-release"
description: "Stratum Firewall v0.28.3 sürümüyle pfBlockerNG tarzı birleşik DNSBL/IP engelleme raporları, çoklu WAN politika tabanlı yönlendirme (Policy Routing), merkezi servis yönetimi ve yenilenen bildirim deneyimi kullanıma sunuldu."
lead: "v0.27.2'den v0.28.3'e uzanan güncelleme paketi; DNS ve IP engellemeleri için detaylı analiz raporları, kural bazlı çoklu WAN yönlendirmesi, canlı servis durumu kontrolü ve daha hızlı bir yapılandırma iş akışı getiriyor."
tags: ["stratum firewall", "stratum fw", "freebsd", "pfblockerng", "dnsbl", "policy routing", "multi-wan", "unbound"]
---

[Stratum Firewall TR](https://stratumfw.com/tr/) için yayınladığımız **v0.28.3** sürümü, ağ yöneticilerinin en sık ihtiyaç duyduğu görünürlük, çoklu internet hattı kontrolü ve günlük operasyon hızını tek bir güncelleme serisinde birleştiriyor. [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) sayfasında detaylandırılan `v0.27.2` ile `v0.28.3` arasındaki yenilikler; pfBlockerNG tarzı birleşik engelleme raporlarından kural bazlı WAN seçimine (Policy Routing) ve merkezi servis yönetimine kadar uzanıyor.

### 1. Birleşik Engelleme Listesi Raporları ve Kontrol Paneli Kartı (v0.28.2 & v0.28.3)

Bir güvenlik duvarında on binlerce zararlı alan adını ve tehdit IP adresini engellemek kadar, *hangi cihazın hangi kurala takıldığını* anında görebilmek de kritiktir. **Güvenlik Duvarı > Engelleme Listeleri > Engellenenler (`Firewall > Blocklists > Blocked`)** ekranı, pfBlockerNG raporlarına benzer kapsamlı bir analiz merkezi sunuyor ([Özellikler](https://stratumfw.com/tr/product/features/)):

* **Üç Katman Tek Raporda:** DNS engelleme listeleri (DNSBL), kategori bazlı Web Filtresi ve gelen/giden `pf` IP engelleme listeleri tek bir görünümde birleştirildi ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).
* **Saatlik/Günlük Grafikler ve En Çok Engellenenler:** Son 1 saat, 24 saat veya 7 günlük pencerelerde kaç DNS sorgusunun ve IP paketinin durdurulduğunu, engellemeyi hangi listenin yaptığını, en sık engellenen alan adlarını/IP adreslerini ve en fazla engelleme üreten yerel cihazları anında inceleyebilirsiniz.
* **Anlık Arama ve Kontrol Paneli (Dashboard) Eklentisi:** Her kayıt türe, listeye, alan adına, IP adresine veya cihaza göre filtrelenebilir. Ayrıca **Kontrol Paneli > Paneller** menüsünden ana ekranınıza yeni **Engelleme Listeleri (Blocklists)** kartını ekleyerek son 24 saatin özetini canlı izleyebilirsiniz.
* **Unbound 1.26 Uyumluluğu ve Gizlilik:** Veriler her dakika Unbound ve güvenlik duvarı günlüklerinden toplanır; Web Etkinliği (Web Activity) kapalı olsa bile yalnızca engellenen sorgular ayrı bir günlükten okunur. `v0.28.3` ile Unbound 1.26'nın `rpz-nxdomain` günlük formatı tam uyumlu hale getirildi ve izin verilen (`rpz-passthru`) sorguların engelleme sayısına dahil edilmesi önlendi. Rapor yalnızca yöneticilere açıktır ve her görüntüleme sistem günlüğüne kaydedilir.

### 2. Politika Tabanlı Çoklu WAN Yönlendirmesi — Policy Routing (v0.27.2)

Birden fazla internet hattına (Multi-WAN) sahip yapılarda artık yalnızca hat yedeklemesi (failover) değil, kural bazlı hat seçimi de yapılabiliyor ([Mimari](https://stratumfw.com/tr/product/architecture/)):

* **Ağ veya Kural Bazlı Çıkış Hattı Seçimi:** Dahili bir arayüzdeki veya arayüz grubundaki herhangi bir izin (`pass`) kuralında trafiğin hangi WAN ağ geçidinden (statik veya PPPoE) çıkacağını seçebilirsiniz — örneğin misafir Wi-Fi VLAN'ını ikinci internet sağlayıcısına yönlendirirken sunucu trafiğini ana fiber hatta tutabilirsiniz ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).
* **Akıllı Yerel Ağ Koruması:** Kural kapsamındaki trafiğin yerel LAN/VLAN ağlarına, VPN tünellerine, statik rotalı alt ağlara veya güvenlik duvarının kendisine giden kısmı otomatik olarak normal yerel yönlendirmede kalır; iç ağ trafiği yanlışlıkla internet sağlayıcısına gönderilmez.
* **Kesintisiz Yedeklilik ve OPNsense Göç Uyumu:** Seçilen WAN ağ geçidi çökerse ağ geçidi izleyicisi kuralları saniyeler içinde varsayılan rotaya (veya tercih edilirse yalnızca seçili hatta) geçirir. Ayrıca OPNsense `config.xml` içe aktarma aracı artık kurallardaki ağ geçidi tanımlarını da eşleşen WAN arayüzüne otomatik taşır.

### 3. Merkezi Servis Durumu ve Güvenli Süreç Yönetimi (v0.28.1)

**Servisler > Servis Durumu (`Services > Service Status`)** sayfası, güvenlik duvarı üzerindeki tüm arka plan servislerini tek bir tabloda toplar ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)):

* **Canlı Kaynak İzleme:** Her servisin durumu (çalışıyor, durduruldu, kapalı), çalışma süresi (uptime), bellek kullanımı ve süreç kimliği (`PID`) tek bakışta görülür.
* **Güvenli Başlat / Durdur / Yeniden Başlat:** Yöneticiler servisleri doğrudan bu ekrandan yönetebilir. Yanlışlıkla kesinti yaşanmaması için durdurma işlemi ikinci tıklama onayı ister; `pf`, web sunucusu, web paneli, SSH ve zamanlayıcı gibi çekirdek bileşenler ise yalnızca güvenli şekilde yeniden başlatılabilir (`pf` yeniden başlatıldığında kurallar ve IP engelleme tabloları tazelenir).

### 4. Kişiselleştirilmiş Bildirimler ve Hızlandırılmış Uygula (Apply) Akışı (v0.28.0)

Çok yöneticili ekiplerin günlük kullanım alışkanlıklarına yönelik iki önemli arayüz iyileştirmesi yapıldı ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)):

* **Kullanıcıya Özel Bildirim Kutusu:** Sağ üstteki bildirim zilinde bildirimleri tek tek veya topluca okundu işaretleme, silme ve listeyi temizleme işlemleri artık her yönetici için bağımsız tutulur; sizin temizlediğiniz liste diğer yöneticilerin bildirimlerini veya sistem olay geçmişini silmez.
* **Kaydet Yanında Uygula ve Sabit Üst Çubuk:** Yapılandırmayı gerçekten değiştiren bir ayar kaydedildiğinde (OPNsense ve pfSense'de olduğu gibi) **Kaydet** düğmesinin hemen yanında **Değişiklikleri uygula** düğmesi belirir; ayrıca **Uygula / Değişiklikleri koru** çubuğu uzun sayfalarda aşağı kaydırırken üstte sabit kalarak her an erişilebilir olur.

### Hemen Güncelleyin veya Tarayıcıda Canlı Deneyin

Mevcut [Stratum Firewall TR](https://stratumfw.com/tr/) kurulumlarınızı yönetim panelindeki **Sistem > Güncelleme** ekranından otomatik ZFS önyükleme ortamı (Boot Environment) yedeğiyle saniyeler içinde **v0.28.3** sürümüne yükseltebilirsiniz. Henüz kurulum yapmadıysanız [Canlı Demo](https://demo.stratumfw.com/) üzerinden gerçek FreeBSD 15.1 sanal makinesine bağlanabilir veya [İndir](https://stratumfw.com/tr/download/) sayfasından kurulum imajlarını edinebilirsiniz.
