---
title: "Tanılama > Paket Yakalama"
seo_title: "10.6 Tanılama > Paket Yakalama (Diagnostics > Packet Capture) — Stratum FW Kullanım Kılavuzu"
description: "Bir arayüzdeki paketleri Wireshark'ta incelemek üzere .pcap dosyasına kaydetme: süzgeçler, süre/boyut sınırları, kendi oturumunuzu dışarıda bırakma, indirme ve gizlilik."
weight: 1006
chapter_num: "10.6"
url: "/tr/manual/diagnostics-packet-capture/"
aliases:
  - "/tr/manual/10-06-diagnostics-packet-capture/"
translationKey: "manual-10-06-diagnostics-packet-capture"
---

Paket yakalama (**Packet Capture**), seçilen bir arayüzden geçen paketleri kendi bilgisayarınızda **Wireshark** ile açabileceğiniz bir `.pcap` dosyasına kaydeder. Diğer tanılama araçlarının açıklayamadığı karmaşık ağ sorunlarında son sözü söyler: tel üzerinde gerçekte ne gönderilip ne alındığını paket paket görürsünüz - örneğin bir VoIP telefonun operatöründen yanıt alıp almadığını ya da bir sunucunun hiç cevap dönüp dönmediğini kesin olarak ortaya koyar.

![Diagnostics > Packet Capture](/images/manual/pcap.png)

## Yakalama ayarları (Capture)

| Alan | Anlamı |
| :--- | :--- |
| **Interface** | Paketlerin dinleneceği arayüz. |
| **Protocol** | **Any**, **TCP**, **UDP** veya **ICMP**. |
| **Host or network** | İsteğe bağlı: yalnızca belirtilen IP adresi veya ağdan gelen/giden paketleri yakalar (örneğin `192.168.1.20` veya `10.0.0.0/24`). |
| **Port** | İsteğe bağlı: yalnızca belirtilen kaynak veya hedef porta ait paketleri yakalar (örneğin `443`). |
| **Stop after (seconds)** | `10`-`600` saniye (varsayılan `60`). |
| **or this size (MB)** | `1`-`100` MB (varsayılan `20`) - süre veya boyuttan hangisi önce dolarsa yakalama durur. Yakalama ayrıca bir milyon pakete ulaştığında da otomatik durur. |
| **Leave out my own panel and SSH traffic** | Varsayılan olarak açıktır; böylece yakalama dosyası sizin kendi yönetim paneli ve SSH oturumunuzun paketleriyle dolmaz. |

**Start capture** yakalamayı başlatır. Sayfa ilerlemeyi canlı gösterir; **Stop** yakalamayı erken sonlandırır.

Mümkün olduğunca bir IP adresi veya port süzgeci kullanın: yoğun bir WAN hattı 20 MB sınırı saniyeler içinde doldurur.

**Hangi arayüz seçilmeli?** İçerideki bir cihazın ne gönderdiğini görmek için cihazın bağlı olduğu yerel arayüzü ve süzgeç olarak cihazın IP adresini seçin. Güvenlik duvarından internete - NAT çevirisinden sonra - gerçekte ne çıktığını görmek içinse WAN arayüzünde yakalama yapın.

## Son yakalama (Last capture)

**Last capture** kartı arayüzü, dosya boyutunu, süreyi, bitiş nedenini, kim tarafından başlatıldığını ve uygulanan süzgeci gösterir. **Download .pcap** dosyayı bilgisayarınıza indirir; **Delete** siler.

Aynı anda tek bir yakalama çalıştırılabilir. Dosya siz silene, yeni bir yakalama başlatılana veya 24 saat dolana kadar saklanır.

## Gizlilik

Bir paket yakalama dosyası **ağ trafiğinin bizzat kendisini** içerir: şifrelenmemiş içerikler ve eski protokollerin (FTP, Telnet, şifresiz HTTP) parolaları paketin içindedir. Yalnızca yöneticiler yakalama başlatabilir ve indirebilir; her başlatma, durdurma ve indirme işlemi sistem günlüğüne yazılır ve dosya yalnızca `root` kullanıcısının okuyabildiği bir klasörde tutulur. İşiniz bittiğinde yakalama dosyasını silin ve gerekmedikçe başkalarıyla paylaşmayın.
