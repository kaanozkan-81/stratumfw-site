---
title: "Arayüzler > Ağ Geçitleri"
seo_title: "05.4 Arayüzler > Ağ Geçitleri (Interfaces > Gateways) — Stratum FW Kullanım Kılavuzu"
description: "Ağ geçitleri ve çoklu WAN: her internet bağlantısını izleme, öncelik ve geri dönüş süreli WAN yedekliliği (failover), doğru WAN üzerinden yanıt dönme ve seçilen trafiği istenen WAN hattından gönderme (policy routing)."
weight: 404
chapter_num: "05.4"
url: "/tr/manual/interfaces-gateways/"
aliases:
  - "/tr/manual/05-04-interfaces-gateways/"
translationKey: "manual-05-04-interfaces-gateways"
---

WAN tipindeki her arayüzün arkasında servis sağlayıcının yönlendiricisi olan bir **ağ geçidi** (gateway) bulunur. **Interfaces > Gateways** sayfasında güvenlik duvarı bu ağ geçitlerinin her birini sürekli izler ve birden fazla internet bağlantınız olduğunda aktif hat kesilirse varsayılan rotayı (default route) çalışan diğer hatta devredebilir.

Stratum FW birden fazla WAN hattıyla şunları yapar:

| Özellik | İşlevi | Yönetildiği yer |
| :--- | :--- | :--- |
| **Gateway monitoring** (Ağ geçidi izleme) | Her ağ geçidinin gecikme süresini (latency) ve paket kaybını her saniye ölçer. | Bu sayfa |
| **WAN failover** (WAN yedekliliği) | Aktif ağ geçidi düştüğünde varsayılan rotayı sıradaki çalışan ağ geçidine taşır; ana hat düzeldiğinde geri alır. | Bu sayfa |
| **Replies on the right WAN** (Doğru WAN'dan yanıt) | Bir WAN hattından içeri gelen bağlantılara verilen yanıtlar her zaman aynı WAN hattından geri çıkar - ek ayar gerektirmez. | Otomatik |
| **Policy routing** (Politika yönlendirme) | Seçilen bir kurala uyan trafiği belirlediğiniz WAN hattından çıkarır - örneğin misafir ağını ikinci internet hattından gönderme. | **Firewall > Rules** |

Stratum FW tek bir ağın bağlantılarını aynı anda birden fazla hatta dağıtmaz (yük dengeleme / load balancing yapmaz): her kuralın trafiği belirli bir anda tek bir hattı kullanır.

## Ağ geçidi listesi

![Interfaces > Gateways](/images/manual/gw-list.png)

| Sütun | Anlamı |
| :--- | :--- |
| **Name** | Arayüzün açıklamasından üretilir: *Fibre* açıklaması `Fibre_GW`, açıklaması olmayan bir arayüz `WAN_GW` adını alır. |
| **Interface** | İlgili WAN tipindeki arayüz. |
| **Gateway** | Servis sağlayıcının yönlendiricisi: sabit adreste girdiğiniz ağ geçidi, DHCP ile alınan adres veya PPPoE oturumu. |
| **Monitor** | Ping gönderilen adres: ağ geçidinin kendisi veya belirlediğiniz özel izleme adresi. |
| **Priority** | Yedeklilik (failover) sırası: daha küçük sayı daha yüksek öncelik demektir. |
| **Status** | **online**, **warning**, **down** veya **pending** (henüz ölçülüyor); çevrimiçi olmadığında nedeni de yanında yazılır - örneğin *no link*, *packet loss 35 %*, *latency 620 ms*. |
| **Latency**, **Loss** | Son 30 saniyenin gecikme ve paket kaybı ortalamaları. |
| **Last hour** | Son bir saatin küçük gecikme grafiği. |

## Bir ağ geçidinin durumu nasıl değerlendirilir

Her ağ geçidine saniyede bir ping gönderilir ve bu ping her zaman o hattın kendi üzerinden çıkar - böylece yedek bir hat varsayılan rotayı taşımadığı sırada bile sürekli ölçülür. Karar son 30 saniyelik ölçüme göre verilir:

| Durum | Koşul |
| :--- | :--- |
| **down** | Arayüzde fiziksel hat (link), IP adresi veya ağ geçidi yoksa; ya da paket kaybı **Down at packet loss** sınırına (varsayılan %20) veya ortalama gecikme **Down at latency** sınırına (varsayılan 500 ms) ulaştığında. |
| **warning** | Sınırlardan herhangi birinin yarısına ulaşıldığında (varsayılanlarla %10 paket kaybı veya 250 ms gecikme). Hat geçişi yapılmaz. |
| **pending** | Yeni başladığında veya adres yeni değiştiğinde: henüz beklenen yanıt sayısı 10'dan azdır ve hiç yanıt alınmamıştır. |
| **online** | Diğer tüm normal durumlar. |

Düşen veya yeniden çevrimiçi olan bir ağ geçidi panel bildirimlerinde (ve ayarlanmışsa Telegram ya da e-posta ile) raporlanır.

## Ağ geçidi ayarları

Ayarlarını değiştirmek için listedeki bir ağ geçidine tıklayın.

![Bir ağ geçidini düzenleme](/images/manual/gw-edit.png)

| Ayar | Anlamı |
| :--- | :--- |
| **Description** | Ağ geçidi için isteğe bağlı açıklama notu. |
| **Monitor address** | Ping gönderilecek IP adresi. Boş bırakılırsa ağ geçidinin kendisine ping atılır. Pek çok servis sağlayıcı yönlendiricisi ping yanıtı vermez; bu durumda `1.1.1.1` veya `8.8.8.8` gibi genel bir adres kullanın. Her ağ geçidinin kendine ait farklı bir izleme adresi olmalıdır (güvenlik duvarı her izleme adresini ilgili hattın içinden yönlendirir) ve bu adres iç ağlarınızdan birinin içinde yer alamaz. |
| **Priority** | 1 ile 255 arası; küçük değer daha önceliklidir. WAN yedekliliği açıkken ayakta olan en iyi öncelikli ağ geçidi varsayılan rotayı taşır. Varsayılan olarak ağ geçitleri arayüz sırasına göre 10, 20, 30 ... şeklinde numaralandırılır; dolayısıyla ana WAN önceliklidir. |
| **Mark the gateway down by packet loss and latency** | Varsayılan olarak açıktır. Kapatıldığında yalnızca fiziksel hat veya IP kaybı ağ geçidini *down* olarak işaretler - hat kalitesini ölçmek istemediğiniz ya da doğası gereği yüksek gecikmeli olan bağlantılar (uydu, bazı mobil hatlar) için kullanışlıdır. |
| **Down at packet loss (%)** | 1 ile 100 arası. Varsayılan 20. |
| **Down at latency (ms)** | 0 ile 10000 arası. Varsayılan 500. `0` girilirse gecikme süresi hiçbir zaman hattı düşürmez. |

> **İpucu:** Servis sağlayıcının yönlendiricisinden daha ilerideki bir hedefi izleyin. Yönlendiricinin ping yanıtı vermesi yalnızca ona giden kablonun çalıştığını kanıtlar; o hat üzerinden `1.1.1.1` adresine ulaşabilmek ise internetin gerçekten erişilebilir olduğunu kanıtlar. Her WAN hattı için farklı bir genel IP adresi kullanın.

## WAN yedekliliği (WAN failover)

En az iki WAN tipinde arayüz bulunduğunda listenin altında **WAN failover** kartı görünür:

| Ayar | Anlamı |
| :--- | :--- |
| **Switch the default route to the next gateway (by priority) when the active one is down** | WAN yedekliliğini açar. Varsayılan olarak kapalıdır: kapalıyken ne olursa olsun varsayılan rota ana WAN üzerinde kalır. |
| **Return to a preferred gateway after (s)** | Daha yüksek öncelikli bir ağ geçidi tekrar çevrimiçi olduğunda varsayılan rotanın ona geri dönmesi için kesintisiz çevrimiçi kalması gereken süre (varsayılan 60 saniye, 0 ile 3600 arası). Gidip gelen kararsız bir hattın trafiği sürekli ileri geri sallamasını önler. |
| **Close the connections of the gateway that went down** | Varsayılan olarak açıktır. Düşen hat üzerindeki açık bağlantılar kapatılır; böylece istemci cihazlar kendi zaman aşımlarını beklemek yerine anında yeni hat üzerinden yeniden bağlanır. |

Bu ayarlar **Apply changes** sonrasında devreye girer.

**Bir kesinti anında neler olur:**

1. Aktif ağ geçidi **down** olarak değerlendirilir.
2. Varsayılan rota ayakta olan en yüksek öncelikli ağ geçidine taşınır. Bu geçiş günlüğe yazılır ve bildirim olarak iletilir.
3. Seçenek açıksa düşen hat üzerindeki mevcut bağlantılar kapatılır; cihazlar yeni hat üzerinden hızla yeniden bağlanır.
4. Tercih edilen ana ağ geçidi geri dönüş süresi (failback) boyunca kesintisiz çevrimiçi kaldığında varsayılan rota tekrar ana hatta döner.

Kontrol panelindeki (**Dashboard**) **Gateways** kartı varsayılan rotayı hangi ağ geçidinin ne zamandan beri taşıdığını canlı gösterir.

> **Not:** Yedeklilik (failover) güvenlik duvarının *varsayılan* rotasını taşır. Bir kuralın özellikle belirli bir WAN hattına yönlendirdiği trafik (aşağıdaki politika yönlendirmesi), ağ geçidi düştüğünde o kuralın kendi ayarına uyar.

## Gelen bağlantılara aynı WAN üzerinden yanıt dönme

Birden fazla WAN bulunduğunda bir WAN hattından içeri giren her bağlantı - port yönlendirme, VPN veya panel erişimi - varsayılan rotayı hangi hat taşıyor olursa olsun her zaman geldiği WAN hattından yanıtlanır. Bu mekanizma olmasaydı yanıt paketi diğer servis sağlayıcıdan çıkar ve karşı tarafça düşürülürdü. Stratum FW bunu sabit IP, DHCP ve PPPoE WAN hatlarının tamamında otomatik yapar; herhangi bir ayar gerekmez.

## Politika tabanlı yönlendirme (Policy routing): seçilen trafiği seçilen WAN'dan gönderme

Belirli bir trafiği özellikle seçtiğiniz bir internet hattından çıkarmak için - örneğin misafir ağını ikinci servis sağlayıcıdan veya bir sunucunun yedekleme trafiğini daha ekonomik hattan göndermek için - ilgili kural üzerinde bir **Gateway** seçin:

1. **Firewall > Rules** sayfasında ilgili iç arayüzün (veya tüm üyeleri iç arayüz olan bir grubun) sekmesini açın.
2. Trafiğe izin veren *pass* kuralını düzenleyin (veya yeni bir kural ekleyin).
3. **Gateway** alanında istediğiniz WAN hattını seçin. **Default (routing table)** normal yönlendirme tablosunu kullanır.
4. **If the gateway is down** (Seçilen ağ geçidi düşerse):
   - **Use the default route** - standart seçim: seçilen hat kesikken trafik normal varsayılan rotayı (yedeklilik dahil) kullanır, hat düzeldiğinde tekrar kendi hattına döner.
   - **Keep using this gateway (traffic fails)** - başka bir hattan asla çıkmaması gereken (örneğin diğer servis sağlayıcının görmemesi gereken) trafik için kullanılır.
5. Kaydedin ve **Apply changes** düğmesine tıklayın.

Böyle bir kuralda kendi yerel ağlarınıza - iç ağlara, VPN ağlarına ve güvenlik duvarının kendisine - giden trafik WAN'a gönderilmez: güvenlik duvarı bu trafiği otomatik olarak yerel tutar; dolayısıyla hedefi *any* olan bir kural bile misafirlerin güvenlik duvarındaki yerel DNS servisine ulaşmasına izin verir.

Bir ağ geçidi düştüğünde veya geri geldiğinde kurallar birkaç saniye içinde kendiliğinden yeniden yüklenir; halihazırda açık olan bağlantılar kesilmeden devam eder.

## Birden fazla WAN ve DHCP

Ana WAN arayüzünün DHCP kirası her zamanki gibi varsayılan rotayı ve DNS sunucularını belirler. Adresini DHCP ile alan ek bir WAN arayüzü ise varsayılan rotayı veya DNS sunucularını hiçbir zaman kendi başına devralmaz: kirasındaki ağ geçidi yalnızca yedeklilik, yanıt trafiği ve politika yönlendirmesi için kullanılır ve kira yenilendiğinde yeni ağ geçidini otomatik izler.
