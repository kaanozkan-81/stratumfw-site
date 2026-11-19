---
title: "Servisler > NTP (Zaman Sunucusu)"
seo_title: "07.4 Servisler > NTP (Services > NTP) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarı saatini doğru tutma: zaman sunucuları, saat dilimi, iç ağlara zaman servisi sunma ve eşitleme durumu."
weight: 704
chapter_num: "07.4"
url: "/tr/manual/services-ntp/"
aliases:
  - "/tr/manual/07-04-services-ntp/"
translationKey: "manual-07-04-services-ntp"
---

Doğru sistem saati bir güvenlik duvarında her yerden daha kritiktir: günlük kayıtları zaman damgalarıyla karşılaştırılır, zamanlamalar ([**Firewall > Schedules**](/tr/manual/firewall-schedules/)) kuralları saate göre açıp kapatır, sertifikalar ve iki aşamalı doğrulama (2FA) kodları yalnızca kendi zaman pencerelerinde geçerlidir ve yanlış bir saat VPN bağlantılarını bile durdurabilir. **Services > NTP** sistem saatini internetteki zaman sunucularıyla eşit tutar ve zaman bilgisini yerel ağlarınızdaki cihazlara dağıtabilir.

![Services > NTP](/images/manual/ntp.png)

## Ayarlar (Settings)

| Ayar | Anlamı |
| :--- | :--- |
| **Time servers** | Her satıra bir sunucu olmak üzere en fazla altı adres. Varsayılan `pool.ntp.org`. Bulunduğunuz ülkenin havuz adları - örneğin `tr.pool.ntp.org` veya `de.pool.ntp.org` - aynı anda birden fazla sunucuya bağlandığı için tek bir sunucudan daha güvenilirdir. |
| **Timezone** | Sistem saati, günlük zaman damgaları ve zamanlanmış görevler için kullanılan saat dilimi. **System > Settings > General** sayfasındaki ayarla aynıdır. |
| **Act as time server for internal networks** | LAN tipi arayüzlerdeki cihazların saati güvenlik duvarından almasını sağlar ve DHCP istemcilerine otomatik olarak güvenlik duvarını NTP sunucusu olarak bildirir. WAN üzerinden gelen NTP istekleri her zaman reddedilir. Varsayılan olarak kapalıdır. |

**Save settings**, **Apply changes** ve **Keep changes** düğmelerine tıklayın.

> **İpucu:** Kamera, kayıt cihazı (NVR), kartlı geçiş sistemi veya endüstriyel kontrol cihazlarının bulunduğu ağlarda **Act as time server** seçeneğini açın. Bu cihazlar çoğu zaman internete çıkamaz - ve çıkmamalıdır - ancak kayıtları ve günlükleri için doğru saate ihtiyaç duyarlar.

## Eşitleme durumu (Status)

**Status** kartı anlık **System time** bilgisini, saatin eşitlenip eşitlenmediğini ve her zaman sunucusu için bir durum satırı gösterir:

| Sütun | Anlamı |
| :--- | :--- |
| **Server** | Zaman sunucusunun adresi. |
| **State** | **Selected** sistem saatinin takip ettiği birincil sunucudur; diğerleri aday (*Candidate*) veya reddedilen sunuculardır. |
| **Reference** | İlgili sunucunun kendi zaman bilgisini nereden aldığı. |
| **Stratum** | Referans atom/GPS saatine olan katman uzaklığı (`1` = doğrudan referans saate bağlı). |
| **Reach** | Son isteklerin yanıtlanıp yanıtlanmadığı (`377` = son sekiz yoklamanın tamamı başarılı). |
| **Delay**, **Offset**, **Jitter** (ms) | Sunucuya gidiş-dönüş süresi (gecikme), sunucu saati ile güvenlik duvarı saati arasındaki fark ve bu farkın dalgalanma miktarı. |

Durum kartı her 10 saniyede bir kendiliğinden yenilenir. Servis yeni başlatıldığında bir sunucunun **Selected** durumuna geçmesi birkaç dakika sürebilir.

**Hiçbir sunucuya ulaşılamıyorsa:** güvenlik duvarının dışarıya doğru UDP port 123 erişimine ihtiyacı vardır. WAN bağlantısını ve güvenlik duvarı başka bir üst güvenlik duvarının arkasındaysa orada giden NTP trafiğine izin verildiğini kontrol edin.
