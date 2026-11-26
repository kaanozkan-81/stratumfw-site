---
title: "Sistem > Sertifikalar"
seo_title: "09.6 Sistem > Sertifikalar, Block Page CA ve Inspection CA — Stratum FW Kullanım Kılavuzu"
description: "Web panelinin TLS sertifikası, Uygulama Kontrolü için Block Page CA ve İndirme Kontrolü (Download Control) için adanmış Inspection CA (.cer GPO / .crt PEM) yönetimi."
weight: 906
chapter_num: "09.6"
url: "/tr/manual/system-certificates/"
aliases:
  - "/tr/manual/09-06-system-certificates/"
translationKey: "manual-09-06-system-certificates"
---

Web yönetim paneli her zaman HTTPS üzerinden sunulur. **System > Certificates**, hem tarayıcının yönetim paneli için gördüğü TLS sertifikasını hem de [**Firewall > Application Control**](/tr/manual/firewall-application-control/#engelleme-sayfası-block-page) HTTPS engelleme sayfalarında kullanılan **Block Page CA** ile [**Firewall > Download Control**](/tr/manual/firewall-download-control/) vekil sunucusunun kullandığı **Inspection CA** sertifika otoritelerini yönetir.

![System > Certificates](/images/manual/certs.png)

## Mevcut sertifika (The current certificate)

**Web panel certificate** kartı sertifikanın kime düzenlendiğini, kapsadığı alan adlarını ve IP adreslerini, düzenleyen makamı, geçerlilik tarihlerini, kalan gün sayısını (21 günden az kaldığında veya süresi dolduğunda vurgulanır) ve **SHA-256 parmak izini (fingerprint)** gösterir. Aktif yönetim paneli sertifikasını doğrudan sayfa üzerindeki indirme düğmesiyle bilgisayarınıza indirebilirsiniz.

Parmak izi gerçekten kendi güvenlik duvarınızla konuştuğunuzu doğrulamanızı sağlar: buradaki değeri tarayıcınızın gösterdiği parmak iziyle (adres çubuğundaki kilit simgesi > sertifika ayrıntıları) karşılaştırabilirsiniz.

Sertifikanın süresinin dolmasına 21 gün kala bildirim gönderilmesi sağlanabilir ([**System > Settings > Notifications**](/tr/manual/system-settings-logging-notifications/)).

## Kendinden imzalı sertifika (The self-signed certificate)

Kurulum sırasında güvenlik duvarı 10 yıl geçerli kendi sertifikasını üretir. Tarayıcılar kendinden imzalı her sertifika için siz bir kez onaylayana (veya sertifikayı bilgisayarınızda güvenilen kök olarak tanıtana) kadar uyarı gösterir - parmak izi eşleştiği sürece bu normaldir ve bir saldırı belirtisi değildir.

**New self-signed certificate** yeni bir kendinden imzalı sertifika üretir. **Host names and addresses the panel is opened with** alanına tarayıcı adres çubuğuna yazdığınız tüm adları ve IP adreslerini her satıra bir tane gelecek şekilde yazın; örneğin:

```
192.168.1.1
fw-belek.lan
fw-belek.example.com
```

Adres çubuğuna yazılan adres sertifikanın içinde yer almıyorsa tarayıcı yeniden uyarı verir.

**Create and install** düğmesine tıkladığınızda yeni sertifika anında devreye girer.

### Sertifikayı bilgisayarlarınızda güvenilir tanıtma

Ticari sertifika satın almadan tarayıcı uyarısını tamamen kaldırmak için yönetim paneli sertifikasını doğrudan **System > Certificates** sayfasından indirip güvenlik duvarını yöneten bilgisayarlarda güvenilir kök olarak içe aktarın: Windows'ta *Güvenilen Kök Sertifika Yetkilileri* (*Trusted Root Certification Authorities*) deposuna, macOS'ta ise Anahtarlık (*Keychain*) içine *Her Zaman Güven* (*Always trust*) seçerek ekleyin. MCP istemcileri de aynı güven adımına ihtiyaç duyar ([**System > Settings: SNMP and MCP**](/tr/manual/system-settings-snmp-mcp/) bölümüne bakınız).

## Kendi sertifikanızı kullanma (Use your own certificate)

Kurum içi sertifika otoritenizden (CA) veya `fw.example.com` gibi genel bir alan adı için ticari bir sağlayıcıdan alınan sertifika tarayıcılar tarafından uyarısız kabul edilir.

1. **Certificate (PEM)** içeriğini yapıştırın veya **Load from file** ile dosyadan yükleyin. Dosya sunucu sertifikasının hemen altında ara (*intermediate*) sertifikaları da içerebilir.
2. **Private key (PEM)** içeriğini yapıştırın veya **Load from file** ile yükleyin. Özel anahtar parola korumalı (şifreli) olmamalıdır.
3. **Check and install** düğmesine tıklayın. Panel sertifikayı kurmadan önce özel anahtarın sertifikayla eşleştiğini ve süresinin dolmadığını doğrular.

Yeni sertifika **Apply** gerekmeden anında devreye girer.

> **Not:** Otomatik Let's Encrypt (ACME) sertifika desteği ilerleyen sürümlerde eklenecektir.

## Block Page CA

[**Firewall > Application Control**](/tr/manual/firewall-application-control/#engelleme-sayfası-block-page) altındaki bir kural `https://` siteler için **engelleme sayfası (block page)** gösterecek şekilde ayarlandığında, yalıtılmış engelleme sayfası sunucusu (`_stratumbp`) istenen alan adına ait sertifikayı o anda üretir ve **Block Page CA** ile imzalar:

1. **System > Certificates** sayfasında **Block Page CA** sertifikasını bir kez oluşturun. Özel anahtar güvenlik duvarından asla dışarı çıkmaz (yalnızca `root` ve `_stratumbp` okuyabilir).
2. Sertifikayı **`.cer`** (Windows ve Active Directory **Group Policy** için — Grup İlkesi dağıtım adımları sayfada gösterilir) veya **`.crt`** (PEM biçimi; macOS, Linux ve tarayıcılar için) olarak indirip yönetilen cihazlara yükleyin.
3. Sertifikanın yüklü olmadığı cihazlarda engellenen siteye yine ulaşılamaz; yalnızca özel sayfa yerine tarayıcı sertifika uyarısı görünür.

**Block Page CA** buradan yenilenebilir veya silinebilir; aktif bir Uygulama Kontrolü kuralı tarafından kullanımda olan bir CA silinemez.

## Inspection CA (İndirme Kontrolü Sertifika Otoritesi)

[**Firewall > Download Control**](/tr/manual/firewall-download-control/) (port `3128` üzerindeki Squid vekil sunucu), HTTPS üzerinden indirilen dosyaları denetleyebilmek için **Block Page CA**'dan tamamen ayrı olan kendi **Inspection CA** sertifika otoritesini kullanır:

- **Block Page CA'dan bağımsızdır:** Birini oluşturmak veya yenilemek diğerini etkilemez; yalnızca vekil sunucuyu kullanan cihazlara yüklenir.
- **İstemci cihazlara dağıtım:** Sertifikayı Windows ve Active Directory **Group Policy** için **`.cer`** veya diğer sistemler için **`.crt`** (PEM) biçiminde indirip vekil sunucuyu kullanan cihazlara yükleyin.
- **SHA-256 parmak izi doğrulaması:** İstemci cihazlarda gördüğünüz sertifikayla birebir karşılaştırabilmeniz için **SHA-256 parmak izi** sayfada açıkça gösterilir.
- **Özel anahtar cihazdan çıkmaz:** Inspection CA özel anahtarı güvenlik duvarından asla dışarı çıkmaz.

## Tarayıcı artık bağlanamıyorsa

Bir önceki çalışan web paneli sertifikası her zaman saklanır. Konsoldan (**8) Shell**) şu komutu çalıştırın:

```
stratum-cert restore
```

Panel anında bir önceki sertifikayla yeniden sunulmaya başlar.
