---
title: "Servisler > DHCP Sunucusu"
seo_title: "07.2 Servisler > DHCP Sunucusu (Services > DHCP Server) — Stratum FW Kullanım Kılavuzu"
description: "İç ağlarda otomatik IP adresi dağıtımı: adres aralığı, kira süresi, istemciler için ağ geçidi ve DNS, statik eşlemeler ve aktif kiralamalar."
weight: 702
chapter_num: "07.2"
url: "/tr/manual/services-dhcp-server/"
aliases:
  - "/tr/manual/07-02-services-dhcp-server/"
translationKey: "manual-07-02-services-dhcp-server"
---

DHCP sunucusu bir iç ağdaki cihazlara IP adreslerini, kullanmaları gereken ağ geçidi (gateway) ve DNS sunucusu bilgileriyle birlikte otomatik olarak dağıtır. Statik IP adresine sahip her LAN tipi arayüzün **Services > DHCP Server** sayfasında kendine ait ayrı bir sekmesi ve DHCP sunucusu vardır.

![Services > DHCP Server](/images/manual/dhcp.png)

## Ayarlar

| Ayar | Anlamı |
| :--- | :--- |
| **Enable the DHCP server on this interface** | Bu ağ için DHCP sunucusunu etkinleştirir. |
| **Range start**, **Range end** | Statik eşlemesi olmayan cihazlara dağıtılacak IP adresi aralığı. Aralığın arayüzün kendi alt ağı içinde kalması gerekir. Aralık güvenlik duvarının kendi arayüz adresini veya ağ geçidini kapsasa bile bu adresler hiçbir zaman istemcilere dağıtılmaz. |
| **Lease time** | Bir cihazın aldığı IP adresini yenilemeden önce ne kadar süre tutacağı: 1 saat, 4 saat, 12 saat (varsayılan), 1 gün veya 7 gün. Kısa kira süreleri yoğun misafir ağlarında boşa çıkan adresleri hızla geri kazandırır; uzun kira süreleri sabit ofis ağları için uygundur. |
| **Gateway for clients** | Boş bırakıldığında güvenlik duvarının bu ağdaki kendi IP adresi verilir (standart tercih). Yalnızca cihazların başka bir yönlendiriciyi kullanması gerektiğinde farklı bir adres girin. |
| **DNS servers for clients** | Boş bırakıldığında güvenlik duvarının kendisi verilir; böylece statik eşlemelerin yerel adları çözülür, [DNS engelleme listeleri](/tr/manual/firewall-blocklists/) ve [Web Filtresi](/tr/manual/firewall-web-filter/) uygulanır. Başka DNS sunucuları vermek isterseniz virgülle ayrılmış en fazla üç IP adresi yazabilirsiniz. |

**Save settings** düğmesine tıklayın, ardından **Apply changes** ve **Keep changes** ile onaylayın.

> **Not:** Özel bir nedeniniz yoksa **DNS servers for clients** alanını boş bırakın. Başka DNS sunucuları alan cihazlar DNS engelleme listelerini ve Web Filtresini atlar; ayrıca alan adı sorguları Web Activity ekranında görünmez. Captive Portal, DHCP sunucusu dış DNS dağıtan bir bölgeyi (zone) kabul etmez.

**Aralığı ağın büyüklüğüne göre planlayın.** Yüzlerce telefonun bağlandığı bir otel misafir ağı geniş bir alt ağa (örneğin yaklaşık 1000 adreslik `/22`) ve kısa bir kira süresine ihtiyaç duyar; 12 saat kira süreli bir `/24` ağ yoğun misafir giriş-çıkışında hızla dolar. Ağ büyüklüğü [**Interfaces > Assignments**](/tr/manual/interfaces-assignments/) altından belirlenir; alt ağ değiştirildiğinde DHCP aralığı yeni ağa uyacak şekilde otomatik kaydırılır.

## Statik eşlemeler (Static mappings)

Statik eşleme, DHCP sunucusunun MAC adresinden tanıdığı belirli bir cihaza her zaman aynı IP adresini vermesini sağlar. Yazıcılar, sunucular, kameralar, kablosuz erişim noktaları (AP) ve diğer cihazların ya da güvenlik duvarı kurallarının sabit adresle eriştiği her donanım için kullanın.

**Add mapping** düğmesine tıklayın ya da daha hızlı bir yöntem olarak **Active leases** tablosundaki bir cihazın satırında **Make static** düğmesini kullanın: form cihazın mevcut IP adresi ve adıyla hazır doldurulmuş olarak açılır.

| Alan | Anlamı |
| :--- | :--- |
| **MAC address** | Cihazın donanım adresi. Yaygın tüm yazım biçimleri kabul edilir (`aa:bb:cc:dd:ee:ff`, `aa-bb-cc-dd-ee-ff`, `aabb.ccdd.eeff`). Her MAC adresi yalnızca tek bir eşlemeye sahip olabilir. |
| **IP address** | Cihaza verilecek, arayüzün kendi ağı içindeki IP adresi. Dinamik dağıtım aralığının içinde veya dışında olabilir. Her IP adresi tek bir eşlemeye ait olabilir. |
| **Hostname** | İsteğe bağlı; harf, rakam ve kısa çizgi içerebilir. Güvenlik duvarının DNS servisi bu adı yerel alan adı altında çözer - örneğin `printer-2f.lan`. |
| **Description** | Açıklama notunuz, örneğin *2. kat yazıcısı*. |

**Save** ve **Apply changes** düğmelerine tıklayın. Cihaz kirasını yenilediğinde veya ağa yeniden bağlandığında yeni adresini alır - hemen geçerli olması için ağ kablosunu çıkarıp takabilir veya cihazın Wi-Fi bağlantısını kapatıp açabilirsiniz.

> **İpucu:** Telefonlar ve dizüstü bilgisayarlar çoğu zaman her ağ için değişebilen *rastgele* (özel/gizli) MAC adresi kullanır. Böyle bir cihaz için tanımlanan statik eşleme yalnızca MAC adresi aynı kaldığı sürece çalışır; ilgili cihazda bu ağ için rastgele MAC adresi özelliğini kapatın.

## Aktif kiralamalar (Active leases)

**Active leases** tablosu şu anda IP adresi almış olan cihazları listeler: **IP address**, **MAC address**, **Hostname** (cihazın bildirdiği ad), kiranın ne zaman dolacağı (**Expires**) ve türü (**Type**: `dynamic` veya `static`). Liste her 15 saniyede bir kendiliğinden yenilenir.

[Kontrol panelinin](/tr/manual/dashboard/) **DHCP** kartı da ağdaki cihaz sayısını ve her ağın adres havuzunun ne kadarının dolu olduğunu gösterir.
