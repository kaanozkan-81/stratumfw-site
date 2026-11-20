---
title: "Tanılama > Sistem Günlükleri"
seo_title: "10.2 Tanılama > Sistem Günlükleri (Diagnostics > System Logs) — Stratum FW Kullanım Kılavuzu"
description: "Sistem ve servis günlükleri: System, DHCP/DNS, PPPoE, NTP, Authentication, Web panel ve Configuration sekmeleri, arama ve renklendirilmiş hata/uyarı satırları."
weight: 1002
chapter_num: "10.2"
url: "/tr/manual/diagnostics-system-logs/"
aliases:
  - "/tr/manual/10-02-diagnostics-system-logs/"
translationKey: "manual-10-02-diagnostics-system-logs"
---

[**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) güvenlik duvarındaki işletim sistemi ve servislerin günlüklerini gösterir - bir şey beklendiği gibi çalışmadığında bakılacak ilk yerdir.

![Diagnostics > System Logs](/images/manual/logs.png)

## Günlük sekmeleri

| Sekme | Dosya | İçeriği |
| :--- | :--- | :--- |
| **System** | `/var/log/messages` | Sistem ve servislerin bildirdiği tüm olaylar: arayüzler, ağ geçitleri, VPN'ler, IPsec, Captive Portal, saldırı tespit, engelleme listeleri, güncellemeler ... |
| **DHCP / DNS** | `/var/log/dnsmasq.log` | DHCP kiralamaları (hangi cihazın hangi adresi aldığı) ve DNS iletileri. |
| **PPPoE** | `/var/log/messages` (yalnızca `mpd5`) | PPPoE bağlantıları: kimlik doğrulama, alınan IP adresi, kopmalar. |
| **NTP** | `/var/log/messages` (yalnızca `ntpd`) | Zaman eşitleme kayıtları. |
| **Authentication** | `/var/log/auth.log` | Web paneli, SSH ve konsol girişleri; OpenVPN oturum açmaları; başarısız denemeler ve engellenen IP adresleri. |
| **Web panel** | `/var/log/stratum.log` | Yönetim panelinin kendi günlüğü: kimin, hangi adresten neyi değiştirdiği. |
| **Configuration** | `/var/log/stratum-apply.log` | Her Apply işlemi: hangi dosyaların yazıldığı, hangi servislerin yeniden yüklendiği, hatalar, **Keep changes** onayları ve otomatik geri almalar. |

Her sekme en yeni satırları en üstte olmak üzere 500 satıra kadar gösterir ve her 5 saniyede bir yenilenir. **Search in this log** satırları süzer; **Pause** yenilemeyi durdurur.

Hata içeren satırlar (*error*, *failed*, *denied*, *refused* ...) kırmızıyla, uyarı içeren satırlar (*timeout*, *retry*, *rollback*, *not responding* ...) sarıyla vurgulanır.

## Hangi soruda nereye bakılmalı?

| Soru | İlgili sekme |
| :--- | :--- |
| Apply neden başarısız oldu veya değişikliklerim neden geri alındı? | **Configuration** |
| Bu ayarı kim, ne zaman değiştirdi? | **Web panel** |
| Kim giriş yaptı ve kim başarısız deneme yaptı? | **Authentication** |
| Dün `192.168.1.57` adresini hangi cihaz kullanıyordu? | **DHCP / DNS** |
| PPPoE neden bağlanmıyor? | **PPPoE** - *authentication failed* kullanıcı adı veya parola hatası demektir |
| Bir WAN hattı neden *down* olarak işaretlendi? | **System** - ağ geçidi izleyicisinin (*gateway monitor*) satırları |
| Bir VPN kullanıcısı neden bağlanamıyor? | **Authentication** (OpenVPN girişi) ve **System** (OpenVPN servisi) |
| Captive Portal giriş ve çıkışları | **System**, program adı `stratum-captive` |

Yapay Zeka Asistanı da bu günlükleri okuyup açıklayabilir ([**Diagnostics > AI Assistant**](/tr/manual/diagnostics-ai-assistant/)).

Uzun süreli saklama için günlükleri uzak bir syslog sunucusuna gönderin ([**System > Settings > Logging**](/tr/manual/system-settings-logging-notifications/)).
