---
title: "Giriş"
seo_title: "01. Giriş — Stratum FW Kullanım Kılavuzu"
description: "Stratum FW nedir, yapılandırma sistemi nasıl çalışır, bir bakışta yönetim paneli, kullanıcılar ve roller ile bu kılavuzun yazım kuralları."
weight: 1
chapter_num: "01"
url: "/tr/manual/introduction/"
aliases:
  - "/tr/manual/00-introduction/"
  - "/tr/manual/01-introduction/"
translationKey: "manual-00-introduction"
---

## Bu kılavuz hakkında

Bu kılavuz, bir Stratum FW güvenlik duvarını kuran ve işleten kişiler içindir: ağ ve sistem yöneticileri ile daha önce pfSense, OPNsense veya ticari bir güvenlik duvarı kullanmış olan herkes. IP adresi, alt ağ (subnet), ağ geçidi (gateway) ve güvenlik duvarı kuralı kavramlarını bildiğinizi varsayar; FreeBSD bilmenizi ise varsaymaz.

Kılavuz boyunca kullanılan yazım kuralları:

| Gördüğünüz ifade | Anlamı |
| :--- | :--- |
| **Firewall > Rules** | Paneldeki bir sayfa: önce menü bölümü, ardından sayfa adı. |
| **Apply changes** | Panelde göründüğü şekliyle birebir yazılmış bir düğme, alan veya mesaj. |
| `stratum-config apply` | Güvenlik duvarının konsolunda veya SSH üzerinden yazılan bir komut. |
| `/usr/local/etc/stratum/config.json` | Güvenlik duvarı üzerindeki bir dosya. |
| *LAN*, *OPT1* | Bir arayüzün rolü (bkz. [Bir bakışta panel](#the-panel-at-a-glance)). |

> **Not:** Yönetim paneli, mesajları ve günlük kayıtları İngilizcedir. Bu kılavuzda ekran öğeleri ve sistem mesajları oldukları gibi aktarılmıştır; böylece burada okuduğunuz ifade ekranda gördüğünüzle birebir aynıdır.

## Stratum FW nedir

Stratum FW bir güvenlik duvarı ve yönlendirici işletim sistemidir. Bir bilgisayarı, sunucuyu, ağ cihazını veya sanal makineyi ağlarınız ile internet arasındaki ağ geçidine dönüştürür ve tamamen web paneli üzerinden yönetilir. Kullanımı ücretsizdir: abonelik, lisans anahtarı veya ücretli özellik katmanı yoktur.

Panelin altında iyi bilinen, kendini kanıtlamış bileşenleriyle **FreeBSD 15.1** çalışır:

| Görev | Bileşen |
| :--- | :--- |
| Paket filtreleme, NAT, politika yönlendirme | FreeBSD **pf** |
| Bant genişliği sınırları | FreeBSD **dummynet** |
| DNS çözümleyici, DNS engelleme listeleri, Web Filtresi | **Unbound** (veya dnsmasq) |
| DHCP sunucusu | **dnsmasq** |
| Uzaktan erişim VPN | **OpenVPN** ve çekirdek **WireGuard** |
| Noktadan noktaya (Site-to-site) VPN | **IPsec** (strongSwan) ve **WireGuard** |
| Saldırı tespit ve önleme | **Suricata** |
| PPPoE | **mpd5** |
| Web paneli | Python (FastAPI) uygulamasının önünde **nginx** |

Sistem **ZFS** üzerine kurulur. Her güncelleme bir *önyükleme ortamı* (boot environment) - yani istediğiniz zaman geri dönebileceğiniz tam bir sistem anlık görüntüsü - oluşturur; bu sayede her güncelleme geri alınabilir.

<span id="how-a-change-reaches-the-system"></span>
## Bir değişiklik sisteme nasıl ulaşır

Bu, Stratum FW'deki en önemli temel ilkedir ve herhangi bir ayarı değiştirmeden önce anlaşılması gerekir.

### Tek yapılandırma dosyası

Panelde ayarladığınız her şey - arayüzler, kurallar, VPN'ler, servisler - tek bir dosyada, `/usr/local/etc/stratum/config.json` içinde tutulur. Sistemin kendi yapılandırma dosyaları (`/etc/pf.conf`, DNS ve DHCP yapılandırması, VPN yapılandırması vb.) bu dosyadan **otomatik üretilir**. Bu dosyaları elle düzenlemeyin: bir sonraki Apply işlemi onları yeniden yazar.

Gizli veriler bunun istisnasıdır: parolalar, özel anahtarlar, ön paylaşımlı anahtarlar (PSK), RADIUS sırları ve API anahtarları yalnızca sistemin okuyabildiği ayrı dosyalarda saklanır. Asla `config.json` içine yazılmaz ve tarayıcıya geri gönderilmez; ilgili sayfa gizli bilginin kendisini değil, yalnızca kayıtlı olduğunu gösterir.

### Önce Kaydet, sonra Uygula (Save, then Apply)

Bir sayfayı kaydetmek çalışan sistemi değil, yapılandırmayı değiştirir. Kaydedilmiş ancak henüz etkinleştirilmemiş bir değişiklik olduğunda her sayfanın üst kısmının hemen altında bir çubuk belirir:

> The configuration has been changed. Apply the changes to make them active.  **Apply changes**  **Discard**

Birden fazla sayfada değişiklik yapıp hepsini tek seferde uygulayabilirsiniz. **Discard**, son Apply işleminden bu yana kaydedilen tüm değişiklikleri atar (iki kez tıklayın; ilk tıklama onay ister).

![Kaydedilmiş değişiklik bulunan uygulama çubuğu](/images/manual/intro-applybar-pending.png)

### Apply: önce denetlenir, sonra etkinleştirilir

**Apply changes** düğmesine tıkladığınızda Stratum FW çalışan sisteme dokunmadan önce yeni yapılandırmayı denetler: paket filtresi kuralları, web sunucusu, DNS ve DHCP yapılandırması, SSH ve saldırı tespit sistemi kendi doğrulama araçlarıyla test edilir; değişikliğin ihtiyaç duyduğu programların ve çekirdek modüllerinin (VPN, bant genişliği sınırları vb.) mevcut olduğu doğrulanır. Herhangi bir denetim başarısız olursa hiçbir şey değiştirilmez ve çubukta nedeni gösterilir; tam çıktı [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) altındaki *Configuration* sekmesinde yer alır.

Denetimler başarılı olursa mevcut durum bir anlık görüntü olarak kaydedilir ve yeni yapılandırma etkinleştirilir. Yalnızca değişiklikten etkilenen servisler yeniden yüklenir veya yeniden başlatılır.

<span id="keep-changes-or-it-is-undone"></span>
### Değişiklikleri koruma veya otomatik geri alma

Yeni bir yapılandırma erişiminizi kesebilir - hatalı bir kural, değiştirilmiş bir panel portu veya yanlış bir arayüz ataması buna yol açabilir. Bu nedenle panelden yapılan her Apply işleminden sonra üst çubuk geri saymaya başlar:

> New configuration is active. Keep it within **120** s, otherwise it is reverted automatically.
> **Keep changes**  **Revert now**

- Bulunduğunuz sayfa dahil her şeyin düzgün çalıştığını doğruladığınızda **Keep changes** düğmesine tıklayın. Değişiklik kalıcı hale gelir.
- Hemen önceki duruma dönmek için **Revert now** düğmesine tıklayın.
- Hiçbir şey yapmazsanız - örneğin artık panele erişemiyorsanız - 120 saniye sonunda önceki yapılandırma otomatik olarak geri yüklenir. Yaptığınız son değişiklik dışında hiçbir şey kaybetmezsiniz.

![Apply sonrası geri sayım çubuğu](/images/manual/intro-applybar-confirm.png)

> **Pratik kural:** **Keep changes** düğmesine tıklamadan önce değiştirdiğiniz şeyi test edin: artık açılması gereken sayfayı açın, engellenmesi gereken hedefe ping atın. Panelin kendi portunu veya adresini değiştirdiyseniz üst çubuk yeni adresi gösterir; yeni adresi açıp değişikliği oradan onaylayın.

Konsoldan `stratum-config apply` komutuyla uygulama yaptığınızda süre 180 saniyedir; `stratum-config confirm` ile onaylayabilir veya `stratum-config rollback` ile geri alabilirsiniz (konsol ve komut satırı bölümüne bakınız).

### Anında etkili olan değişiklikler

Bazı ayarlar yalnızca panelin kendisini ilgilendirir veya zamanlanmış görev olarak çalışır; bunlar Apply gerektirmeden kaydedildiği anda devreye girer: oturum zaman aşımı ve giriş bildirimi, bildirim kanalları, otomatik yedeklemeler, MCP erişimi, engelleme listeleri ile Web Filtresi ve saldırı tespit sisteminin güncelleme zamanlamaları, ayrıca saldırı tespit kategorileri ve devre dışı bırakılan kurallar. **Discard** bu ayarlara dokunmaz.

Aynı durum operasyonel işlemler ve hesap listeleri için de geçerlidir: açık bağlantıları sonlandırma, captive portal cihaz bağlantısını kesme, captive portal kullanıcı hesapları ve logoları. Bu şekilde çalışan sayfalar ilgili ayarın yanında bunu açıkça belirtir.

<span id="the-panel-at-a-glance"></span>
## Bir bakışta panel

Panele güvenlik duvarının LAN adresi üzerinden HTTPS ile ulaşılır; örneğin `https://192.168.1.1/`. Yeni bir kurulumda sertifika kendinden imzalıdır (self-signed), bu nedenle tarayıcı ilk girişte bir kez uyarır; daha sonra **System > Certificates** altından kendi sertifikanızı yükleyebilirsiniz.

![Panel: solda menü, üst çubuk ve sayfa alanı](/images/manual/intro-panel.png)

Soldaki **menü** yedi bölümden oluşur:

| Bölüm | İçeriği |
| :--- | :--- |
| **Overview** | **Dashboard**: kendi düzenlediğiniz panellerle güvenlik duvarının genel durumu. |
| **Interfaces** | Ağ portları ve rolleri, VLAN'lar, arayüz grupları, ağ geçitleri ve çoklu WAN, sanal IP'ler, statik rotalar. |
| **Firewall** | Kurallar, NAT, takma adlar (aliases), zamanlamalar, IP ve DNS engelleme listeleri, Web Filtresi, bant genişliği sınırları (Shaper). |
| **Services** | Servis durumları, DHCP sunucusu, DNS çözümleyici, NTP, saldırı tespit sistemi, captive portal. |
| **VPN** | OpenVPN, WireGuard, noktadan noktaya WireGuard, IPsec. |
| **System** | Ayarlar, panel kullanıcıları, sertifikalar, yedekleme, güncelleme, güç yönetimi. |
| **Diagnostics** | Güvenlik duvarı ve sistem günlükleri, ağ araçları, trafik grafikleri, Web Activity, bağlantı durumları (states), ARP tablosu, paket yakalama, AI Assistant. |

**Üst çubuk** sağ tarafta şunları gösterir: **bildirim** zili (bir WAN hattının düşmesi veya başarısız yedekleme gibi uyarılar; Telegram veya e-posta ile de gönderilebilen olaylar), **tema** anahtarı (açık, koyu veya sistem ayarı; her kullanıcı için ayrı hatırlanır), **hesabınız** (parola, iki aşamalı giriş) ve **Sign out**.

**Sayfalar ve formlar.** Listeler mevcut kayıtları gösterir; ekleme veya düzenleme işlemi asla açılır pencere (pop-up) açmaz, kendi tam sayfasını açar. **Save** sonrasında yeni kaydın göründüğü listeye dönersiniz. Silme işlemi aynı düğme veya satır üzerinde onay ister.

**Arayüz adları.** Her arayüzün bir *rolü* (*WAN*, *LAN*, *OPT1*, *OPT2* ...) ve sizin belirlediğiniz *GUEST* veya *CAMERAS* gibi bir açıklaması vardır. Panel açıklamayı büyük, rolü ise hemen altında küçük olarak gösterir; böylece iki VLAN hiçbir zaman birbiriyle karışmaz. Kurallar, DHCP ayarları ve diğer tüm yapılandırmalar role bağlı kalır.

## Kullanıcılar ve roller

Panele giriş yapan her kişinin **System > Users** altında üç rolden birine sahip kendi hesabı bulunur:

| Rol | Görebildikleri | Değiştirebildikleri |
| :--- | :--- | :--- |
| **Administrator** | Her şey. | Apply, kullanıcılar, güncellemeler ve yedeklemeler dahil her şey. |
| **Operator** | Aşağıdaki yalnızca yöneticilere özel sayfalar dışındaki her şey. | Yalnızca günlük operasyonel işlemler: ağ araçları (ping, traceroute, DNS sorgusu), bağlantıları kapatma (states), Wake-on-LAN, ARP kayıtları ile captive portal kullanıcı hesapları ve oturumları. Yapılandırma değişikliği yapamaz. |
| **Read-only** | Aşağıdaki yalnızca yöneticilere özel sayfalar dışındaki her şey. | Kendi parolası, iki aşamalı girişi, teması ve kontrol paneli düzeni dışında hiçbir şey. |

Bazı sayfalar özel veriler içerdiği veya doğrudan sistem kontrolü sağladığı için görüntüleme açısından bile yalnızca yöneticilere açıktır: yönetici erişimi, MCP ve SNMP için **Settings** sayfaları, **Users**, **Backup**, **Update**, **Power**, **Web Activity**, engelleme listelerinin **Blocked** raporu, **Packet Capture**, **AI Assistant**, IPsec günlüğü ve VPN istemci profili indirmeleri.

Herkes için geçerli temel güvenlik önlemleri şunlardır:

- **Giriş koruması:** varsayılan olarak, bir adresten 5 dakika içinde yapılan 5 başarısız giriş denemesi o adresi 15 dakika boyunca engeller (**System > Settings > Admin Access** altından ayarlanabilir).
- **İki aşamalı giriş (TOTP):** her kullanıcı kendi hesabı altından kurtarma kodlarıyla birlikte etkinleştirebilir. QR kodu doğrudan güvenlik duvarı üzerinde üretilir.
- **Oturum zaman aşımı:** bir oturum varsayılan olarak 4 saat sonra sona erer (**Admin Access** altından 15 dakika ile 24 saat arasında ayarlanabilir).
- **Günlük kaydı:** her oturum açma ve her değişiklik, kullanıcı adı ve geldiği IP adresiyle birlikte sistem günlüğüne yazılır.

## Güvenlik ağları

Stratum FW, olası bir hatanın kolayca geri alınabilmesi için tasarlanmıştır:

| Durum ... | Sonuç ... |
| :--- | :--- |
| bir değişiklik erişiminizi keserse | 120 saniye sonra otomatik olarak geri alınır (bkz. [Değişiklikleri koruma veya otomatik geri alma](#keep-changes-or-it-is-undone)). |
| bir değişiklik ön denetimlerden geçmezse | hiç uygulanmaz ve üst çubukta nedeni gösterilir. |
| bir güncelleme sorun çıkarırsa | açılış menüsünden bir önceki önyükleme ortamını (boot environment) seçerek sistemi başlatabilirsiniz. |
| donanım arızalanırsa | [**System > Backup**](/tr/manual/system-backup/) altından yedek geri yükleyebilirsiniz (manuel indirmeler veya kendi sunucunuzdaki zamanlanmış şifreli kopyalar). |
| yönetici parolasını unutursanız | konsol menüsünden sıfırlayabilirsiniz (**3) Reset admin password**). |

## Sıradaki adımlar

- İlk panel ayarları için: [**02 İlk Yapılandırma**](/tr/manual/initial-configuration/) bölümüyle devam edin.
- Sıfırdan kurulum yapıyorsanız: [**03 Kurulum**](/tr/manual/installation/) bölümüne geçin.
- OPNsense'ten geçiş yapıyorsanız: [**12 OPNsense'ten Geçiş**](/tr/manual/migrating-from-opnsense/) bölümü mevcut yapılandırmanızı içe aktarır.
- Sisteminiz zaten çalışıyorsa: içindekiler menüsünden ihtiyaç duyduğunuz bölüme doğrudan geçebilirsiniz.
