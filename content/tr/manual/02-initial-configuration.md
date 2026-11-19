---
title: "İlk Yapılandırma"
seo_title: "02. İlk Yapılandırma — Stratum FW Kullanım Kılavuzu"
description: "Web paneline ilk giriş, iki aşamalı giriş ile hesabınızı güvenceye alma, kurulum sihirbazının hazır bıraktığı ayarlar, internet bağlantısı (statik, DHCP, PPPoE), DNS ve ilk kontroller."
weight: 2
chapter_num: "02"
url: "/tr/manual/initial-configuration/"
aliases:
  - "/tr/manual/02-initial-configuration/"
translationKey: "manual-02-initial-configuration"
---

Kurulum sihirbazı size çalışan bir güvenlik duvarı bırakır: LAN internete çıkar ve dışarıdan gelen hiçbir bağlantı LAN tarafına ulaşamaz. Bu bölüm web paneline yapılan ilk ziyareti - oturum açma, hesabınızı güvenceye alma, temel sistem ayarları, internet bağlantısı ve DNS yapılandırmasını - adım adım ele alır ve güvenlik duvarını canlı hizmete almadan önce yapılması önerilen kısa bir kontrol listesiyle tamamlanır.

## Oturum açma

LAN üzerindeki bir bilgisayardan konsolda **Web panel** olarak gösterilen adresi açın; örneğin `https://192.168.1.1/`.

**Sertifika uyarısı.** Yeni bir kurulumda panel, güvenlik duvarının kendi kendine ürettiği bir sertifikayı kullanır; bu nedenle tarayıcı *Bağlantınız gizli değil* (*Your connection is not private*) benzeri bir uyarı gösterir. Bu beklenen bir durumdur: *Gelişmiş* (*Advanced*) seçeneğine tıklayıp adrese devam edin. Daha sonra **System > Certificates** altından kendi sertifikanızı yüklediğinizde (veya tarayıcınıza bu sertifikaya güvenmesini söylediğinizde) uyarı ortadan kalkar.

Kurulum sihirbazında belirlediğiniz parolayla `admin` kullanıcısı olarak oturum açın.

![Oturum açma sayfası](/images/manual/initial-signin.png)

- **Invalid username or password.** Kullanıcı adı veya parola hatalıdır. Kullanıcı adları büyük/küçük harfe duyarlıdır.
- **Too many failed attempts from this address. Try again later.** Giriş koruması IP adresinizi geçici olarak engellemiştir: varsayılan olarak 5 dakika içinde yapılan 5 başarısız deneme ilgili adresi 15 dakika boyunca engeller. Sürenin dolmasını bekleyin veya başka bir bilgisayardan **System > Settings > Admin Access** sayfasına girerek engeli kaldırın. Engellemeler bildirim olarak da gönderilebilir.
- Kullanıcı için iki aşamalı giriş açıksa ikinci bir ekran kimlik doğrulama uygulamasındaki **Code** değerini sorar (aşağıya bakınız).

**System > Settings > General > Sign-in notice** altında tanımlanan metin (örneğin bir yasal uyarı metni) oturum açma sayfasında gösterilir.

## Hesabınızı güvenceye alın

Sağ üst köşedeki kullanıcı adınıza tıklayarak **Your account** sayfasını açın.

![Hesap sayfası](/images/manual/initial-account.png)

### Parolanızı değiştirme

Önce mevcut parolanızı (**Current password**), ardından yeni parolanızı (**New password**) iki kez girin. Parolalar en az 10 karakter olmalıdır. Parolayı değiştirdiğinizde bulunduğunuz tarayıcıda oturumunuz açık kalır, diğer tüm cihazlardaki açık oturumlarınız ise sonlandırılır.

### İki aşamalı giriş (Two-factor sign-in)

