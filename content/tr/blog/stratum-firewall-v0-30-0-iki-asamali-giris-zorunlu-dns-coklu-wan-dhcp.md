---
title: "Stratum Firewall v0.30.0: İki Aşamalı Giriş (TOTP 2FA), Zorunlu DNS ve Ek WAN Üzerinde DHCP"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-30-0-release"
description: "Stratum Firewall v0.30.0 sürümüyle yerel QR kodlu TOTP iki aşamalı kimlik doğrulama (2FA), Active Directory istisnalı Zorunlu DNS yönlendirmesi, 4G/5G ve kablo modemler için ek WAN DHCP desteği ve gelişmiş OPNsense geçişi yayınlandı."
lead: "v0.30.0 kilometre taşına ulaşan Stratum Firewall; yönetim paneli için TOTP iki aşamalı giriş, aşılması imkansız DNSBL/Web Filtresi zorlaması ve yedek internet hatlarında tak-çalıştır DHCP WAN desteği sunuyor."
tags: ["firewall", "stratum firewall", "freebsd 15.1", "totp 2fa", "zorunlu dns", "coklu wan dhcp", "v0.30.0"]
---

Zamanlanmış kurallar, 1:1 NAT ve Güvenlik Duvarı Günlük Özeti güncellemelerinin hemen ardından gelen **Stratum Firewall v0.30.0**, kurumsal güvenlik ve çoklu internet yedekliliği için üç kritik yeteneği (`v0.29.1`, `v0.29.2` ve `v0.30.0`) devreye alıyor. Tüm sürüm geçmişini [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) sayfasında inceleyebilirsiniz.

### 1. İki Aşamalı Giriş (`TOTP 2FA`) ve Kurtarma Kodları (`v0.30.0`)

Yönetici ve operatör hesapları artık **Hesabım (`My Account`)** ekranından standart 6 haneli kimlik doğrulama uygulamalarıyla (`Google Authenticator`, `Microsoft Authenticator`, `1Password`, `Bitwarden` vb.) korunabiliyor:

* **Tamamen Yerel Karekod Üretimi:** Kurulum karekodu dışarıya hiçbir veri göndermeden yerleşik kütüphaneyle tarayıcı içinde çizilir; gizli anahtar güvenlik duvarı dışına çıkmaz ve kurulumdan sonra bir daha gösterilmez.
* **Tek Kullanımlık Kod ve Oturum Güvenliği:** Her kod yalnızca bir kez kabul edilir, 2FA açıldığı anda diğer açık oturumlar kapatılır ve hatalı denemeler giriş engelleme korumasına sayılır.
* **Telefon Kaybına Karşı Tam Koruma:** Kurulumda tek seferlik **10 adet kurtarma kodu** verilir (kullanıldığında bildirim üretir); ayrıca yöneticiler **Sistem -> Kullanıcılar** ekranından veya fiziksel konsol menüsünden 2FA kilidini sıfırlayabilir.

### 2. DNS'i Güvenlik Duvarından Zorla (`Force DNS`) ve Akıllı OPNsense Geçişi (`v0.29.2`)

**Servisler -> DNS Çözümleyici -> DNS'i Güvenlik Duvarından Zorla** seçeneğiyle, seçilen yerel ağlardaki cihazlara elle `8.8.8.8` gibi harici DNS yazılmış olsa bile tüm sorgular güvenlik duvarının Unbound çözümleyicisine yönlendirilir ve DNS over TLS (`DoT`) kapatılır:

* **Active Directory İstisna Takma Adı:** Etki alanı denetleyicileri (Domain Controller) gibi dahili sunucuları istisna takma adıyla muaf tutarken tüm kullanıcı cihazlarında DNSBL ve 55 kategorili Web Filtresi'ni gezinme kaydı tutmadan zorunlu kılabilirsiniz.
* **OPNsense Kural ve Tehdit Listesi Dönüşümü:** OPNsense `config.xml` içe aktarma sihirbazı artık DNS yönlendirme port kurallarını istisnalarıyla birlikte otomatik olarak **Zorunlu DNS** ayarına çevirir; ayrıca `CINS Army` veya `blocklist.de` gibi katalog listelerini indiren URL tablolarını boş bırakmak yerine ilgili IP tehdit listesini doğrudan aktif eder.

### 3. İkinci ve Üçüncü WAN Hatlarında Dinamik DHCP (`v0.29.1`)

Yedek hat olarak bağlanan 4G/5G LTE modemler, kablo internet veya uydu terminalleri artık statik IP gerektirmeden doğrudan **DHCP** ile adres alabilir ([Çoklu WAN Rehberi](https://stratumfw.com/tr/resources/documentation/multi-wan/)):

* **Ana Rota Koruması:** Ek DHCP WAN hattı varsayılan rotayı veya sistem DNS ayarlarını kendiliğinden değiştirmez; hat geçiş kararı her zaman 1 saniyelik Ağ Geçidi İzleyicisi'nde kalır.
* **Uygula Gerektirmeyen Dinamik Ağ Geçidi Takibi:** Operatör yeni bir DHCP ağ geçidi atadığında yedekleme (`failover`), dönüş trafiği (`reply-to`) ve kural bazlı politika yönlendirmesi saniyeler içinde otomatik güncellenir.

### Hemen v0.30.0 Sürümüne Yükseltin

Mevcut kurulumlarınızı **Sistem -> Güncelleme** ekranından otomatik ZFS önyükleme ortamı yedeğiyle saniyeler içinde **v0.30.0** sürümüne yükseltebilir, [Canlı Demo](https://demo.stratumfw.com/) üzerinde arayüzü inceleyebilir veya [İndir](https://stratumfw.com/tr/download/) sayfasından kurulum imajlarını edinebilirsiniz.
