---
title: "Sistem > Ayarlar: Güvenlik Duvarı & NAT, Ağ ve Çekirdek Parametreleri"
seo_title: "09.2 Sistem > Ayarlar: Firewall & NAT, Networking ve Tunables — Stratum FW Kullanım Kılavuzu"
description: "Engellenen bağlantıların yanıtlanma ve günlüklenme biçimi, bogon ve özel ağlar, WAN ping, durum tablosu ve zaman aşımları, SYN flood koruması, NAT yansıması, statik port, donanım hızlandırma, işlemci güç yönetimi, ARP günlükleme ve sysctl çekirdek parametreleri."
weight: 902
chapter_num: "09.2"
url: "/tr/manual/system-settings-firewall-networking-tunables/"
aliases:
  - "/tr/manual/09-02-system-settings-firewall-networking-tunables/"
translationKey: "manual-09-02-system-settings-firewall-networking-tunables"
---

Bu üç sekme güvenlik duvarının ileri düzey ağ ve paket filtresi ayarlarını barındırır. Varsayılan değerler hemen her ağ için güvenlidir: **her seferinde tek bir ayarı değiştirin** ve bir sonrakine geçmeden önce güvenlik duvarı günlüğünü ve kontrol panelini gözlemleyin.

## Güvenlik Duvarı ve NAT (Firewall & NAT)

![System > Settings > Firewall & NAT](/images/manual/set-firewall.png)

### Filtreleme (Filtering)

| Ayar | Varsayılan | Anlamı |
| :--- | :--- | :--- |
| **Blocked connections** | `Drop silently` | **Drop silently**: karşı tarafa hiçbir yanıt dönülmez; dış tarayıcılar hiçbir tepki görmez. **Reject**: göndericiye anında TCP sıfırlama (`RST`) veya ICMP erişilemez mesajı döner - kendi iç ağlarınızdaki cihazlar zaman aşımı beklemek yerine anında haberdar olur. Eylemi açıkça *Reject* seçilmiş kurallar her zaman reddederek yanıtlar. |
| **Log packets blocked by the default rule** | açık | Hiçbir kuralın izin vermediği paketler güvenlik duvarı günlüğüne yazılır. |
| **Also on the WAN** | açık | Kapatıldığında internet taramaları ve kapanmış bağlantılardan WAN'a geç gelen yanıtlar günlük satırı üretmeden sessizce düşürülür - çok daha sakin bir günlük sağlar. İç arayüzlerde ve VPN'lerdeki engellemeler günlüklenmeye devam eder. |
| **Log blocked broadcast and multicast noise** | kapalı | Yerel ağlardaki yayın paketleri (cihaz keşif protokolleri vb.) günlük satırı oluşturmadan sessizce engellenir. |
| **Block bogon networks (from the internet)** | açık | WAN üzerinden gelen ayrılmış ve henüz kimseye tahsis edilmemiş IP aralıklarını engeller. |
| **Block private networks (from the internet)** | kapalı | WAN üzerinden gelen `10/8`, `172.16/12`, `192.168/16`, `100.64/10` paketlerini engeller. **WAN arayüzü özel IP dağıtan bir üst modem veya yönlendirici arkasındaysa kapalı bırakın.** |
| **Answer ping on the WAN address** | açık | Dış izleme ve sorun giderme için yararlıdır; kapatıldığında güvenlik duvarı dışarıdan gelen ping isteklerine görünmez olur. |

### Durum tablosu ve performans (State table and performance)

| Ayar | Anlamı |
| :--- | :--- |
| **Maximum connections (states)** | Güvenlik duvarının aynı anda izleyebileceği en fazla bağlantı sayısı. Boş veya `0`: `pf` varsayılanı (`100.000`). Sayfa kurulu belleğe uygun bir değer önerir ve o anda kaç durum kaydının kullanımda olduğunu gösterir. Büyük misafir ağlarında artırın. |
| **Timeouts for idle connections** | **Normal** (varsayılan); uydu hatları için **High latency**; boşta kalan SSH ve VPN bağlantılarını daha uzun yaşatan **Conservative**; boşta kalan bağlantıları hızla temizleyen **Aggressive** (çok sayıda misafirin binlerce bağlantı açtığı ağlarda yararlıdır). |
| **SYN flood protection** | Durum tablosu dolduğunda SYN çerezlerini (*SYN cookies*) devreye alır. Varsayılan kapalıdır. |
| **Randomise the IP ID of outgoing packets** | Güvenlik duvarı arkasındaki cihazların işletim sistemi parmak izini gizler. Varsayılan kapalıdır. |
| **Clear the don't-fragment bit** | Yalnızca parçalanmış paketlerde sorun yaşayan özel cihazlar (bazı eski NFS istemcileri gibi) için kullanılır. Varsayılan kapalıdır. |

### NAT

| Ayar | Anlamı |
| :--- | :--- |
| **NAT reflection** | Yerel ağdaki cihazların bir port yönlendirme kuralına güvenlik duvarının dış IP adresi üzerinden (`https://<genel IP>`) erişebilmesini sağlar. Varsayılan kapalıdır. [**Services > DNS Resolver**](/tr/manual/services-dns-resolver/) üzerinde bir *host override* tanımlamak genellikle çok daha temiz çözümdür. |
| **Keep source ports in outbound NAT (static port)** | Giden NAT çevirisinde kaynak port numarasını değiştirmeden korur. Yalnızca buna ihtiyaç duyan uygulamalar (bazı VoIP santralleri ve oyun konsolları) için açılır; varsayılan kapalıdır. |

