---
title: "Tanılama > Trafik Grafikleri"
seo_title: "10.4 Tanılama > Trafik Grafikleri (Diagnostics > Traffic Graphs) — Stratum FW Kullanım Kılavuzu"
description: "Son 1 saat, 24 saat ve 30 gün için arayüz başına ortalama ve tepe (peak) hızları gösteren bant genişliği grafikleri."
weight: 1004
chapter_num: "10.4"
url: "/tr/manual/diagnostics-traffic-graphs/"
aliases:
  - "/tr/manual/10-04-diagnostics-traffic-graphs/"
translationKey: "manual-10-04-diagnostics-traffic-graphs"
---

**Diagnostics > Traffic Graphs** her arayüzün ne kadar gelen ve giden trafik taşıdığını **son 1 saat**, **24 saat** veya **30 gün** için grafik olarak gösterir.

![Diagnostics > Traffic Graphs](/images/manual/traffic.png)

## Grafikler

Her arayüz için **In** ve **Out** eğrilerini içeren ayrı bir grafik yer alır:

| Dönem | Çözünürlük | Saklama yeri |
| :--- | :--- | :--- |
| **1 hour** | 10 saniye | bellekte (yeniden başlatma sonrasında sıfırdan başlar) |
| **24 hours** | 5 dakika, ortalama ve tepe (*peak*) | diskte |
| **30 days** | 1 saat, ortalama ve tepe (*peak*) | diskte |

*In* ve *Out* yönleri güvenlik duvarının ilgili arayüzünün bakış açısından okunur: bir **WAN** arayüzünde *In* internetten indirdiğiniz trafiktir; bir **LAN** arayüzünde ise *In* ağdaki cihazların güvenlik duvarına gönderdiği (yani cihazların yüklediği / upload) trafiktir.

**Tepe (peak)** değeri ortalamanın gizlediği kısa süreli ani yüklenmeleri gösterir - örneğin 100 Mbit/s kapasiteli bir hattın gün içinde zaman zaman tamamen dolup dolmadığını ya da gerçekten yarı kapasitede mi çalıştığını net biçimde ortaya koyar.

[Kontrol paneli](/tr/manual/dashboard/) her arayüzün anlık hızını gösterir; Yapay Zeka Asistanı ve MCP araçları da ortalama ve tepe değerlerini raporlayabilir (*"dün en yoğun arayüz hangisiydi?"*).

> **Sayfada trafik toplayıcısının çalışmadığı uyarısı varsa**, bekleyen değişiklikleri Apply ile uygulayın (otomatik başlatılır) ya da konsoldan `service stratum_traffic start` komutunu çalıştırın.

## Cihaz bazında kullanım

Buradaki grafikler arayüz bazındadır. Bant genişliğini **hangi cihazın** kullandığını görmek için:

- [**Diagnostics > States**](/tr/manual/diagnostics-tools-states-arp/) sayfasında ilgili ağı aratıp açık bağlantıların bayt sayaçlarına bakın;
- **Diagnostics > Web Activity** sayfasından hangi cihazların hangi siteleri ziyaret ettiğini inceleyin;
- uzun dönemli kurumsal grafikler için bir SNMP izleme sistemi bağlayın ([**System > Settings > SNMP**](/tr/manual/system-settings-snmp-mcp/)).

Bant genişliğini sınırlandırmak için [**Firewall > Shaper**](/tr/manual/firewall-shaper/) bölümüne bakınız.
