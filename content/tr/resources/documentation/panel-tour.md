---
title: "Panel Turu ve Operasyonlar"
description: "Stratum Firewall web arayüzünü keşfedin — 12 sütunlu özelleştirilebilir kontrol paneli, kişisel bildirimler, sabit Safe-Apply çubuğu, Servis Durumu, RBAC ve şifreli SFTP yedeklemeleri."
lead: "Stratum web panelinde uzmanlaşın — kontrol panelinizi özelleştirin, çalışan servisleri denetleyin, kullanıcı rolleri atayın ve şifreli uzak sunucu yedeklemelerini otomatikleştirin."
---

Stratum yönetim paneli; Python 3.12, FastAPI ve hafif modüler JavaScript mimarisiyle (hem **Koyu** hem **Açık** tema, mobil ekran uyumu ve anlık **TR / EN** dil geçişi desteğiyle) yüksek hızda çalışacak şekilde tasarlanmıştır.

## 1. Özelleştirilebilir 12 Sütunlu Kontrol Paneli (`v0.23.0` – `v0.28.2`)

![Stratum Dashboard](/images/docs/dashboard.png)

Her kullanıcı kendi kontrol paneli düzenini Uygula (Apply) gerektirmeden anında kaydedebilir:

* **Sürükle ve Boyutlandır (`gridstack`):** **Özelleştir (`Customize`)** düğmesiyle kartları sürükleyip 12 sütunlu ızgara üzerinde boyutlandırabilir, **Düzeni sıfırla** ile varsayılan görünüme dönebilirsiniz.
* **Kontrol Paneli → Paneller:** *Sistem*, *WAN*, *CPU*, *Bellek*, *Disk*, *Sıcaklık*, *Bağlantılar*, *Trafik*, *Ağ Geçitleri*, *Bant Genişliği Sınırları*, *Saldırı Tespit*, *Engelleme Listeleri (`v0.28.2`)*, *DHCP*, *VPN Bağlantıları*, *Yedekleme* ve *Web Etkinliği* kartlarını dilediğiniz gibi ekleyip çıkarabilirsiniz.

## 2. Kaydet Yanında Uygula ve Sabit Safe-Apply Çubuğu (`v0.28.0`)

* Yapılandırmayı değiştiren bir ayar kaydedildiğinde **Kaydet** düğmesinin hemen yanında **Değişiklikleri uygula (`Apply changes`)** düğmesi belirir.
* **Değişiklikleri uygula / Değişiklikleri koru** çubuğu uzun kural tablolarında aşağı kaydırırken üst barın altında sabit kalır. Uygulama sonrasında **60 saniye** içinde onay verilmezse sistem önceki çalışan `config.json` durumuna otomatik geri döner.

## 3. Kişisel Bildirim Merkezi ve Telegram / E-posta Uyarıları (`v0.28.0`)

* **Sağ Üst Bildirim Zili:** WAN hat geçişleri, engellenen hatalı girişler, yüksek önem dereceli Suricata IPS uyarıları, sertifika süresi ve yedekleme durumlarını gösterir. Okundu işaretleme ve listeyi temizleme işlemleri her yönetici için kişiseldir; diğer kullanıcıların listesini etkilemez.
* **Telegram ve SMTP Bildirimleri (`Sistem → Ayarlar → Bildirimler`):** Kritik olayları anlık olarak Telegram botunuza veya e-posta sunucunuza iletebilirsiniz.

## 4. Merkezi Servis Durumu (`Servisler → Servis Durumu`, `v0.28.1`)

Güvenlik duvarındaki tüm servisleri çalışma durumu (`çalışıyor`, `durduruldu`, `kapalı`), çalışma süresi, bellek kullanımı ve `PID` bilgisiyle tek listede izleyin:
* Yöneticiler servisleri doğrudan başlatabilir, çift tıklama onayıyla durdurabilir veya yeniden başlatabilir.
* Çekirdek servisler (`pf`, Nginx, web paneli, SSH, sistem günlüğü, zamanlayıcı) güvenlik nedeniyle yalnızca yeniden başlatılabilir; `pf` yeniden başlatıldığında kurallar ve IP engelleme tabloları kesintisiz yenilenir.

## 5. Rol Tabanlı Erişim (RBAC) ve Şifreli SFTP Yedekleme (`v0.22.0`)

* **Kullanıcılar ve Roller (`Sistem → Kullanıcılar`):** **Yönetici (Admin)**, **Operatör** (tanılama ve Wake-on-LAN yetkili), **Salt Okunur (Read-Only)** ([Canlı Demo](https://demo.stratumfw.com) rolü) ve **VPN-only** (yalnızca OpenVPN bağlantısı) rollerini atayın.
* **Şifreli Yedekler (`.stbk`) ve Otomatik SFTP (`Sistem → Yedekleme`):** Tüm yapılandırmayı ve isteğe bağlı olarak OpenVPN/TLS sertifikalarını, WireGuard anahtarlarını ve gizli parolaları `AES-256-GCM` ile şifrelenmiş tek bir `.stbk` dosyasında yedekleyin veya zamanlanmış görevle NAS / SFTP sunucunuza otomatik aktarın.
