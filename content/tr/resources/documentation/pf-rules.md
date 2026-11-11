---
title: "pf Kuralları, Zamanlamalar, 1:1 NAT ve Takma Adlar"
description: "FreeBSD pf güvenlik duvarı kuralları, Zaman Bazlı Kural Zamanlamaları, Çift Yönlü 1:1 NAT, Arayüz Grupları, Port Yönlendirme, NAT Yansıması, Dinamik Hostname ve Ücretsiz Ülke (GeoIP) Takma Adları ile dummynet Hız Sınırlaması."
lead: "Trafiği çekirdek hızında yönetin — durum bilgili pf kuralları, dakikalık kural zamanlamaları, çift yönlü 1:1 NAT, lisanssız GeoIP/Hostname takma adları ve cihaz başına hız sınırları."
aliases:
  - /tr/resources/documentation/firewall-rules/
---

Stratum Firewall, tanımladığınız kuralları doğrudan FreeBSD çekirdeğindeki **pf (Packet Filter)** ve **dummynet** trafik şekillendiricisine derler; canlı paket/bayt sayaçları, sürükle-bırak sıralama ve 60 saniyelik Safe-Apply güvencesi sunar.

## 1. Kural Değerlendirme Sırası, WAN Günlük Gürültüsü ve Arayüz Grupları (`v0.17.0`, `v0.28.4`)

![pf Rules](/images/docs/pf-rules.png)

**Güvenlik Duvarı → Kurallar (`Firewall → Rules`)** altında kurallar **yukarıdan aşağıya doğru (ilk eşleşen kural uygulanır)** değerlendirilir ve eşleşmeyen tüm trafik **Varsayılan Engelle (Default Deny)** ilkesiyle durdurulur:

1. **Sistem ve Kilitlenme Önleme (Anti-Lockout) Kuralları:** Her arayüz sekmesinin üstünde şeffaf biçimde gösterilir.
2. **Arayüz Grupları (`Arayüzler → Gruplar`):** Birden fazla VLAN veya WAN arayüzünü tek grupta toplayabilirsiniz; grup kuralları üye arayüzlerin kendi kurallarından *önce* çalışır.
3. **Arayüz Kuralları:** **İzin Ver (`Pass`)**, **Engelle (`Block`)** veya **Reddet (`Reject`)** kuralları oluşturabilir ve her kuralda çıkış hattı ([Politika Yönlendirme](/tr/resources/documentation/multi-wan/)) seçebilirsiniz.
4. **WAN Varsayılan Engel Günlüğünü Susturma (`Sistem → Ayarlar → Firewall & NAT`, `v0.28.4`):** Varsayılan engelleme günlüğünün altındaki **"WAN üzerinde de (`Also on the WAN`)"** seçeneğini kapatarak internetten gelen sürekli port taraması gürültüsünü günlükten düşürebilir, buna karşın iç LAN/VLAN ve VPN ağlarındaki engellemeleri kaydetmeye devam edebilirsiniz.

## 2. Zaman Bazlı Güvenlik Duvarı Kuralları (`Güvenlik Duvarı → Zamanlamalar`, `v0.29.0`)

`"Mon-Fri 08:00-18:00; Sat 09:00-13:00"` gibi isimlendirilmiş saat aralıkları tanımlayıp dilediğiniz kurala bağlayın:

* **Süre Dışında Kural Devre Dışı:** Zamanlamalı bir kural yalnızca tanımlı saat aralığında var olur; süresi dışında izin kuralı izin vermez, engelleme kuralı engellemez ve altındaki kurallar karar verir.
* **Açık Bağlantıların Otomatik Kapatılması:** Bir izin (`Pass`) kuralının süresi dolduğu anda, o kuralın açtığı **mevcut aktif bağlantılar (`pf` state) otomatik kapatılır**; açık oturumlar mesai sonrasında devam edemez.
* **Uygula Gerektirmeyen Dakikalık Takip (`stratum-pfrefresh`):** Kurallar güvenlik duvarının saat dilimine göre bir dakika içinde kendiliğinden devreye girer veya çıkar; aktif zamanlamalar kural listesinde yeşil rozetle gösterilir.

## 3. Statik, Hostname ve Ücretsiz Ülke (GeoIP) Takma Adları (`v0.26.0`)

Kurallarda ham IP yazmak yerine **Güvenlik Duvarı → Takma Adlar (`Firewall → Aliases`)** modülünü kullanın:

* **Sunucu, Ağ ve Port Takma Adları:** IP adreslerini, alt ağları ve port aralıklarını tek isim altında gruplayın.
* **Hostname (Alan Adı) Takma Adları:** Her **5 dakikada bir** yerel DNS üzerinden sorgulanır ve CDN IP değişimlerinde kopma yaşanmaması için **1 saat** tabloda tutulur.
* **Ülke (GeoIP) Takma Adları:** Beş bölgesel internet kayıt kuruluşundan (**ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC**) haftalık indirilen ülke IP bloklarını kullanır — **ücretsizdir ve harici lisans anahtarı gerektirmez**.

## 4. Port Yönlendirme, Çift Yönlü 1:1 NAT ve Giden NAT (`v0.28.9`)

**Güvenlik Duvarı → NAT** altında öncelik sırası **Port Yönlendirme → 1:1 NAT → Giden NAT** şeklindedir:

* **Port Yönlendirme ve NAT Yansıması:** WAN veya [Sanal IP'ye (VIP)](/tr/resources/documentation/multi-wan/) gelen istekleri iç sunuculara yönlendirin; **NAT Yansıması** açıksa iç ağdaki cihazlar da dış genel IP üzerinden sunucuya erişebilir.
* **Çift Yönlü 1:1 NAT (`Güvenlik Duvarı → NAT → 1:1`, `v0.28.9`):** İç ağdaki bir sunucuya (veya alt ağa) kendine ait bir genel [Sanal IP](/tr/resources/documentation/multi-wan/) (veya sağlayıcının yönlendirdiği aynı boyutta genel ağ) atayın:
  * **Kilitlenme Koruması:** WAN arayüzünün kendi ana IP adresi 1:1 NAT için reddedilir; böylece güvenlik duvarı yönetimine erişim asla kaybedilmez.
  * **Açık WAN Kuralı Denetimi:** pfSense'de olduğu gibi 1:1 NAT tek başına port açmaz; dışarıdan gelen trafik için WAN üzerinde iç adrese izin (`pass`) kuralı gerekir (liste her sunucuya kaç WAN kuralının izin verdiğini veya hiç yoksa `"kapalı / closed"` durumunu gösterir).
* **Giden NAT (`Outbound NAT`):** **Otomatik**, **Hibrit** veya **Manuel** modlardan birini seçin.

## 5. Bant Genişliği Sınırları (`Güvenlik Duvarı → Hız Sınırlayıcı`, `v0.18.0`)

FreeBSD çekirdeğindeki **dummynet** borularıyla her yerel ağ için **toplam paylaşımlı hız sınırı** veya **cihaz başına hız sınırı** tanımlayabilirsiniz.
