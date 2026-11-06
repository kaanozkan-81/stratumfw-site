---
title: "pf ve Güvenlik Duvarı Kuralları"
lead: "pf paket filtresinde uzmanlaşın — kuralların nasıl değerlendirildiğini, sıralandığını ve güvenli bir şekilde nasıl uygulandığını öğrenin."
---

Stratum Firewall, FreeBSD'ye yerleşik endüstri standardı güvenlik duvarı motoru olan **pf (Packet Filter)** kullanır. Karmaşık, tescilli kural sözdizimleri kullanan eski güvenlik duvarlarının aksine pf, temiz ve okunabilir bir yapılandırma dili kullanır.

## pf Kuralları Nasıl Değerlendirir?

![pf Rules](/images/docs/pf-rules.png)

Anlaşılması gereken en kritik kavram şudur: **pf, kuralları yukarıdan aşağıya doğru değerlendirir ve ilk eşleşen kural kazanır.**

Bir paket bir kuralla eşleşirse, işlem (geçiş, engelleme veya reddetme) hemen gerçekleştirilir ve başka hiçbir kural değerlendirilmez.

- **Geçiş (Pass):** Paketin geçmesine izin verir.
- **Engelle (Block):** Paketi sessizce düşürür. Gönderen taraf herhangi bir yanıt almaz (güvenlik için iyidir).
- **Reddet (Reject):** Paketi düşürür ve gönderene bir TCP RST veya ICMP ulaşılamıyor yanıtı gönderir (hata ayıklama için iyidir).

Stratum varsayılan olarak **Varsayılan Reddet (Default Deny)** politikasını uygular. Bir paket hiçbir kuralla eşleşmezse engellenir.

## Bir Kuralın Anatomisi

Stratum panelinde (**Güvenlik Duvarı → Kurallar**) bir kural oluşturduğunuzda, aşağıdaki parametreleri tanımlıyorsunuz demektir:

- **Arayüz:** Bu trafik hangi arayüze giriyor/çıkıyor? (örn. WAN, LAN).
- **Yön:** `In` (güvenlik duvarına giren) veya `Out` (güvenlik duvarından çıkan).
- **İşlem:** Geçiş (Pass), Engelleme (Block) veya Reddetme (Reject).
- **Protokol:** TCP, UDP, ICMP veya herhangi biri.
- **Kaynak / Hedef:** IP adresleri, ağlar veya Takma Adlar (Aliases).
- **Port:** Hedef port (örn. HTTP için 80, SSH için 22).

## Takma Adlar ve Makrolar

Kural setinizi temiz tutmak için ham IP adresleri veya portlar yazmaktan kaçının. Bunun yerine **Takma Adlar** (Aliases) kullanın (**Güvenlik Duvarı → Takma Adlar**).

- **Ana Bilgisayar Takma Adı:** Tek IP adreslerinin listesi (örn. `Web_Sunuculari`).
- **Ağ Takma Adı:** Alt ağların listesi (örn. `Dahili_Alt_Aglar`).
- **Port Takma Adı:** Portların listesi (örn. `Web_Portlari` = 80, 443).

Bir Takma Adı güncellediğinizde, `pf` tam bir güvenlik duvarı yeniden yüklemesi gerektirmeden ona başvuran tüm kuralları otomatik olarak günceller.

## Güvenli Uygulama (Safe-Apply) İş Akışı

Güvenlik duvarı kuralları yazmak bir risk taşır: yanlışlıkla kendi yönetim erişiminizi engelleyebilirsiniz. Stratum bunu **Güvenli Uygulama** ile çözer.

Kural düzenleyicide **Uygula**'ya tıkladığınızda:
1. Sistem `pfctl -nf` kullanarak sözdizimi kontrolü yapar.
2. Yeni kural setini çekirdeğe uygular.
3. Ekranın üst kısmında 60 saniyelik geri sayımlı bir **onay bandı** belirir.
4. Değişiklikleri korumak için **Onayla**'ya tıklamalısınız. Bağlantıyı kaybederseniz, sistem zaman aşımından sonra otomatik olarak önceki kural setine geri döner.

## En İyi Uygulamalar

1. **Sıra Önemlidir:** Daha spesifik kuralları genel kuralların üstüne yerleştirin.
2. **Anti-Lockout Kurallarını Kullanın:** Stratum, LAN arayüzü için varsayılan olarak otomatik bir anti-lockout kuralı ekler. Ne yaptığınızı kesinlikle bilmiyorsanız silmeyin.
3. **Her Şeyi Günlüğe Kaydedin:** Hangi trafiğin düşürüldüğünü görmek için engelleme kurallarında günlüğe kaydetmeyi etkinleştirin (**Güvenlik Duvarı Günlüğü**).
4. **Teşhislerle Test Edin:** Katı bir kuralı uygulamadan önce, trafik modellerini doğrulamak için **Teşhisler → Paket Yakalama** aracını kullanın.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>WireGuard VPN Kurun</h3>
    <p>Korumalı ağınıza güvenli uzaktan erişime izin verin.</p>
    <a href="/tr/resources/documentation/wireguard-vpn/" class="card-link">WireGuard Kılavuzu →</a>
  </div>
  <div class="card">
    <h3>Tehdit İstihbaratını Etkinleştirin</h3>
    <p>pf tablolarını kullanarak kötü amaçlı IP'leri otomatik olarak engelleyin.</p>
    <a href="/tr/resources/documentation/blocklists/" class="card-link">Karalisteler Kılavuzu →</a>
  </div>
</div>
