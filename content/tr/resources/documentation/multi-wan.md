---
title: "Çoklu WAN Yedekliliği"
lead: "Birden fazla internet bağlantısı yapılandırın, böylece birincil bağlantınız koptuğunda ağınız çevrimiçi kalır."
---

Modern ağlar için tek bir internet bağlantısı tek bir hata noktasıdır. Stratum Firewall, birden fazla WAN arayüzünü yönetmenize ve birincil bağlantı başarısız olduğunda otomatik yedeklilik sağlamanıza olanak tanır.

## Kavramlar

- **WAN:** Geniş Alan Ağı — ağınız ile internet arasındaki bağlantı.
- **Yedeklilik (Failover):** Bir bağlantı koptuğunda trafiğin otomatik olarak diğerine kaydırılması.
- **Yük Dengeleme:** Trafiğin mevcut tüm bağlantılara dağıtılması.
- **Ağ Geçidi Grubu:** Birden fazla WAN arayüzünü birlikte yönetmek için mantıksal bir grup.

## Adım 1: Birden Fazla WAN Arayüzü Atama

1. **Arayüzler → Atamalar** bölümüne gidin.
2. Ek fiziksel arayüzleri **WAN** veya **WAN tipi OPT** rolüne atayın.
3. Her birini ayrı ayrı yapılandırın (**Arayüzler → WAN1**, **WAN2** vb.).

## Adım 2: Ağ Geçidi Grupları Oluşturma

1. **Arayüzler → Ağ Geçidi Grupları** bölümüne gidin.
2. **Grup Ekle**'ye tıklayın.
3. Bir ad girin (örn. `wan-failover`).
4. Arayüzleri öncelik sırasına göre ekleyin.

### Katman Türleri

- **Failover:** Birincil bağlantı koptuğunda yedeğe geçer.
- **Load Balance (Yük Dengeleme):** Trafiği mevcut tüm bağlantılara dağıtır.

## Adım 3: Yedeklilik Kuralları

**Güvenlik Duvarı → Kurallar** altında, kuralınızın **Ağ Geçidi** seçeneğini oluşturduğunuz gruba ayarlayın.

## Sağlık Kontrolleri

Stratum, bağlantı sağlığını çeşitli yöntemlerle izler:
- **ICMP:** Güvenilir uzak ana bilgisayarlara ping atar.
- **HTTP/HTTPS:** Bilinen bir uç noktaya ulaşır.
- **DNS:** Bilinen bir alan adını çözer.

## Kullanım Senaryoları

### KOBİ: Birincil + Yedek
İki bağlantı: fiber birincil, LTE yedek. Birincil çöktüğünde otomatik geçiş.

### Kurumsal: Aktif/Aktif
İki eşit bağlantı: trafik her ikisine de dağıtılır. Biri çökerse diğeri tüm yükü alır.

## Sorun Giderme

### Yedeklilik Çalışmıyor
- Her arayüzün ayrı ayrı IP aldığını doğrulayın.
- Sağlık kontrol ayarlarını kontrol edin (**Sistem → Ayarlar → Ağ Geçitleri**).
- Test için birincil bağlantının kablosunu çıkarın.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>pf ve Kurallar</h3>
    <p>Yedeklilik kuralları yazın ve uygulayın.</p>
    <a href="/tr/resources/documentation/pf-rules/" class="card-link">pf Kılavuzu →</a>
  </div>
  <div class="card">
    <h3>WireGuard VPN</h3>
    <p>Uzaktan çalışanlar için güvenli erişim sağlayın.</p>
    <a href="/tr/resources/documentation/wireguard-vpn/" class="card-link">VPN Kılavuzu →</a>
  </div>
</div>
