---
title: "Arayüzler > VLAN'lar"
seo_title: "05.2 Arayüzler > VLAN'lar (Interfaces > VLANs) — Stratum FW Kullanım Kılavuzu"
description: "802.1Q VLAN yapıları: tek bir kablo üzerinden birden fazla yalıtılmış ağ - VLAN oluşturma, arayüz olarak atama, anahtar (switch) tarafı ayarları ve VLAN güncelleme veya silme."
weight: 402
chapter_num: "05.2"
url: "/tr/manual/interfaces-vlans/"
aliases:
  - "/tr/manual/05-02-interfaces-vlans/"
translationKey: "manual-05-02-interfaces-vlans"
---

Bir VLAN (IEEE 802.1Q), ortak bir kablo üzerinden birbirinden ayrı ağlar taşır. Her ağ 1 ile 4094 arasında bir numarayla, yani *etiketiyle* (tag) tanımlanır. VLAN sayesinde güvenlik duvarının tek bir fiziksel portu personel ağına, misafir Wi-Fi ağına, kameralara ve IP telefonlara aynı anda hizmet verebilir; üstelik her birinin sanki kendine ait fiziksel bir portu varmış gibi ayrı IP aralığı, DHCP sunucusu ve güvenlik duvarı kuralları olur.

Tipik kullanım alanları:

- Yönetilebilir bir anahtara (managed switch) bağlı tek bir LAN portu üzerinden misafir, personel ve cihaz ağlarını ayırmak.
- Aynı fiziksel kablodan farklı etiketlerle gelen iki ayrı servis sağlayıcı hattını sonlandırmak.
- WAN tarafında belirli bir VLAN etiketi zorunlu tutan internet servis sağlayıcılarına bağlanmak (fiber bağlantılarda yaygındır; etiket numarasını servis sağlayıcı verir).

## Yapının genel işleyişi

```
                       trunk: etiket 10, 20, 30
 [ Stratum FW igb1 ] =========================== [ yönetilebilir switch ]
     igb1.10  LAN-Staff   10.10.10.1/24              port 1-8   etiketsiz (untagged) VLAN 10
     igb1.20  Guests      10.20.0.1/22               erişim noktaları: etiketli 20 (+ yönetim için 10)
     igb1.30  Cameras     10.30.0.1/24               port 20-24 etiketsiz (untagged) VLAN 30
```

Bir VLAN'ı kullanıma hazır hale getirmek üç adımdan oluşur:

1. **Interfaces > VLANs sayfasında VLAN oluşturun**: üst fiziksel portu ve etiketi seçin. Bu işlem VLAN portunu (örneğin `igb1.20`) oluşturur.
2. **[Interfaces > Assignments](/tr/manual/interfaces-assignments/) sayfasında arayüz olarak atayın**: yeni VLAN **Available network ports** altında görünür; IP adresiyle birlikte bir LAN (veya WAN) olarak ekleyin. Bu andan itibaren kendi kuralları, DHCP sunucusu ve diğer ayarlarıyla diğer tüm ağlar gibidir.
3. **Anahtarı (switch) yapılandırın**: güvenlik duvarının takılı olduğu anahtar portu aynı etiketleri taşıyan bir *trunk* (etiketli port) olmalıdır.

## VLAN listesi

![Interfaces > VLANs](/images/manual/if-vlans.png)

| Sütun | Anlamı |
| :--- | :--- |
| **VLAN interface** | Oluşturulan VLAN portu: üst port ve etiket birleşimi, örneğin `igb1.20`. |
| **Parent port** | VLAN trafiğini taşıyan fiziksel port. |
| **Tag** | 802.1Q VLAN kimlik numarası (VLAN ID). |
| **Description** | VLAN için yazdığınız açıklama notu. |
| **State** | *up* (üst portta aktif bağlantı var), *down* / *no carrier* veya *pending* (oluşturuldu, henüz Apply edilmedi). |
| **Assigned as** | Bu VLAN'ı kullanan arayüz (*OPT3*) veya *not assigned* (atanmamış). |

## VLAN oluşturma

**Add VLAN** düğmesine tıklayın.

