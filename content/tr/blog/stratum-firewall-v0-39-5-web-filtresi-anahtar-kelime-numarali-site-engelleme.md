---
title: "Stratum Firewall v0.39.5: Web Filtresinde Anahtar Kelime Tespiti — Numaralı ve İsim Değiştiren Siteleri İlk DNS Sorgusunda Yakalayın"
date: 2026-11-24
author: "Stratum Security"
translationKey: "v0-39-5-web-filter-keywords"
description: "Stratum Firewall v0.39.5 ile gelen Firewall > Web Filter > Keywords; sürekli numara değiştirerek geri dönen bahis, kumar ve kopya siteleri (site1234.com -> site1235.com) ilk Unbound DNS sorgusunda yakalar, tek tıkla inceleme veya saniyeler içinde otomatik engelleme sunar."
lead: "Sürekli adres değiştiren bahis ve casino sitelerini ertesi günün kara liste güncellemesini beklemeden, ağda ilk sorulduğu saniyede yakalayın ve engelleyin."
images: ["/images/web-filter-keywords-hero.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "web filtresi", "web filter", "unbound", "dns", "keywords", "v0.39.5"]
---

![Stratum Firewall v0.39.5 Web Filtresi Anahtar Kelime Tespiti](/images/web-filter-keywords-hero.jpg)

Günlük güncellenen kategori kara listeleri milyonlarca bilinen adresi başarıyla engeller; ancak özellikle Türkiye'deki kurum, okul, yurt ve misafir ağlarının en sık karşılaştığı sorun **her birkaç saatte bir numara artırarak yeni alan adına taşınan sitelerdir** (`site1234.com`, `site1235.com`, `site1236.com` ...).

[Stratum Firewall](https://stratumfw.com/tr/) **`v0.39.5`** sürümüyle gelen **`Firewall > Web Filter > Keywords`**, bu 24 saatlik gecikme penceresini tamamen kapatıyor: ağdaki herhangi bir cihaz Unbound DNS çözümleyicisine izlenen bir marka adını veya kelimeyi (*bahis*, *casino* vb.) içeren yeni bir alan adı sorduğu anda güvenlik duvarı siteyi gerçek zamanlı tespit ediyor. Ayrıntılı kullanım rehberi [Bölüm 06.6 — Güvenlik Duvarı > Web Filtresi](/tr/manual/firewall-web-filter/) ve [Sürüm Notları](/tr/resources/changelog/) sayfalarında yayında.

![Stratum Firewall v0.39.5 Web Filter Keywords Mimarisi](/images/web-filter-keywords-architecture.svg)

### 1. İster Önce İnceleyin (`List them only`), İster Saniyeler İçinde Otomatik Engelleyin (`Block them at once`)

![Firewall > Web Filter > Keywords Panel Ekranı](/images/manual/wf-keywords.png)

Yöneticiler anahtar kelime yakalama davranışını iki farklı modda çalıştırabilir:

- **`List them only` (Yalnızca Listele — Varsayılan Güvenli Denetim):** Anahtar kelimeyle eşleşen yeni alan adları; eşleşen kelime, saat ve ilk sorgulayan cihaz IP'siyle birlikte tabloya düşer. Sağdaki düğmelerle tek tıklamayla **Block** (**Your own list** içine alır), **Ignore** (yok sayar) veya **Exception** (istisna listesine ekler) kararı verebilirsiniz.
- **`Block them at once` (Saniyeler İçinde Otomatik Engelleme):** Yakalanan yeni siteler doğrudan **Your own list** (Kendi listeniz) kategorisine yazılır ve bu kategoriyi kullanan tüm Web Filtresi politikalarında saniyeler içinde engellenir (politikada engelleme sayfası açıksa kullanıcıya özel engelleme ekranı gösterilir).

### 2. Yalnızca Ana Alan Adı Eşleşmesi ve Sıfır Gezinme Kaydı

| Tasarım Güvencesi | `v0.39.5` Yanlış Engellemeleri Nasıl Önler ve Gizliliği Nasıl Korur? |
| :--- | :--- |
| **Yalnızca Sitenin Kendi Adı** | Sadece kayıtlı ana alan adına bakar; alt alan adları (`casino.haber-portali.tr`) ve uzantılar (`.com`, `.bet`) eşleşme tetiklemez |
| **İstisnalar Her Zaman Öncelikli** | **Web Filter > Exceptions** altındaki kurumsal veya izinli siteler anahtar kelime içerse bile hiçbir zaman engellenmez |
| **Web Activity Olmadan Çalışır** | [Web Activity](/tr/manual/diagnostics-web-activity/) kapalıyken toplayıcı DNS sorgularını yalnızca kelimeler için izler ve diske **hiçbir gezinme kaydı yazmaz** |
| **Politika Bazlı Esneklik** | Otomatik engellenen adresler **Your own list** kategorisine girdiği için yalnızca ilgili kategoriyi seçen misafir/personel politikalarında uygulanır |

### `v0.39.5` Sürümüne Yükseltin

Mevcut kurulumlarınızı **`System > Update`** (`stratum-update run`) üzerinden OpenZFS Önyükleme Ortamı güvencesiyle tek tıkla `v0.39.5` sürümüne yükseltebilir, güncellenen [Bölüm 06.6 Web Filtresi Kılavuzunu](/tr/manual/firewall-web-filter/) inceleyebilir, kurulum medyasını [İndir](/tr/download/) sayfasından çekebilir ve arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde deneyebilirsiniz.
