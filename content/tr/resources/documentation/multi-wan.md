---
title: "Çoklu WAN, Politika Yönlendirme ve Sanal IP'ler"
description: "Stratum Firewall üzerinde 1 saniyelik ağ geçidi izleme, otomatik Çoklu WAN yedekliliği (failover), kural bazlı Politika Yönlendirme, Sanal IP'ler (VIP) ve Statik Rotalar."
lead: "İnternet kesintilerinde çevrimiçi kalın ve trafiği VLAN veya kural bazında istediğiniz hatta yönlendirin — ağ geçidi izleme, otomatik failover, politika yönlendirme, Sanal IP ve statik rotalar."
---

Stratum Firewall; **Arayüzler → Ağ Geçitleri**, **Arayüzler → Sanal IP'ler**, **Arayüzler → Rotalar** ve **Güvenlik Duvarı → Kurallar** modülleriyle tam kapsamlı çoklu internet hattı (Multi-WAN) ve kurumsal yönlendirme yetenekleri sunar.

## 1. Saniyelik Ağ Geçidi İzleme (`Arayüzler → Ağ Geçitleri`, `v0.16.0`)

WAN rolündeki her arayüz (`WAN` veya WAN tipinde atanmış `OPT` portları), arka planda çalışan izleme servisi tarafından **saniyede bir kez** denetlenir:

* **Özel İzleme IP'si:** ISS ağ geçidi ping yanıtı vermiyorsa, yalnızca o WAN hattı üzerinden çıkacak şekilde harici bir hedef (`1.1.1.1` vb.) tanımlayabilirsiniz.
* **Gecikme ve Paket Kaybı Eşikleri:** Varsayılan olarak son 30 saniyede `%20` paket kaybı veya `500 ms` gecikme aşıldığında hat `Down` (Çöktü) olarak işaretlenir ve bildirim gönderilir.

## 2. Otomatik Hat Yedekleme (WAN Failover) ve Simetrik Yanıt (`v0.16.1`)

* **Öncelik Tabanlı Geçiş:** Ana internet hattı koptuğunda varsayılan rota anında en yüksek öncelikli sağlam yedek hatta aktarılır. Ana hat geri geldiğinde dalgalanmayı (flapping) önlemek için varsayılan olarak **60 saniye** kesintisiz sağlıklı kaldıktan sonra trafiği geri devralır.
* **Simetrik Dönüş (`reply-to`):** İkinci WAN hattına gelen port yönlendirmeleri ve OpenVPN (`multihome`) bağlantıları, varsayılan rota hangi hatta olursa olsun geldiği WAN üzerinden yanıtlanır.

## 3. Kural Bazlı Politika Yönlendirme — Policy Routing (`v0.27.2`)

Dahili bir arayüzdeki veya [Arayüz Grubundaki](/tr/resources/documentation/pf-rules/) herhangi bir **İzin (`Pass`)** kuralında trafiğin hangi WAN ağ geçidinden (statik veya PPPoE) çıkacağını seçebilirsiniz (örneğin misafir VLAN ağını ikinci internet hattından çıkarmak):

* **Otomatik Yerel Ağ Koruması:** Bu kurala giren cihazların yerel LAN/VLAN ağlarına, WireGuard/OpenVPN tünellerine, statik rotalı iç ağlara veya güvenlik duvarının kendisine giden trafiği otomatik olarak yerel yönlendirmede kalır; ISS'ye gönderilmez.
* **Hat Çökmesi Davranışı:** Seçilen ağ geçidi çöktüğünde trafiğin saniyeler içinde varsayılan rotaya (yedek hatta) düşmesini veya yalnızca o hatta bağlı kalmasını seçebilirsiniz.

## 4. Sanal IP'ler (VIP) ve Statik Rotalar (`stratum-netextra`, `v0.27.0`)

* **Sanal IP'ler (`Arayüzler → Sanal IP'ler`):** `/29` gibi birden fazla genel IP adresine sahip WAN hatlarında ek IP'leri arayüze tanımlayın; her genel IP'yi farklı bir iç sunucuya port yönlendirin veya Giden NAT'ta çıkış IP'si olarak kullanın.
* **Statik Rotalar (`Arayüzler → Rotalar`):** İç ağdaki alt yönlendiricilerin arkasındaki ağları tanımlayın; bu ağlar internete otomatik olarak NAT'lanır ve `/etc/rc.conf` kirletilmeden `stratum-netextra` ile çekirdekte senkronize tutulur.
