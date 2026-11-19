---
title: "VPN > WireGuard S2S (Noktadan Noktaya)"
seo_title: "08.3 VPN > WireGuard S2S — Stratum FW Kullanım Kılavuzu"
description: "Çekirdek WireGuard ile noktadan noktaya (site-to-site) tüneller: ofisleri ve şubeleri internet üzerinden birleştirme, anahtarlar, bağlantı yönü, yerel ve uzak ağlar, karşı tarafın kurulumu ve tünel durumu."
weight: 803
chapter_num: "08.3"
url: "/tr/manual/vpn-wireguard-s2s/"
aliases:
  - "/tr/manual/08-03-vpn-wireguard-s2s/"
translationKey: "manual-08-03-vpn-wireguard-s2s"
---

Noktadan noktaya (site-to-site) bir tünel iki farklı yerleşkenin ağlarını - merkez ofis ile bir şubeyi, iki oteli veya bir ofis ile veri merkezini - internet üzerinden birleştirir; böylece her iki taraftaki cihazlar sanki tek bir yerel ağdaymış gibi birbirine erişir. Kimsenin oturum açmasına gerek yoktur: iki güvenlik duvarı tüneli kendi aralarında sürekli ayakta tutar.

**VPN > WireGuard S2S** bu tünelleri çekirdek WireGuard ile kurar. Karşı taraf başka bir Stratum FW, OPNsense, pfSense, MikroTik RouterOS 7 veya Linux sunucu olabilir. WireGuard desteklemeyen cihazlar (FortiGate, Cisco ve çoğu bulut VPN ağ geçidi) için [**VPN > IPsec**](/tr/manual/vpn-ipsec/) kullanın.

![VPN > WireGuard S2S](/images/manual/wgs2s.png)

## Nasıl çalışır?

- Her tünel kendi bağımsız arayüzüne sahiptir: `wg1`, `wg2` ... ([**VPN > WireGuard**](/tr/manual/vpn-wireguard/) altındaki uzaktan erişim sunucusu ise `wg0` arayüzünü kullanır ve tamamen ayrıdır).
- Tünel **rota tabanlıdır (route-based)**: karşı tarafın ağları tünel arayüzüne yönlendirilir ve karşı taraf yalnızca tanımlı bu ağlardan paket gönderebilir.
- Her tarafın kendine ait bir anahtar çifti vardır; her taraf karşı tarafın **açık anahtarına (public key)** ihtiyaç duyar. Özel anahtarlar hiçbir zaman güvenlik duvarının dışına çıkmaz.
- Güvenlik duvarı kuralları sizin yerinize otomatik oluşturulur: tünelin UDP portu WAN üzerinde (biliniyorsa yalnızca karşı tarafın IP adresi için) açılır ve seçtiğinizde uzak ağlardan yerel ağlara doğru gelen trafiğe izin verilir. Büyük paketlerin boyutu tünele sığacak şekilde otomatik ayarlanır.

## Başlamadan önce

- İki taraftaki yerel ağlar **birbiriyle çakışmamalıdır**. Her ikisi de `192.168.1.0/24` kullanan iki ofis birleştirilemez - önce bir tarafın alt ağını değiştirin.
- En az bir tarafın dışarıdan erişilebilir bir genel IP adresine (veya alan adına) sahip olması ve tünelin UDP portunun güvenlik duvarına ulaşması gerekir - servis sağlayıcı modemi yönlendirici modundaysa portu güvenlik duvarına yönlendirin.

## Tünel ekleme (Add a tunnel)

**Add tunnel** sayfasında üç bölüm yer alır: **This side**, **Other side** ve **Networks**.

### Bu taraf (This side)

| Alan | Anlamı |
| :--- | :--- |
| **Name**, **Description** | Örneğin *Branch Antalya*. |
| **Tunnel enabled** | Devre dışı bırakılan bir tünel ayarlarını ve anahtarlarını korur. |
| **Interface** | `wg1` ... `wg99`; sıradaki boş arayüz numarası otomatik önerilir. |
| **Listen port (UDP)** | `51821` ve üzeri; her tünelin kendine ait bir UDP portu vardır. |
| **Open the port on** | Tüm WAN hatları veya seçilen tek bir arayüz. |
| **Tunnel address (optional)** | Tünelin kendi içinde bu tarafa verilecek isteğe bağlı adres, örneğin `10.203.0.1/30` - yalnızca tünel ucuna doğrudan ping atabilmek için kullanılır. Sayfa karşı tarafa verilecek eş adresi de gösterir. |
| **This side's public key** | Tünel ilk kez kaydedildiğinde üretilir. Bu anahtarı karşı tarafa verin. |

### Karşı taraf (Other side)

| Alan | Anlamı |
| :--- | :--- |
| **Its public key** | Karşı tarafın WireGuard açık anahtarı. |
| **Its address**, **Its port** | Karşı tarafa ulaşılacak dış IPv4 adresi veya alan adı ve port (varsayılan `51820`). Karşı taraf NAT arkasındaysa veya değişken IP kullanıyorsa adresi boş bırakın - bu durumda bağlantıyı karşı taraf size doğru başlatır. |
| **Preshared key** | İsteğe bağlı ancak önerilir: her iki tarafta aynı girilen ek paylaşımlı gizli anahtar. **Generate** üretir; **Show** karşı tarafa kopyalayabilmeniz için gösterir. **Remove the saved preshared key** kayıtlı anahtarı kaldırır. |

