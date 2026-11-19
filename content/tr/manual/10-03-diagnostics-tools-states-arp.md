---
title: "Tanılama > Ping / Traceroute / DNS, Durum Tablosu ve ARP Tablosu"
seo_title: "10.3 Tanılama > Ping / Traceroute / DNS, States ve ARP Table — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarı üzerinden erişilebilirlik, rota ve DNS testi yapma, durum tablosundaki (States) aktif bağlantıları görme ve sonlandırma, ARP tablosu ve Wake-on-LAN ile cihaz uyandırma."
weight: 1003
chapter_num: "10.3"
url: "/tr/manual/diagnostics-tools-states-arp/"
aliases:
  - "/tr/manual/10-03-diagnostics-tools-states-arp/"
translationKey: "manual-10-03-diagnostics-tools-states-arp"
---

## Ping / Traceroute / DNS

![Diagnostics > Ping](/images/manual/diag-tools.png)

Bu araçlar doğrudan **güvenlik duvarının üzerinde** çalışır - güvenlik duvarının kendisinin nereye ulaşabildiğini gösterir; bu da bir sorunun güvenlik duvarında mı, internet hattında mı yoksa istemci cihazda mı olduğunu ayırt etmenin en hızlı yoludur.

Üç sekme bulunur:

| Araç | Alanlar |
| :--- | :--- |
| **Ping** | **Target** (IP adresi veya alan adı), **Source interface**, **Count** (`1`-`10`, varsayılan `4`). |
| **Traceroute** | **Target**, **Source interface**, **Maximum hops** (`1`-`30`, varsayılan `20`). |
| **DNS lookup** | **Target** (bir alan adı, örneğin `freebsd.org` veya `nas.lan`), **Record type** (`A`, `AAAA`, `MX`, `TXT`, `NS`, `CNAME`, `SOA`, `PTR`), **DNS server** (boş bırakılırsa bu güvenlik duvarı). |

**Source interface** - **Automatic (routing table)** veya kaynak adresi kullanılacak belirli bir arayüz. Belirli bir WAN hattını tek başına sınamak ya da **bir VPN tünelinin içinden** test yapmak için kullanın: kaynak olarak LAN arayüzünü seçip karşı şubedeki bir IP adresine ping attığınızda paket tıpkı içerideki cihazların trafiği gibi noktadan noktaya VPN tünelinden geçer.

**Run** çıktıyı geldikçe canlı gösterir; **Stop** işlemi durdurur. Operatör rolündeki kullanıcılar da bu araçları kullanabilir.

Tipik tanılama kontrolleri:

| Soru | Yapılacak test |
| :--- | :--- |
| Güvenlik duvarı internete çıkıyor mu? | `1.1.1.1` adresine Ping atın. |
| Alan adları çözülüyor mu? | `freebsd.org` için DNS sorgusu yapın. DNS çalışıyor ama `1.1.1.1` ping yanıtlamıyorsa internet hattı kesiktir; ping çalışıyor ama DNS yanıtlamıyorsa sorun DNS ayarlarındadır. |
| WAN 2 hattı tek başına çalışıyor mu? | Kaynak arayüz olarak WAN 2 seçip `1.1.1.1` adresine Ping atın. |
| Şube VPN tüneli çalışıyor mu? | Kaynak arayüz olarak LAN seçip karşı şubedeki bir cihaza Ping atın. |
| Paketler yol üzerinde nerede kesiliyor? | Hedefe **Traceroute** çalıştırın. |

## Durum Tablosu (States)

![Diagnostics > States](/images/manual/diag-states.png)

**Durum tablosu (state table)** güvenlik duvarının o anda izlediği her aktif bağlantıyı listeler. Her satırda yönüyle birlikte **Interface**, **Protocol**, **Source**, **Destination** (NAT sonrası adresle birlikte), **State**, **Age**, **Packets**, **Bytes** ve bağlantıya izin veren **Rule** gösterilir.

**Search** alanından IP adresi, port, protokol, arayüz veya kural adına göre süzebilirsiniz. Sayfa toplam durum sayısını ve ilk 500 eşleşmeyi gösterir; her 10 saniyede bir yenilenir.

**Kill** düğmesi seçilen tek bir bağlantıyı (onay sonrasında) anında sonlandırır - örneğin bir güvenlik duvarı kuralını değiştirdikten sonra hemen kesilmesi gereken açık bir bağlantı için kullanılır: değiştirilen kurallar yeni bağlantılara uygulanır, önceden açılmış mevcut bağlantılar ise sonlandırılana kadar devam eder.

Kullanımdaki ve izin verilen en yüksek durum sayısı ayrıca [**System > Settings > Firewall & NAT**](/tr/manual/system-settings-firewall-networking-tunables/) sayfasında ve [kontrol panelinde](/tr/manual/dashboard/) gösterilir.

## ARP Tablosu (ARP Table)

![Diagnostics > ARP Table](/images/manual/diag-arp.png)

Güvenlik duvarının yakın zamanda kendi yerel ağlarında gördüğü cihazları listeler: **IP address**, **MAC address**, **Name** (DHCP'den), **Interface** ve kaydın ne zaman sona ereceği (**Expires**). **Search** ile IP adresi, MAC, cihaz adı veya arayüze göre arama yapabilirsiniz.

**Delete** bir ARP kaydını siler; cihazdan gelen ilk pakette adres yeniden öğrenilir - özellikle bir cihaz değiştirilip eski cihazın IP adresini devraldığında kullanışlıdır.

ARP değişiklikleri (bir IP adresinin başka bir MAC adresine geçmesi) sistem günlüğüne kaydedilir - ayrıntılar için [**System > Settings > Networking**](/tr/manual/system-settings-firewall-networking-tunables/) bölümüne bakınız.

### Wake-on-LAN (Ağ Üzerinden Uyandırma)

Kapalı bir bilgisayarı yerel ağ üzerinden uzaktan açmak için:

- ARP tablosundaki veya **Devices with a DHCP reservation** listesindeki cihazın yanında **Wake** düğmesine tıklayın; ya da
- **Wake a device** kartından **Interface** seçin, **MAC address** girin ve **Send wake-up** düğmesine tıklayın.

Hedef bilgisayarın BIOS / ağ kartı ayarlarında Wake-on-LAN özelliğinin açık olması ve ağa kabloyla bağlı olması gerekir.
