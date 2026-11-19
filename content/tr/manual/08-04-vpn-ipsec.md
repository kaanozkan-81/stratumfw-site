---
title: "VPN > IPsec"
seo_title: "08.4 VPN > IPsec — Stratum FW Kullanım Kılavuzu"
description: "IPsec (strongSwan) ile noktadan noktaya tüneller: FortiGate, MikroTik, Cisco, pfSense/OPNsense ve bulut ağ geçitlerine bağlantı - kimlikler, ön paylaşımlı anahtar, ağlar, Faz 1 ve Faz 2, durum, bağlantı kontrolü ve IPsec günlüğü."
weight: 804
chapter_num: "08.4"
url: "/tr/manual/vpn-ipsec/"
aliases:
  - "/tr/manual/08-04-vpn-ipsec/"
translationKey: "manual-08-04-vpn-ipsec"
---

IPsec noktadan noktaya kurumsal VPN bağlantılarının evrensel standardıdır: hemen her güvenlik duvarı ve yönlendirici bu protokolü konuşur - FortiGate, MikroTik, Cisco, Palo Alto, pfSense, OPNsense ve büyük bulut sağlayıcılarının VPN ağ geçitleri. Karşı taraf WireGuard desteklemediğinde veya iş ortağınız IPsec zorunlu tuttuğunda kullanın.

Stratum FW, IKEv2 veya IKEv1 ve **ön paylaşımlı anahtar (preshared key)** ile **strongSwan** altyapısını kullanır. Tüneller **politika tabanlıdır (policy-based)**: yerel ağlar ile uzak ağlar arasındaki eşleşen trafik şifrelenir; diğer tüm trafik normal yönlendirmesine devam eder.

![VPN > IPsec](/images/manual/ipsec.png)

## IPsec servisini açma

| Ayar | Anlamı |
| :--- | :--- |
| **Enable IPsec (tunnels to other firewalls and routers)** | IPsec servisini başlatır. Servis kapalıyken de tüneller önceden hazırlanabilir. |
| **TCP MSS limit in tunnels** | Varsayılan `1350`. Büyük paketlerin şifreleme sonrasında parçalanmasını (fragmentation) önler. `0` = kapalı. |

**Save** düğmesine tıklayın.

Güvenlik duvarı kuralları otomatik oluşturulur: ilgili WAN arayüzünde karşı taraftan gelen IKE (UDP `500` ve `4500`) ve ESP paketlerine izin verilir, tünel trafiği hiçbir zaman NAT ile çevrilmez ve uzak ağlardan gelen trafiğe her tünelin kendi ayarına göre izin verilir.

## Tünel ekleme (Add a tunnel)

**Add tunnel**. İki tarafın beş temel noktada birebir uzlaşması gerekir: **IKE sürümü**, **ön paylaşımlı anahtar**, **Faz 1 (Phase 1)** ve **Faz 2 (Phase 2)** şifreleme ayarları ve ayna simetrik **ağlar**.

### Karşı taraf (Other side)

| Alan | Anlamı |
| :--- | :--- |
| **Name**, **Description** | Örneğin *Partner Istanbul*. |
| **Tunnel enabled** | Devre dışı bırakılan tünel ayarlarını korur. |
| **Remote gateway** | Karşı tarafın genel IPv4 adresi veya alan adı. **`any`** değişken IP adresli bir karşı tarafı kabul eder: bu durumda tünel yalnızca karşı tarafın bağlanmasını bekler ve bir **Remote ID** zorunlu olur. |
| **Our side (WAN)** | Tünelin kullanacağı WAN arayüzü. |
| **Remote ID** | Karşı tarafın gönderdiği kimlik - FortiGate'te *Local ID*; MikroTik'te *my-id*. Boş bırakıldığında karşı tarafın IP adresi kullanılır. Alan adı veya `any` kullanıldığında zorunludur. |
| **Local ID** | Bu tarafın gönderdiği kimlik. Boş bırakıldığında WAN IP adresi gönderilir. **NAT arkasındayken** kendi WAN adresimiz özel IP olacağı için karşı tarafın beklediği kimliği açıkça yazın. |
| **Connection** | **Connect at start and keep it up** (açılışta bağlan ve ayakta tut) veya **Wait for the other side (respond only)** (yalnızca karşı taraftan gelen bağlantıya yanıt ver). |

