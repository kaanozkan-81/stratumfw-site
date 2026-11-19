---
title: "VPN > OpenVPN"
seo_title: "08.1 VPN > OpenVPN — Stratum FW Kullanım Kılavuzu"
description: "OpenVPN ile uzaktan erişim: sunucu yapılandırması, VPN kullanıcılarının erişebileceği ağlar, istemci DNS ayarları, gelişmiş ayarlar, kullanıcı yetkilendirme, istemci profilleri, sabit VPN adresleri ve bağlı istemciler."
weight: 801
chapter_num: "08.1"
url: "/tr/manual/vpn-openvpn/"
aliases:
  - "/tr/manual/08-01-vpn-openvpn/"
translationKey: "manual-08-01-vpn-openvpn"
---

OpenVPN çalışanların ofis dışındayken sanki ofisteymiş gibi güvenli biçimde çalışmasını sağlar: dizüstü bilgisayarları veya telefonları güvenlik duvarına bağlanır, VPN ağından bir IP adresi alır ve seçtiğiniz iç ağlara erişir. Her kullanıcı **kendi kişisel sertifikası ve kendi parolasıyla** oturum açar - parola olmadan çalınmış bir bilgisayar ya da bilgisayar olmadan ele geçirilmiş bir parola tek başına yeterli değildir.

OpenVPN istemcisi Windows, macOS, Linux, iOS ve Android üzerinde çalışır (OpenVPN Connect veya OpenVPN GUI / Tunnelblick / işletim sisteminin ağ yöneticisi).

![VPN > OpenVPN](/images/manual/ovpn.png)

## Sunucu yapılandırması (The server)

**Enable the OpenVPN server (remote access)** anahtarını açın ve aşağıdaki bölümleri yapılandırın:

### Bağlantı (Connection)

| Ayar | Anlamı |
| :--- | :--- |
| **Protocol** | **UDP (recommended)** veya UDP trafiğini engelleyen ağlar (bazı oteller ve misafir ağları) için **TCP**. |
| **Port** | Varsayılan `1194`. WAN üzerinde bu port için geçiş kuralı otomatik olarak eklenir. |
| **Accept connections on** | Tüm WAN hatları veya seçilen tek bir arayüz. |
| **External address clients connect to** | İstemci profillerinin içine yazılacak olan, güvenlik duvarının dış IP adresi veya alan adı - örneğin `vpn.example.com`. Genel IP adresiniz değişebiliyorsa alan adı kullanın. |

### Tünel ve erişim (Tunnel and access)

| Ayar | Anlamı |
| :--- | :--- |
| **VPN network** | VPN istemcilerine dağıtılacak adres ağı, varsayılan `10.201.0.0/24`. Hiçbir yerel ağla veya kullanıcıların bağlandığı ev ağlarıyla çakışmamalıdır (`192.168.0.0/24` ve `192.168.1.0/24` bloklarından kaçının). |
| **Maximum connections** | Aynı anda bağlanabilecek en fazla istemci sayısı (varsayılan `64`). |
| **Local networks VPN clients may reach** | Yalnızca burada işaretlenen ağlar VPN istemcilerine yönlendirilir ve güvenlik duvarı tarafından izin verilir. **Sonradan eklenen yeni arayüzler ve VLAN'lar otomatik eklenmez** - gerektiğinde buradan işaretleyin. |
| **Extra networks** | Bir yerel ağdaki başka bir yönlendiricinin arkasında kalan ek ağlar ([**Interfaces > Routes**](/tr/manual/interfaces-routes/)), en fazla 32 adet. |
| **Send all internet traffic of VPN clients through the firewall** | Kapalı: yalnızca iç ağlarınıza giden trafik VPN tünelini kullanır (*split tunnel*). Açık: tüm internet trafiği tünelden geçer - güvenilmeyen ağlardaki kullanıcıları korumak veya güvenlik duvarı filtrelerini VPN istemcilerine de uygulamak için kullanılır. |
| **VPN clients may reach each other** | Varsayılan olarak kapalıdır. |
| **A user may connect from several devices at the same time** | Kapalı: aynı kullanıcının yeni bağlantısı önceki açık oturumunu sonlandırır. |
| **Windows clients use only the VPN's DNS servers (block-outside-dns)** | Windows istemcilerin iç alan adlarını kendi yerel ev/otel DNS sunucularına sormasını engeller. |
| **Require the client certificate usage "TLS client"** | Açık bırakın. Yalnızca başka bir sistemden içe aktarılmış ve bu işareti taşımayan eski sertifikalar için kapatın. |

