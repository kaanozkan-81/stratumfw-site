---
title: "pf Kuralları, NAT ve Takma Adlar"
description: "FreeBSD pf güvenlik duvarı kuralları, Arayüz Grupları, Port Yönlendirme, NAT Yansıması, Dinamik Hostname ve Ücretsiz Ülke (GeoIP) Takma Adları ile dummynet Hız Sınırlaması."
lead: "Trafiği çekirdek hızında yönetin — durum bilgili pf kuralları, arayüz grupları, lisanssız GeoIP ve Hostname takma adları, NAT yansıması ve cihaz başına hız sınırları."
aliases:
  - /tr/resources/documentation/firewall-rules/
---

Stratum Firewall, tanımladığınız kuralları doğrudan FreeBSD çekirdeğindeki **pf (Packet Filter)** ve **dummynet** trafik şekillendiricisine derler; canlı paket/bayt sayaçları, sürükle-bırak sıralama ve 60 saniyelik Safe-Apply güvencesi sunar.

## 1. Kural Değerlendirme Sırası ve Arayüz Grupları (`v0.17.0`)

![pf Rules](/images/docs/pf-rules.png)

**Güvenlik Duvarı → Kurallar (`Firewall → Rules`)** altında kurallar **yukarıdan aşağıya doğru (ilk eşleşen kural uygulanır)** değerlendirilir ve eşleşmeyen tüm trafik **Varsayılan Engelle (Default Deny)** ilkesiyle durdurulur:

1. **Sistem ve Kilitlenme Önleme (Anti-Lockout) Kuralları:** Her arayüz sekmesinin üstünde şeffaf biçimde gösterilir.
2. **Arayüz Grupları (`Arayüzler → Gruplar`):** Birden fazla VLAN veya WAN arayüzünü tek grupta toplayabilirsiniz; grup kuralları üye arayüzlerin kendi kurallarından *önce* çalışır.
3. **Arayüz Kuralları:** **İzin Ver (`Pass`)**, **Engelle (`Block` — sessiz düşürme)** veya **Reddet (`Reject` — TCP RST / ICMP yanıtlı)** kuralları oluşturabilir ve her kuralda çıkış hattı ([Politika Yönlendirme](/tr/resources/documentation/multi-wan/)) seçebilirsiniz.

## 2. Statik, Hostname ve Ücretsiz Ülke (GeoIP) Takma Adları (`v0.26.0`)

Kurallarda ham IP yazmak yerine **Güvenlik Duvarı → Takma Adlar (`Firewall → Aliases`)** modülünü kullanın:

* **Sunucu, Ağ ve Port Takma Adları:** IP adreslerini, alt ağları ve port aralıklarını tek isim altında gruplayın.
* **Hostname (Alan Adı) Takma Adları:** `update.example.com` gibi alan adları her **5 dakikada bir** güvenlik duvarının kendi DNS çözümleyicisi üzerinden sorgulanır; CDN IP değişimlerinde bağlantı kopmaması için adresler son görüldükten sonra **1 saat** tabloda tutulur.
* **Ülke (GeoIP) Takma Adları:** Port yönlendirmelerini belirli ülkelerle sınırlandırmak (örn. *"yalnızca Türkiye erişebilsin"*) veya ülkeleri engellemek için beş bölgesel internet kayıt kuruluşundan (**ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC**) haftalık indirilen ülke IP bloklarını kullanır — **ücretsizdir ve harici API/lisans anahtarı gerektirmez**.

Her iki dinamik takma ad türü de `pf` çalışırken Uygula (Apply) gerektirmeden bellekte canlı güncellenir.

## 3. Port Yönlendirme, Giden NAT ve NAT Yansıması

**Güvenlik Duvarı → NAT** altında:

* **Port Yönlendirme (`Port Forwards`):** WAN adresine veya belirli bir [Sanal IP'ye (VIP)](/tr/resources/documentation/multi-wan/) gelen istekleri iç sunuculara yönlendirin. **NAT Yansıması (`NAT Reflection`)** açıksa yerel ağdaki cihazlar da dış genel IP üzerinden iç sunuculara erişebilir.
* **Giden NAT (`Outbound NAT`):** **Otomatik**, **Hibrit** (otomatik + Sanal IP çıkış kuralları) veya **Manuel** modlardan birini ve gerekirse statik kaynak portu seçeneğini kullanın.

## 4. Bant Genişliği Sınırları (`Güvenlik Duvarı → Hız Sınırlayıcı`, `v0.18.0`)

FreeBSD çekirdeğindeki **dummynet** borularını doğrudan `pf` üzerinden kullanarak her yerel ağ için **toplam paylaşımlı hız sınırı** (örn. ofis VLAN'ı için `250 Mbit/sn`) veya **cihaz başına hız sınırı** (örn. misafir telefonları için cihaz başı `20 Mbit/sn`) tanımlayabilirsiniz.
