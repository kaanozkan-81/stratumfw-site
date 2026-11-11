---
title: "İlk Yapılandırma ve İki Aşamalı Giriş (2FA)"
description: "Kurulum sonrasında Stratum Firewall'ı devreye alın — kurtarma kodlu TOTP 2FA, Yönetici Erişimi güvenliği, WAN/LAN ve DHCP Çoklu WAN arayüzleri, Zorunlu DNS, Safe-Apply ve ZFS güncellemeleri."
lead: "Yeni kurduğunuz Stratum Firewall cihazını güvenli şekilde devreye alın — TOTP iki aşamalı doğrulama, yönetim güvenliği, arayüzler, Zorunlu DNS ve imzalı ZFS güncellemeleri."
---

[VGA ISO veya Seri Konsol IMG](/tr/resources/documentation/installation/) medyasından kurulumu tamamlayıp tarayıcınızdan `https://192.168.1.1/` adresine giriş yaptıktan sonra aşağıdaki adımlarla temel yapılandırmanızı tamamlayabilirsiniz.

## 1. Genel Ayarlar, Yönetici Güvenliği ve TOTP 2FA (`v0.30.0`)

**Sistem → Ayarlar (`System → Settings`)** ve sağ üstteki **Hesabım (`My Account`)** menülerinden yönetim katmanını sıkılaştırın:

* **İki Aşamalı Giriş — TOTP 2FA (`Hesabım`, `v0.30.0`):** Paroladan sonra *Google Authenticator*, *Microsoft Authenticator*, *1Password* veya *Bitwarden* üzerinden 6 haneli doğrulama kodu isteyin:
  * **%100 Cihaz İçi Gizlilik:** Kurulum QR kodu gömülü kütüphaneyle doğrudan tarayıcınızda çizilir; gizli anahtar **güvenlik duvarından dışarı çıkmaz** ve kurulum tamamlandıktan sonra bir daha gösterilmez.
  * **Tek Kullanımlık Kod ve Giriş Koruması:** Her kod yalnızca bir kez geçerlidir (`±30 sn` saat farkı toleransı), hatalı denemeler **Giriş Koruması** sayacına işlenir ve 2FA etkinleştirildiğinde kullanıcının diğer açık oturumları otomatik kapatılır.
  * **10 Adet Kurtarma Kodu ve Konsol Sıfırlama:** Telefon kaybı durumları için 10 adet tek kullanımlık kurtarma kodu üretilir (kullanıldığında sistem bildirimi oluşur). Ayrıca yöneticiler **Sistem → Kullanıcılar** ekranından veya fiziksel konsoldaki (`stratum-menu`) parola sıfırlama adımından 2FA kilidini kaldırabilir.
* **Genel Sekmesi ve Açılış Melodileri (`v0.28.5` – `v0.28.6`):** Cihaz adını (**Hostname**), yerel alan adını, saat dilimini ve PC hoparlörü bulunan donanımlar için **Açılış / Kapanış Melodisini** (*Stratum*, *Klasik bip*, *Westminster*, *Neşeye Övgü*, *Für Elise* veya *Arcade* — canlı **Test** düğmesiyle) seçin.
* **Yönetici Erişimi ve Sertifikalar:** Web paneli HTTPS portunu, yönetim erişimine açık iç ağları, kendi IP adresinizi kilitlemeye izin vermeyen SSH ayarlarını ve **Sistem → Sertifikalar** altından TLS sertifikalarınızı yapılandırın.

## 2. Arayüzler, 802.1Q VLAN ve Ek WAN Hatları

1. **Arayüzler → Atamalar (`Interfaces → Assignments`)** ekranından fiziksel ağ kartlarını (`igb0`, `vtnet0` vb.) veya **802.1Q VLAN** alt arayüzlerini `WAN`, `LAN` ve `OPT1..N` rollerine atayın.
2. **Arayüzler** altından ana ve yedek internet hatlarınızı ayarlayın:
   * **DHCP (`v0.29.1`):** Hem birincil `WAN` hem de ikincil/üçüncül WAN hatlarında (kablo modem, 4G/5G LTE veya Starlink) desteklenir; ek DHCP WAN hatları varsayılan rotayı veya sistem DNS sunucularını tek başına devralmaz.
   * **Statik IPv4** veya **PPPoE (`mpd5`):** Sabit IP/ağ geçidi bilgilerinizi ya da fiber/DSL parolanızı girin.
3. **Kaydet (`Save`)** ve hemen yanındaki **Değişiklikleri uygula (`Apply changes`)** düğmesine tıklayın; **60 saniyelik Güvenli Uygulama (Safe-Apply)** süresi içinde **Değişiklikleri koru** ile onaylayın.

## 3. DHCP, DNS Çözümleyici ve Zorunlu DNS (`v0.29.2`)

* **DHCP Sunucusu (`Servisler → DHCP`):** Her LAN/VLAN için IP havuzlarını ve aktif kiralamalardan tek tıkla sabit IP rezervasyonlarını (*Make static*) tanımlayın.
* **DNS Çözümleyici ve Zorunlu DNS (`Servisler → DNS`):** DNSSEC doğrulamalı **Unbound** veya **dnsmasq** motorunu seçin. Seçtiğiniz iç ağlarda **Güvenlik duvarı üzerinden Zorunlu DNS (`Force DNS`, `v0.29.2`)** özelliğini açarak elle `8.8.8.8` girilmiş cihazların DNS sorgularını şeffaf biçimde güvenlik duvarına yönlendirin ve DNS over TLS (`TCP 853`) kaçışlarını engelleyin (Active Directory etki alanı denetleyicileri için **İstisna takma adı** tanımlanabilir; hiçbir gezinme kaydı tutulmaz).

## 4. İmzalı Yazılım Güncellemeleri (ZFS Önyükleme Ortamları)

**Sistem → Güncelleme (`System → Update`)** menüsünden **v0.30.0** ve sonraki imzalı güncellemeleri çevrimiçi veya çevrimdışı `.sfu` dosyasıyla uygulayın. Her güncelleme öncesinde otomatik olarak yeni bir **ZFS Önyükleme Ortamı (Boot Environment)** oluşturulur (son 5 ortam saklanır).
