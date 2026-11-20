---
title: "Sistem > Ayarlar: Genel ve Yönetici Erişimi"
seo_title: "09.1 Sistem > Ayarlar: Genel ve Yönetici Erişimi (General & Admin Access) — Stratum FW Kullanım Kılavuzu"
description: "Sistem adı, yerel alan adı, saat dilimi, giriş uyarısı ve açılış sesi; web paneli portu ve oturum zaman aşımı, güvenlik duvarını kimlerin yönetebileceği, anti-lockout koruması, WAN erişimi, giriş koruması ve SSH."
weight: 901
chapter_num: "09.1"
url: "/tr/manual/system-settings-general-access/"
aliases:
  - "/tr/manual/09-01-system-settings-general-access/"
translationKey: "manual-09-01-system-settings-general-access"
---

**System > Settings** sayfası güvenlik duvarının kendi sistem ayarlarını dokuz sekmede toplar: **General**, **Admin Access**, **Firewall & NAT**, **Logging**, **Notifications**, **Networking**, **Tunables**, **SNMP** ve **MCP**. Bu bölüm ilk iki sekmeyi ele alır; diğer sekmeler izleyen bölümlerde açıklanmıştır.

Sistem ayarlarını yalnızca yöneticiler değiştirebilir.

## Genel (General)

![System > Settings > General](/images/manual/set-general.png)

| Ayar | Anlamı |
| :--- | :--- |
| **Hostname** | Her sayfanın üst çubuğunda ve konsol menüsünde gösterilen sistem adı. Her güvenlik duvarına ayırt edici bir ad verin, örneğin `fw-belek`. Varsayılan `stratum`. |
| **Local domain** | İç ağlarınızın yerel alan adı (varsayılan `lan`). Cihazlara `isim.<alanadı>` biçiminde ulaşılır. [**Services > DNS Resolver**](/tr/manual/services-dns-resolver/) altındaki ayarla aynıdır. |
| **Time zone** | Günlük kayıtları, grafikler ve zamanlamalar için kullanılan saat dilimi. Zaman sunucuları [**Services > NTP**](/tr/manual/services-ntp/) sayfasındadır. |
| **Sign-in notice** | Oturum açma ekranında gösterilecek yasal veya kurumsal bilgilendirme metni - örneğin *Yalnızca yetkili erişim. Tüm işlemler kayıt altına alınmaktadır.* En fazla 1000 karakter; istemiyorsanız boş bırakın. Kaydedildiğinde anında geçerli olur. |
| **Play a sound at start-up and shutdown** | Güvenlik duvarı açılışını tamamladığında dahili PC hoparlöründen (buzzer) kısa bir melodi, kapanırken veya yeniden başlarken ise kapanış versiyonunu çalar - sunucu odasında cihazın ayağa kalktığını duymak için kullanışlıdır. Melodiler: *Stratum*, *Classic beep*, *Chime (Westminster)*, *Ode to Joy*, *Für Elise*, *Arcade*. **Test** sesi tarayıcıda değil doğrudan güvenlik duvarı donanımında çalar. Hoparlörü olmayan cihazlar sessiz kalır. |

Sistem adı, yerel alan adı ve saat dilimi **Apply changes** sonrasında etkinleşir; sistem adı yeniden başlatma gerekmeden değişir.

## Yönetici Erişimi (Admin Access)

![System > Settings > Admin Access](/images/manual/set-access.png)

Bu sekme güvenlik duvarının nasıl ve nereden yönetilebileceğini belirler. Burada yapılacak bir hata kendinizi dışarıda bırakmanıza yol açabileceği için panel sizi üç ayrı katmanda korur:

- Yeni ayarlar şu anda bağlandığınız IP adresinden erişiminizi kesecekse **kaydetme işlemi doğrudan reddedilir**.
- Değişiklikler **Apply changes** ile devreye girer ve süresi içinde **Keep changes** düğmesine tıklamazsanız **otomatik olarak geri alınır** - panele ulaşamaz hale gelirseniz yalnızca bekleyin, erişim kendiliğinden geri döner.
- Fiziksel konsol menüsündeki **6) Management access** > **2) Reset management access to the defaults** seçeneği port `443`, tüm iç ağlar, *anti-lockout* açık ve parolalı SSH açık durumuna geri döndürür (WAN erişim ayarı ve kayıtlı SSH anahtarları korunur). Aynı menünün **1** numaralı seçeneği WAN yönetim erişimini açar veya kapatır.

### Web paneli (Web panel)

