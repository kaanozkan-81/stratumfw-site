---
title: "VPN > WireGuard"
seo_title: "08.2 VPN > WireGuard — Stratum FW Kullanım Kılavuzu"
description: "Çekirdek WireGuard ile uzaktan erişim: sunucu ayarları, eşlerin (peer) erişebileceği ağlar, QR kod veya cihazın kendi anahtarıyla eş ekleme, ön paylaşımlı anahtarlar ve eş durumları."
weight: 802
chapter_num: "08.2"
url: "/tr/manual/vpn-wireguard/"
aliases:
  - "/tr/manual/08-02-vpn-wireguard/"
translationKey: "manual-08-02-vpn-wireguard"
---

WireGuard, doğrudan FreeBSD çekirdeğinde çalışan (çekirdek WireGuard) modern, hızlı ve sade bir VPN protokolüdür. Özellikle telefonlar için en iyi tercihtir: anında bağlanır, Wi-Fi ile mobil veri arasındaki ağ geçişlerinde kopmaz ve çok az pil tüketir.

Her cihaz kendine ait anahtar çifti olan bir **eş (peer)** olarak tanımlanır. Kullanıcı adı veya parola yoktur: kriptografik anahtarın *kendisi* kimliktir - bu nedenle her cihaza ayrı bir eş kaydı açılır ve kaybolan bir cihazın erişimi yalnızca kendi eş kaydı silinerek anında kesilir.

WireGuard uygulaması Windows, macOS, Linux, iOS ve Android üzerinde çalışır.

![VPN > WireGuard](/images/manual/wg.png)

## Sunucu yapılandırması (The server)

**Enable WireGuard (remote access)** anahtarını açın ve aşağıdaki ayarları yapılandırın:

### Bağlantı (Connection)

| Ayar | Anlamı |
| :--- | :--- |
| **Port (UDP)** | Varsayılan `51820`. WAN üzerinde bu port için geçiş kuralı otomatik eklenir. |
| **Accept connections on** | Tüm WAN hatları veya seçilen tek bir arayüz. |
| **External address peers connect to** | Eş yapılandırmalarının içine yazılacak olan, güvenlik duvarının dış IP adresi veya alan adı. |
| **Firewall public key** | Bilgi amaçlı gösterilir; yalnızca yapılandırması doğrudan cihaz üzerinde elle oluşturulan eşler için gereklidir. Güvenlik duvarının özel anahtarı hiçbir zaman dışarı çıkmaz. |

### Tünel ve erişim (Tunnel and access)

| Ayar | Anlamı |
| :--- | :--- |
| **Tunnel network** | Varsayılan `10.202.0.0/24`. İlk adresi güvenlik duvarı alır, diğerleri eşlere verilir. Yerel ağlarla çakışmamalıdır. |
| **MTU** | Varsayılan `1420`. Yalnızca paket boyutu sorunu yaşanan hatlarda düşürün. |
| **Local networks peers may reach** | Yalnızca seçilen ağlar eşlere yönlendirilir ve güvenlik duvarı tarafından izin verilir. **Sonradan eklenen yeni arayüzler ve VLAN'lar otomatik eklenmez.** |
| **Extra networks** | Yerel ağdaki başka bir yönlendiricinin arkasında kalan ağlar. |
| **Send all internet traffic of peers through the firewall** | Kapalı: yalnızca iç ağlarınıza giden trafik tüneli kullanır. Açık: tüm internet trafiği güvenlik duvarı üzerinden çıkar. |
| **Peers may reach each other** | Varsayılan olarak kapalıdır. |

### Eş ayarları (Peer settings)

| Ayar | Anlamı |
| :--- | :--- |
| **DNS servers** | En fazla dört sunucu. Boş bırakıldığında güvenlik duvarının kendisi verilir. |
| **Keepalive (seconds)** | Varsayılan `25`. Mobil veri ve NAT arkasındaki bağlantıların açık kalmasını sağlar. `0` = kapalı. |

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın.

