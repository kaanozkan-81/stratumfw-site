---
title: "Güvenlik ve Doğrulama"
seoTitle: "Stratum Firewall Güvenlik Mimarisi, Kriptografi ve İmzalı Güncelleme"
description: "Stratum Firewall imzalı paket doğrulaması, 0600 izole gizli anahtar koruması, OpenSSL/çekirdek kriptografisi, ZFS geri dönüş mimarisi ve güvenlik bildirimi."
seoDescription: "Stratum Firewall güvenlik modeli: imzalı paketler ve .sfu güncellemeleri, 0600 izole anahtarlar, OpenSSL/FreeBSD çekirdek kriptografisi ve security@stratumfw.com."
translationKey: "product-security"
aliases: ["/tr/security/"]
---

Stratum Firewall, kapalı kutu servisler yerine doğrudan denetlenebilir işletim sistemi bileşenleri üzerine inşa edilmiştir. Cihaz üzerindeki her kriptografik işlem, güncelleme paketi, anahtar dosyası ve yapılandırma geçişi kendi FreeBSD 15.1 sisteminizde doğrudan incelenebilir.

## 1. Kriptografik Mimari (Sıfır Özel Kriptografi)

Stratum Firewall **kendi özel kriptografisini yazmaz veya uygulamaz**:

* **WireGuard (`if_wg`):** Veri düzlemi tünel şifrelemesi doğrudan **FreeBSD 15.1 çekirdeğindeki `if_wg`** sürücüsünde çalışır; Curve25519 anahtar çiftleri **OpenSSL** (Python `cryptography` kütüphanesi üzerinden, `v0.34.2+`) ile üretilir.
* **Merkezden Şubeye IPsec (`enc0`):** IKEv2 ve IKEv1 anahtar müzakeresi **`strongSwan`** (`swanctl`) tarafından yürütülür; paket şifrelemesi (`AES-GCM`, `AES-CBC`, `SHA-2`, `DH 14-21`, `Curve25519`) FreeBSD çekirdek ESP katmanında (`enc0`) gerçekleşir.
* **OpenVPN ve TLS:** Sertifika otoritesi (CA), EC sunucu/istemci sertifikaları, `tls-crypt` anahtarları ve HTTPS oturum güvenliği doğrudan sistem **OpenSSL** altyapısını kullanır.

## 2. İmzalı Güncellemeler ve Çevrimdışı Doğrulama

* **Kriptografik İmzalı Paket Deposu:** Çevrimiçi güncelleme paketleri ve depo kataloğu kriptografik olarak imzalıdır; değiştirilmiş veya bozulmuş paketler kurulum başlamadan reddedilir. İmza doğrulaması her yeni sürümde test edilir.
* **İmzalı Çevrimdışı Paketler (`.sfu`):** İnternete kapalı ağlar için sunulan çevrimdışı güncelleme dosyaları (`.sfu`) aynı imzalı depo yapısını taşır ve yüklenmeden önce cihaz üzerinde doğrulanır.
* **Otomatik ZFS Önyükleme Ortamları (`bectl`):** Her güncelleme öncesinde OpenZFS kök dosya sisteminde yeni bir önyükleme ortamı oluşturulur; böylece önceki çalışan sürüme konsoldan veya web arayüzünden anında geri dönülebilir.

## 3. Gizli Anahtar İzolasyonu ve Safe-Apply Geri Alma

* **Yalnızca `root` Erişimli Gizli Dosyalar (`0600`):** Hassas veriler (VPN özel anahtarları, parola özetleri, IPsec paylaşımlı anahtarları, SFTP yedekleme anahtarları) `config.json` dışında, yalnızca `root` kullanıcısının okuyabildiği `0600` izinli dosyalarda tutulur ve **asla** tarayıcıya gönderilmez.
* **Ön Kontrollü Safe-Apply ve Otomatik Geri Alma:** Her değişiklik uygulanmadan önce sözdizimi ve servis testlerinden (`pfctl -n`, `nginx -t`, `unbound-checkconf`, `dnsmasq --test`, `sshd -t`) geçirilir. 60 saniye içinde onaylanmayan değişiklikler kendiliğinden geri alınarak erişim kaybı önlenir.
* **Tasarımdan Şeffaf Yapı (`Open by Design`):** Üretilen tüm servis dosyaları (`config.json`, `pf.conf`, `swanctl.conf`, `wg*.conf`, `unbound.conf`) okunabilir düz metin halindedir. FastAPI panel kaynak kodu `v1.0` öncesi geliştirme sürecinde özel depoda tutulmakta olup GitHub üzerinde yayınlanacaktır.

## 4. Erişim Kontrolü, Yerel 2FA ve Sıfır Dış Telemetri

* **Yerel QR Kodlu TOTP İki Aşamalı Giriş:** TOTP doğrulama kodları ve QR görseli üçüncü taraf bir servise istek atılmadan tamamen yerel olarak üretilir; 10 adet tek kullanımlık kurtarma kodu ve HTTPS `Secure` çerez korumasıyla desteklenir.
* **Rol Tabanlı Erişim (RBAC) ve Denetim Günlüğü:** `Yönetici`, `Operatör` ve `Salt Okunur` rolleri birbirinden ayrılmıştır; hassas günlükler (web aktivite kayıtları, paket yakalamaları) yalnızca yöneticilere açıktır ve erişimler kayıt altına alınır.
* **Telemetri Yok:** Cihaz dışarıya kullanım istatistiği veya yapılandırma verisi göndermez; dış ağ istekleri yalnızca yöneticinin etkinleştirdiği imzalı güncelleme kontrolleri ve tehdit engelleme listesi indirmelerinden ibarettir.

## 5. Güvenlik Açığı Bildirimi

Stratum Firewall, kurulum imajları veya `stratumfw.com` üzerinde bir güvenlik zafiyeti tespit ederseniz, imzalı yamayı hızla yayınlayabilmemiz için lütfen doğrudan bize bildirin:

* **E-posta:** [security@stratumfw.com](mailto:security@stratumfw.com)
* **RFC 9116 Güvenlik Politikası:** [`https://stratumfw.com/.well-known/security.txt`](/.well-known/security.txt)

Bildiriminizde etkilenen sürüm numarasını ve tekrarlama adımlarını paylaşmanız yeterlidir; doğrulanan bildirimler düzeltme yayınıyla birlikte [Resmi Sürüm Notları](/tr/resources/changelog/) sayfasında teşekkür notuyla paylaşılır.
