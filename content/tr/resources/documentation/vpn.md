---
title: "VPN Genel Bakış (WireGuard ve OpenVPN)"
description: "Stratum Firewall üzerinde WireGuard ve OpenVPN uzaktan erişim çözümlerini karşılaştırın — FreeBSD çekirdek if_wg, yerleşik OpenVPN sertifika otoritesi, Çift WAN multihome ve VPN-only hesaplar."
lead: "Uzaktaki çalışanlarınızı ve cihazlarınızı FreeBSD çekirdek tabanlı WireGuard veya yerleşik PKI ve Çoklu WAN destekli OpenVPN ile güvenle ağınıza bağlayın."
---

Stratum Firewall, hem **FreeBSD 15.1 çekirdeğinde (`if_wg`)** çalışan yüksek hızlı **WireGuard** hem de yerleşik Sertifika Otoritesi (CA), parola doğrulaması ve kesintisiz OPNsense göç desteğine sahip **OpenVPN** çözümlerini yerleşik olarak sunar.

## 1. WireGuard ve OpenVPN Karşılaştırması

| Özellik | WireGuard (`VPN → WireGuard`) | OpenVPN (`VPN → OpenVPN`) |
| :--- | :--- | :--- |
| **Çalışma Katmanı** | FreeBSD 15.1 çekirdeği (`if_wg`) | `tls-crypt` / `tls-auth` korumalı servis |
| **Kimlik Doğrulama** | Anahtar çifti (`X25519`) + isteğe bağlı PSK | Kullanıcı Sertifikası + Parola (`bcrypt` / `argon2id`) veya yalnızca sertifika |
| **İstemci Kurulumu** | Anında QR kod okutma ve `.conf` indirme | Her şey gömülü tekil `.ovpn` profil dosyası |
| **Çoklu WAN Desteği** | Otomatik genel IP algılama | Çoklu sıralı `remote` satırı + `multihome` simetrik WAN yanıtı (`v0.27.1`) |
| **En Uygun Senaryo** | Maksimum hız, mobil ağ geçişleri (Wi-Fi ↔ LTE) | Parolalı kurumsal hesaplar, Windows DNS kilidi, [OPNsense göçü](/tr/resources/documentation/opnsense-migration/) |

## 2. OpenVPN Uzaktan Erişim Özellikleri (`v0.7.0` – `v0.27.1`)

**VPN → OpenVPN** menüsünden harici hiçbir araç kullanmadan eksiksiz bir VPN sunucusu kurabilirsiniz:

* **Yerleşik Eliptik Eğri Sertifika Otoritesi (EC CA):** CA, sunucu sertifikası, iptal listesi (`CRL`) ve `tls-crypt` anahtarı otomatik oluşturulur; bir kullanıcının VPN yetkisi kaldırıldığında sertifikası anında iptal edilir.
* **VPN-Only Hesaplar ve Sabit Tünel IP'si (`Sistem → Kullanıcılar`):** Web yönetim paneline giriş yetkisi olmayan yalnızca VPN erişimli (**VPN-only**) kullanıcılar oluşturabilir, kullanıcı başına sabit VPN IP adresi atayabilir ve `.ovpn` profilini tek tıkla indirebilirsiniz.
* **VLAN Bazlı Erişim ve Windows DNS Koruması:** VPN istemcilerinin hangi LAN ve VLAN ağlarına erişebileceğini kutucuklarla seçebilir ve Windows istemcileri için **`block-outside-dns`** seçeneğini açabilirsiniz.
* **Çift WAN Uyumluluğu (`v0.27.1`):** İstemci profillerine birden fazla WAN adresi ekleyebilir; `multihome` ve `pf` yönlendirmesi sayesinde ikinci WAN hattından gelen bağlantıların aynı hattan yanıtlanmasını sağlayabilirsiniz.

Adım adım WireGuard kurulumu ve QR kodlu cihaz tanımlama için [WireGuard VPN Kurulumu](/tr/resources/documentation/wireguard-vpn/) rehberine geçebilirsiniz.
