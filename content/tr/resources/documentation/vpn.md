---
title: "VPN Genel Bakış"
lead: "Stratum Gateway, OpenVPN ve WireGuard ile yerel VPN çözümleri sunar."
---

Stratum Gateway, uzaktan erişim için iki tam entegre VPN çözümü içerir:

- **WireGuard:** Modern, yüksek performanslı, çekirdek içi tüneller için.
- **OpenVPN:** Klasik uyumluluk ve geniş istemci desteği için.

## Hangi VPN'i Seçmeliyim?

| Özellik | WireGuard | OpenVPN |
|---|---|---|
| **Performans** | ⭐⭐⭐⭐⭐ (Çekirdek) | ⭐⭐⭐ (Kullanıcı alanı) |
| **Basitlik** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Uyumluluk** | Modern istemciler | Eski sistemler + geniş |
| **Kurulum** | QR kod + .conf | Sertifika + .ovpn |
| **En İyi Kullanım** | Modern mobil/dizüstü | Kurumsal/uyumluluk |

**Önerimiz:** Mümkünse **WireGuard** kullanın. Hızlı, basit ve kurulumu QR kod ile saniyeler alır. Ancak eski cihazlarla uyumluluk gerekiyorsa OpenVPN'i tercih edin.

## WireGuard Hızlı Başlangıç

1. **VPN → WireGuard** bölümüne gidin.
2. **WireGuard'ı Etkinleştir** seçeneğini açın.
3. Tünel ağını yapılandırın (örn. `10.202.0.0/24`).
4. **Eş Ekle** ile cihaz ekleyin.
5. QR kodunu taratın veya `.conf` dosyasını indirin.

Detaylı kılavuz: [WireGuard VPN](/tr/resources/documentation/wireguard-vpn/)

## OpenVPN Hızlı Başlangıç

1. **VPN → OpenVPN** bölümüne gidin.
2. **OpenVPN'i Etkinleştir** seçeneğini açın.
3. Yerleşik CA'yı yapılandırın.
4. **Kullanıcı Ekle** ile kullanıcı oluşturun.
5. `.ovpn` profilini indirin.

## Güvenlik Önerileri

- **PSK (Ön Paylaşılan Anahtar):** WireGuard için ekstra bir şifreleme katmanıdır, mutlaka açın.
- **Sertifika İptali:** OpenVPN kullanıcısı ayrıldığında sertifikasını iptal edin.
- **Tek Kullanıcı = Tek Cihaz:** Kullanıcı başına bir eş/cihaz prensibine uyun.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>WireGuard Rehberi</h3>
    <p>Modern VPN ile hızlı kurulum.</p>
    <a href="/tr/resources/documentation/wireguard-vpn/" class="card-link">WireGuard Kılavuzu →</a>
  </div>
  <div class="card">
    <h3>pf Kuralları</h3>
    <p>VPN trafiği için güvenlik duvarı kuralları.</p>
    <a href="/tr/resources/documentation/pf-rules/" class="card-link">pf Kılavuzu →</a>
  </div>
</div>
