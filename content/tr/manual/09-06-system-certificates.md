---
title: "Sistem > Sertifikalar"
seo_title: "09.6 Sistem > Sertifikalar (System > Certificates) — Stratum FW Kullanım Kılavuzu"
description: "Web panelinin TLS sertifikası: kendinden imzalı sertifika, kurumsal CA veya ticari sağlayıcıdan alınan özel sertifikayı yükleme, tarayıcı uyarısı, süre bitimi ve önceki sertifikaya geri dönme."
weight: 906
chapter_num: "09.6"
url: "/tr/manual/system-certificates/"
aliases:
  - "/tr/manual/09-06-system-certificates/"
translationKey: "manual-09-06-system-certificates"
---

Web yönetim paneli her zaman HTTPS üzerinden sunulur. **System > Certificates** tarayıcının gördüğü TLS sertifikasını yönetir.

![System > Certificates](/images/manual/certs.png)

## Mevcut sertifika (The current certificate)

**Web panel certificate** kartı sertifikanın kime düzenlendiğini, kapsadığı alan adlarını ve IP adreslerini, düzenleyen makamı, geçerlilik tarihlerini, kalan gün sayısını (21 günden az kaldığında veya süresi dolduğunda vurgulanır) ve **SHA-256 parmak izini (fingerprint)** gösterir.

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

Ticari sertifika satın almadan tarayıcı uyarısını tamamen kaldırmak için güvenlik duvarını yöneten bilgisayarlarda sertifikayı güvenilir kök olarak içe aktarın: sertifikayı tarayıcıdan dışa aktarın (sertifika ayrıntıları > dışa aktar) ve Windows'ta *Güvenilen Kök Sertifika Yetkilileri* (*Trusted Root Certification Authorities*) deposuna, macOS'ta ise Anahtarlık (*Keychain*) içine *Her Zaman Güven* (*Always trust*) seçerek ekleyin. MCP istemcileri de aynı güven adımına ihtiyaç duyar ([**System > Settings: SNMP and MCP**](/tr/manual/system-settings-snmp-mcp/) bölümüne bakınız).

## Kendi sertifikanızı kullanma (Use your own certificate)

Kurum içi sertifika otoritenizden (CA) veya `fw.example.com` gibi genel bir alan adı için ticari bir sağlayıcıdan alınan sertifika tarayıcılar tarafından uyarısız kabul edilir.

1. **Certificate (PEM)** içeriğini yapıştırın veya **Load from file** ile dosyadan yükleyin. Dosya sunucu sertifikasının hemen altında ara (*intermediate*) sertifikaları da içerebilir.
2. **Private key (PEM)** içeriğini yapıştırın veya **Load from file** ile yükleyin. Özel anahtar parola korumalı (şifreli) olmamalıdır.
3. **Check and install** düğmesine tıklayın. Panel sertifikayı kurmadan önce özel anahtarın sertifikayla eşleştiğini ve süresinin dolmadığını doğrular.

Yeni sertifika **Apply** gerekmeden anında devreye girer.

> **Not:** Otomatik Let's Encrypt (ACME) sertifika desteği ilerleyen sürümlerde eklenecektir.

## Tarayıcı artık bağlanamıyorsa

Bir önceki çalışan sertifika her zaman saklanır. Konsoldan (**8) Shell**) şu komutu çalıştırın:

```
stratum-cert restore
```

Panel anında bir önceki sertifikayla yeniden sunulmaya başlar.
