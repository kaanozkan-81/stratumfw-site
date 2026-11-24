---
title: "Sistem > Ayarlar: Günlükleme ve Bildirimler"
seo_title: "09.3 Sistem > Ayarlar: Günlükleme ve Bildirimler (Logging & Notifications) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarı günlük döndürme ayarları, uzun süreli veya yasal saklama için uzak syslog sunucusuna günlük gönderme ve Telegram ile e-posta bildirimleri."
weight: 903
chapter_num: "09.3"
url: "/tr/manual/system-settings-logging-notifications/"
aliases:
  - "/tr/manual/09-03-system-settings-logging-notifications/"
translationKey: "manual-09-03-system-settings-logging-notifications"
---

## Günlükleme (Logging)

![System > Settings > Logging](/images/manual/set-logging.png)

### Bu güvenlik duvarı üzerindeki günlükler

**Firewall log: start a new file at** *5* **MB and keep** *3* **old files** (sıkıştırılmış olarak). Doğrudan güvenlik duvarı üzerinde daha uzun geçmiş tutmak istiyorsanız her iki sayıyı da artırabilirsiniz; anlık disk kullanımı [kontrol panelinde](/tr/manual/dashboard/) gösterilir.

Günlükler [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) ve [**Diagnostics > Firewall Log**](/tr/manual/diagnostics-firewall-log/) sayfalarından okunur.

### Günlükleri uzak bir syslog sunucusuna gönderme

Günlükleri uzun süreli saklayan bir NAS veya merkezi günlük sunucusu için - örneğin otel ve halka açık misafir ağlarında **yasal mevzuat gereği saklanması zorunlu kayıtlar** için - en fazla iki uzak syslog sunucusu tanımlanabilir.

Her sunucu için:

| Alan | Anlamı |
| :--- | :--- |
| **Server** | Sunucunun IP adresi veya alan adı. |
| **Port** | Varsayılan `514` (UDP). Hedef sunucu bu porttan syslog paketlerini kabul etmelidir. |
| **What to send** | **System** (tüm servisler, *notice* ve üzeri önem düzeyi); **Sign-ins** (panel, SSH, VPN girişleri); **Firewall log** (günlüklenen her paket); **DHCP and DNS** (IP kiralamaları ve DNS sorguları). |

**Send test message** düğmesi **Apply** öncesinde bile anında çalışır ve uzak sunucunun güvenlik duvarından syslog mesajı alabildiğini doğrular. Diğer ayarlar **Apply changes** ile etkinleşir.

Uzak sunucuda güvenlik duvarı paket kayıtları paket başına okunabilir tek satır halinde `filterlog` etiketiyle (`local5` tesisi), DHCP ve DNS kayıtları ise `local6` tesisiyle ulaşır. Güvenlik duvarının kendi yerel günlükleri de eskisi gibi tutulmaya devam eder.

> **İpucu - Captive Portal ve misafir ağları:** misafir ağlarında **Sign-ins**, **Firewall log** ve **DHCP and DNS** seçeneklerinin üçünü birlikte gönderin. Bu üçü birlikte hangi cihazın (IP ve MAC adresi) hangi saatte hangi IP adresini aldığını, portalda kimin oturum açtığını ve hangi bağlantıları kurduğunu eksiksiz kaydeder.

## Bildirimler (Notifications)

![System > Settings > Notifications](/images/manual/set-notify.png)

Güvenlik duvarı önemli durumları ve sorunları **Telegram** ve/veya **e-posta** yoluyla anında bildirebilir.

### Telegram

1. Telegram uygulamasında **@BotFather** ile konuşun, yeni bir bot oluşturun (`/newbot`) ve verdiği **token** değerini kopyalayın.
2. Oluşturduğunuz bota kendi hesabınızdan bir kez herhangi bir mesaj gönderin (bir bot size ilk mesajı kendisi başlatamaz).
3. Örneğin **@userinfobot** üzerinden **Chat ID** numaranızı öğrenin. Grup bildirimleri için botu gruba ekleyin; grup kimlikleri `-100` ile başlar. Kanallar `@kanaladi` biçiminde yazılabilir.
4. **Send notifications with a Telegram bot** anahtarını açın, **Bot token** ve **Chat ID** alanlarını doldurun ve **Send test message** düğmesine tıklayın.

### E-posta

| Alan | Anlamı |
| :--- | :--- |
| **Mail server (SMTP)**, **Port** | Örneğin `smtp.office365.com`, `587`. |
| **Security** | **STARTTLS (587)**, **TLS (465)** veya **None (25, local relay)**. |
| **User name**, **Password** | Sunucu kimlik doğrulaması istemiyorsa boş bırakın. |
| **Sender address**, **Recipients** | Gönderici adresi ve bir veya daha fazla alıcı adresi. |

**Send test message** ayarları anında sınar.

Telegram bot jetonu ve e-posta parolası yalnızca güvenlik duvarı üzerinde (`root` okuma izniyle) saklanır ve bir daha gösterilmez. Kayıtlı değeri korumak için alanı boş bırakın.

### Bildirilecek olaylar (What to report)

| Olay | Varsayılan |
| :--- | :--- |
| Yeni bir yazılım güncellemesi mevcut | açık |
| Yapılandırma uygulama işlemi (Apply) başarısız oldu | açık |
| Değişiklikler süresi içinde onaylanmadığı için otomatik geri alındı | açık |
| WAN bağlantısı koptu, tekrar geldi veya dış IP adresi değişti | açık |
| Hatalı giriş denemeleri sonrasında bir IP adresi engellendi | açık |
| Yönetim paneli sertifikasının dolmasına 21 günden az kaldı | açık |
| IP veya DNS engelleme listesi indirmeleri başarısız oldu | açık |
| Disk neredeyse doldu - varsayılan eşik **%90** | açık |
| İşlemci sıcaklığı yükseldi - varsayılan eşik **85 °C** | açık |
| Yüksek önem dereceli saldırı tespit (IDS) uyarıları (içeriden çıkan, iç sunuculara yönelen veya güvenlik duvarında açık bir porta gelen) | açık |
| ... güvenlik duvarında kapalı WAN portlarına gelen tarama uyarıları da dahil (zaten düşürülen paketler) | kapalı |
| Uzak sunucuya (SFTP veya SMB) otomatik yedekleme başarısız oldu | açık |

Güvenlik duvarı WAN durumunu, diski, sıcaklığı, sertifikayı, engelleme listelerini ve güncellemeleri her 5 dakikada bir denetler; başarısız Apply işlemleri, otomatik geri almalar ve engellenen girişler ise anında bildirilir. Aynı sorun bildirim yağmuru oluşturmamak için belirli bir aradan sonra tekrar hatırlatılır (disk ve engelleme listeleri günde bir, sıcaklık 6 saatte bir); WAN değişiklikleri ise her gerçekleştiğinde bildirilir.

**Save** düğmesi ayarları anında devreye alır.
