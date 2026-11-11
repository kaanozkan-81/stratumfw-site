---
title: "İlk Yapılandırma"
description: "Kurulum sonrasında Stratum Firewall'ı devreye alın — WAN/LAN arayüzleri, 802.1Q VLAN'lar, DNS/DHCP, Yönetici Erişimi güvenliği, Safe-Apply ve ZFS güncellemeleri."
lead: "Yeni kurduğunuz Stratum Firewall cihazını güvenli şekilde devreye alın — arayüzler, DNS, DHCP, yönetim güvenliği ve imzalı güncellemeler."
---

[VGA ISO veya Seri Konsol IMG](/tr/resources/documentation/installation/) medyasından kurulumu tamamlayıp tarayıcınızdan `https://192.168.1.1/` adresine giriş yaptıktan sonra aşağıdaki adımlarla temel yapılandırmanızı tamamlayabilirsiniz.

## 1. Genel Ayarlar ve Yönetici Erişimi Güvenliği

**Sistem → Ayarlar (`System → Settings`)** menüsüne gidin:

* **Genel (`General`) Sekmesi:** Cihaz adını (**Hostname**), yerel alan adını (örn. `ofis.yerel`), saat dilimini ve isteğe bağlı giriş ekranı duyurusunu tanımlayın.
* **Yönetici Erişimi (`Admin Access`) Sekmesi:**
  * Web paneli HTTPS portunu ve yönetim erişimine izin verilen iç ağ arayüzlerini seçin (WAN üzerinden yönetim erişimi varsayılan olarak kapalıdır).
  * **Giriş Korumasını** (hatalı parola denemelerinden sonra otomatik IP engelleme) yapılandırın.
  * **SSH** erişimini (açık/kapalı, özel port, yalnızca anahtarla giriş ve yetkili SSH açık anahtarları) ayarlayın. O anda bağlı olduğunuz IP adresini kilitleyecek bir değişiklik kaydedilmeye çalışılırsa sistem bunu otomatik olarak reddeder.
* **Sertifikalar (`Sistem → Sertifikalar`):** Panel TLS sertifikasının SHA-256 parmak izini kontrol edebilir, cihaz adınıza uygun yeni sertifika üretebilir veya kendi kurumsal CA sertifikanızı yükleyebilirsiniz.

## 2. Arayüzler ve 802.1Q VLAN Tanımları

1. **Arayüzler → Atamalar (`Interfaces → Assignments`)** ekranından fiziksel ağ kartlarını (`igb0`, `vtnet0` vb.) veya **802.1Q VLAN** alt arayüzlerini `WAN`, `LAN` ve `OPT1..N` rollerine atayın (`OPT` arayüzleri ek yerel ağ veya ikinci WAN olarak kullanılabilir).
2. **Arayüzler → WAN** altından internet bağlantı türünüzü seçin:
   * **DHCP:** ISS modem/hattından otomatik IP ve ağ geçidi alır.
   * **Statik IPv4:** Sabit IP, alt ağ maskesi, ağ geçidi ve gerekirse özel MTU değerini girin.
   * **PPPoE (`mpd5`):** Fiber/DSL kullanıcı adı ve parolanızı girin.
3. **Kaydet (`Save`)** düğmesine ve hemen yanında beliren **Değişiklikleri uygula (`Apply changes`)** düğmesine tıklayın. Sistem tüm yapılandırmaları önceden doğrular (`pfctl -n` vb.) ve **60 saniyelik Güvenli Uygulama (Safe-Apply)** sayacını başlatır; bağlantınız sürüyorsa **Değişiklikleri koru (`Keep changes`)** ile kalıcı hale getirin.

## 3. DHCP ve DNS Çözümleyici (Unbound / dnsmasq)

* **DHCP Sunucusu (`Servisler → DHCP`):** Her LAN/VLAN için IP havuzlarını ve aktif kiralamalardan tek tıkla sabit IP rezervasyonlarını (*Make static*) tanımlayın.
* **DNS Çözümleyici (`Servisler → DNS`):** DNSSEC doğrulamalı **Unbound** (tam özyinelemeli veya yönlendirici mod) ya da **dnsmasq** motorunu seçin; DHCP istemci isimleri her iki modda da otomatik çözümlenir.

## 4. İmzalı Yazılım Güncellemeleri (ZFS Önyükleme Ortamları)

**Sistem → Güncelleme (`System → Update`)** menüsünden çevrimiçi güncelleme kontrolü yapabilir veya kapalı devre ağlarda çevrimdışı `.sfu` güncelleme dosyasını yükleyebilirsiniz. Her güncelleme öncesinde otomatik olarak yeni bir **ZFS Önyükleme Ortamı (Boot Environment)** oluşturulur (en güncel 5 ortam saklanır); böylece istediğiniz an önceki sürüme güvenle geri dönebilirsiniz.