| Ayar | Anlamı |
| :--- | :--- |
| **HTTPS port** | Varsayılan `443`. Değiştirildiğinde onay çubuğu yeni adresi gösterir. Diğer servislerin kullandığı portlar (DNS, DHCP, NTP ...) reddedilir. |
| **Sign out after inactivity** | Hareketsizlik sonrası otomatik oturum kapatma süresi: 15 veya 30 dakika, 1, 4 (varsayılan) veya 8 saat ya da 1 gün. Kendiliğinden yenilenen sayfalar (kontrol paneli gibi) kullanıcı etkinliği sayılmaz. Anında geçerli olur. |
| **Answer plain HTTP (port 80) with a redirect to the panel** | `http://firewall` isteklerini otomatik olarak `https://firewall` adresine yönlendirir. Varsayılan olarak açıktır. |

Panelin SSL/TLS sertifikası [**System > Certificates**](/tr/manual/system-certificates/) sayfasından yönetilir.

### Güvenlik duvarını kimler yönetebilir? (Who may manage the firewall)

| Ayar | Anlamı |
| :--- | :--- |
| **From internal networks** | **All internal networks** - her arayüzün kendi güvenlik duvarı kuralları karar verir (varsayılan); veya **Only the selected interfaces** - güvenlik duvarı kuralları ne derse desin seçilmeyen diğer tüm arayüzlerden panele ve SSH servisine erişim reddedilir. Misafir ve IoT ağlarını yönetim panelinden kesin olarak uzak tutmak için bu seçeneği kullanın. |
| **Anti-lockout: always allow the panel and SSH from LAN** | LAN kurallarında hata yapsanız bile LAN ağından panele ve SSH'a erişimi koruyan en üst öncelikli kuraldır. Varsayılan olarak açıktır; kendi kurallarınızdan tamamen emin olana kadar açık bırakın. |
| **Allow the panel and SSH from WAN (the internet)** | Varsayılan olarak kapalıdır. |
| **Only from these addresses or networks** | WAN erişimi açıldığında bağlanmasına izin verilen IP adresleri veya ağlar (her satıra bir kayıt). **Şiddetle önerilir** - tüm internete açık bırakılan bir yönetim paneli dakikalar içinde taranmaya başlar. Daha güvenli yöntem: WAN erişimini kapalı tutup VPN üzerinden yönetin. |

Sayfa kullanılan aktif portları gösterir. VPN istemcileri panele VPN erişim ayarlarına göre (erişmelerine izin verilen yerel ağlar üzerinden) ulaşır.

### Giriş koruması (Sign-in protection)

**Block an address after** *5* **failed sign-ins within** *5* **minutes, for** *15* **minutes** (varsayılan değerler; 3-20 deneme, 1-60 ve 1-1440 dakika aralığında ayarlanabilir). Hem web paneli girişlerine hem de MCP erişim jetonlarına uygulanır ve kaydedildiği anda devreye girer. Engellenen adresler Telegram veya e-posta ile bildirilebilir ([**Notifications**](/tr/manual/system-settings-logging-notifications/), *An address was blocked after failed sign-ins* olayı).

Kullanıcılar ayrıca kendi hesaplarını iki aşamalı doğrulama (2FA) ile koruyabilir ([**System > Users**](/tr/manual/system-users/)).

### SSH

| Ayar | Anlamı |
| :--- | :--- |
| **Enable SSH** | Ağ üzerinden `root` kullanıcısıyla konsol erişimi. Varsayılan olarak açıktır; giriş yaptıktan sonra [konsol menüsünü](/tr/manual/console-menu/) açmak için `stratum-menu` yazın. |
| **Port** | Varsayılan `22`. |
| **Sign in with** | **Password or key** (parola veya anahtar) ya da **Key only (recommended)** (yalnızca açık anahtar). |
| **Authorised public keys** | Her satıra bir açık anahtar - `id_ed25519.pub` (veya başka bir `.pub` dosyasının) içeriği. **Buraya kesinlikle özel anahtar (private key) yapıştırmayın.** |

SSH anahtarı kullanmak için: kendi bilgisayarınızda `ssh-keygen -t ed25519` çalıştırın, `~/.ssh/id_ed25519.pub` içeriğini buraya yapıştırıp kaydedin ve Apply ile etkinleştirin; `ssh root@<firewall>` bağlantısının anahtarla çalıştığını doğruladıktan sonra **Key only** moduna geçin.

**Save** düğmesi sayfadaki tüm bölümleri birlikte kaydeder.