İki aşamalı giriş etkinleştirildiğinde çalınmış bir parola tek başına yeterli olmaz: paroladan sonra oturum açma ekranı telefonunuzdaki kimlik doğrulama uygulamasından (Google veya Microsoft Authenticator, 1Password, Bitwarden ya da zamana dayalı TOTP destekleyen herhangi bir uygulama) 6 haneli bir kod ister. Her yönetici hesabı için etkinleştirilmesini önemle öneririz.

1. **Two-factor sign-in** bölümünde parolanızı (**Your password**) girin ve **Set up two-factor sign-in** düğmesine tıklayın.
2. QR kodunu kimlik doğrulama uygulamasıyla tarayın veya yanında gösterilen **Key** değerini elle girin. QR kodu doğrudan güvenlik duvarının kendisi tarafından çizilir; gizli anahtar cihazdan dışarı çıkmaz.
3. Uygulamadaki 6 haneli kodu (**Code from the app**) girin ve **Switch on** düğmesine tıklayın.
4. Panel **10 kurtarma kodu** (`3f9a-1c07-b2e4` gibi) gösterir. Bunları hemen güvenli bir yere kaydedin; yalnızca bir kez gösterilirler. Telefonunuz yanınızda olmadığında her bir kod bir defaya mahsus giriş yapmanızı sağlar. Kaydettikten sonra **I have saved them** düğmesine tıklayın.

İki aşamalı girişi açmak diğer tüm cihazlardaki oturumlarınızı sonlandırır.

Etkin durumdayken ilgili kartta kaç adet kurtarma kodunuzun kaldığı görünür. **Your password** ve güncel bir doğrulama kodu (**Current code**) girerek **New recovery codes** (eski kodlar geçersiz olur) üretebilir veya **Switch off** ile iki aşamalı girişi kapatabilirsiniz.

> **Telefon kaybı:** kurtarma kodlarınızdan biriyle giriş yapın. Kurtarma kodunuz yoksa başka bir yönetici **System > Users** sayfasından sizin için iki aşamalı girişi kapatabilir veya fiziksel konsol erişiminiz varsa konsol menüsündeki **3) Reset admin password** seçeneği yeni parola belirlerken iki aşamalı girişi devre dışı bırakma imkanı sunar.

## Hazır yapılandırılmış olarak gelen ayarlar

Kurulum sihirbazı ve yerleşik varsayılan ayarlar güvenlik duvarını şu durumda bırakır:

| Alan | Kurulum sihirbazı sonrasındaki durum | Değiştirildiği yer |
| :--- | :--- | :--- |
| Arayüzler | WAN, LAN ve atadığınız tüm OPT arayüzleri, girdiğiniz IP adresleriyle hazırdır | **Interfaces > Assignments** |
| Güvenlik duvarı kuralları | Sihirbazda atanan her arayüzde tek bir kural bulunur: **Default allow ... to any** - ilgili ağ her yere erişebilir. Daha sonra eklenen arayüzler kuralsız başlar: siz kural ekleyene kadar o arayüzlerden gelen tüm trafik engellenir. | **Firewall > Rules** |
| İnternetten gelen trafik | İçeriden başlatılan bağlantıların yanıtları hariç WAN üzerinden gelen her şey engellenir. Güvenlik duvarı WAN adresinde ping yanıtı verir. Bogon adreslerinden (internette bulunmaması gereken IP aralıklarından) gelen trafik düşürülür. | **Firewall > Rules**, **System > Settings > Firewall** |
| NAT | Otomatik giden NAT (outbound NAT): tüm iç ağlar WAN adresi üzerinden internete çıkar. | **Firewall > NAT** |
| Panel erişimi | Port 443 üzerinden HTTPS; port 80 otomatik olarak HTTPS'e yönlendirilir. İç ağlardan erişimde ilgili arayüzün kuralları geçerlidir; *anti-lockout* kuralı diğer kurallar ne derse desin LAN ağının panele her zaman ulaşabilmesini sağlar. Sihirbazda açmadıysanız WAN tarafından kapalıdır. | **System > Settings > Admin Access** |
| SSH | Açık, port 22, kurulum aracında belirlenen parolayla `root` kullanıcısı; erişim kapsamı panel ile aynıdır | **System > Settings > Admin Access** |
| DHCP | LAN için (ve sihirbazda açtığınız OPT arayüzleri için) etkin; girdiğiniz IP aralığı ve 12 saatlik kiralama (lease) süresiyle çalışır | **Services > DHCP Server** |
| DNS | Güvenlik duvarı bağlı ağlar için DNS yanıtı verir (dnsmasq) ve sorguları `1.1.1.1` ile `8.8.8.8` adreslerine iletir | **Services > DNS Resolver** |
| İsimlendirme | Cihaz adı (hostname) `stratum`, yerel alan adı `lan` | **System > Settings > General** |
| Zaman | Seçtiğiniz saat dilimi; sistem saati internet üzerinden (`pool.ntp.org`) eşitlenir | **System > Settings > General**, **Services > NTP** |

