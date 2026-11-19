---
title: "Servisler > Captive Portal"
seo_title: "07.6 Servisler > Captive Portal (Services > Captive Portal) — Stratum FW Kullanım Kılavuzu"
description: "Misafir ağları için karşılama ve oturum açma sayfası: bölgeler (zones), parolasız kullanım koşullarıyla giriş, yerel kullanıcılar veya RADIUS sunucusu, muhasebe (accounting), süre ve boşta kalma sınırları, cihaz başına hız sınırı, portal sayfası, istisnalar ve aktif oturumlar."
weight: 706
chapter_num: "07.6"
url: "/tr/manual/services-captive-portal/"
aliases:
  - "/tr/manual/07-06-services-captive-portal/"
translationKey: "manual-07-06-services-captive-portal"
---

Captive Portal bir ağın önüne oturum açma sayfası yerleştirir: misafir Wi-Fi ağına katılan bir cihaz oturum açana kadar - kullanım koşullarını kabul ederek, bir kullanıcı adı ve parolayla ya da RADIUS sunucunuzdaki bir hesapla - karşılama sayfası dışında hiçbir yere erişemez. Oturum açıldıktan sonra cihaza ağın normal güvenlik duvarı kuralları uygulanır.

Tipik kullanım alanları: otel ve kafe misafir Wi-Fi ağları, personel lojmanları, toplantı ve konferans salonları, ağ erişim kaydı tutması gereken halka açık ağlar.

**Services > Captive Portal** sayfasında üç sekme bulunur: **Zones**, **Signed-in devices** ve **Users**.

![Services > Captive Portal](/images/manual/cp-zones.png)

## Nasıl çalışır?

- Bir **bölge (zone)** bir veya daha fazla iç arayüzü kapsar. Bu ağlardaki her cihaz internete çıkmadan önce oturum açmalıdır.
- Oturum açmadan önce bir cihaz yalnızca **güvenlik duvarındaki DNS servisine**, **oturum açma sayfasına** ve bölgenin **izin verilen hedeflerine** erişebilir - başka hiçbir yere çıkamaz. Şifresiz web istekleri (`http://`) otomatik olarak oturum açma sayfasına yönlendirilir; `https://` bağlantıları ise reddedilir.
- Telefonlar ve dizüstü bilgisayarlar ağa bağlandıklarında çevrimiçi olup olmadıklarını `http://` tabanlı bir yoklama adresiyle denetler. Bu istek karşılama sayfasına yönlendirildiği için cihazlar sayfayı kendiliğinden açar (*"Ağda oturum açın"* / *"Sign in to network"*). Dizüstü bilgisayarda herhangi bir `http://` adresi açıldığında da sayfa görüntülenir.
- Oturum açıldıktan sonra cihazın adresine geçiş izni verilir ve arayüzün kendi güvenlik duvarı kuralları devreye girer - örneğin *misafirler internete çıkabilir ancak iç ağlara erişemez* ([**Firewall > Rules**](/tr/manual/firewall-rules/)).
- Oturumlar bölgenin toplam süre sınırı dolduğunda, boşta kalma süresi aşıldığında, aynı IP adresini başka bir cihaz aldığında (MAC adresi denetlenir), kullanıcı çıkış yaptığında veya bir operatör bağlantıyı kestiğinde sona erer.

Oturum açma sayfası, bölgedeki güvenlik duvarı IP adresinin `8002` numaralı portunda yetkisizleştirilmiş (unprivileged) ayrı bir servis olarak çalışır. Bu servis gizli hiçbir veriyi bilmez: kimlik doğrulaması izole bir arka plan servisi tarafından yapılır; parolalar ve RADIUS paylaşımlı gizli anahtarları hiçbir zaman web sayfası katmanına ulaşmaz.

> **Başlamadan önce:** bölge ağının DHCP sunucusu istemcilere **DNS sunucusu olarak güvenlik duvarını** vermelidir ([**Services > DHCP Server**](/tr/manual/services-dhcp-server/) altında **DNS servers for clients** alanını boş bırakın). Oturum açmadan önce alan adı çözemeyen cihazlar karşılama sayfasını hiçbir zaman göremez; panel bu koşulu sağlamayan bir bölgeyi kabul etmez.

## Bölge ekleme (Add zone)