VPN istemcilerinin güvenlik duvarı geçiş kuralları doğrudan **Local networks VPN clients may reach** seçiminden üretilir; [**Firewall > Rules**](/tr/manual/firewall-rules/) altında ayrıca kural eklemeniz gerekmez.

### İstemci ayarları (Client settings)

| Ayar | Anlamı |
| :--- | :--- |
| **DNS servers** | Virgülle ayrılmış en fazla dört sunucu. Boş bırakıldığında güvenlik duvarının kendisi verilir - böylece VPN kullanıcıları yerel cihaz adlarını çözebilir ve [DNS engelleme listeleri](/tr/manual/firewall-blocklists/) uygulanır. |
| **DNS domain** | Kısa adlar için varsayılan alan adı, örneğin `office.lan`. |
| **Search domains** | Kısa ad sorgularında denenecek ek alan adları. |

### Gelişmiş ayarlar (Advanced settings)

Varsayılan değerler hemen her kurulum için en doğru seçenektir.

| Ayar | Anlamı |
| :--- | :--- |
| **Login method** | **Certificate and password (recommended)** veya **Certificate only**. |
| **Log level** | `1` yalnızca hatalar, `3` normal (varsayılan), `4` ayrıntılı, `5` hata ayıklama (bağlantı sorunlarını incelemek için). |
| **Data encryption** | Sunulan şifreleme algoritmaları: `AES-256-GCM`, `AES-128-GCM`, `CHACHA20-POLY1305`. |
| **Authentication digest** | `SHA256` (varsayılan), `SHA384` veya `SHA512`. |
| **Minimum TLS version** | `TLS 1.2` veya `TLS 1.3`. |
| **Key renegotiation (seconds)** | Varsayılan `3600`; `0` = hiçbir zaman. |
| **Keepalive interval / timeout (seconds)** | Varsayılan `10` / `60`: kopan bir bağlantının ne kadar sürede fark edileceği. |
| **Tunnel MTU**, **MSS clamping** | Yalnızca paket boyutu sorunu yaşanan hatlar için (VPN üzerinden yarım kalan ve yüklenmeyen sayfalar). |

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın. Sertifika otoritesi (CA), sunucu sertifikası ve anahtarlar ilk kurulumda doğrudan güvenlik duvarı üzerinde üretilir ve dışarı çıkmaz.

> **Not:** Protokol, port, dış adres, özet algoritması, şifreleme veya giriş yöntemi değiştirildiğinde istemci profilleri de değişir: kullanıcıların güncel profillerini yeniden indirip içe aktarması gerekir.

## Kullanıcılara VPN erişimi verme

VPN kullanıcıları **System > Users** altındaki hesaplardır. Bir hesabın yönetim paneline erişim yetkisi olması gerekmez: panel yetkisi olmayan bir hesap *yalnızca VPN* hesabıdır - personel için standart kullanım budur.

