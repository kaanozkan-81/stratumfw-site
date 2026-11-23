---
title: "Güvenlik Duvarı > Uygulama Kontrolü (Application Control)"
seo_title: "06.8 Güvenlik Duvarı > Uygulama Kontrolü ve Engelleme Sayfası — Stratum FW Kullanım Kılavuzu"
description: "Ağlarınızdaki cihazların hangi uygulamaları kullandığını görme ve hangilerini kullanamayacağını belirleme: arayüz bazlı tespit, Uygulamalar ve Cihazlar raporları, kategoriler ve özel uygulamalar, Monitor only ve Block kuralları, Block Page CA destekli özelleştirilebilir HTTPS Engelleme Sayfası ve Yapay Zeka Asistanı."
weight: 608
chapter_num: "06.8"
url: "/tr/manual/firewall-application-control/"
aliases:
  - "/tr/manual/06-08-firewall-application-control/"
translationKey: "manual-06-08-firewall-application-control"
---

Uygulama Kontrolü (**Application Control**), seçtiğiniz ağlardaki her bağlantının arkasındaki uygulamayı adlandırır - WhatsApp, Instagram, YouTube, Netflix, Microsoft 365, BitTorrent, OpenVPN, WireGuard, AnyDesk ve yüzlerce fazlası - ve bunları kategorilere ayırır: sosyal medya, mesajlaşma ve aramalar, video ve müzik, indirmeler ve P2P, VPN ve vekil sunucular (proxy), uzaktan erişim, oyunlar, yapay zeka asistanları ve diğerleri.

**Kurallar** ile bu uygulamaları engelleyebilir - arayüz bazında, kategori veya uygulama adına göre ve belirli cihazlar için istisnalar tanımlayarak - her kuralı önce hiçbir trafiği kesmeden yalnızca neleri engelleyeceğini kaydeden **Monitor only** (Yalnızca izle) modunda çalıştırabilir ve **Block** modundaki kurallarda bağlantı hatası yerine kurum logonuzu ve üst görselinizi taşıyan özelleştirilebilir bir **Engelleme Sayfası (Block page)** gösterebilirsiniz.

Sayfa yedi sekmeden oluşur: **Applications**, **Devices**, **Rules**, **Blocked**, **Block page**, **Categories** ve **Settings**. Bu sayfayı yalnızca yöneticiler (*administrators*) açabilir.

## Nasıl çalışır?

