---
title: "Stratum Firewall v0.24.2: Yeni Nesil Web Filtreleme ve IPS Kararlılığı"
date: 2026-11-06
description: "Stratum Firewall v0.24.2'nin yeni özelliklerini keşfedin: 55 kategorili DNS tabanlı Web Filtreleme (Unbound RPZ), Güvenli Arama zorunluluğu ve kritik IPS engelleme modu düzeltmeleri."
tags: ["Web Filtre", "IPS", "Suricata", "Güvenlik Duvarı", "Stratum FW"]
categories: ["Güvenlik", "Güncellemeler"]
author: "Stratum Security"
---

**Stratum Firewall v0.24.2** sürümünü büyük bir heyecanla duyuruyoruz! Bu güncelleme, tam donanımlı yeni bir **Web Filtresi (Web Filter)** ve Saldırı Önleme Sistemimizdeki (IPS) kritik düzeltmelerle dolup taşıyor.

İşte en son güncellemeler hakkında bilmeniz gereken her şey.

## 1. Yeni Nesil Web Filtresi (Firewall > Web Filter)
Stratum Firewall artık Unbound RPZ altyapısıyla çalışan, son derece ayrıntılı ve DNS tabanlı bir Web Filtresi içeriyor. İstemci yazılımlarına gerek kalmadan ağınızdaki kullanıcıların nelere erişebileceği üzerinde tam kontrole sahip olun.

*   **Ayrıntılı Politikalar:** Farklı ağlara farklı politikalar atayın (örn. Misafir Wi-Fi için katı, Personel LAN için esnek kurallar). VPN istemcileri de politika ağı olarak tanımlanabilir.
*   **55 Kategori:** Yetişkin içerikten güvenlik tehditlerine, sosyal ağlardan alışverişe kadar UT1 kara listeleri ve Block List Project'ten alınan, günlük güncellenen 55 farklı kategori.
*   **Güvenli Arama (Safe Search):** Google, Bing ve DuckDuckGo üzerinde güvenli sonuçları ve YouTube'da kısıtlı modu zorunlu kılın.
*   **İstisnalar ve Teşhis:** Belirli alan adlarını kolayca beyaz listeye ekleyin ve "Bir siteyi kontrol et" aracıyla bir sitenin hangi kategoriye girdiğini görün.
*   **Detaylı Raporlama:** Son 7 veya 30 gün içinde hangi kategori, site ve cihazların engellendiğini tam olarak görün.

## 2. Saldırı Tespiti (IPS) Kararlılık Düzeltmeleri
v0.21.0 ile gelen satır içi engelleme (IPS) modunda **v0.24.2** ile kritik bir sorunu giderdik: Suricata, başlangıcını görmediği bağlantıları (örn. telefonların açık tuttuğu uzun süreli bağlantılar) düşürüyor ve cihazların Wi-Fi bağlantısının kopmasına neden oluyordu.

*   **Akış Ortası Yakalama (Midstream):** Suricata artık kendisi başlamadan önce açılmış veya zaman aşımına uğramış bağlantıları doğru şekilde takip ediyor.
*   **İstisnalarda Geçiş (Fail-Open):** `exception-policy: ignore` kuralını uyguladık. Artık Suricata bir istisnayla karşılaştığında, akışı tamamen düşürmek yerine trafiğin geçmesine izin veriyor. Yalnızca açıkça belirtilen engelleme kuralları paketleri düşürür, böylece ağınız yoğun denetim altında bile ayakta kalır.

## 3. Diğer Düzeltmeler (v0.24.1)
*   **Web Filtresi Listeleri:** İlk Apply'dan sonra kategori listelerinin indirilmemesi sorunu düzeltildi. Artık kaydedilen yapılandırmaya göre anında indiriliyorlar.

## Sonuç
Stratum Firewall v0.24.2, hem ağ gizliliği hem de kurumsal düzeyde güvenlik açısından devasa bir sıçrama. 

[En son sürümü hemen indirin](/tr/download/) ve tüm teknik detayları [Sürüm Notları](/tr/resources/changelog/) sayfamızdan inceleyin.
