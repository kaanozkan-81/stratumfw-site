---
title: "Stratum Firewall v0.38.0: Capsicum Korumalı Uygulama Kontrolü, Önce-İzle Kuralları ve Cerrahi TLS/QUIC Engelleme"
date: 2026-11-23
author: "Stratum Security"
translationKey: "v0-38-0-application-control"
description: "Stratum Firewall v0.38.0; FreeBSD Capsicum kum havuzunda çalışan stratum-dpi 1.2 motoru, Monitor only denetim modu, paylaşımlı CDN IP'lerini bozmayan TLS/QUIC engelleme ve 16. salt okunur MCP yapay zeka aracını sunuyor."
lead: "Trafik yolunun arasına kırılgan bir vekil sunucu sokmadan ve paylaşımlı Cloudflare/Google IP adreslerindeki iş servislerini bozmadan ağlarınızdaki yüzlerce uygulamayı görün ve yönetin."
images: ["/images/manual/appctl-rules.png"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "uygulama kontrolü", "application control", "dpi", "capsicum", "quic", "mcp", "v0.38.0"]
---

Modern kurumsal ve misafir ağlarında istenmeyen uygulamaları — misafir Wi-Fi ağındaki yetkisiz VPN tünellerinden P2P indirmelere, sosyal medya veya oyun trafiğine kadar — engellemeye çalışırken geleneksel çözümler genellikle iki sorundan birine takılır: ya **paket yolunun tam üstüne (*inline*) oturup en küçük hatada hattı keser** ya da **aynı IP adresinde onlarca iş servisini barındıran Cloudflare, Akamai ve Google IP bloklarını toptan kapatır**.

[Stratum Firewall](https://stratumfw.com/tr/) **`v0.38.0`** sürümüyle gelen **`Firewall > Application Control`**; trafik yolunun dışında çalışan **FreeBSD Capsicum** korumalı `stratum-dpi 1.2` motorunu, **Monitor only** (Yalnızca izle) ön denetim modunu, paylaşımlı CDN dostu cerrahi `pf` engellemesini ve **16. salt okunur MCP aracını (`Application usage`)** bir araya getiriyor. Tüm kullanım adımlarını Kullanım Kılavuzu'ndaki yeni [Bölüm 06.8 — Güvenlik Duvarı > Uygulama Kontrolü](/tr/manual/firewall-application-control/) sayfasında ve [Sürüm Notları](/tr/resources/changelog/) altında inceleyebilirsiniz.

![Stratum Firewall v0.38.0 Uygulama Kontrolü ve stratum-dpi 1.2 Mimarisi](/images/application-control-architecture.svg)

### 1. Trafik Yolu Dışında (`Out-of-Path`) Capsicum Korumalı `stratum-dpi 1.2`

- **Paket Geçiş Yoluna Girmez:** İzlenen her iç ağ arayüzü, trafiğin yalnızca pasif bir kopyasını okuyan yetkisiz (*unprivileged*) bir `stratum-dpi 1.2` süreci çalıştırır. Motor dursa bile canlı ağ trafiği etkilenmez.
- **Anında Güven Düzeyi Sınıflandırması:** `nDPI` (yalnızca LGPLv3 bileşenleri) ile Stratum FW'nin kendi TLS `ClientHello` ve QUIC `Initial` ayrıştırıcısını birleştirir; sunucu adı (SNI) bilinen bağlantıları paket sınırı beklemeden anında adlandırır ve sunucu adını her zaman port/adres tahminlerinin önüne koyar.
- **Gizlilik Odaklı Kayıt Yapısı:** **FreeBSD Capsicum** yetenek kum havuzunda kilitli çalışır; diske tekil URL veya bağlantı geçmişi değil yalnızca cihaz ve uygulama başına günlük toplamlar yazılır.

### 2. Önce İzle (`Monitor only`), Emin Olduğunda Engelle (`Block`)

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

**`Firewall > Application Control > Rules`** altındaki kurallar; izlenen arayüzleri, Stratum kategorilerini (*VPN and proxies*, *Social media*, *Games*, *AI assistants*...), tekil veya özel alan adlı uygulamaları ve isteğe bağlı olarak VPN/tünel yazılımlarının tipik izi olan **sunucu adı içermeyen şifreli bağlantıları** kapsar:

| Özellik | `v0.38.0` Üretim Ağlarını Nasıl Korur? |
| :--- | :--- |
| **Varsayılan `Monitor only` Modu** | Her yeni kural varsayılan olarak **Monitor only** modunda açılır ve hiçbir şeyi kesmeden **Blocked** sekmesinde *Would block* kaydı tutar |
| **Sıfır Tahmin İlkesi** | Yalnızca kesin sunucu adı veya paket yükü eşleşmelerinde işlem yapılır; port/IP tahminlerine, `DNS`/`DHCP`/`NTP` servislerine ve yerel IP'lere dokunulmaz |
| **Paylaşımlı CDN Dostu Kesme** | Sunucu adıyla tanınan HTTPS akışlarını TLS el sıkışmasında tek tek sıfırlar; aynı Cloudflare veya Google IP'sindeki diğer siteler çalışmaya devam eder |
| **10 Dakikalık QUIC -> TCP Geçişi** | İlgili cihazdan hedef sunucuya QUIC trafiğini 10 dakika reddederek tarayıcıyı otomatik olarak TCP'ye yönlendirir ve her bağlantıyı isme göre denetler |
| **Anında Kaldırma ve `pf` Sürekliliği** | Kural kapatıldığında veya servis durduğunda engeller anında kalkar; `Apply` sonrası `pf` yenilendiğinde aktif engeller otomatik geri yüklenir |

### 3. Kontrol Paneli Kartı ve 16. Salt Okunur MCP Aracı (`Application usage`)

- **Applications Kontrol Paneli Kartı:** Son bir saatin en çok veri tüketen uygulamalarını ve kategorilerini cihaz adı paylaşmadan özetler.
- **16 Salt Okunur Yapay Zeka ve MCP Aracı:** Yerleşik [Yapay Zeka Asistanı](/tr/product/ai-assistant/) ve harici MCP istemcileri artık *Application usage* aracıyla ağ/cihaz bazlı uygulama kullanımını, aktif kuralları ve engellenen bağlantıları doğal dilde sorgulayabilir.

### `v0.38.0` Sürümüne Yükseltin

Mevcut kurulumlarınızı **`System > Update`** (`stratum-update run`) üzerinden OpenZFS Önyükleme Ortamı güvencesiyle tek tıkla `v0.38.0` sürümüne yükseltebilir, yeni [Bölüm 06.8 Kullanım Kılavuzu](/tr/manual/firewall-application-control/) sayfasını okuyabilir, kurulum imajlarını [İndir](/tr/download/) sayfasından edinebilir ve arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde test edebilirsiniz.
