---
title: "Tanılama > Güvenlik Duvarı Günlüğü"
seo_title: "10.1 Tanılama > Güvenlik Duvarı Günlüğü (Diagnostics > Firewall Log) — Stratum FW Kullanım Kılavuzu"
description: "Canlı güvenlik duvarı günlüğü, en çok engellenen kaynaklar/portlar/ülkeler/kurallarla trafik özeti ve iç ağlarınızın hangi portlara ihtiyaç duyduğunu gösteren Ağlar Arası (Between networks) raporu."
weight: 1001
chapter_num: "10.1"
url: "/tr/manual/diagnostics-firewall-log/"
aliases:
  - "/tr/manual/10-01-diagnostics-firewall-log/"
translationKey: "manual-10-01-diagnostics-firewall-log"
---

Güvenlik duvarı günlüğü `pf` paket filtresinin kaydettiği paketleri gösterir: bir kural veya varsayılan kural tarafından **engellenen** tüm paketler ve üzerinde **Log** seçeneği açık olan kuralların **izin verdiği** paketler. Sayfada üç sekme yer alır: **Live**, **Summary** ve **Between networks**.

## Canlı Akış (Live)

![Diagnostics > Firewall Log > Live](/images/manual/fwlog-live.png)

En yeni kayıtlar en üstte olacak şekilde 300 satıra kadar listelenir ve her 3 saniyede bir yenilenir. Her satırda **Time**, **Action** (`block` veya `pass`), **Interface**, **Protocol**, **Source**, **Destination** ve eşleşen **Rule** gösterilir - kural adına tıklayarak doğrudan ilgili kuralı açabilirsiniz.

Süzgeçler:

- **All actions**, **Blocked** veya **Passed**;
- **All interfaces** veya tek bir arayüz;
- **Search** - IP adresi, port, protokol veya kural adına göre arama (örneğin `192.168.1.50`, `:443` veya `udp`);
- **Pause** rahat okumanız için otomatik yenilemeyi durdurur; **Resume** devam ettirir.

**Log broadcast and multicast packets** - yerel ağların yayın paketleri (cihaz keşif protokolleri vb.) normalde günlük satırı üretmeden sessizce engellenir; bunları da görmek isterseniz bu kutuyu açın. Aynı ayar [**System > Settings > Firewall & NAT**](/tr/manual/system-settings-firewall-networking-tunables/) sayfasında da bulunur.

### Neler günlüklenir?

| Paketler | Ne zaman günlüklenir? |
| :--- | :--- |
| Varsayılan kuralın engelledikleri (hiçbir kural izin vermediğinde) | **Log packets blocked by the default rule** açıkken (varsayılan açık), WAN tarafında ise **Also on the WAN** da açıkken (varsayılan açık) - [**System > Settings > Firewall & NAT**](/tr/manual/system-settings-firewall-networking-tunables/) |
| Kendi kurallarınızın engellediği veya izin verdiği paketler | Kural üzerinde **Log** seçeneği açıkken ([**Firewall > Rules**](/tr/manual/firewall-rules/)) |
| IP engelleme listelerinin durdurdukları | IP engelleme listeleri için günlükleme açıkken ([**Firewall > Blocklists**](/tr/manual/firewall-blocklists/)); kural adı *IP blocklist: inbound* veya *outbound* olarak görünür |

Yerel günlük dosyası varsayılan olarak 5 MB boyuta ulaştığında döndürülür ve 3 eski dosya saklanır ([**System > Settings > Logging**](/tr/manual/system-settings-logging-notifications/)). Uzun süreli saklama için güvenlik duvarı günlüğünü uzak bir syslog sunucusuna gönderin.

> **Sayfada günlük toplayıcısının çalışmadığı uyarısı varsa**, bekleyen değişiklikleri Apply ile uygulayın (otomatik başlatılır) ya da konsoldan `service stratum_fwlog start` komutunu çalıştırın.

### Günlüğü doğru okuma

