---
title: "Arayüzler > Rotalar"
seo_title: "05.6 Arayüzler > Rotalar (Interfaces > Routes) — Stratum FW Kullanım Kılavuzu"
description: "Statik rotalar: doğrudan bağlı bir ağ üzerindeki başka bir yönlendiricinin arkasında kalan alt ağlara erişim ve Stratum FW'nin bu ağlar için otomatik yaptıkları."
weight: 506
chapter_num: "05.6"
url: "/tr/manual/interfaces-routes/"
aliases:
  - "/tr/manual/05-06-interfaces-routes/"
translationKey: "manual-05-06-interfaces-routes"
---

Bir statik rota (static route), güvenlik duvarına belirli bir ağa hangi yönlendirici (router) üzerinden ulaşılacağını söyler. Güvenlik duvarının doğrudan bağlı olmadığı ağlar bulunduğunda statik rotaya ihtiyaç duyarsınız; örneğin:

- LAN üzerinde arkasında başka ağlar barındıran ikinci bir yönlendirici varsa (farklı bir bina, laboratuvar veya VLAN'ları kendi içinde yönlendiren katman-3 omurga anahtar);
- WAN tarafında ana internet ağ geçidi dışındaki başka bir yönlendirici üzerinden ulaşılan bir ağ varsa (servis sağlayıcının MPLS hattı veya ayrı bir yönetim ağı gibi).

```
 Internet ── [ Stratum FW ] ── LAN 192.168.1.0/24 ── [ router 192.168.1.254 ] ── 10.50.0.0/24
                                                                                  10.60.0.0/24
 Rotalar: 10.50.0.0/24 -> 192.168.1.254 üzerinden
          10.60.0.0/24 -> 192.168.1.254 üzerinden
```

VPN tünelleri (OpenVPN, WireGuard, IPsec) üzerinden ulaşılan ağlar için statik rota eklemeniz gerekmez: VPN ayarları kendi rotalarını otomatik ekler.

## Rota listesi

**Interfaces > Routes** sayfası her rotayı hedef ağı (**Network**), ağ geçidi (**Gateway**), çıktığı arayüz (**Via**), açıklaması (**Description**) ve durumu (**State**) ile birlikte listeler.

![Interfaces > Routes](/images/manual/routes-list.png)

## Rota ekleme

**Add route** düğmesine tıklayın.

| Ayar | Anlamı |
| :--- | :--- |
| **Destination network** | Diğer yönlendiricinin arkasındaki hedef ağ ve önek uzunluğu: `10.50.0.0/24`. İstemci (host) bitleri sıfır olmalıdır (`10.50.0.5/24` reddedilir). Mevcut bir arayüzün ağıyla veya başka bir statik rotayla çakışmamalıdır. |
| **Gateway** | İkinci yönlendiricinin IP adresi. **Sabit (static)** IP adresine sahip bir arayüzün kendi ağı içinde yer almalı ve güvenlik duvarının kendi adresi olmamalıdır. İlgili arayüz otomatik olarak bulunur ve **Via** sütununda gösterilir. |
| **Route active** | Rotayı silmeden geçici olarak devre dışı bırakmanızı sağlar. |
| **Description** | İsteğe bağlı açıklama notunuz, örneğin *Building B core switch*. |

**Save**, **Apply changes** ve **Keep changes** düğmelerine tıklayın. Yeniden başlatma veya arayüz yenilemesi sonrasında güvenlik duvarı statik rotaları kendiliğinden tekrar kurar.

> **Not:** Varsayılan rota (`0.0.0.0/0`) bir statik rota değildir: WAN ağ geçidinden ve birden fazla WAN hattı olduğunda [**Interfaces > Gateways**](/tr/manual/interfaces-gateways/) ayarlarından gelir.

## Otomatik olarak yapılanlar

Bir **iç** (LAN tipindeki) arayüz üzerindeki yönlendirici aracılığıyla ulaşılan ağlar için Stratum FW şunları otomatik yapar:

- **İnternet erişimi:** tıpkı arayüzün kendi yerel ağında olduğu gibi bu ağların internete çıkan trafiği de WAN adresine çevrilir (otomatik giden NAT).
- **Politika yönlendirmesi (Policy routing):** trafiği seçilen bir WAN hattına gönderen kurallar, bu yönlendirilmiş iç ağlara *giden* trafiği de WAN'a çıkarmayıp yerel tutar.

Sizin ayrıca yapmanız gerekenler:

- **Trafiğine izin verin:** paketlerin güvenlik duvarına *geldiği* arayüzde izin kuralı ekleyin - yukarıdaki örnekte **Firewall > Rules** altında **LAN** sekmesine kaynak olarak `10.50.0.0/24` (veya yönlendirilen ağları içeren bir alias) yazan bir kural eklenmelidir. Arayüzün varsayılan *Default allow LAN to any* kuralı yalnızca LAN'ın kendi doğrudan alt ağını kapsar.
- **Diğer yönlendiriciye dönüş rotası verin:** LAN arkasındaki yönlendiricinin varsayılan rotası güvenlik duvarını (`192.168.1.1`) göstermelidir.
- **DNS:** DNS çözümleyici olarak Unbound kullanıldığında güvenlik duvarı yalnızca kendi doğrudan arayüzlerine ve VPN ağlarına DNS yanıtı verir. Yönlendirilen alt ağlardaki cihazlara başka bir DNS sunucusu (örneğin LAN arkasındaki yönlendiriciyi) tanımlayın.

## Asimetrik yönlendirme (Asymmetric routing)

LAN üzerindeki cihazlar varsayılan ağ geçidi olarak güvenlik duvarı yerine *diğer yönlendiriciyi* kullanırsa, internete giden trafikleri `LAN → yönlendirici → güvenlik duvarı` yolunu izlerken dönüş paketleri doğrudan `güvenlik duvarı → LAN` yolunu izler. Durum bilgili (stateful) bir güvenlik duvarı böyle bir bağlantının yalnızca yarısını görür ve paketleri düşürebilir. Bunu önlemek için LAN cihazlarının ağ geçidini doğrudan güvenlik duvarı yapın veya ikinci yönlendiriciyi güvenlik duvarına LAN yerine ayrı bir *geçiş (transfer) ağı* (örneğin ikisi arasında küçük bir `/30` veya `/29` alt ağı) üzerinden bağlayın.

En fazla 256 statik rota tanımlanabilir.