## Temel sistem ayarları

**System > Settings** sayfasını ve **General** sekmesini açın.

![System > Settings > General](/images/manual/initial-general.png)

| Ayar | İşlevi |
| :--- | :--- |
| **Hostname** | Güvenlik duvarının adı: küçük harfler, rakamlar ve `-`, en fazla 63 karakter. Panelde, konsolda, bildirimlerde ve uzak sunuculara gönderilen günlüklerde görünür. Her güvenlik duvarına `fw-office` gibi kendine özgü bir ad verin. |
| **Local domain** | İç ağlarınızın alan adı (varsayılan `lan`). DHCP sunucusundan adres alan cihazlara `isim.<alan-adı>` biçiminde isimle ulaşılabilir ve bu alan adına yönelik sorgular güvenlik duvarından dışarı çıkmaz. İç ağınızda halihazırda bir alan adı varsa (örneğin bir Active Directory etki alanı), bu değeri değiştirmeden önce DNS bölümünü inceleyin. |
| **Time zone** | Günlük kayıtları, grafikler, zamanlamalar ve raporlar için kullanılan saat dilimi. |
| **Sign-in notice** | Oturum açma ekranında gösterilen metin (örneğin yetkili erişim uyarısı). Kaydedildiği anda devreye girer; göstermek istemiyorsanız boş bırakın. |
| **Play a sound at start-up and shutdown** | Güvenlik duvarı açıldığında ve kapanırken kasa hoparlöründen (PC speaker) kısa bir melodi çalar; ekransız donanım cihazları için oldukça kullanışlıdır. **Test** düğmesi sesi tarayıcıda değil, doğrudan güvenlik duvarı üzerinde çalar. |

**Save** düğmesine, ardından üst çubuktaki **Apply changes** ve doğrulama sonrasında **Keep changes** düğmesine tıklayın.

## İnternet bağlantısı

Kurulum sihirbazı WAN arayüzünü DHCP veya sabit IP adresine ayarlar. Bunu değiştirmek ya da PPPoE kurmak için **Interfaces > Assignments** sayfasını açın ve **WAN** arayüzünü düzenleyin.

![WAN arayüzünü düzenleme ekranı](/images/manual/initial-wan.png)

Panele düzenlemekte olduğunuz arayüz üzerinden bağlıysanız sayfa, bağlantıyı kesen bir değişikliğin **Apply changes** sonrasında 120 saniye içinde otomatik olarak geri alınacağını hatırlatır.

**Description**, arayüzün panel genelinde gösterilecek görünen adıdır (örneğin *Fibre*). WAN arayüzü her zaman etkindir.

**IPv4 configuration** türünü seçin:

