---
title: "Stratum Firewall v0.29.0: Zamanlanmış Güvenlik Kuralları, 1:1 NAT ve Günlük Özeti Analitiği"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-29-0-release"
description: "Stratum Firewall v0.29.0 sürümüyle saat/gün bazlı kural zamanlamaları, Sanal IP blokları için çift yönlü 1:1 NAT, etkileşimli Güvenlik Duvarı Günlük Özeti, WAN tarama gürültüsü bastırma ve donanım açılış melodileri yayınlandı."
lead: "v0.28.4'ten v0.29.0'a uzanan yedi sürüm; kurumsal 1:1 NAT eşleştirmesi, mesai ve ders saatine göre çalışan kurallar, görsel günlük analitiği ve donanım kutuları için açılış melodileri getiriyor."
tags: ["stratum firewall", "freebsd 15.1", "pf", "1:1 nat", "kural zamanlama", "gunluk analitigi", "v0.29.0"]
---

Birleşik engelleme listesi raporları ve politika yönlendirmesinin hemen ardından yayınladığımız **Stratum Firewall v0.29.0**, kurumsal ağlar, çoklu sabit IP kullanan veri merkezleri ve eğitim kampüsleri için yedi ardışık güncellemeyi (`v0.28.4` – `v0.29.0`) bir araya getiriyor. Tüm detayları [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) sayfasında inceleyebilirsiniz.

### 1. Saat ve Gün Bazlı Kural Zamanlamaları (`v0.29.0`)

**Güvenlik Duvarı -> Zamanlamalar (`Firewall -> Schedules`)** ekranından `"Pzt-Cum 08:00-18:00; Cmt 09:00-13:00"` gibi zaman dilimleri tanımlayarak herhangi bir izin veya engelleme kuralına bağlayabilirsiniz:

* **Uygula Gerektirmeyen Otomatik Geçiş:** `stratum-pfrefresh` servisi kuralları sistem saatine göre bir dakika içinde kendiliğinden devreye alır veya çıkarır.
* **Açık Oturumların Otomatik Kapatılması:** Bir izin (`pass`) kuralının süresi dolduğunda o kural üzerinden açılmış aktif bağlantılar derhal sonlandırılır; mesai veya ders saati bitiminde açık oturum kalmaz.
* **Güvenli OPNsense Geçişi:** OPNsense `config.xml` dosyasından aktarılan zamanlamalı kurallar gün boyu yanlışlıkla açık kalmaması için pasif olarak içe aktarılır ve raporda vurgulanır.

### 2. Sabit IP Blokları İçin Çift Yönlü 1:1 NAT (`v0.28.9`)

`/29` veya `/28` genel IP bloklarına sahip kurumlar artık **Güvenlik Duvarı -> NAT -> 1:1** menüsünden iç ağdaki sunucularını birebir genel IP adresleriyle eşleştirebilir ([Özellikler](https://stratumfw.com/tr/product/features/)):

* **Tam Güvenlik Kontrolü:** Eşleştirilen sunucu internete kendi genel IP adresiyle çıkar; dışarıdan gelen trafik ise yine yalnızca açıkça tanımladığınız WAN `pass` kuralları izin verdiğinde içeri alınır.
* **Kilitlenme Koruması:** Güvenlik duvarının kendi ana WAN IP adresi 1:1 NAT için seçilemez; yönetim erişimi her zaman korunur.

### 3. Güvenlik Duvarı Günlük Özeti (`v0.28.8`) ve WAN Gürültü Kontrolü (`v0.28.4`)

**Tanılama -> Güvenlik Duvarı Günlüğü -> Özet** sekmesi, canlı paket akışının yanına son 1 saat ve 24 saatlik görsel bir analiz merkezi ekler:

* **Kaynak Ülke, Port ve Kural Dağılımı:** En çok engellenen kaynak IP adreslerini, ülkeleri (ücretsiz 5-RIR GeoIP verisiyle), hedef portları (`SSH`, `RDP` vb.) ve arayüz istatistiklerini tek ekranda görebilir; herhangi bir IP veya porta tıklayarak canlı günlüğü anında filtreleyebilirsiniz.
* **Daha Temiz Günlükler:** **Sistem -> Ayarlar -> Güvenlik Duvarı ve NAT** altındaki yeni **WAN üzerinde de** seçeneğini kapatarak sürekli internet bot taramalarının günlükleri şişirmesini önleyebilir, yalnızca iç VLAN ve VPN engellemelerini kaydedebilirsiniz.

### 4. Fiziksel Cihazlar İçin Açılış ve Kapanış Melodileri (`v0.28.5` – `v0.28.6`)

Dahili hoparlöre (PC speaker) sahip fiziksel x86-64 güvenlik duvarı cihazları artık açılış ve kapanışta sesli bildirim veriyor; **Sistem -> Ayarlar -> Genel** ekranından **Stratum**, **Klasik bip**, **Westminster Çanı**, **Neşeye Övgü**, **Für Elise** ve **Arcade** melodileri arasından seçim yapıp anında test edebilirsiniz.

### Hemen Yükseltin veya Tarayıcıda Canlı Deneyin

Kurulu sistemlerinizi yönetim panelindeki **Sistem -> Güncelleme** menüsünden otomatik ZFS önyükleme ortamı yedeğiyle saniyeler içinde **v0.29.0** sürümüne yükseltebilirsiniz. Henüz denemediyseniz [Canlı Demo](https://demo.stratumfw.com/) üzerindeki gerçek FreeBSD 15.1 sanal makinesine bağlanabilir veya [İndir](https://stratumfw.com/tr/download/) sayfasından kurulum imajlarını edinebilirsiniz.
