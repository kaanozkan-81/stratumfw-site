---
title: "Engelleme Listeleri, Web Filtresi ve Suricata IPS"
description: "Stratum Firewall çok katmanlı tehdit savunması — 63 IP tehdit listesi, Unbound RPZ DNSBL, 55 kategorili Web Filtresi, Çift Motorlu Suricata 8 IPS/IDS ve Birleşik Engelleme Raporları."
lead: "Tehditleri her katmanda durdurun — çekirdek pf IP listeleri, Unbound RPZ DNS engelleme, 55 kategorili Web Filtresi, satır içi Suricata 8 IPS ve pfBlockerNG tarzı birleşik raporlar."
---

Stratum Firewall, harici abonelik veya lisans anahtarı gerektirmeyen dört entegre savunma katmanını tek bir yönetim panelinde birleştirir.

## 1. IP Engelleme Listeleri (`Güvenlik Duvarı → Engelleme Listeleri → IP`, `v0.9.0`)

![IP Blocklists](/images/docs/blocklists.png)

* **63 Hazır Tehdit Beslemesi:** *Spamhaus DROP, Abuse.ch Feodo Tracker, Emerging Threats, PRI1–PRI5, Tarayıcılar, TOR* dahil 63 güncel IPv4 beslemesi doğrudan çekirdek `pf` tablolarına yüklenir.
* **Gelen ve Giden Koruma:** İnternetten gelen saldırılar sessizce düşürülürken içeriden dışarıya giden zararlı bağlantılar hızla reddedilir; kendi izin listeniz her zaman önceliklidir ve yerel ağ aralıkları yanlışlıkla engellenmeye karşı korumalıdır.

## 2. DNSBL ve 55 Kategorili Web Filtresi (`v0.24.0`)

* **DNS Engelleme Listeleri (`Engelleme Listeleri → DNSBL`):** pfBlockerNG ve **HaGeZi** kataloglarıyla reklam, izleyici, oltalama (phishing) ve zararlı alan adlarını **Unbound RPZ** katmanında durdurun.
* **Ağ Bazlı Web Filtresi (`Güvenlik Duvarı → Web Filtresi`):** Her LAN, VLAN veya VPN ağına farklı politikalar atayarak **55 kategori** (*UT1 Université Toulouse Capitole* ve *Block List Project* — yetişkin içerik, zararlı siteler, filtre aşma araçları, sosyal ağlar vb.) bazında filtreleme yapın ve Google (`google.com.tr` dahil), Bing, DuckDuckGo **Güvenli Arama (Safe Search)** ile YouTube Kısıtlı Modunu zorunlu kılın.

## 3. Birleşik Engelleme Raporları (`Engelleme Listeleri → Engellenenler`, `v0.28.2` – `v0.28.3`)

pfBlockerNG raporlarına benzer şekilde **DNSBL**, **Web Filtresi** ve **IP Engelleme Listelerinin** durdurduğu tüm trafiği tek ekranda (ve Kontrol Paneli **Engelleme Listeleri** kartında) inceleyin:
* Son **1 saat, 24 saat veya 7 gün** için saatlik/günlük grafikleri, hangi listenin engelleme yaptığını, en çok engellenen alan adlarını/IP'leri ve en fazla engelleme üreten cihazları filtreleyin (Unbound 1.26 RPZ günlük formatıyla tam uyumludur).

## 4. Arayüz Bazlı Çift Motorlu Suricata 8 IPS / IDS (`Servisler → Saldırı Tespit`, `v0.24.4`)

* **Arayüz Başına İzle veya Engelle:** Her arayüzü bağımsız olarak **Kapalı**, **İzle (`Watch` / IDS)** veya **Engelle (`Block` / satır içi `netmap` IPS)** moduna alabilirsiniz. İki mod farklı arayüzlerde birlikte seçildiğinde Stratum otomatik olarak `netmap` ve `pcap` tabanlı iki ayrı Suricata sürecini yönetir.
* **Kesintisiz Kural Güncelleme (`SIGUSR2`):** ET Open kural kategorilerinde yapılan değişiklikler ve tek tıkla kural devre dışı bırakma işlemleri FreeBSD `SIGUSR2` sinyaliyle trafiği kesmeden canlı uygulanır.
