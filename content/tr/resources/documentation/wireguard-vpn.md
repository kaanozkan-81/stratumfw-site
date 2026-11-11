---
title: "WireGuard VPN Kurulumu"
description: "Stratum Firewall üzerinde FreeBSD çekirdek tabanlı WireGuard (if_wg) kurulumu — sıfır bağımlılıklı X25519 anahtar üretimi, QR kod, VLAN erişim kontrolü ve kesintisiz eş yönetimi."
lead: "FreeBSD çekirdeğinde çalışan WireGuard tünellerini dahili X25519 kriptografisi ve anında QR kod kurulumuyla saniyeler içinde devreye alın."
---

Stratum Firewall, FreeBSD 15.1 çekirdeğindeki yerel **WireGuard (`if_wg`)** sürücüsünü kullanır ve **RFC 7748 X25519** anahtar üretimini harici paket bağımlılığı olmadan doğrudan kendi içinde gerçekleştirir.

## 1. WireGuard Sunucusunu Etkinleştirme

![WireGuard Setup](/images/docs/wireguard.png)

1. **VPN → WireGuard** sayfasına gidin ve **WireGuard'ı Etkinleştir** anahtarını açın.
2. **Dinleme Portu ve WAN:** Varsayılan `UDP 51820` portunu koruyun (veya özel port girin) ve dinlenecek WAN arayüzünü seçin; dış IP adresiniz otomatik algılanır (veya dinamik DNS / Sanal IP adresi yazabilirsiniz).
3. **Tünel Ağı:** VPN istemcileri için ayrı bir özel alt ağ belirleyin (örn. `10.202.0.0/24`). Güvenlik duvarı `.1` adresini alır, eşlere sırayla tekil adresler atanır.
4. **Erişim İzinleri:** İstemcilerin hangi yerel LAN ve VLAN ağlarına ulaşabileceğini seçin; isteğe bağlı olarak **Tam Tünel (Full Tunnel)** (tüm internet çıkışını güvenlik duvarından geçirme) ve istemciler arası iletişimi açın.
5. **Kaydet** ve **Değişiklikleri uygula** düğmelerine tıklayın; WAN portu izin kuralı, tünel kuralları, NAT ve DNS erişimi otomatik oluşturulur.

## 2. Kesintisiz Eş (Cihaz) Ekleme

1. **Eşler (`Peers`)** bölümünden **Eş Ekle**'ye tıklayın ve cihaza açıklayıcı bir ad verin (örn. `Kaan-Telefon` veya `Dizustu`).
2. **Anahtar çiftini güvenlik duvarında oluştur** ve ekstra simetrik şifreleme katmanı sağlayan **Ön Paylaşılan Anahtar (PSK)** seçeneklerini açık bırakıp kaydedin:
   * **Mobil Cihazlar (iOS / Android):** Resmi WireGuard uygulamasından ekrandaki **QR Kodu** okutun.
   * **Bilgisayarlar:** `.conf` dosyasını indirip WireGuard istemcisine aktarın.

Yeni bir eş eklemek veya silmek, o anda bağlı olan diğer WireGuard kullanıcılarının bağlantısını **koparmadan** canlı uygulanır. Özel anahtarlar `config.json` dışında yalnızca `root` erişimli dosyalarda saklanır.
