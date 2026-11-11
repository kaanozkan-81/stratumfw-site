---
title: "Çoklu WAN (DHCP/PPPoE/Statik), Politika Yönlendirme ve Sanal IP'ler"
description: "Stratum Firewall üzerinde 1 saniyelik ağ geçidi izleme, dinamik DHCP ikincil WAN hatları (kablo, 4G/5G, Starlink), otomatik Çoklu WAN yedekliliği, Politika Yönlendirme, Sanal IP (VIP) ve Statik Rotalar."
lead: "İnternet kesintilerinde çevrimiçi kalın ve trafiği VLAN veya kural bazında istediğiniz hatta yönlendirin — saniyelik izleme, dinamik DHCP ek WAN hatları, otomatik failover, politika yönlendirme ve Sanal IP'ler."
---

Stratum Firewall; **Arayüzler**, **Arayüzler → Ağ Geçitleri**, **Arayüzler → Sanal IP'ler**, **Arayüzler → Rotalar** ve **Güvenlik Duvarı → Kurallar** modülleriyle eksiksiz çoklu internet hattı (Multi-WAN) ve kurumsal yönlendirme yetenekleri sunar.

## 1. Statik, PPPoE ve Dinamik DHCP Ek WAN Hatları (`v0.29.1`)

WAN rolündeki her arayüz (`WAN` veya ek WAN olarak atanan `OPT` portları); **Statik IPv4**, **PPPoE (`mpd5`)** ve **Dinamik DHCP (`v0.29.1`)** bağlantı modlarını destekler:

* **Kablo, 4G/5G LTE ve Starlink Yedek Hatları:** İkinci veya üçüncü bir internet hattı (`WAN2`, `WAN3`) adresini DHCP ile alabilir; ek bir DHCP WAN **varsayılan rotayı veya DNS sunucularını tek başına asla üzerine almaz**—hangi hattın varsayılan rotayı taşıyacağına yalnızca ağ geçidi izleyicisi (failover) karar verir.
* **Uygula Gerektirmeyen Dinamik Ağ Geçidi Takibi (`stratum-pfrefresh`):** DHCP kiralamasından gelen ağ geçidi otomatik olarak yedeklemeye, gelen bağlantı yanıtlarına (`reply-to`) ve Politika Yönlendirmeye bağlanır. Operatör yeni bir ağ geçidi verdiğinde güvenlik duvarı kuralları **Uygula (Apply) gerektirmeden saniyeler içinde** güncellenir.

## 2. Saniyelik Ağ Geçidi İzleme (`Arayüzler → Ağ Geçitleri`, `v0.16.0`)

Her WAN ağ geçidi arka planda **saniyede bir kez** denetlenir:

* **Özel İzleme IP'si:** ISS ağ geçidi ping yanıtı vermiyorsa yalnızca o WAN hattı üzerinden çıkacak şekilde harici bir hedef (`1.1.1.1` vb.) tanımlayabilirsiniz.
* **Gecikme ve Paket Kaybı Eşikleri:** Varsayılan olarak son 30 saniyede `%20` paket kaybı veya `500 ms` gecikme aşıldığında hat `Down` (Çöktü) olarak işaretlenir ve bildirim gönderilir.

## 3. Otomatik Hat Yedekleme (WAN Failover) ve Simetrik Yanıt (`v0.16.1`)

* **Öncelik Tabanlı Geçiş ve Dalgalanma Koruması:** Ana hat koptuğunda varsayılan rota anında en yüksek öncelikli sağlam yedek hatta geçer; ana hat geri geldiğinde varsayılan olarak **60 saniye** kesintisiz sağlıklı kaldıktan sonra trafiği geri devralır.
* **Simetrik Dönüş (`reply-to`):** İkinci WAN hattına (statik, PPPoE veya DHCP) gelen port yönlendirmeleri, [1:1 NAT](/tr/resources/documentation/pf-rules/) ve OpenVPN (`multihome`) bağlantıları her zaman geldiği WAN üzerinden yanıtlanır.

## 4. Kural Bazlı Politika Yönlendirme — Policy Routing (`v0.27.2`, `v0.29.1`)

Dahili bir arayüzdeki veya [Arayüz Grubundaki](/tr/resources/documentation/pf-rules/) herhangi bir **İzin (`Pass`)** kuralında trafiğin hangi WAN ağ geçidinden (**DHCP WAN dahil** — kiralama alınana kadar normal yönlendirme kullanılır) çıkacağını seçebilirsiniz:

* **Otomatik Yerel Ağ Koruması:** Yerel LAN/VLAN ağlarına, WireGuard/OpenVPN tünellerine, statik rotalı iç ağlara veya güvenlik duvarının kendisine giden trafik otomatik olarak yerel yönlendirmede kalır.
* **Hat Çökmesi Davranışı:** Seçilen ağ geçidi çöktüğünde trafiğin saniyeler içinde aktif yedek hatta düşmesini veya yalnızca o hatta bağlı kalmasını seçebilirsiniz.

## 5. Sanal IP'ler (VIP) ve Statik Rotalar (`stratum-netextra`, `v0.27.0`)

* **Sanal IP'ler (`Arayüzler → Sanal IP'ler`):** `/29` gibi birden fazla genel IP adresini arayüze tanımlayarak **Port Yönlendirme**, **[Çift Yönlü 1:1 NAT](/tr/resources/documentation/pf-rules/)** veya **Giden NAT** kurallarında kullanın.
* **Statik Rotalar (`Arayüzler → Rotalar`):** Alt yönlendiricilerin arkasındaki iç ağları tanımlayın; `stratum-netextra` Sanal IP ve rotaları `/etc/rc.conf` dosyasını kirletmeden çekirdekte senkronize tutar.
