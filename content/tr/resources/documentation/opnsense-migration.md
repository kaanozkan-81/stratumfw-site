---
title: "OPNsense'den Geçiş Rehberi"
description: "OPNsense config.xml dosyasını Stratum Firewall'a aktarın — etkileşimli port eşleştirme, kurallar, NAT, Sanal IP, GeoIP/Hostname takma adları, Politika Yönlendirme ve sıfır kesintili OpenVPN devralma."
lead: "OPNsense yapılandırmanızı dakikalar içinde Stratum Firewall'a taşıyın — güvenlik duvarı kurallarınızı, VLAN ve Sanal IP yapınızı ve tüm OpenVPN sunucunuzu kullanıcı bilgisayarlarına dokunmadan devralın."
---

Güvenlik duvarı değiştirmek normalde yüzlerce kuralı baştan yazmayı ve tüm çalışanların cihazlarına yeni VPN profilleri dağıtmayı gerektirir. Stratum Firewall (`v0.25.0` – `v0.29.0`), **Sistem → Yedekleme → OPNsense'den İçe Aktar (`System → Backup → Import from OPNsense`)** aracıyla bir OPNsense `config.xml` yedeğini etkileşimli önizleme raporuyla doğrudan bekleyen yapılandırmaya dönüştürür.

## 1. Otomatik Aktarılan Bileşenler

| Kategori | Aktarılan Öğeler |
| :--- | :--- |
| **Ağ ve Yönlendirme** | Arayüzler, 802.1Q VLAN'lar, ağ geçitleri, varsayılan ağ geçidi, Sanal IP'ler (`IP Alias`), statik rotalar ve arayüz grupları. |
| **Güvenlik Duvarı &amp; Politika Yönlendirme** | Arayüz ve grup kuralları (eski ve yeni OPNsense XML biçimleri), kural bazlı WAN ağ geçidi seçimleri ([Politika Yönlendirme](/tr/resources/documentation/multi-wan/)) ve bant genişliği sınırları. |
| **NAT ve Erişim Sınırları** | Port yönlendirmeleri (Sanal IP hedefleri ve ilişkili WAN kurallarındaki kaynak IP kısıtlamaları korunarak) ve Giden NAT (Outbound NAT) kuralları. |
| **Dinamik &amp; Statik Takma Adlar** | IP, ağ ve port takma adları (iç içe geçmiş listeler çözümlenir), **Hostname (FQDN)** takma adları ve lisans anahtarı gerektirmeyen **Ülke (GeoIP)** takma adları. |
| **Temel Servisler** | DHCP aralıkları ve statik rezervasyonlar (`dnsmasq` ve `ISC dhcpd`), DNS geçersiz kılmaları, cihaz adı, alan adı, saat dilimi, DNS ve NTP sunucuları. |
| **OpenVPN Sunucusunu Devralma** | *(İsteğe bağlı kutucuk)* CA ve özel anahtarı, sunucu sertifikası, `tls-crypt`/`tls-auth` anahtarı, tüm kullanıcı sertifikaları, tünel/şifreleme/DNS/`block-outside-dns` ayarları ve mevcut OPNsense parola özetleriyle (PHP `$2y$` bcrypt ve `argon2id`) birlikte VPN kullanıcıları. |

## 2. Adım Adım Göç İş Akışı

1. **OPNsense'den `config.xml` Yedeğini Alın:** OPNsense panelinde *System → Configuration → Backups* menüsünden şifrelenmemiş `config.xml` dosyasını indirin.
2. **Stratum FW Üzerine Yükleyin:** **Sistem → Yedekleme → OPNsense'den İçe Aktar** ekranını açın, `config.xml` dosyasını seçin ve OpenVPN sunucunuzu da taşımak istiyorsanız **"OpenVPN'i de içe aktar"** kutucuğunu işaretleyin.
3. **Etkileşimli Port Eşleştirme:** OPNsense cihazındaki ağ portlarını (`igb0`, `igb1`, VLAN'lar vb.) yeni Stratum donanımınızdaki fiziksel portlarla eşleştirin.
4. **Uyumluluk ve Önizleme Raporunu İnceleyin:** Hiçbir ayar diske yazılmadan önce Stratum nelerin aktarıldığını, LAN IP değişimi veya WAN alt ağı çakışması varsa uyarıları ve desteklenmeyen öğeleri (CARP VIP, IPsec veya eklentiler gibi) açıkça raporlar. Güvenlik gereği erişim kuralı bulunmayan port yönlendirmeleri herkese açılmaz; kapalı (devre dışı) olarak aktarılır.
5. **Safe-Apply ile Güvenle Uygulayın:** İçe aktarma işlemi bekleyen değişiklik olarak kaydedilir; **Değişiklikleri uygula** düğmesine basıp 60 saniyelik **Safe-Apply** süresi içinde bağlantınızı doğrulayarak **Değişiklikleri koru** ile geçişi tamamlayın.

## 3. Kullanıcı Bilgisayarlarına Dokunmadan OpenVPN Devamlılığı

**"OpenVPN'i de içe aktar"** seçildiğinde:
* Aktarılan VPN kullanıcıları yalnızca VPN yetkili (**VPN-only**) hesaplara dönüştürülür (web yönetim paneline giremezler; aynı isimde mevcut bir Stratum kullanıcısı varsa mevcut parolası korunur).
* Hem PHP `$2y$` bcrypt hem de `argon2id` parola özetleri doğrudan çalışır (`argon2id` parolalar ilk başarılı bağlantıda güvenli biçimde bcrypt'e dönüştürülür).
* Kullanıcıların bilgisayarlarındaki ve telefonlarındaki mevcut `.ovpn` profilleri **hiçbir ayar veya sertifika değişikliği yapılmadan** yeni güvenlik duvarına bağlanmaya devam eder.