1. Kullanıcıyı **System > Users** sayfasından oluşturun (veya mevcut bir kullanıcıyı seçin). Kullanıcının parolası aynı zamanda VPN parolasıdır.
2. **VPN > OpenVPN** sayfasında **Users with VPN access** tablosundan ilgili kullanıcı için **VPN access** kutusunu açın. Kullanıcı için anında bir istemci sertifikası üretilir.
3. Kullanıcının **Profile** (`.ovpn`) dosyasını indirin ve güvenli bir yolla teslim edin - dosya kullanıcının sertifikasını ve özel anahtarını içerir. E-posta veya mesajlaşma uygulamalarıyla göndermeyin; USB bellekle ya da yalnızca o kullanıcının erişebildiği güvenli bir paylaşım üzerinden verin.
4. Kullanıcı profili OpenVPN istemcisine aktarır ve kendi kullanıcı adı ile parolasını girerek bağlanır.

Erişim yetkisi değişiklikleri **Apply** gerekmeden anında devreye girer. **Erişimi kaldırmak sertifikayı anında iptal eder (revoke)**: kullanıcı `.ovpn` dosyasının bir kopyasını saklamış olsa bile profil derhal çalışmayı durdurur.

Kullanıcıyı **System > Users** sayfasından devre dışı bırakmak veya silmek de sertifikayı aynı şekilde iptal eder.

**Sabit VPN adresleri (Fixed VPN addresses)** - IP adresi güvenlik duvarı kurallarında tekil olarak kullanılan kullanıcılar için - **System > Users** altından (**Fixed VPN address**) tanımlanır. Sabit adresler VPN ağının alt yarısından verilir (`10.201.0.0/24` için `10.201.0.2` - `10.201.0.127`); üst yarı diğer tüm kullanıcıların dinamik havuzudur. **A user may connect from several devices at the same time** açıkken sabit VPN adresi kullanılamaz.

Yalnızca yöneticiler istemci profillerini indirebilir.

## Bağlı istemciler (Connected clients)

**Connected clients** tablosu o anda bağlı olan kullanıcıları listeler: **User**, **From** (bağlandıkları genel IP adresi), **VPN address**, ne zamandan beri bağlı olduğu (**Connected**) ve **Received** / **Sent** trafik miktarı. [Kontrol panelinin](/tr/manual/dashboard/) **VPN connections** kartı da aynı özeti sunar.

## OPNsense veya pfSense'ten geçiş

Kullanıcılar bilgisayarlarında kurulu mevcut profillerini değiştirmeden kullanmaya devam edebilir: OPNsense içe aktarma aracı (**OPNsense'ten Geçiş** bölümüne bakınız) sertifika otoritesini, sunucu sertifikasını, kullanıcı sertifikalarını ve TLS anahtarını devralır; böylece Stratum FW eski güvenlik duvarının dış adresini devraldığında mevcut profiller kesintisiz çalışmayı sürdürür.

## Sorun giderme

| Belirti | Kontrol edilecek nokta |
| :--- | :--- |
| İstemci hiç bağlanamıyor | Profildeki **External address** ve **Port** değerlerinin doğru ve dışarıdan erişilebilir olduğunu kontrol edin; servis sağlayıcı modemi yönlendirici (router) modundaysa portu güvenlik duvarına yönlendirmelidir; kullanıcının bulunduğu ağ UDP'ye izin vermiyorsa TCP kullanın. |
| *AUTH_FAILED* | Kullanıcı adı veya parola hatalıdır ya da kullanıcının VPN erişim yetkisi kapalıdır. **Diagnostics > System Logs** kesin nedeni gösterir. |
| Bağlantı kuruluyor ama iç sunucular yanıt vermiyor | Sunucunun bulunduğu ağ **Local networks VPN clients may reach** altında işaretli olmalıdır; sunucunun kendi güvenlik duvarı (örneğin Windows Güvenlik Duvarı) VPN alt ağına izin vermelidir; VPN ağı kullanıcının ev ağıyla çakışmamalıdır. |
| İç alan adları çözülmüyor | **DNS servers** boş (güvenlik duvarı) veya iç DNS sunucusu olmalı; **DNS domain** tanımlı olmalı; Windows istemcilerde **block-outside-dns** açık olmalıdır. |