| Tür | Ne zaman kullanılır | Ayarlar |
| :--- | :--- | :--- |
| **Static** | İnternet servis sağlayıcınız size sabit bir IP adresi verdiğinde. | **IPv4 address / prefix** - güvenlik duvarının önek uzunluğuyla birlikte kendi adresi, örneğin `203.0.113.10/29`; **Gateway** - servis sağlayıcının aynı ağ içindeki yönlendirici adresi. |
| **DHCP** | Adres otomatik geldiğinde: güvenlik duvarının önünde yönlendirici kipinde bir modem, kurumsal ağ veya veri merkezi hattı bulunduğunda. | Ek ayar gerekmez. |
| **PPPoE** | Servis sağlayıcı oturumunu doğrudan güvenlik duvarının açtığı durumlarda - köprü (bridge) kipindeki fiber ve DSL modemlerde yaygındır. | Servis sağlayıcıdan alınan **Username** ve **Password**; **Service name** yalnızca sağlayıcı özellikle isterse doldurulur (genellikle boştur). |
| **None** | Fiziksel portun kendi başına bir IPv4 adresi taşımadığı durumlarda (örneğin yalnızca VLAN etiketli trafik taşıyorsa). | Ek ayar gerekmez. |

**MTU** - varsayılan değer (standart Ethernet için 1500, PPPoE için 1492) için boş bırakın. Yalnızca servis sağlayıcınız farklı bir değer istediğinde değiştirin.

**PPPoE hakkında.** Fiziksel port PPPoE oturumunu taşır; bağlantının kendisi ayrı bir `pppoe` arayüzü olarak görünür ve güvenlik duvarı kuralları ile NAT bunu otomatik olarak izler. Parola yalnızca `root` kullanıcısının okuyabildiği ayrı bir dosyada saklanır ve bir daha asla ekranda gösterilmez; arayüzü daha sonra düzenlerken mevcut parolayı korumak için parola alanını boş bırakmanız yeterlidir.

**Save**, **Apply changes** ve bağlantının çalıştığını doğruladıktan sonra **Keep changes** düğmesine tıklayın. **Interfaces > Assignments** sayfasındaki WAN satırı ve kontrol paneli (Dashboard) alınan IP adresini gösterir.

> **İpucu:** Köprü kipinde olmayan (kendisi de yönlendirme yapan) bir servis sağlayıcı modemi genellikle kendi LAN tarafında DHCP ile IP dağıtır. Bu durumda WAN için **DHCP** seçin ve güvenlik duvarının LAN ağının modemin ağından farklı olduğundan emin olun - örneğin modem `192.168.1.0/24` kullanıyorsa güvenlik duvarı LAN tarafında `10.10.10.0/24` kullanın.

İkinci bir internet hattı, WAN tipinde ek bir arayüz olarak eklenir; hatlar arasında yedeklilik (failover) ve seçilen trafiği ikinci hat üzerinden gönderme [**Interfaces > Gateways**](/tr/manual/interfaces-gateways/) bölümünde anlatılmaktadır.

## DNS

Ağlarınızdaki cihazlar alan adlarını güvenlik duvarına sorar; güvenlik duvarı da internete sorar. **Services > DNS Resolver** sayfasını açın.

![Services > DNS Resolver](/images/manual/initial-dns.png)

| Ayar | İşlevi |
| :--- | :--- |
| **Local domain** | **System > Settings > General** sayfasındaki yerel alan adı ayarıyla aynıdır. |
| **Upstream DNS servers** | Güvenlik duvarının sorgu ilettiği üst DNS sunucuları: virgülle ayrılmış en fazla dört adres. Varsayılan `1.1.1.1, 8.8.8.8`. |
| **Use only these upstream servers** | Başka yollarla öğrenilen DNS sunucularını (örneğin DHCP WAN üzerinden gelenleri) yok sayar. Varsayılan olarak açıktır. |
| **DNS resolver** | **dnsmasq** (DNS ve DHCP tek serviste - varsayılan) veya **Unbound** (doğrulayıcı çözümleyici; DHCP görevini dnsmasq sürdürür). |
| **Unbound mode** | **Forwarder** - yukarıdaki üst DNS sunucularına sorar; **Resolver** - üçüncü taraf bir DNS sağlayıcısı kullanmadan doğrudan internetin kök (root) sunucularına sorar. |
| **Validate DNSSEC signatures** | Unbound kullanılırken DNSSEC imzası geçersiz olan yanıtları reddeder. |