**Zones > Add zone** (yalnızca yöneticiler). Form üzerindeki bölümler sırasıyla aşağıda açıklanmıştır.

### Ad ve ağlar (Name and networks)

| Alan | Anlamı |
| :--- | :--- |
| **Name** | Örneğin *Guests*. 1-40 karakter; harf, rakam, boşluk, `.`, `_` veya `-`. |
| **Description** | İsteğe bağlı açıklama. |
| **Zone enabled** | Devre dışı bırakılan bir bölge ayarlarını korur ancak hiçbir denetim yapmaz: ağları sanki portal yokmuş gibi yalnızca kendi güvenlik duvarı kurallarıyla çalışır. |
| **Interfaces** | Bölgeye dahil edilecek statik adresli iç ağlar. Bir ağ aynı anda yalnızca tek bir etkin bölgede yer alabilir. |

### Oturum açma (Sign-in)

| Alan | Anlamı |
| :--- | :--- |
| **Method** | **No password - accept the terms and connect** (parolasız, koşulları kabul et ve bağlan); **Local users** (**Users** sekmesindeki hesaplar); veya **RADIUS server**. |
| **Devices per user** | (Local users, RADIUS.) Tek bir hesabın aynı anda kaç cihazda açık oturum tutabileceği. `0` = sınır yok. |
| **Time limit (minutes)** | Cihazın oturum açtıktan kaç dakika sonra otomatik çıkarılacağı. `0` = süresiz; `1440` = bir gün. |
| **Idle time (minutes)** | Cihazın trafik üretmeden kaç dakika boşta kaldıktan sonra çıkarılacağı. `0` = hiçbir zaman. |
| **Download / Upload per device (Mbit/s)** | Oturum açan her cihaz için bağımsız indirme / yükleme hız sınırı. `0` = sınır yok. |
| **Page after sign-in** | Oturum açıldıktan sonra gösterilecek isteğe bağlı `http://` veya `https://` adresi. Boş bırakılırsa cihazın ilk başta istediği sayfa açılır. |

Bir bölgenin oturum açma yöntemi değiştirildiğinde eski yöntemle bağlanmış cihazların oturumu kapatılır ve yeni yöntemle tekrar giriş yapmaları istenir.

### RADIUS

**RADIUS server** seçildiğinde portal, kullanıcı adı ve parolanın geçerli olup olmadığını RADIUS sunucunuza (FreeRADIUS, Microsoft NPS ve diğerleri) sorar - tıpkı bir MikroTik veya kurumsal hotspot cihazının çalıştığı biçimde.

| Alan | Anlamı |
| :--- | :--- |
| **RADIUS server** | RADIUS sunucusunun IP adresi veya alan adı. |
| **Second server** | İsteğe bağlı yedek sunucu; birincil sunucu yanıt vermediğinde kullanılır. |
| **Authentication port**, **Accounting port** | Varsayılan `1812` ve `1813`. |
| **Shared secret** | RADIUS sunucusundaki istemci (NAS) tanımında yer alan paylaşımlı gizli anahtar; boşluk içermeyen 8-128 basılabilir karakter. Yalnızca `root` erişimli bir dosyada saklanır ve bir daha gösterilmez - kayıtlı bir anahtar olduğunda alan bunu belirtir; korumak için boş bırakın. |
| **NAS identifier** | Güvenlik duvarının RADIUS sunucusuna kendisini hangi adla tanıttığı. Boş bırakılırsa sistemin `hostname` değeri kullanılır. |
| **Send accounting** | Her oturum için başlangıç (*Start*), periyodik ara güncelleme (*Interim-Update*) ve bitiş (*Stop*) muhasebe kayıtları gönderir. |
| **Accounting updates (minutes)** | Devam eden bir oturumun kaç dakikada bir raporlanacağı. `0` = yalnızca başlangıç ve bitiş. |

**RADIUS sunucusu tarafında**, güvenlik duvarını *sunucunun gördüğü IP adresi* ve paylaşımlı gizli anahtarla istemci (client) olarak ekleyin. FreeRADIUS için `clients.conf` örneği:

```
client stratum-fw {
    ipaddr = 192.168.1.1
    secret = <paylasimli gizli anahtar>
}
```

**RADIUS sunucusunun geri gönderebileceği** ve portalın otomatik uyguladığı öznitelikler:

| Öznitelik (Attribute) | Etkisi |
| :--- | :--- |
| `Session-Timeout` | Cihazın toplam oturum süresi sınırı (bölge ayarının önüne geçer). |
| `Idle-Timeout` | Cihazın boşta kalma süresi sınırı (bölge ayarının önüne geçer). |
| `Acct-Interim-Interval` | Muhasebe ara güncellemelerinin gönderilme sıklığı (bölge ayarının önüne geçer). |
| `WISPr-Bandwidth-Max-Down` / `-Up` | Cihaz için hız sınırı (bit/sn). |
| `Mikrotik-Rate-Limit` | MikroTik yazım biçiminde hız sınırı (`upload/download`, örneğin `2M/10M`) - mevcut bir MikroTik RADIUS altyapısı hiçbir değişiklik gerektirmeden olduğu gibi çalışır. |
| `Class` | Muhasebe kayıtlarında sunucuya geri gönderilir. |
| `Reply-Message` | Sunucu bir girişi reddettiğinde sistem günlüğüne yazılır. |

**Muhasebe (accounting) kayıtları** kullanıcı adını, cihazın IP ve MAC adresini (`Calling-Station-Id`), bölge adını (`Called-Station-Id`), oturum süresini, veri trafiğini ve oturumun kapanış nedenini (`User-Request`, `Idle-Timeout`, `Session-Timeout`, `Admin-Reset`, `Lost-Carrier` ...) taşır. Veri trafiği dakikada bir kez cihazın aktif bağlantılarından sayılır ve gerçek tüketimin yakın bir alt sınırını verir.

Her RADIUS isteği `Message-Authenticator` içerir ve gelen her yanıt kabul edilmeden önce paylaşımlı gizli anahtarla doğrulanır - yanlış anahtarla imzalanmış bir yanıt hiç yanıt gelmemiş gibi reddedilir.

### Karşılama sayfası (The page)

| Alan | Anlamı |
| :--- | :--- |
| **Page language** | **English** veya **Türkçe** - sayfanın kendi yerleşik etiketlerinin dili (*User name*, *Connect* ...). Sizin aşağıya yazdığınız metinler ise yazdığınız gibi gösterilir. |
| **Colour** | Sayfanın vurgu rengi. |
| **Title** | Örneğin *Welcome to Hotel Example*. |
| **Button text** | Boş bırakılırsa *Connect* (Türkçe dilinde *Bağlan*). |
| **Message** | Başlığın altındaki kısa karşılama metni, örneğin *Free Wi-Fi for our guests.* |
| **Terms of use** | Doldurulduğunda kullanıcının bağlanabilmek için *I accept the terms of use* kutusunu işaretlemesi zorunlu olur. |
| **Logo** | PNG, JPEG veya GIF, en fazla 300 KB. Bölge kaydedildikten sonra yüklenebilir; **Apply** gerekmeden sayfada anında güncellenir. |

Karşılama sayfası telefon ekranlarına ve koyu temaya (dark mode) kendiliğinden uyum sağlar.

![Karşılama ve oturum açma sayfası](/images/manual/cp-page.png)

### İstisnalar (Exceptions)

| Alan | Anlamı |
| :--- | :--- |
| **Devices that never sign in** | Her satıra bir IPv4 adresi - yazıcılar, TV kutuları, POS terminalleri. Adreslerinin değişmemesi için bu cihazlara statik DHCP eşlemesi tanımlayın. |
| **Destinations allowed before sign-in** | Her satıra bir IPv4 adresi veya alt ağ (en geniş `/8`) - örneğin karşılama sayfasının bağlantı verdiği otel web sitesi veya rezervasyon sunucusu. |

Bölgeyi kaydedin, ardından **Apply changes** ve **Keep changes** ile onaylayın.

## Yerel kullanıcılar (Users)

**Users** sekmesi **Local users** yöntemiyle giriş yapan bölgelerin hesaplarını barındırır. Operatör rolündeki kullanıcılar da (örneğin otel resepsiyonu) bu hesapları yönetebilir.

**Add user** alanları:

| Alan | Anlamı |
| :--- | :--- |
| **User name** | Harf, rakam, `.`, `_`, `@` ve `-`, en fazla 64 karakter - örneğin `room214`. |
| **Description** | Örneğin *Oda 214, Cuma gününe kadar*. |
| **Password** | 6-128 karakter. **Generate** rastgele parola üretir - parolayı hemen kullanıcıya verin; kaydedildikten sonra bir daha gösterilmez. |
| **Expires** | İsteğe bağlı bitiş tarihi: hesap seçilen günün sonuna kadar geçerli kalır. |
| **User enabled** | Bir hesabı silmeden geçici olarak devre dışı bırakır. |

Kullanıcı değişiklikleri **Apply** gerekmeden anında geçerli olur. Bir kullanıcıyı devre dışı bırakmak, yeniden adlandırmak, silmek veya parolasını değiştirmek o hesabın açık oturumlarını anında sonlandırır. Parolalar yalnızca `root` erişimli bir dosyada `bcrypt` özeti olarak saklanır ve hiçbir zaman tarayıcıya gönderilmez.

## Oturum açmış cihazlar (Signed-in devices)

**Signed-in devices** sekmesi oturum açmış tüm cihazları canlı olarak listeler: **Address**, **MAC**, **Zone**, **User** (RADIUS girişleri için *RADIUS* rozetiyle birlikte), **Signed in** zamanı, **Last activity**, **Traffic** ve cihazın hız sınırı. **Disconnect** düğmesi cihazın oturumunu anında kapatır (operatörler ve yöneticiler).

Bir cihazın oturumunu kapatmak internet bağlantılarını sonlandırır ancak güvenlik duvarının kendisine olan bağlantılarını kesmez - dizüstü bilgisayarı misafir bölgesinde bulunan bir yönetici yönetim panelini kaybetmez.

[Kontrol panelindeki](/tr/manual/dashboard/) **Captive portal** kartı da bölge bazında bağlı cihaz sayısını, son girişleri ve servisin durumunu özetler.

## Günlükler ve kayıtlar

Her başarılı giriş, başarısız deneme ve oturum kapanışı sistem günlüğüne (**Diagnostics > System Logs**, program `stratum-captive`) bölge, kullanıcı, IP ve MAC adresiyle, çıkışlarda ise kapanış nedeni ve oturum süresiyle birlikte yazılır:

```
captive: sign-in zone=Guests user=room214 ip=10.20.0.57 mac=7c:4d:8f:06:9b:c8
captive: sign-out (idle) zone=Guests user=room214 ip=10.20.0.57 mac=7c:4d:8f:06:9b:c8 minutes=95
```

RADIUS sunucusu kullanıldığında muhasebe (accounting) kayıtları aynı bilgileri RADIUS veritabanına da işler.

**Parola deneme koruması:** aynı IP adresinden 5 dakika içinde yapılan 5 hatalı parola denemesi o adresi 5 dakika boyunca kilitler.

## Sorun giderme

| Belirti | Kontrol edilecek nokta |
| :--- | :--- |
| Oturum açma sayfası açılmıyor | Cihazın DNS sunucusu olarak güvenlik duvarını aldığından emin olun; `http://neverssl.com` gibi şifresiz bir `http://` adresi açın (`https://` siteler yönlendirme sayfasını gösteremez). |
| *The sign-in service is not available right now* | [**Services > Service Status**](/tr/manual/services-service-status/): *Captive portal* servisi çalışıyor olmalıdır. RADIUS kullanılıyorsa sunucunun erişilebilir olduğunu ve yanıt verdiğini doğrulayın (sistem günlüğü *RADIUS did not answer* yazar). |
| RADIUS girişleri reddediliyor | Günlükte *sign-in rejected by RADIUS* yazıyorsa kullanıcı adı/parola veya RADIUS politikası reddetmiştir. Günlükte *RADIUS did not answer ... the answer's authenticator is wrong* yazıyorsa iki taraftaki paylaşımlı gizli anahtar (`Shared secret`) birbiriyle uyuşmuyordur. |
| Bir cihaz sürekli oturumdan düşüyor | Boşta kalma süresi (*Idle time*) kısa tutulmuş ve telefon uykuya geçmiş olabilir ya da cihaz sürekli değişen rastgele MAC adresi kullanıyordur. |
| Oturum açıldı ama internet yok | Arayüzün kendi güvenlik duvarı kuralları ([**Firewall > Rules**](/tr/manual/firewall-rules/)) internet trafiğine izin vermelidir; portal yalnızca kimin oturum açtığına karar verir. |