## Eş ekleme (Add a peer)

Telefon, dizüstü bilgisayar, tablet gibi her cihaz için ayrı bir eş ekleyin. **Add peer** alanları:

| Alan | Anlamı |
| :--- | :--- |
| **Device name** | Örneğin *Kaan - iPhone*. |
| **Description** | İsteğe bağlı açıklama. |
| **Tunnel address** | Cihazın tünel ağındaki IP adresi. Sıradaki boş adres otomatik önerilir. |
| **Peer enabled** | Eş kaydını silmeden geçici olarak devre dışı bırakır. |
| **Preshared key** | Anahtar çiftine ek olarak yalnızca bu eş ile güvenlik duvarı arasında paylaşılan ek gizli anahtar - açılması önerilir. |
| **Keys** | **Generate the key pair on the firewall** - yapılandırma hazır QR kod ve dosya olarak sunulur; veya **The device has its own key pair** - cihazda üretilen **açık anahtarı (public key)** girersiniz; cihazın özel anahtarı hiçbir zaman cihazdan dışarı çıkmaz. |

**Save** ve **Apply changes** düğmelerine tıklayın. Bağlı diğer eşlerin oturumu kesintisiz devam eder.

### Cihazın kurulması

- **Telefon (anahtarlar güvenlik duvarında üretildiyse):** WireGuard uygulamasını açın, *Add tunnel* > *Scan from QR code* seçeneğine dokunun ve WireGuard sayfasında ilgili eşin altında gösterilen QR kodu okutun.
- **Dizüstü bilgisayar (anahtarlar güvenlik duvarında üretildiyse):** yapılandırma dosyasını (`.conf`) indirin ve WireGuard uygulamasına aktarın.
- **Kendi anahtar çiftini üreten cihaz:** tüneli cihazdaki uygulamada oluşturun, açık anahtarını kopyalayıp güvenlik duvarındaki eş kaydına yapıştırın; cihaza ise güvenlik duvarının açık anahtarını, dış adres ve portu, cihazın tünel adresini ve yönlendirilecek ağları girin.

> **Uyarı:** Bir eşin yapılandırması (QR kod veya dosya) o cihazın özel anahtarını barındırır. QR kodu yalnızca cihazın sahibine gösterin ve yapılandırma dosyasını e-posta veya mesajlaşma uygulamalarıyla göndermeyin. Yapılandırmalar yalnızca yöneticilere gösterilir.

Bir eşin anahtarları yerinde yenilenemez: bir cihaza yeni anahtar çifti vermek için mevcut eş kaydını silip yeniden ekleyin.

## Eş durumu (Peer status)

**Peers** listesi her eşi **Tunnel address**, **Status**, **Last handshake** ve **Received** / **Sent** trafik bilgileriyle gösterir. WireGuard'da klasik anlamda sürekli açık bir oturum kavramı yoktur: son el sıkışması (*handshake*) yakın zamandaysa (yaklaşık son iki dakika içinde) eş **Active** kabul edilir. [Kontrol panelinin](/tr/manual/dashboard/) **VPN connections** kartı da aktif eşleri listeler.

## OpenVPN mi yoksa WireGuard mı?

| Özellik | [OpenVPN](/tr/manual/vpn-openvpn/) | WireGuard |
| :--- | :--- | :--- |
| Kimlik doğrulama | Kullanıcı başına sertifika **ve** parola | Cihaz başına kriptografik anahtar |
| Hız ve pil ömrü | İyi | Daha yüksek hız, çok düşük pil tüketimi |
| Ağ geçişi (Wi-Fi ⇄ mobil veri) | Yeniden bağlanır | Kesintisiz geçiş yapar |
| UDP engelleyen ağlardan geçiş | Evet (TCP moduyla) | Hayır (yalnızca UDP çalışır) |
| En uygun kullanım | Parola zorunluluğu istenen personel bilgisayarları | Telefonlar, tabletler ve kişisel cihazlar |

Her iki VPN sunucusu aynı anda birlikte çalışabilir.