> DNS engelleme listelerini, Web Filtresini veya Web Activity raporlarını kullanmak istiyorsanız **Unbound seçin**; bu özelliklerin tamamı Unbound içinde çalışır. Yalnızca yönlendirme yapan sade bir güvenlik duvarı için dnsmasq daha hafif bir seçenektir.

Aynı sayfa üzerinden seçilen ağların DNS sorgularını güvenlik duvarına zorunlu olarak yönlendirebilirsiniz; böylece istemci cihazlar farklı bir DNS sunucusu yazarak engelleme listelerini aşamaz. Bu özellik ve diğer DNS ayarları (ana bilgisayar ve alan adı geçersiz kılmaları) **Services > DNS Resolver** bölümünde ayrıntılı olarak ele alınmaktadır.

## Her şeyin çalıştığını kontrol edin

1. **LAN üzerindeki bir bilgisayardan:** bilgisayar DHCP aralığından bir IP adresi almalı, ağ geçidi ve DNS sunucusu olarak güvenlik duvarını görmelidir. Birkaç web sitesi açarak bağlantıyı deneyin.
2. **Kontrol panelinde (Dashboard):** WAN kartı bir IP adresi, ağ geçidi ve gecikme (latency) değeri göstermelidir.
3. **Güvenlik duvarı üzerinden:** **Diagnostics > Ping / Traceroute / DNS** sayfasından `1.1.1.1` adresine ping atın (hat erişimi) ve `stratumfw.com` gibi bir alan adını sorgulayın (DNS çözümleme).
4. **Sistem saati:** **Diagnostics > System Logs** içindeki zaman damgalarının kendi saatinizle eşleştiğini doğrulayın. Yanlış sistem saati sertifikaları, iki aşamalı giriş kodlarını ve günlük kayıtlarını bozar.

LAN tarafında internet yoksa: önce **Interfaces > Assignments** sayfasından WAN adresini, ardından engellenen paketler için **Diagnostics > Firewall Log** kaydını ve son olarak tanılama sayfasındaki DNS sorgusunu kontrol edin. **Troubleshooting** (Sorun Giderme) bölümü bu adımları sırasıyla açıklar.

## Güvenlik duvarını devreye almadan önce

Kısa bir kontrol listesi - her maddenin kendi ayrıntılı bölümü vardır:

| | Madde | İlgili Sayfa |
| :--- | :--- | :--- |
| ☐ | Her yönetici için iki aşamalı giriş (2FA) | **Your account** |
| ☐ | Güvenlik duvarını yöneten her kişi için ayrı kişisel hesap - ortak `admin` parolası kullanmama | **System > Users** |
| ☐ | En güncel sürümü yükleme | **System > Update** |
| ☐ | Manuel yedek indirme ve kendi sunucunuza otomatik şifreli yedekleme kurma | **System > Backup** |
| ☐ | Telegram veya e-posta bildirimlerini ayarlama (WAN düşmesi, başarısız yedekleme, engellenen girişler vb.) | **System > Settings > Notifications** |
| ☐ | Panele ve SSH'a kimlerin ulaşabileceğini sınırlandırma; SSH erişimini yalnızca anahtar tabanlı yapma | **System > Settings > Admin Access** |
| ☐ | Her iç ağ için "her yere izin ver" yerine yalnızca ulaşması gereken hedeflere izin veren kurallar yazma | **Firewall > Rules** |
| ☐ | Panel için kendi sertifikanızı yükleme (isteğe bağlı) | **System > Certificates** |