- **WAN üzerinde dünyanın dört bir yanından rastgele portlara gelen çok sayıda engelleme** - internet genelindeki otomatik taramalardır. Tamamen normal ve zararsızdır; güvenlik duvarı hepsini kapıda düşürür. Daha sakin bir günlük için **Also on the WAN** seçeneğini kapatabilirsiniz.
- **Ağınızdaki bir cihaz dışarı çıkarken engelleniyor** - ya gerekli bir geçiş kuralı eksiktir ya da engelleme kasıtlıdır. **Rule** sütunu hangi kuralın engellediğini söyler.
- **Bir bağlantı çalışmıyor ama günlükte hiçbir şey görünmüyor** - trafik ya güvenlik duvarından geçmiştir (izin verilen paketler yalnızca kuralda **Log** açıksa yazılır) ya da paket güvenlik duvarına hiç ulaşmıyordur.

## Özet (Summary)

![Diagnostics > Firewall Log > Summary](/images/manual/fwlog-summary.png)

Tüm arayüzlerde veya seçilen tek bir arayüzde **Last hour** ya da **Last 24 hours** dönemi için **Blocked**, **Passed (logged rules)** veya her ikisinin toplamlarını sunar:

- **Blocked packets**, **Different sources**, **All blocked**, **All passed (logged)**;
- saatlik engellenen ve geçen paketleri gösteren **Per hour** grafiği;
- **Top sources**, **Top destination ports**, **Top countries**, **Top rules**, **Top destinations** ve **By interface** listeleri.

Bir IP adresine, porta veya kurala tıkladığınızda ilgili kayıtlar **Live** sekmesinde süzülmüş olarak açılır.

Yapay Zeka Asistanı (**Diagnostics > AI Assistant**) özeti sizin için yorumlayabilir - *"son bir saatte neler engellendi, endişelenmem gereken bir şey var mı?"*

## Ağlar Arası Trafik (Between networks)

![Diagnostics > Firewall Log > Between networks](/images/manual/fwlog-networks.png)

Güvenlik duvarının günlüklediği **iç ağlarınız arasındaki** trafiği - **Last 24 hours**, **7 days** veya **30 days** dönemleri için - özetler:

- **Network to network**: **From**, **To**, **Passed**, **Blocked**, **Devices** ve **Last seen**;
- **Destinations and ports**: her iç ağın hangi hedefe ve porta bağlandığı, **Connections** ve **Devices** sayısı, **First seen** ve **Last seen** zamanı.

Bu kayıtlar güvenlik duvarı günlüğünün dosya döndürmesinden bağımsız olarak son görülmeden itibaren 30 gün saklanır ve her dakika güncellenir.

### Bir ağı işleri bozmadan izole etme yöntemi

Bu rapor, bugün her yere erişebilen bir iç ağı (örneğin bir IoT veya kamera VLAN'ını) kapatmadan önce *"bu ağ gerçekte hangi bağlantılara ihtiyaç duyuyor?"* sorusunu yanıtlamak için tasarlanmıştır:

1. İlgili ağın [**Firewall > Rules**](/tr/manual/firewall-rules/) sayfasında genel *allow all* kuralının **üstüne**, bu ağdan diğer iç ağlara (bir takma ad veya arayüz grubu) giden ve **Log** seçeneği açık olan bir **pass** kuralı ekleyin.
2. Bir veya iki hafta boyunca gerçek trafiği toplamasını bekleyin.
3. **Between networks > Destinations and ports** tablosuna bakın: ağın gerçekten kullandığı bağlantılar tam olarak bunlardır - örneğin kameraların port `554` üzerinden kayıt cihazına (NVR) gitmesi.
4. Yalnızca bu gerekli hedefler için izin kuralları ekleyin, hemen altına diğer iç ağlara giden trafiği kapatan bir **block** kuralı koyun ve geçici günlükleme kuralını kaldırın.
5. Artık engellenen denemeler de bu ekranda görünür - aralarında ihtiyaç duyulan meşru bir bağlantı kalmadığını kolayca doğrulayabilirsiniz.