| Ayar | Anlamı |
| :--- | :--- |
| **Parent port** | Anahtarın trunk portuna bağlı fiziksel port. Bir VLAN başka bir VLAN üzerine kurulamaz. |
| **VLAN tag** | 1 ile 4094 arası. Anahtar (switch) üzerinde yapılandırılan VLAN ID ile birebir eşleşmelidir. Bir etiket aynı üst portta yalnızca bir kez kullanılabilir. |
| **Description** | İsteğe bağlı açıklama, en fazla 40 karakter. |
| **Interface name** | Bilgi amaçlı gösterilir: üst port ve etiketten otomatik üretilir (`igb1` + `20` = `igb1.20`). |

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın. VLAN portu artık oluşturulmuştur ancak bir arayüz olarak atanana kadar trafik taşımaz: [**Interfaces > Assignments**](/tr/manual/interfaces-assignments/) sayfasına gidin, **Available network ports** altından yeni VLAN portunu bulun ve **Add interface** düğmesine tıklayın.

> **İpucu:** Bir fiziksel port hem etiketli VLAN'ları hem de etiketsiz bir ağı aynı anda taşıyabilir: portun kendisi (`igb1`) LAN olarak çalışırken `igb1.20` ve `igb1.30` OPT arayüzleri olabilir. Fiziksel portun yalnızca etiketli VLAN trafiği taşımasını istiyorsanız portun kendisine hiçbir arayüz atamayın veya IPv4 türünü **None** seçerek atayın.

## Anahtar (Switch) tarafı

Güvenlik duvarı VLAN trafiğini *etiketli* (tagged) olarak gönderir ve bekler. Anahtar üzerinde:

- Güvenlik duvarına bağlı port: güvenlik duvarının hizmet vereceği her VLAN için **trunk** / **tagged** olmalıdır. Üst port aynı zamanda etiketsiz olarak da kullanılıyorsa (yukarıdaki ipucunda olduğu gibi), o ağ ilgili portun etiketsiz (native / PVID) VLAN'ı olmalıdır.
- Sıradan uç cihazların (bilgisayarlar, kameralar, yazıcılar) bağlı olduğu portlar: kendi VLAN'larında **access** / **untagged** olmalıdır.
- Birden fazla kablosuz ağ (SSID) yayınlayan Wi-Fi erişim noktaları: her SSID'nin VLAN'ı için etiketli (tagged), kendi yönetim ağı için ise erişim noktasının kılavuzunda belirtildiği şekilde etiketsiz (veya etiketli) olmalıdır.

Terimler üreticiden üreticiye değişse de (Cisco *trunk*/*access*, HP/Aruba *tagged*/*untagged*, MikroTik köprü VLAN filtreleme) çalışma mantığı aynıdır.

## VLAN değiştirme veya silme

- **Description** alanı her zaman değiştirilebilir.
- **Parent port** ve **VLAN tag** yalnızca VLAN herhangi bir arayüze atanmamışken değiştirilebilir. Atanmış durumdaysa sayfa bunu açıkça belirtir: *This VLAN is assigned to OPT3. Its port and tag can only be changed after the interface is removed.*
- **Delete** işlemi de aynı şekilde çalışır: önce [**Interfaces > Assignments**](/tr/manual/interfaces-assignments/) sayfasından ilgili arayüzü kaldırın, ardından VLAN kaydını silin.

Bir ağı ayarlarını kaybetmeden bir VLAN'dan diğerine taşımak için önce yeni VLAN'ı oluşturun, [**Interfaces > Assignments**](/tr/manual/interfaces-assignments/) sayfasındaki **Network port** seçicisiyle arayüzü yeni VLAN üzerine taşıyın ve ardından eski VLAN'ı silin.

## Bir VLAN çalışmadığında kontrol edilecekler

| Belirti | Kontrol edilecek nokta |
| :--- | :--- |
| *State* sütunu *down* görünüyor | Üst fiziksel portta bağlantı (link) yoktur: kabloyu, anahtar portunu ve bağlantı hızını kontrol edin. |
| VLAN içindeki cihazlar IP adresi alamıyor | İlgili arayüz için DHCP sunucusunun açık olduğunu (**Services > DHCP Server**), uç cihazın bağlı olduğu anahtar portunun doğru VLAN'da etiketsiz (untagged) olduğunu ve güvenlik duvarına giden trunk hattının bu etiketi taşıdığını doğrulayın. |
| Cihazlar IP alıyor ancak hiçbir yere erişemiyor | Kurulum sihirbazından sonra eklenen LAN tipindeki bir arayüzde siz ekleyene kadar hiçbir kural bulunmaz (**Firewall > Rules**, ilgili arayüz sekmesi). **Diagnostics > Firewall Log** neyin engellendiğini gösterir. |
| Trafik yanlış ağa düşüyor | Anahtar ile güvenlik duvarındaki etiket numaraları uyuşmuyordur veya trunk hattındaki native VLAN yanlıştır. Üst port üzerinde **Diagnostics > Packet Capture** çalıştırarak gelen paketlerin VLAN etiketlerini görebilirsiniz. |
