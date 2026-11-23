---
title: "Stratum Firewall v0.39.0: Capsicum Korumalı Uygulama Kontrolü, Özelleştirilebilir HTTPS Engelleme Sayfası ve Block Page CA"
date: 2026-11-23
author: "Stratum Security"
translationKey: "v0-39-0-application-control-block-page"
aliases:
  - /tr/blog/stratum-firewall-v0-38-0-uygulama-kontrolu-stratum-dpi-engelleme-kurallari/
description: "Stratum Firewall v0.39.0; FreeBSD Capsicum korumalı stratum-dpi 1.2 Uygulama Kontrolü, kendi logonuz ve üst görselinizle özelleştirilebilen HTTPS Engelleme Sayfası, Group Policy uyumlu Block Page CA (.cer / .crt) ve 16. salt okunur MCP aracını sunuyor."
lead: "Normal HTTPS trafiğinin şifresini çözmeden ağlarınızdaki istenmeyen uygulamaları engelleyin ve kullanıcılara kurum kimliğinizi taşıyan net bir HTTPS engelleme sayfası gösterin."
images: ["/images/manual/appctl-blockpage.png"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "uygulama kontrolü", "engelleme sayfası", "block page", "capsicum", "group policy", "mcp", "v0.39.0"]
---

Kurumsal bir ağda kısıtlanmış bir uygulamaya girildiğinde tarayıcıda yalnızca ham bir bağlantı hatası çıkması, kullanıcıların *"internet mi kesildi?"* diyerek destek masasını aramasına yol açar; geleneksel güvenlik duvarları ise HTTPS engelleme sayfası gösterebilmek için **tüm normal HTTPS trafiğinin şifresini hat üzerinde (*inline MITM*) çözmeyi** şart koşar.

[Stratum Firewall](https://stratumfw.com/tr/) **`v0.39.0`** sürümüyle gelen **`Firewall > Application Control`**; trafik yolunun dışında çalışan **FreeBSD Capsicum** korumalı `stratum-dpi 1.2` motorunu, **Monitor only** ön denetim modunu ve **Block Page CA** destekli **özelleştirilebilir HTTPS Engelleme Sayfası** mimarisini birleştiriyor — normal trafiğin şifresine dokunmadan yalnızca engellenen alan adları için sertifika üretiyor. Adım adım yapılandırmayı Kullanım Kılavuzu [Bölüm 06.8 — Uygulama Kontrolü](/tr/manual/firewall-application-control/), [Bölüm 09.6 — Sertifikalar](/tr/manual/system-certificates/#block-page-ca) ve [Sürüm Notları](/tr/resources/changelog/) sayfalarında bulabilirsiniz.

![Stratum Firewall v0.39.0 Uygulama Kontrolü, Özel Engelleme Sayfası ve stratum-dpi 1.2 Mimarisi](/images/application-control-architecture.svg)

### 1. Özelleştirilebilir HTTPS Engelleme Sayfası ve Group Policy Uyumlu `Block Page CA` (`v0.39.0`)

![Firewall > Application Control > Block page](/images/manual/appctl-blockpage.png)

**Block** modundaki kurallar artık bağlantı sıfırlama hatası yerine **site**, **uygulama**, **kural**, **cihaz** ve **zaman** bilgilerini gösteren kurumsal bir engelleme sayfası sunabilir:

- **Unbound `198.18.0.1` Yönlendirmesi:** Güvenlik duvarındaki Unbound DNS, kuralın kapsadığı ağlarda (istisna cihazlar hariç) ~1.000 nDPI uygulama alan adını ve özel tanımlı uygulamalarınızı `198.18.0.1` ile yanıtlar; QUIC'i reddedip tarayıcıyı TCP üzerinden HTTPS'e geçirir, Firefox'a DoH kullanmamasını bildirir ve isteği yalıtılmış `_stratumbp` sunucusuna yönlendirir.
- **`System > Certificates > Block Page CA`:** Sertifika otoritesini tek tıkla oluşturup Active Directory **Group Policy** için **`.cer`** (GPO adımları sayfada yer alır) veya **`.crt`** (PEM) olarak indirebilirsiniz. Ayrıca artık aktif **web yönetim paneli sertifikası** da aynı sayfadan doğrudan indirilebiliyor.
- **Logo, Üst Banner Görseli ve Canlı Önizleme:** **`Application Control > Block page`** sekmesinden kendi **logonuzu** ve sayfa üstünü kaplayan geniş **üst görselinizi** (`PNG` / `JPEG` / `WebP`) yükleyebilir; kurum adını, başlığı, metinleri, iletişim satırını ve vurgu rengini canlı önizleme ile özelleştirebilirsiniz. Kaydedilen değişiklikler **Apply** gerektirmeden **anında** devreye girer.
- **`_stratumbp` Yetki Ayrımı ve Harici DNS Yedekliliği:** Engelleme sayfası sunucusu yetkisiz `_stratumbp` kullanıcısıyla çalışır ve dışarıdan doğrudan erişilemez; farklı DNS kullanan cihazlar ise `stratum-dpi 1.2` + `pf` bağlantı kesme katmanıyla engellenmeye devam eder.

### 2. Trafik Yolu Dışı `stratum-dpi 1.2`, Önce-İzle Kuralları ve 16. MCP Aracı

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

| Yetenek | `v0.38.0 – v0.39.0` Üretim Ağlarını Nasıl Korur? |
| :--- | :--- |
| **Capsicum Korumalı `stratum-dpi 1.2`** | VLAN başına pasif paket kopyasını okur (`nDPI` LGPLv3 + Stratum TLS/QUIC ayrıştırıcı); servis dursa bile trafik kesilmez |
| **Varsayılan `Monitor only` Modu** | Yeni kurallar önce **Monitor only** olarak başlar; **Blocked** sekmesinde *Page shown*, *Blocked* ve *Would block* sayıları ayrı ayrı izlenir |
| **Paylaşımlı CDN Dostu Kesme ve VPN Engeli** | Port/IP tahminlerine ve `DNS`/`DHCP`/`NTP` servislerine dokunmaz; paylaşımlı Cloudflare/Google IP'lerindeki diğer siteleri bozmadan bağlantıyı keser, sunucu adsız VPN tünellerini 10 dk engeller |
| **16. Salt Okunur MCP Aracı** | [Yapay Zeka Asistanı](/tr/product/ai-assistant/) ve MCP istemcileri *Application usage* ile kuralları ve engellenenleri %100 salt okunur sorgular |

### `v0.39.0` Sürümüne Yükseltin

Mevcut kurulumlarınızı **`System > Update`** (`stratum-update run`) üzerinden OpenZFS Önyükleme Ortamı güvencesiyle tek tıkla `v0.39.0` sürümüne yükseltebilir, [Bölüm 06.8](/tr/manual/firewall-application-control/) ve [Bölüm 09.6](/tr/manual/system-certificates/#block-page-ca) kılavuz sayfalarını inceleyebilir, kurulum medyasını [İndir](/tr/download/) sayfasından çekebilir ve arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde deneyebilirsiniz.
