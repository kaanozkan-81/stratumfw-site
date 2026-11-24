---
title: "Tanılama > Web Etkinliği"
seo_title: "10.5 Tanılama > Web Etkinliği (Diagnostics > Web Activity) — Stratum FW Kullanım Kılavuzu"
description: "Ağdaki cihazların hangi siteleri sorguladığı (cihaz ve site bazında), hangi IP adresini ne zaman kimin kullandığını gösteren DHCP geçmişi, tüm DNS sorgularını güvenlik duvarına yönlendirme, saklama süresi ve 5651 uyumlu kayıtlar."
weight: 1005
chapter_num: "10.5"
url: "/tr/manual/diagnostics-web-activity/"
aliases:
  - "/tr/manual/10-05-diagnostics-web-activity/"
translationKey: "manual-10-05-diagnostics-web-activity"
---

Web Etkinliği (**Web Activity**), ağlarınızdaki cihazların **hangi web sitelerini sorguladığını** - cihaz ve gün bazında - ve **hangi IP adresini ne zaman hangi cihazın kullandığını** kaydeder. *"Bu şüpheli siteyle sürekli hangi cihaz iletişim kuruyor?"*, *"192.168.1.57 dün ne yaptı?"* ya da halka açık bir ağda *"o tarihte ve saatte bu IP adresi kimdeydi?"* sorularını doğrudan yanıtlar.

Sayfada dört sekme bulunur: **Devices**, **Sites**, **DHCP history** ve **Settings**.

> **Gizlilik:** Web Etkinliği ağınızdaki kişilerin hangi alan adlarına eriştiğini gösterir. Bu sayfaları yalnızca yöneticiler açabilir ve verilerin her görüntülenişi sistem günlüğüne kaydedilir. Kullanıcılarınızı yerel mevzuatın gerektirdiği biçimde bilgilendirin.

## Nasıl çalışır?

Her cihaz bir web sitesine bağlanmadan önce alan adını güvenlik duvarının DNS çözümleyicisine sorar. Web Etkinliği bu sorguları cihaz ve gün bazında sayar: hangi alan adları, kaç kez, ilk ve son sorgu zamanı.

- Sayfa içeriklerini değil yalnızca **alan adlarını** kaydeder: örneğin orada ne okunduğunu değil, `www.example.com` sorgulandığını yazar. DNS sorgusu yapmadan doğrudan IP adresine kurulan bağlantılar burada görünmez.
- **Unbound** DNS çözümleyicisini gerektirir ([**Services > DNS Resolver**](/tr/manual/services-dns-resolver/)); `dnsmasq` seçiliyse sayfa bunu belirtir.
- Başka bir DNS sunucusu kullanan ya da uygulama içi şifreli DNS over HTTPS kullanan cihazlar burada görünmez - aşağıdaki **Send all DNS to the firewall** ayarına bakınız.

## Ayarlar (Settings)

![Diagnostics > Web Activity > Settings](/images/manual/webact-settings.png)

| Ayar | Anlamı |
| :--- | :--- |
| **Record which sites the devices look up** | Web Etkinliği kaydını açar. Varsayılan olarak kapalıdır. |
| **Send all DNS of the internal networks to the firewall and block DNS over TLS (port 853)** | Elle başka bir DNS sunucusuna (`8.8.8.8` vb.) ayarlanmış cihazları yakalar: sorguları cihaz fark etmeden güvenlik duvarı tarafından yanıtlanır. Ayrıca Firefox tarayıcısına kendi yerleşik DNS over HTTPS özelliğini kullanmamasını bildirir (`use-application-dns.net`). Uygulama içine gömülü DoH kullanan diğer tarayıcı ve uygulamalar ise engelleme listesi olmadan bu yolla yakalanamaz. Bu seçenek [**Services > DNS Resolver**](/tr/manual/services-dns-resolver/) altındaki **Force DNS** özelliğini tüm iç ağlar için tek seferde etkinleştirir. |
| **Keep the records (days)** | `1`-`730` gün (varsayılan `30`). Hem site sorgularına hem de DHCP geçmişine uygulanır; daha eski kayıtlar otomatik silinir. |