- Motor, seçilen her arayüzdeki trafiğin bir **kopyasını** okur. Paketlerin geçiş yolunun üzerinde (*inline*) değildir: motor dursa bile ağ trafiği bundan etkilenmez. Engelleme işlemi motorun bildirimine dayanarak doğrudan güvenlik duvarının kendisi (`pf`) tarafından yapılır - bkz. [Kurallar](#kurallar).
- Her bağlantının ilk paketlerine bakar: protokolün kendisine (OpenVPN, WireGuard, BitTorrent, SSH, oyun protokolleri ...), HTTPS ve QUIC / HTTP/3 bağlantılarındaki sunucu adına (SNI) ve servislerin bilinen adreslerine.
- Motor kendi yetkisiz (*unprivileged*) kullanıcısı altında korumalı bir kum havuzunda (**FreeBSD Capsicum**) çalışır: yalnızca trafik kopyasını okuyabilir ve sonuçlarını yazabilir - başka hiçbir şeye erişemez.
- Diskte yalnızca **cihaz ve uygulama başına günlük toplamlar** tutulur: bağlantı sayısı, indirilen ve yüklenen veri miktarı, ilk ve son görülme zamanı. Tekil sunucu adları veya tek tek bağlantılar diske yazılmaz.
- Cihaz adları, [**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/) bölümünde olduğu gibi DHCP ve ARP geçmişinden alınır.

Motor, nDPI kütüphanesini (yalnızca LGPLv3 bileşenlerini) ve TLS ile QUIC için Stratum FW'nin kendi kodunu kullanır.

## Etkinleştirme

![Firewall > Application Control > Settings](/images/manual/appctl-settings.png)

1. **Settings** sekmesinde **Identify the applications used in the selected networks** seçeneğini açın.
2. İzlenecek **Interfaces** (arayüzler) kutularını işaretleyin. VLAN'lar dahil yalnızca statik adresli iç ağ arayüzleri sunulur. Her arayüz kendi ayrı motor sürecini alır.
3. **Keep the records (days)**: `1-730` gün, varsayılan `30`. Kontrol paneli için tutulan saatlik toplamlar en fazla 14 gün saklanır.
4. **Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın.

**Status** tablosu her arayüzün motor durumunu gösterir: çalışıp çalışmadığı, saniyedeki paket sayısı, açık bağlantılar, son 30 saniyede kaçırılan (*dropped*) paketler ve bellek kullanımı. İlk uygulamalar yaklaşık bir dakika içinde raporlarda görünmeye başlar.

> **Hangi arayüzler seçilmeli?** Cihazların bulunduğu iç ağ arayüzleri: WAN arayüzünde tüm cihazlar güvenlik duvarının genel adresi arkasında gizlenir. Yoğun bir arayüzü izlemek bağlantı sayısına bağlı olarak arayüz başına yaklaşık 100-300 MB bellek kullanır.

## Uygulamalar (Applications)

![Firewall > Application Control > Applications](/images/manual/appctl-apps.png)

**Categories** çubuğu, seçili dönemdeki toplam verinin kategoriler arasında nasıl dağıldığını gösterir. Yalnızca o kategorideki uygulamaları listelemek için çubuğun altındaki bir kategoriye tıklayın veya **All categories** menüsünden seçim yapın.

Tablo her uygulamayı **Category** (Kategori), kullanan **Devices** (Cihaz sayısı), **Download** ve **Upload** veri miktarı, **Connections** (Bağlantı sayısı) ve **Last seen** (Son görülme) bilgisiyle - en çok veri tüketenden başlayarak - listeler. Döneme (**Today**, **Last 7 days** ... **Last year**), kategoriye veya ağa göre filtreleyebilir ya da uygulama/kategori adı arayabilirsiniz. Başka bir kategoriye taşıdığınız uygulamaların yanında **changed** etiketi görünür.

Bir uygulamanın kategorisini ve onu kullanan cihazları (veri miktarları, ilk ve son kullanım zamanlarıyla birlikte) görmek için uygulama satırına tıklayın.

Bazı bağlantılar yalnızca protokolleriyle adlandırılabilir:

| Gösterim | Anlamı |
| :--- | :--- |
| **Other HTTPS** / **Other QUIC** | Bilinen bir uygulaması olmayan bir siteye yapılan şifreli bağlantı veya motor başladığında zaten açık olan bağlantı. Adlandırmak için siteyi özel uygulama (*custom application*) olarak ekleyin. |
| **Encrypted site (ECH)** | Tarayıcı sitenin adını gizlemiştir (*Encrypted Client Hello*). Yol üzerindeki hiçbir sistem site adını göremez. |
| **chatgpt.com**, **claude.ai** ... | Motorun yalnızca kategorisini bildiği ve doğrudan alan adıyla gösterdiği site. |
| **Unknown** | Motorun tanımadığı bir protokol veya anlaşılması için çok kısa sürmüş bağlantı. |

## Cihazlar (Devices)

**Devices** sekmesi her cihazı kullandığı uygulama sayısı, en çok veri tükettiği üç uygulama ve toplam verisiyle listeler. Cihazın tüm uygulamalarını görmek için satırına tıklayın. Açılan sayfadaki **Sites in Web Activity** bağlantısı (Web Activity açıksa) o cihazın sorguladığı siteleri gösterir.

## Kategoriler (Categories)

![Firewall > Application Control > Categories](/images/manual/appctl-categories.png)

Motor her uygulamayı nDPI kategorilerinden biriyle adlandırır. Stratum bunları kendi kategorilerine - raporlarda, kontrol panelinde, Yapay Zeka Asistanı'nda gördüğünüz ve [Kuralların](#kurallar) kullandığı kategorilere - ayırır. Sağdaki panel her kategoriyi örnekleriyle ve kapsadığı nDPI kategorileriyle listeler.

Stratum ayrıca bazı uygulamaları doğal olarak arayacağınız yere taşır: WhatsApp fotoğraf ve videoları (**WhatsAppFiles**) indirme değil **Messaging and calls** altındadır; HTTP vekil sunucuları, SOCKS ve şifreli DNS (DoH / DoT) güvenlik duvarının filtrelerini atlattığı için **VPN and proxies** altındadır; Zoom ve Webex ise aramalar grubundadır.

### Bir uygulamanın kategorisini değiştirme

**Applications** tablosu son 30 günün tüm uygulamalarını **Category**, **nDPI category** ve veri miktarıyla listeler. İlgili satırdan başka bir kategori seçin; varsayılan kategori *(default)* olarak işaretlidir ve tekrar seçildiğinde değişikliği geri alır. **Changed only** yalnızca sizin değiştirdiklerinizi listeler. **Save** ve ardından **Apply** uygulayın. Değişiklik geçmişe de yansır: önceki günlerin raporları yeni kategoriyi kullanır.

### Özel uygulamalar (Custom applications)

Motorun tanımadığı siteleri - şirket portalı, çevrimiçi bir araç veya yerel bir servis - kendiniz adlandırabilirsiniz. **Add** düğmesine tıklayın ve şu bilgileri girin:

| Alan | Anlamı |
| :--- | :--- |
| **Name** | Raporlarda görünecek ad, ör. `DeepL`. |
| **Category** | Ait olduğu kategori. |
| **Domains** | Boşlukla ayrılmış bir veya birden fazla alan adı. Bir alan adı alt alan adlarını da kapsar: `deepl.com` tanımı `www.deepl.com` ile eşleşir ancak `notdeepl.com` ile eşleşmez. Tam URL yapıştırabilirsiniz; yalnızca sunucu adı tutulur. |

Özel uygulamalar HTTPS ve QUIC bağlantılarının sunucu adından (SNI) tanındığı için şifreli sitelerde tam olarak çalışır, [Engelleme Sayfası](#engelleme-sayfası-block-page) DNS listesine otomatik dahil edilir ve motorun kendi adlandırmasına göre önceliklidir. **Apply** işleminden sonraki bağlantılar için geçerli olur; uygulama işlemi motorları yeniden başlatmaz.

## Kurallar (Rules)

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

Bir kural hangi ağlardaki hangi bağlantıların engelleneceğini belirler. Liste yukarıdan aşağıya kontrol edilir: **eşleşen ilk kural karar verir**. Yeni kural için **Add rule** düğmesine, düzenlemek için kuralın açıklamasına tıklayın:

| Alan | Anlamı |
| :--- | :--- |
| **Rule is on** | Kapatıldığında kural listede kalır ancak hiçbir etkisi olmaz. |
| **Description** | Listelerde, engelleme sayfasında ve raporlarda gösterilen açıklama, ör. *No VPN for guests*. |
| **Interfaces** | Kuralın kapsadığı ağlar. Yalnızca **Settings** sekmesinde izlenen arayüzler sunulur. |
| **Categories** | Stratum kategorileri (bkz. [Kategoriler](#kategoriler)): *VPN and proxies*, *Social media*, *Games* ... **Network services** (DNS, DHCP, NTP) seçilemez: ağın çalışması için bunlara ihtiyacı vardır. |
| **Applications** | Virgülle ayrılmış olarak Applications sekmesindeki uygulama adları - özel uygulamalar dahil. |
| **Encrypted connections without a server name** | Hangi siteyi istediğini belirtmeyen 443 portlu HTTPS / QUIC bağlantıları. Web tarayıcıları her zaman sunucu adını gönderir; VPN ve tünel uygulamaları ise çoğunlukla göndermez. Bazı eski uygulamalar ve cihazlar da göndermeyebilir. |
| **Except these devices** | Kuralın dokunmayacağı IP adresleri veya ağlar, ör. bilgi işlem dizüstü bilgisayarları. |
| **Mode** | **Monitor only** hiçbir şeyi engellemez, yalnızca nelerin engelleneceğini kaydeder. **Block** ise engeller (ve isteğe bağlı olarak tarayıcıda **engelleme sayfası** gösterebilir — bkz. [Engelleme Sayfası](#engelleme-sayfası-block-page)). |

Listede ok simgesi kuralı yukarı taşır, kutucuk açıp kapatır; **Save order and on / off** her ikisini kaydeder. **Today** sütunu her kuralın bugün kaç kez engellediğini, sayfa gösterdiğini veya izleme modunda yakaladığını gösterir. Kurallar **Apply** sonrasında devreye girer; motorlar kesintisiz çalışmaya devam eder.

> **Her kuralı önce Monitor only modunda başlatın.** Özellikle *Encrypted connections without a server name* seçeneği açıkken birkaç gün boyunca **Blocked** sekmesini izleyin; kural yalnızca hedeflediğiniz trafiği yakaladığında **Block** moduna geçin.

### Block modu tam olarak ne yapar?

- Yalnızca motorun **kesin olarak emin olduğu** adlar üzerinde işlem yapılır: bağlantının sunucu adı veya paket içeriği. Port numarasından ya da IP aralığından yapılan bir tahmin hiçbir zaman engelleme tetiklemez.
- Özel (yerel) IP adreslerine yapılan bağlantılara, motor başladığında zaten açık olan bağlantılara ve motorun ancak bağlantı kapanırken adlandırabildiği akışlara dokunulmaz.
- **Sunucu adıyla tanınan uygulamalar** (TikTok, Instagram, özel bir uygulama ...) motor adı tespit ettiği anda - genellikle TLS el sıkışması tamamlanmadan önce - tekil bağlantı bazında kesilir ve her yeni denemede tekrar kesilir. Aynı sunucu IP adresi çoğu zaman pek çok başka siteye de hizmet verdiği için (Google, Akamai, Cloudflare) diğer sitelere dokunulmaz. QUIC (HTTP/3) bağlantılarında ise ilgili cihazdan o sunucuya giden QUIC trafiği 10 dakika boyunca reddedilir; böylece tarayıcı o adresteki tüm siteler için otomatik olarak TCP üzerinden HTTPS'e döner ve her bağlantı sunucu adına göre tek tek denetlenir.
- **Sunucu adı olmayan bağlantılar** (VPN tünelleri, paket içeriğinden yakalanan protokoller) bir sonraki denemeyi ayıracak bir sunucu adı bulunmadığı için o cihazın ilgili sunucu adresine erişimini 10 dakika boyunca engeller.
- Kuralda **engelleme sayfası gösterimi** açılmamışsa (bkz. [Engelleme Sayfası](#engelleme-sayfası-block-page)), cihaz anında **connection reset** (bağlantı sıfırlandı) yanıtı alır; HTTPS sitelerde özel engelleme sayfasının uyarısız görünmesi için güvenlik duvarının [**Block Page CA**](/tr/manual/system-certificates/#block-page-ca) sertifikasının cihazlara yüklenmiş olması gerekir.
- Toplayıcı servis durduğunda veya kural kapatıldığında tüm engeller anında kaldırılır.

### Engellenenler (Blocked)

![Firewall > Application Control > Blocked](/images/manual/appctl-blocked.png)

Cihaz, uygulama ve kural bazında; ilk ve son engellenme zamanını, sayıyı ve işlemin **Page shown** (Engelleme sayfası gösterildi), **Blocked** (Bağlantı kesildi) veya **Would block** (Monitor only modundaki kural) olduğunu gösterir. Moda, ağa ve döneme göre filtreleyebilir veya arama yapabilirsiniz. Tablonun üstündeki durum satırı toplayıcı başladığından bu yana kaç kez engelleme sayfası gösterildiğini, kaç bağlantının kesildiğini ve şu anda sunucu adı olmayan hedeflerden dolayı kaç cihazda geçici engel bulunduğunu özetler.

### Engelleme Sayfası (Block page)

![Firewall > Application Control > Block page](/images/manual/appctl-blockpage.png)

**Block** modundaki bir kural, tarayıcıda bağlantı hatası yerine sitenin engellendiğini açıklayan özelleştirilebilir bir sayfa gösterebilir. Sayfada engellenen **site**, **uygulama**, **kural**, **cihaz** ve **zaman** bilgileri yer alır:

- **Nasıl çalışır (`198.18.0.1` + Unbound DNS):** Güvenlik duvarının [**Unbound DNS çözümleyicisi**](/tr/manual/services-dns-resolver/), kuralın kapsadığı iç ağlar için (kuraldaki istisna cihazlar hariç) engellenen uygulamalara ait alan adlarını (nDPI uygulama listelerindeki ~1.000 alan adı ve sizin **Özel uygulamalarınız**) `198.18.0.1` adresiyle yanıtlar; güvenlik duvarı da bu adrese giden web trafiğini yalıtılmış engelleme sayfası sunucusuna yönlendirir. `198.18.0.1` adresine QUIC reddedildiği için tarayıcılar anında TCP üzerinden HTTPS kullanır; ayrıca kural engelleme sayfası gösterirken Firefox'a DNS over HTTPS (DoH) moduna geçmemesi bildirilir.
- **HTTPS ve Block Page CA:** `https://` siteler için engelleme sayfası sunucusu, istenen alan adına özel sertifikayı o anda üretir ve yeni [**Block Page CA**](/tr/manual/system-certificates/#block-page-ca) (**System > Certificates > Block Page CA**) ile imzalar. Bu CA'yı bir kez oluşturup `.cer` (Windows / Group Policy) veya `.crt` (PEM) olarak indirerek yönetilen cihazlara dağıtın; CA yüklü olmayan cihazlar sayfa yerine sertifika uyarısı görür ancak erişim her iki durumda da engellenir.
- **Farklı DNS kullanan cihazlar:** Güvenlik duvarının Unbound DNS'i yerine başka bir DNS sunucusu kullanan cihazlar da sayfa gösterilmeden doğrudan bağlantı kesme yöntemiyle engellenmeye devam eder.
- **Block page sekmesinde özelleştirme:** Kendi **logonuzu** ve sayfanın üst kısmını kaplayan geniş **üst görseli / banner resmini** (PNG, JPEG veya WebP; örneğin kurumsal `1920x1080` görselinizi) yükleyebilir; kurum adını, başlığı, açıklama metinlerini, iletişim satırını, vurgu rengini ve hangi detay alanlarının hangi etiketlerle gösterileceğini dilediğiniz dilde **canlı önizleme** eşliğinde düzenleyebilirsiniz. Kaydedilen değişiklikler **Apply** gerektirmeden **anında** devreye girer.
- **Yetki ayrıştırılmış güvenlik (`_stratumbp`):** Engelleme sayfası sunucusu kendi yetkisiz kullanıcısı (`_stratumbp`) altında çalışır; CA özel anahtarını yalnızca `_stratumbp` ve `root` okuyabilir ve servisin kendi portlarına hiçbir ağdan doğrudan ulaşılamaz.

## Kontrol Paneli ve Yapay Zeka Asistanı

- [Kontrol panelindeki](/tr/manual/dashboard/) **Applications** kartı son bir saatin en çok veri kullanan uygulamalarını ve kategorilerini gösterir. Cihaz adı göstermediği için tüm kullanıcı rolleri bu kartı ekleyebilir.
- [Yapay Zeka Asistanı](/tr/manual/diagnostics-ai-assistant/) ve MCP istemcileri **Application usage** aracıyla şu soruları yanıtlayabilir: *"Bugün misafir ağı en çok hangi uygulamaları kullandı?"*, *"10.90.0.57 hangi uygulamaları kullanıyor?"*, *"Bu hafta kurallar neleri engelledi?"*. Araç salt okunurdur - kurallar yalnızca panelden değiştirilir. İstenirse [**System > Settings > MCP**](/tr/manual/system-settings-snmp-mcp/) altından kapatılabilir.

## Gizlilik

Uygulama Kontrolü ağlarınızdaki kişilerin hangi uygulamaları kullandığını gösterir. Raporları yalnızca yöneticiler açabilir ve verilere yapılan her erişim sistem günlüğüne kaydedilir. Kullanıcılarınızı yerel mevzuatınızın gerektirdiği şekilde bilgilendirin.

## Sorun Giderme

| Belirti | Kontrol Edilecek Nokta |
| :--- | :--- |
| *Apply to start* uyarısı kalıyor | Değişiklikleri uygulayın (**Apply changes**) ve onaylayın (**Keep changes**). |
| Bir arayüz **Restarting** veya **Stopped** görünüyor | Altındaki hata mesajını ve [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) sayfasını (`stratum-appd` programı) kontrol edin. 10 dakika içinde 5 kez duran arayüz, servis yeniden başlatılana kadar ([**Services > Service Status**](/tr/manual/services-service-status/)) kapalı bırakılır. |
| **Dropped** paketler artıyor | Motor ilgili arayüzün hızına yetişemiyordur: bazı bağlantılar adsız veya eksik veriyle sayılır. Ağ trafiğinin kendisi etkilenmez. |
| Çok fazla **Other HTTPS** veya **Other QUIC** var | Motor başladığında zaten açık olan bağlantılar kapanana kadar adsız kalır; gün içinde kendiliğinden azalır. Önemli özel siteler için özel uygulama (*custom application*) ekleyin. |
| Engelleme sayfası yerine sertifika uyarısı veya bağlantı hatası çıkıyor | Cihazın DNS sunucusu olarak güvenlik duvarındaki Unbound'u kullandığını ([**Force DNS**](/tr/manual/services-dns-resolver/) ile zorlayabilirsiniz) ve [**System > Certificates > Block Page CA**](/tr/manual/system-certificates/#block-page-ca) sertifikasının cihazda güvenilen kök olarak yüklü olduğunu kontrol edin. |
| **Block** modundaki kural bir siteyi engellemiyor | Bağlantı önceden açık mıydı? Tarayıcıyı veya uygulamayı kapatıp yeniden açın: kuraldan (veya toplayıcı yeniden başlamadan) önce açık olan bağlantılara dokunulmaz. Kuralın arayüzlerini, istisnalarını ve uygulamanın Applications sekmesinde *Other HTTPS* yerine kendi adıyla göründüğünü doğrulayın. |
| Kural gereğinden fazla engelleme yapıyor | Kuralı **Monitor only** moduna alıp **Apply** uygulayın: tüm aktif engeller anında kalkar. **Blocked** sekmesinden neleri yakaladığını inceleyin. Genellikle neden *Encrypted connections without a server name* seçeneğidir. |
| Durum satırında **pf: N error(s)** görünüyor | Toplayıcı engelleri `pf` içine yükleyememiştir; ayrıntılı mesaj ipucu balonunda ve sistem günlüğündedir. |
| Saldırı tespit sistemi (**IDS**) **Block** modunda olan arayüzde hiçbir şey görünmüyor | Suricata bu arayüzün paketlerini `netmap` üzerinden alır; iki özelliğin aynı arayüzde birlikte kullanımı halen test aşamasındadır. Şimdilik ilgili arayüzde saldırı tespitini **Watch** modunda kullanın. |