Bu sekmedeki tüm ayarlar **Apply changes** ile devreye girer - `pf` yeniden yüklenir ve açık bağlantılar kopmadan devam eder.

## Ağ (Networking)

![System > Settings > Networking](/images/manual/set-networking.png)

### Donanım hızlandırma (Hardware offload)

Ağ kartları bazı paket işlemlerini CPU üzerinden devralabilir. Ancak bir yönlendirici ve güvenlik duvarında bu özelliklerin bir kısmı yarardan çok zarar verir: LRO yönlendirilecek paketleri birleştirir, bazı ağ kartı sürücüleri ise VLAN ve `pf` ile birlikte TSO veya sağlama toplamlarını (checksum) hatalı işler.

| Ayar | Anlamı |
| :--- | :--- |
| **Switch off LRO (large receive offload)** | Yönlendirici/güvenlik duvarı üzerinde kapatılması önerilir. |
| **Switch off TSO (TCP segmentation offload)** | Güvenlik duvarı üzerinden geçiş hızı beklenenden düşükse veya bağlantılar takılıyorsa kapatın. |
| **Switch off checksum offload** | Yalnızca ağ kartı paketleri bozuyorsa kapatın. |

Bu ayarlar tüm fiziksel portlara uygulanır (VLAN'lar üst fiziksel portun ayarını kullanır); portlar yeniden başlatılmaz. Varsayılan olarak hepsi kapalıdır (kartın kendi varsayılanı korunur).

### İşlemci gücü (CPU power)

**CPU frequency**: **Not managed** (sistem varsayılanı), **Adaptive** (yük altında tam hız, boştayken tasarruf), **Maximum performance** (sürekli en yüksek frekans) veya **Economy** (en düşük güç tüketimi).

### ARP ve IPv6

| Ayar | Anlamı |
| :--- | :--- |
| **Log when an address moves to another MAC address** | ARP zehirlenmesi (*ARP spoofing*) girişimine veya ağda değiştirilen bir cihaza işaret eder. Varsayılan açıktır. |
| **Log ARP replies on the wrong interface** | Kablolama veya VLAN yapılandırma hatasına işaret eder. Varsayılan açıktır. |
| **Forget ARP entries after ... seconds** | Varsayılan `1200` saniye. |
| **IPv6** | Henüz desteklenmemektedir: tüm IPv6 trafiği engellenir ve her arayüzde IPv6 kapalıdır. Destek aşamalı olarak eklenecektir. |

Değişiklikler **Apply changes** ile etkinleşir ve onaylanmazsa otomatik olarak geri alınır.

## Çekirdek Parametreleri (Tunables)

![System > Settings > Tunables](/images/manual/set-tunables.png)

Bir güvenlik duvarında önem taşıyan FreeBSD `sysctl` çekirdek değerleri ve pfSense'in de önerdiği referans değerler:

| Parametre (Tunable) | Önerilen | Ne işe yarar |
| :--- | :--- | :--- |
| `net.inet.tcp.blackhole` | `2` | Güvenlik duvarının kendi kapalı TCP portlarına gelen paketlere `RST` dönmez (taramalardan gizler) |
| `net.inet.udp.blackhole` | `1` | Güvenlik duvarının kendi kapalı UDP portlarına ICMP *port-unreachable* dönmez |
| `net.inet.ip.random_id` | `1` | Güvenlik duvarının kendisinin ürettiği paketlerde rastgele IP ID kullanır |
| `net.inet.tcp.drop_synfin` | `1` | Hem SYN hem FIN bayrağı açık geçersiz TCP paketlerini düşürür (tarayıcı hilesi) |
| `net.inet.icmp.drop_redirect` | `1` | Gelen ICMP yönlendirme mesajlarını yok sayar |
| `net.inet.ip.redirect` | `0` | ICMP yönlendirme mesajı göndermez |
| `net.inet.tcp.syncookies` | `1` | Güvenlik duvarının kendi TCP servisleri için SYN çerezlerini etkinleştirir |
| `net.inet.icmp.icmplim` | `200` | Saniyede gönderilecek en fazla ICMP hata yanıtı sayısı |
| `net.inet.ip.intr_queue_maxlen` | `1000` | Yönlendirilen paketler için giriş kuyruğu uzunluğu (küçük paket patlamaları için) |
| `kern.ipc.soacceptqueue` | `1024` | Dinleyen soket başına bekleyen bağlantı kuyruğu (panel, DNS) |
| `kern.ipc.maxsockbuf` | `4262144` | En büyük soket tampon belleği (yüksek hızlı VPN ve büyük dosya aktarımları için) |

Her parametre için **Default** sistemin kendi varsayılanını bırakır; buradan girilen bir değer ise **Apply** ile anında uygulanır ve yeniden başlatmalar sonrasında da korunur. Tekrar varsayılana döndürüldüğünde önceki değer geri yüklenir. **Use all recommended values** önerilen tüm değerleri tek tıkla doldurur.

**Kendi parametreleriniz (Own tunables)** - **Add tunable** ile ad, değer ve açıklama ekleyebilirsiniz. Yalnızca çalışma zamanında değişebilen (`run-time`) `sysctl` değerleri kabul edilir (açılışta yüklenen `loader` parametreleri sunulamaz) ve Stratum FW'nin doğrudan kendisinin yönettiği kritik değerler (yönlendirme, `pf`, güvenlik seviyeleri ...) reddedilir.
