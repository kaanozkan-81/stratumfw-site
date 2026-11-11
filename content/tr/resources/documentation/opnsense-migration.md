---
title: "OPNsense'den Geçiş Rehberi"
description: "OPNsense config.xml dosyasını Stratum Firewall v0.30.3 sürümüne aktarın — arayüzler, DHCP WAN2, Zorunlu DNS, URL tablo IP engelleme listeleri, kurallar, NAT, Sanal IP ve sıfır kesintili OpenVPN devralma."
lead: "OPNsense yapılandırmanızı dakikalar içinde Stratum Firewall'a taşıyın — güvenlik duvarı kurallarınızı, DHCP ikincil WAN hatlarını, Zorunlu DNS kurallarını, URL tablo tehdit listelerini ve tüm OpenVPN sunucunuzu kullanıcı bilgisayarlarına dokunmadan devralın."
---

Güvenlik duvarı değiştirmek normalde yüzlerce kuralı baştan yazmayı ve tüm çalışanların cihazlarına yeni VPN profilleri dağıtmayı gerektirir. Stratum Firewall (`v0.25.0` – `v0.30.3`), **Sistem → Yedekleme → OPNsense'den İçe Aktar (`System → Backup → Import from OPNsense`)** aracıyla bir OPNsense `config.xml` yedeğini etkileşimli önizleme raporuyla doğrudan bekleyen yapılandırmaya dönüştürür.

## 1. Otomatik Aktarılan Bileşenler

| Kategori | Aktarılan Öğeler |
| :--- | :--- |
| **Ağ ve Çoklu WAN** | Arayüzler, 802.1Q VLAN'lar, birincil ve ikincil **DHCP WAN hatları (`WAN2` / `WAN3`, `v0.29.1` — uyarısız)**, ağ geçitleri, Sanal IP'ler (`IP Alias`), statik rotalar ve arayüz grupları. |
| **Güvenlik Duvarı &amp; Politika Yönlendirme** | Arayüz ve grup kuralları (eski ve yeni OPNsense XML biçimleri), kural bazlı WAN ağ geçidi seçimleri ([Politika Yönlendirme](/tr/resources/documentation/multi-wan/)) ve bant genişliği sınırları. *(Saat zamanlamalı kurallar tüm gün açık kalmaması için güvenli biçimde devre dışı aktarılır ve raporlanır — `v0.29.0`.)* |
| **NAT ve Zorunlu DNS (`v0.29.2`)** | Port yönlendirmeleri (Sanal IP hedefleri ve kaynak kısıtlamaları korunarak), Giden NAT kuralları ve iç ağların DNS trafiğini güvenlik duvarına çeviren port yönlendirmeleri — bunlar ters (`!`) hedef takma adı **İstisna takma adı** olacak şekilde doğrudan **Zorunlu DNS (`Force DNS`)** ayarına dönüştürülür (`1:1 NAT` kayıtları raporda listelenir). |
| **Takma Adlar &amp; Tehdit Listeleri (`v0.29.2`)** | IP/ağ/port takma adları, **Hostname (FQDN)** takma adları, lisanssız **Ülke (GeoIP)** takma adları ve katalogdaki tehdit listelerini (*CINS Army*, *blocklist.de* vb., eski alan adları dahil) indiren **URL Tablosu (`URL Table`)** takma adları — boş aktarılmak yerine ilgili gelen/giden **IP engelleme listesini** otomatik olarak açar. |
| **Temel Servisler** | Arayüz IP'sini kapsayan bölünmez DHCP aralıkları (`v0.30.1`) ve statik rezervasyonlar (`dnsmasq` / `ISC dhcpd`), **Unbound DNS engelleme listeleri** (*Steven Black* ve *AdGuard* dahil katalog eşleşmeleri, özel `https://` listeleri, izin/engelleme ve joker alan adları — `v0.30.2`), DNS geçersiz kılmaları, cihaz adı, alan adı, saat dilimi, DNS ve NTP sunucuları. |
| **OpenVPN Sunucusunu Devralma** | *(İsteğe bağlı kutucuk)* CA ve özel anahtarı, sunucu sertifikası, `tls-crypt`/`tls-auth` anahtarı, tüm kullanıcı sertifikaları, tünel/şifreleme/DNS/`block-outside-dns` ayarları ve mevcut OPNsense parola özetleriyle (PHP `$2y$` bcrypt ve `argon2id`) birlikte VPN kullanıcıları. |

## 2. Adım Adım Göç İş Akışı

1. **OPNsense'den `config.xml` Yedeğini Alın:** OPNsense panelinde *System → Configuration → Backups* menüsünden şifrelenmemiş `config.xml` dosyasını indirin.
2. **Stratum FW Üzerine Yükleyin:** **Sistem → Yedekleme → OPNsense'den İçe Aktar** ekranını açın, `config.xml` dosyasını seçin ve OpenVPN sunucunuzu da taşımak istiyorsanız **"OpenVPN'i de içe aktar"** kutucuğunu işaretleyin.
3. **Etkileşimli Port Eşleştirme:** OPNsense cihazındaki ağ portlarını (`igb0`, `igb1`, VLAN'lar vb.) yeni Stratum donanımınızdaki fiziksel portlarla eşleştirin.
4. **Uyumluluk ve Önizleme Raporunu İnceleyin:** Hiçbir ayar diske yazılmadan önce Stratum aktarılan kuralları, otomatik etkinleştirilen Zorunlu DNS ve URL-tablo IP engelleme listelerini, devre dışı alınan zamanlamalı kuralları ve `1:1 NAT` satırlarını şeffaf biçimde raporlar.
5. **Safe-Apply ile Güvenle Uygulayın:** İçe aktarma işlemi bekleyen değişiklik olarak kaydedilir; **Değişiklikleri uygula** düğmesine basıp 60 saniyelik **Safe-Apply** süresi içinde bağlantınızı doğrulayarak **Değişiklikleri koru** ile geçişi tamamlayın.

## 3. Kullanıcı Bilgisayarlarına Dokunmadan OpenVPN Devamlılığı

**"OpenVPN'i de içe aktar"** seçildiğinde:
* Aktarılan VPN kullanıcıları yalnızca VPN yetkili (**VPN-only**) hesaplara dönüştürülür (web yönetim paneline giremezler; aynı isimde mevcut bir Stratum kullanıcısı varsa mevcut parolası korunur).
* Hem PHP `$2y$` bcrypt hem de `argon2id` parola özetleri doğrudan çalışır (`argon2id` parolalar ilk başarılı bağlantıda güvenli biçimde bcrypt'e dönüştürülür).
* Kullanıcıların bilgisayarlarındaki ve telefonlarındaki mevcut `.ovpn` profilleri **hiçbir ayar veya sertifika değişikliği yapılmadan** yeni güvenlik duvarına bağlanmaya devam eder.