### Ön paylaşımlı anahtar (Preshared key)

Her iki tarafta aynı girilen gizli anahtar: en az 8 karakter, 20 veya daha uzun olması önerilir. **Generate** güçlü bir anahtar üretir; **Show** kopyalayabilmeniz için yazdığınız anahtarı gösterir.

Anahtar yalnızca `root` erişimli bir dosyada saklanır ve kaydedildikten sonra bir daha gösterilmez. Mevcut bir tüneli düzenlerken kayıtlı anahtarı korumak için alanı boş bırakın.

### Ağlar (Networks)

| Alan | Anlamı |
| :--- | :--- |
| **Local networks** | Tünele katılacak statik adresli yerel arayüzleriniz. |
| **Other local networks** | Yerel ağınızdaki başka bir yönlendiricinin arkasında kalan, elle girilen ağlar. |
| **Remote networks** | Karşı tarafın arkasındaki ağlar. `0.0.0.0/0` desteklenmez. |
| **Allow traffic from the remote networks to the local networks** | Açık: karşı taraftaki cihazlar sizin ağlarınıza doğru yeni bağlantı başlatabilir. Kapalı: yalnızca sizin tarafınızdan başlatılan bağlantılar çalışır. |
| **One phase 2 entry per network pair** | Her yerel ve uzak ağ çifti için ayrı bir güvenlik ilişkilendirmesi (SA) kurar - her alt ağ için ayrı Faz 2 seçicisi kullanan FortiGate, Cisco ve benzeri cihazlarla uyum için gereklidir. IKEv1 ile her zaman açıktır. |

Tünel başına en fazla 16 yerel ve 16 uzak ağ tanımlanabilir. Uzak ağlar kendi yerel ağlarınızla veya başka bir tünelin ağlarıyla çakışmamalıdır.

### Faz 1 - Phase 1 (IKE)

| Ayar | Varsayılan | Seçenekler |
| :--- | :--- | :--- |
| **IKE version** | `IKEv2` | `IKEv2` veya eski cihazlar için `IKEv1`. Her iki taraf destekliyorsa daima IKEv2 kullanın. |
| **IKEv1 mode** | `Main mode` | **Aggressive mode** yalnızca karşı taraf zorunlu tutuyorsa seçilmelidir - ön paylaşımlı anahtarla daha zayıftır. |
| **Encryption** | `AES-256-CBC` | `AES-256-GCM`, `AES-128-GCM`, `AES-256/192/128-CBC` |
| **Integrity / PRF** | `SHA-256` | `SHA-256`, `SHA-384`, `SHA-512`, `SHA-1` (eski sistemler) |
| **DH group** | `14 (MODP 2048)` | `14`, `15`, `16`, `19`, `20`, `21`, `31 (Curve25519)`; `5` ve `2` zayıftır, yalnızca çok eski cihazlar için |
| **Lifetime (seconds)** | `28800` | |

### Faz 2 - Phase 2 (ESP)

| Ayar | Varsayılan | Seçenekler |
| :--- | :--- | :--- |
| **Encryption** | `AES-256-CBC` | Faz 1 ile aynı seçenekler |
| **Integrity** | `SHA-256` | Faz 1 ile aynı; kendi bütünlük doğrulamasına sahip `GCM` şifrelemelerde kullanılmaz |
| **PFS group** | `14 (MODP 2048)` | Kapalı (`Off`) veya her iki tarafta aynı seçilen bir DH grubu |
| **Lifetime (seconds)** | `3600` | |

**Dead peer detection (seconds)** - varsayılan `30`: trafik yokken karşı tarafın ne sıklıkta yoklanacağı; yanıt alınamayan kopuk tünel otomatik yeniden kurulur. `0` = kapalı.

**Proposals sent** alanı strongSwan tarafından gönderilen tam algoritma dizilimini gösterir - karşı tarafın yapılandırmasıyla karşılaştırmak için çok pratiktir.

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın.

## Karşı tarafla eşleştirme notları

Ağlar **ayna simetriktir**: bizim yerel ağlarımız karşı tarafın uzak ağlarıdır; onların yerel ağları da bizim uzak ağlarımızdır.