**Apply** sonrasında devreye girer.

> **Halka açık ağlar ve yasal saklama:** oteller, kafeler ve halka açık diğer ağlar yerel mevzuat gereği (Türkiye'de **5651 sayılı Kanun** kapsamında) IP adresi dağıtım kayıtlarını belirli bir süre boyunca saklamakla yükümlü olabilir. **Keep the records** süresini buna uygun belirleyin ve ek olarak günlükleri uzak bir syslog sunucusuna göndermeyi değerlendirin ([**System > Settings > Logging**](/tr/manual/system-settings-logging-notifications/)).

## Cihazlar (Devices)

![Diagnostics > Web Activity > Devices](/images/manual/webact-devices.png)

Seçilen dönemde sorgu yapmış her cihazı listeler: **Device** (cihaz adı ve MAC adresi), **Address**, sorgulanan tekil **Sites** sayısı, toplam **Lookups** ve **Last seen**.

- Dönem: **Today**, **Last 7 days**, **Last 30 days**, **Last 90 days** veya **Last year** (saklama süresi dahilinde).
- **All networks** veya tek bir ağ; IP adresi, cihaz adı veya MAC adresine göre **Search**.

Bir cihaza tıkladığınızda **o cihazın sorguladığı siteler** açılır: **Site**, o site altında sorgulanan alt adlar (**Names looked up** - örneğin `www.`, `api.`, `cdn.`), **Lookups**, **First** ve **Last**. Sayfa ilgili IP adresini hangi cihazın tuttuğunu (*Held by ...*) ve seçilen dönem içinde aynı IP adresini daha önce başka cihazların kullanıp kullanmadığını da açıkça gösterir.

## Siteler (Sites)

Aynı veriyi ters yönden sunar: her site için kaç farklı cihazın sorguladığı (**Devices**), toplam **Lookups** ve **Last seen**. **Search site** ile belirli bir siteyi arayabilirsiniz. Bir siteye tıkladığınızda o siteyi **hangi cihazların** sorguladığı **Lookups**, **First** ve **Last** bilgileriyle listelenir.

Alt alan adları ana tescilli alan adı altında gruplanır - `www.example.com` ve `cdn.example.com` birlikte *example.com* altında, `news.bbc.co.uk` ise *bbc.co.uk* altında toplanır.

## DHCP geçmişi (DHCP history)

Her **Address** (IP adresi) değerini hangi cihazın (**MAC address**, **Host name**) hangi zaman aralığında (**From** - **Until**) kullandığını listeler. **Address** veya **MAC address** ile arama yapılabilir.

DHCP kullanmayan sabit IP adresli cihazlar da kapsama dahildir: bu cihazlar ARP tablosundan izlenerek kaydedilir.

*"3 Mart saat 21:15'te `192.168.20.57` adresi kimdeydi?"* sorusunu yanıtlayan kayıt tam olarak burasıdır.

## Tipik inceleme soruları

| Soru | Bakılacak yer |
| :--- | :--- |
| Bir cihaz şüpheli bir siteyle iletişim kuruyor (IDS uyarısı veya engelleme listesi eşleşmesi) - hangisi? | **Sites** > ilgili site > cihazları |
| Bu cihaz ne iş yapıyor? Buluta veri gönderen bir kamera mı? | **Devices** > ilgili cihaz |
| O tarihte ve saatte bu IP adresi kimdeydi? | **DHCP history**, ardından cihaz detayı için **Devices** |
| Misafir ağında en çok hangi siteler sorgulanıyor? | Ağ süzgeciyle **Sites** |

Siteleri veya uygulamaları engellemek için [**Firewall > Web Filter**](/tr/manual/firewall-web-filter/) (kategoriler ve **Keywords** — **Keywords** özelliği Web Activity kapalıyken de hiçbir gezinme kaydı tutmadan çalışır), [**Firewall > Application Control**](/tr/manual/firewall-application-control/) (uygulama ve kategori bazında trafik görünürlüğü ve engelleme) veya [**Firewall > Blocklists**](/tr/manual/firewall-blocklists/) (DNS engelleme listeleri) sayfalarını kullanın.