**Anahtarlar henüz belli değil mi?** Her iki taraf birbirinin açık anahtarını istediği için önce bir tarafın başlaması gerekir. Tüneli karşı tarafın anahtarını girmeden **devre dışı (disabled)** olarak kaydedin: bu tarafın anahtarı üretilir. Üretilen açık anahtarı karşı tarafa iletin, onların açık anahtarını alın, tekrar dönüp girin ve tüneli etkinleştirin.

### Ağlar (Networks)

| Alan | Anlamı |
| :--- | :--- |
| **Local networks** | Karşı tarafın erişmesine izin verdiğiniz yerel arayüz ağlarınız. |
| **Other local networks** | Yerel ağınızdaki başka bir yönlendiricinin arkasında kalan, elle girilen ağlar. |
| **Remote networks** | Karşı tarafın arkasındaki ağlar. Bu ağlar tünele yönlendirilir ve karşı tarafın yalnızca bu ağlardan trafik göndermesine izin verilir. `0.0.0.0/0` desteklenmez. |
| **Allow traffic from the remote networks to the local networks** | Açık: karşı taraftaki cihazlar sizin ağlarınıza doğru yeni bağlantı başlatabilir. Kapalı: yalnızca sizin tarafınızdan başlatılan bağlantılar (ve yanıtları) çalışır. |
| **Keepalive (seconds)** | Varsayılan `25`: tüneli NAT arkasında açık tutar. `0` = kapalı. NAT arkasındaki tarafta mutlaka açık tutun. |
| **MTU** | Varsayılan `1420`. |

Panel; kendi yerel ağlarınızla, VPN ağlarıyla veya başka bir tünelin ağlarıyla çakışan uzak ağ tanımlarını reddeder.

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın.

## Karşı tarafın kurulması

Ağ tanımları **ayna simetriktir (mirrored)**: sizin yerel ağlarınız karşı tarafın uzak ağlarıdır; karşı tarafın yerel ağları da sizin uzak ağlarınızdır.

**Başka bir Stratum FW:** karşı tarafta yeni bir tünel açın; bu tarafın açık anahtarını, aynı ön paylaşımlı anahtarı (*preshared key*) ve simetrik ağları girin. Sabit genel IP adresi olan tarafın adresini diğer tarafa (veya her iki tarafa da) yazın.

**MikroTik (RouterOS 7):**

```
/interface wireguard add name=wg-office listen-port=51820 private-key="<router tarafindan uretilen>"
/interface wireguard peers add interface=wg-office public-key="<bu tarafin acik anahtari>"     endpoint-address=<dis IP adresimiz> endpoint-port=51821     allowed-address=192.168.1.0/24 preshared-key="<on paylasimli anahtar>" persistent-keepalive=25s
/ip route add dst-address=192.168.1.0/24 gateway=wg-office
/ip firewall filter add chain=input protocol=udp dst-port=51820 action=accept place-before=0
```

(`192.168.1.0/24` sizin yerel ağlarınızı temsil eder.) MikroTik üzerinde `/interface wireguard print` çıktısındaki açık anahtarı kopyalayıp **Its public key** alanına yapıştırın.

**OPNsense / pfSense:** **VPN > WireGuard** altında bir örnek (*instance* - kendi anahtarı ve portu) ve bir eş (*peer* - bu tarafın açık anahtarı, *Allowed IPs* olarak bizim yerel ağlarımız ve *Endpoint* olarak bizim dış adresimiz) tanımlayın; ardından ağlarımız için rota/ağ geçidi ve WireGuard arayüzünde geçiş kuralı ekleyin.

## Tünel durumu (Status)

Tünel listesi her tüneli **Interface**, **Remote endpoint**, **Networks**, **Status** ve **Received** / **Sent** trafik değerleriyle gösterir. Son el sıkışma (*handshake*) yakın zamandaysa (yaklaşık son iki dakika içinde) tünel ayaktadır (**Up**). [Kontrol panelinin](/tr/manual/dashboard/) **VPN connections** kartı noktadan noktaya tünelleri de listeler.

## Sorun giderme

| Belirti | Kontrol edilecek nokta |
| :--- | :--- |
| El sıkışma (handshake) olmuyor | Açık anahtarların çapraz doğru girildiğinden (her tarafta *karşı tarafın* açık anahtarı olmalı), ön paylaşımlı anahtarların birebir aynı olduğundan, bağlanan tarafta dış adres ve portun doğru yazıldığından ve UDP portunun güvenlik duvarına ulaştığından (modem port yönlendirmesi / üst güvenlik duvarı) emin olun. |
| El sıkışma var ama trafik geçmiyor | Ağların tam simetrik tanımlandığını, karşı tarafın bizim ağlarımızı tünele yönlendirdiğini ve güvenlik duvarı kurallarının izin verdiğini doğrulayın; bağlantıyı karşı taraf başlatıyorsa burada **Allow traffic from the remote networks** açık olmalıdır. |
| Tünel üzerinden bazı web sayfaları takılıyor | Her iki tarafta **MTU** değerini düşürün (örneğin `1380`). |
| NAT arkasındaki tünel bir süre sonra düşüyor | NAT arkasındaki tarafta **Keepalive** değerini açık tutun (`25` sn). |

Güvenlik duvarı üzerinden yerel kaynak IP adresiyle veya tünel adresiyle **Diagnostics > Ping** çalıştırmak iki durumu birbirinden ayırmayı kolaylaştırır.