**FortiGate:** *VPN > IPsec Wizard*, şablon **Custom**:
- aynı IKE sürümünü seçin (sihirbaz varsayılan olarak 1 seçer; sürüm 2 önerilir);
- Faz 1 ve Faz 2 şifreleme tekliflerini ve PFS DH grubunu buradakiyle aynı yapın;
- FortiGate'in *Local Address* alanı = bizim uzak ağlarımız, *Remote Address* alanı = bizim yerel ağlarımız;
- FortiGate tarafında her alt ağ için ayrı Faz 2 seçicisi varsa burada **One phase 2 entry per network pair** seçeneğini açın.

**MikroTik (RouterOS 7):** `/ip ipsec profile`, `proposal`, `peer` (`exchange-mode=ike2`), `identity` (gizli anahtar) ve her ağ çifti için bir `policy` tanımlayın. `/ip firewall nat` içinde genel `masquerade` kuralının üstüne tünel trafiği için bir `accept` kuralı ekleyerek NAT dışına çıkarın - aksi halde paketler çevrilir ve IPsec politikasıyla hiçbir zaman eşleşmez.

**pfSense / OPNsense:** aynı ayarlarla bir Phase 1 ve her ağ çifti için bir Phase 2 kaydı oluşturun.

**NAT arkasında çalışma:** NAT geçişi (NAT-T, UDP `4500`) otomatiktir. NAT arkasında kalan taraf bağlantıyı başlatmalı (**Connect at start**), diğer taraf beklemelidir.

## Durum ve bağlantı kontrolü

Tünel listesi her tünelin **Remote gateway**, **Networks**, **Status** ve **Received** / **Sent** trafik bilgilerini gösterir:

| Durum | Anlamı |
| :--- | :--- |
| **connected** | Faz 1 ve en az bir Faz 2 güvenlik ilişkilendirmesi ayakta. |
| **phase 1 only** | İki taraf kimlik doğrulamasını geçti ancak ağlar veya Faz 2 parametreleri uyuşmadı. |
| **connecting** | Müzakere devam ediyor. |
| **not connected** | Tünel kapalı/kopuk. |
| **disabled** | Tünel ayarlarında devre dışı bırakılmış. |

**Connect** ve **Disconnect** düğmeleri yapılandırmayı değiştirmeden bir tüneli anında başlatır veya durdurur. [Kontrol panelinin](/tr/manual/dashboard/) **VPN connections** kartı IPsec tünellerini de gösterir.

## IPsec günlüğü (IPsec log)

Sayfanın altındaki **IPsec log** bölümü strongSwan mesajlarını doğrudan gösterir - kurulmayan bir tünelin nedeni neredeyse her zaman burada yazar:

| Günlük mesajı | Anlamı |
| :--- | :--- |
| `NO_PROPOSAL_CHOSEN` / *no proposal chosen* | Faz 1 veya Faz 2 şifreleme ayarları uyuşmuyor (şifreleme, bütünlük, DH veya PFS grubu). |
| *authentication failed* / `AUTHENTICATION_FAILED` | Ön paylaşımlı anahtar (*preshared key*) veya kimlikler (`Local ID` / `Remote ID`) uyuşmuyor. |
| `TS_UNACCEPTABLE` | Ağ tanımları uyuşmuyor - ayna simetriğini ve ağ çifti başına Faz 2 (*per-pair*) seçeneğini kontrol edin. |
| Hiç yanıt gelmiyor | Karşı tarafa ulaşılamıyor veya yol üzerinde UDP `500`/`4500` engelleniyor. |

## IPsec mi yoksa WireGuard mı?

Her iki tarafın da WireGuard konuştuğu güvenlik duvarları arasında (Stratum FW, OPNsense, pfSense, MikroTik 7) [**WireGuard S2S**](/tr/manual/vpn-wireguard-s2s/) çok daha sadedir - yalnızca iki açık anahtar ve ağlar tanımlanır, karmaşık faz müzakereleri yoktur. Diğer tüm kurumsal cihazlar ve iş ortakları için IPsec kullanın.

> **Not:** IPsec tünelleri OPNsense içe aktarma aracı tarafından devralınmaz; geçiş sonrasında buradan yeniden tanımlayın.
