---
title: "Stratum Firewall v0.34.1: Merkezden Şubeye IPsec, Adanmış WireGuard S2S, 30 Günlük VLAN Trafik Matrisi ve Kesintisiz Suricata Güncellemeleri"
seoTitle: "Stratum FW v0.34.1: IPsec, WireGuard S2S ve VLAN Matrisi"
date: 2026-11-17
author: "Stratum Security"
description: "Stratum Firewall v0.34.1; çoklu üretici uyumlu Site-to-Site IPsec (IKEv2 ve IKEv1), adanmış WireGuard S2S arayüzleri, 30 günlük Ağlar Arası VLAN trafik matrisi ve kesintisiz Suricata 8 güncellemeleriyle yayında."
seoDescription: "FreeBSD 15.1 üzerinde çoklu üretici IKEv2/IKEv1 IPsec, adanmış çekirdek WireGuard S2S (wg1+), 30 günlük VLAN trafik matrisi ve kesintisiz Suricata 8 IPS."
translationKey: "blog-v0-34-1"
tags: ["stratum firewall", "ipsec", "wireguard", "freebsd 15.1", "suricata"]
---

`v0.30.1` ile `v0.34.1` sürümleri arasında [Stratum Firewall](/tr/), tek lokasyonlu bir sınır güvenlik duvarından çok şubeli kurumsal ağları birleştiren tam kapsamlı bir yönlendirme ve güvenlik merkezine dönüştü. **FreeBSD 15.1-RELEASE** çekirdeği, durum bilgili `pf` paket filtresi, OpenZFS ve asenkron Python 3.12 / FastAPI kontrol katmanı üzerinde çalışan bu yeni sürüm serisi; çoklu üretici uyumlu **Merkezden Şubeye (Site-to-Site) IPsec**, izole **WireGuard S2S (`wg1+`)** tünelleri, **30 günlük Ağlar Arası VLAN Trafik Matrisi** ve hat düşürmeyen **Suricata 8** güncelleme mimarisini bir araya getiriyor. Tüm alt sürüm detaylarına [Resmi Sürüm Notları](/tr/resources/changelog/) sayfasından ulaşabilirsiniz.

## 1. Çoklu Üretici Uyumlu Merkezden Şubeye IPsec (`IKEv2` ve `IKEv1`, `v0.33.0` - `v0.33.2`)

Kurumsal topolojilerde merkez ve şubeler her zaman aynı marka donanımı kullanmaz. **VPN -> IPsec** modülü, `strongSwan` anahtar müzakeresini saf FreeBSD çekirdek ESP şifrelemesiyle (`enc0`) birleştirerek **FortiGate**, **Cisco IOS/ASA**, **OPNsense**, **pfSense** ve bulut VPC ağ geçitleriyle doğrudan tünel kurmanızı sağlar:

* **Modern ve Klasik Şifreleme Profilleri:** Hem `IKEv2` hem de eski nesil cihazlar için `IKEv1` (Main ve Aggressive modları) desteğiyle birlikte `AES-256-GCM`, `AES-CBC`, `SHA-2`, `DH 14-21` grupları, `Curve25519`, PFS, DPD kopma tespiti ve otomatik UDP `4500` `NAT-T` kapsüllemesi sunar.
* **Çoklu Alt Ağ (Phase 2) Seçicileri ve `enc0` Denetimi:** Tek bir tünel altında birden fazla yerel ve uzak alt ağ çifti tanımlanabilir; şifresi çözülen şube trafiği FreeBSD `enc0` arayüzü üzerinde `pf` kurallarıyla tam görünürlükle denetlenir.
* **Otomatik NAT Muafiyeti, MSS Sabitleme ve Uç Nokta Koruması:** Şube alt ağları giden WAN NAT kurallarından otomatik muaf tutulur, PMTUD paket parçalanması sorunlarını önlemek için TCP `SYN` paketleri `1360` bayt MSS değerine sabitlenir, paylaşımlı anahtarlar `0600` izinli izole dosyalarda saklanır ve WAN üzerindeki `UDP 500/4500` ile `ESP` portları yalnızca tanımlı uzak IP adreslerine açılır.

<figure style="margin:1.75rem 0;background:#08101f;border:1px solid #1e293b;border-radius:14px;overflow:hidden;box-shadow:0 18px 40px -12px rgba(15,23,42,0.35);">
  <img src="/images/s2s-vpn-topology.jpg" alt="Stratum Firewall Coklu Uretici IPsec ve WireGuard S2S Merkez Mimarisi" loading="lazy" style="display:block;width:100%;height:auto;">
  <figcaption style="padding:0.75rem 1rem;font-size:0.84rem;color:#cbd5e1;text-align:center;border-top:1px solid #1e293b;">FortiGate, Cisco, bulut VPC, MikroTik RouterOS 7 ve uzak Stratum / OPNsense şubelerini tek merkezde birleştiren S2S VPN mimarisi.</figcaption>
</figure>

## 2. Adanmış Çekirdek WireGuard Site-to-Site (`wg1, wg2...`, `v0.34.0` - `v0.34.1`)

Mobil kullanıcı bağlantıları `wg0` üzerinde çalışmaya devam ederken, şubeler arası bağlantılar artık **VPN -> WireGuard -> Site-to-Site** ekranı altından her şube için ayrı bir çekirdek `if_wg` arayüzünde (`wg1`, `wg2` vb.) izole olarak çalıştırılıyor:

* **NAT Arkasında ve MikroTik RouterOS 7 ile Doğrulanmış Uyum:** Operatör veya üst yönlendirici NAT arkası senaryolarında MikroTik RouterOS 7 (`RB4011` ve CCR serisi) cihazlarla test edildi; kalıcı keepalive, isteğe bağlı pinglenebilir `/30` tünel geçiş IP'leri ve trafiği kesmeyen `wg syncconf` canlı yenileme desteğiyle çalışır.
* **Sıkı `AllowedIPs` ve Rota Çakışma Koruması:** Her şube tüneli çekirdek kripto yönlendirme tablosunda yalnızca yetkili uzak alt ağları kabul eder; uygulama öncesi çakışma kontrolü, yerel VLAN bloklarının veya uzaktan erişim VPN havuzlarının yanlış rota tanımlarıyla ezilmesini engeller.

<figure style="margin:1.75rem 0;background:#08101f;border:1px solid #1e293b;border-radius:14px;overflow:hidden;box-shadow:0 18px 40px -12px rgba(15,23,42,0.35);">
  <img src="/images/s2s-vpn-panel.jpg" alt="Stratum Firewall Canli IPsec ve WireGuard S2S Tunel Durum Paneli" loading="lazy" style="display:block;width:100%;height:auto;">
  <figcaption style="padding:0.75rem 1rem;font-size:0.84rem;color:#cbd5e1;text-align:center;border-top:1px solid #1e293b;">IKEv2, IKEv1 ve WireGuard S2S el sıkışma zamanlarını ve çekirdek kripto durumunu tek ekranda gösteren kontrol paneli.</figcaption>
</figure>

## 3. 30 Günlük Ağlar Arası VLAN ve VPN Trafik Matrisi (`v0.32.0`)

Yalnızca anlık akan günlük kayıtlarına bakarak VLAN'lar arası kısıtlama kuralları yazmak, ayda birkaç kez çalışan kritik servisleri fark etmeden kesme riski taşır. **Tanılama -> Güvenlik Duvarı Günlüğü -> Ağlar Arası (Between networks)** görünümü, yerel VLAN'lar ile `enc0` IPsec ve `wg1+` WireGuard şube ağları arasındaki tüm geçişleri **30 günlük kalıcı bir matris** halinde özetler:

* **Gerçek Trafiğe Dayalı Mikro-Segmentasyon:** Hangi alt ağın hangi hedef ağa hangi port ve protokol üzerinden eriştiğini 30 günlük gerçek istatistiklerle görerek sıkılaştırma kurallarını sıfır tahminle oluşturabilirsiniz.
* **Harici Log Sunucusu Gerektirmez:** Veriler OpenZFS üzerinde kontrollü boyutla yerel olarak tutulur; şube cihazları ek bir SIEM kurulumuna ihtiyaç duymadan tam görünürlük sağlar.

## 4. Kesintisiz Suricata 8 Güncellemeleri, Kural Ayırıcılar ve Açık Tema (`v0.30.1` - `v0.31.6`)

Bu sürüm serisinde günlük sistem yönetimi deneyimini doğrudan iyileştiren önemli altyapı dokunuşları da tamamlandı:

* **Güncellemelerde Sıfır WAN Hat Düşmesi (`v0.31.4` - `v0.31.6`):** Sistem yükseltmeleri ve Suricata 8 IPS/IDS servis yenilemeleri sırasında `igb` ve `netmap` ağ kartı donanım offload ayarları korunarak fiziksel portların yeniden el sıkışmaya girmesi (link flap) tamamen önlendi; ayrıca bekleyen `pf` değişiklikleri için üst bildirim çubuğu eklendi.
* **Renkli Kural Ayırıcılar ve Görünüm Filtreleri (`v0.31.0` - `v0.31.3`):** Yoğun güvenlik duvarı ve NAT tablolarını pfSense tarzı renkli başlık çubuklarıyla gruplayabilir; **Tümü**, **Varsayılan** ve **Özel** sekmeleriyle kuralları anında süzebilirsiniz.
* **Göz Yormayan Açık Tema ve Güvenli Oturum Çerezleri (`v0.30.4` - `v0.31.2`):** Gündüz kullanımında parlamayı azaltan kırık beyaz Açık Tema eklendi ve HTTPS oturumlarında `Secure` çerez bayrağı zorunlu hale getirildi.
* **Gelişmiş OPNsense Geçişi ve Çoklu WAN OpenVPN (`v0.30.1` - `v0.30.5`):** OPNsense `config.xml` geçiş aracı artık Unbound DNSBL engelleme listelerini ve tam aralıklı DHCP havuzlarını doğrudan aktarır; Çoklu WAN OpenVPN istemci profillerine yedek WAN adresleri sıralı `remote` satırları olarak otomatik eklenir.

## `v0.34.1` Kurulum İmajlarını İndirin veya Canlı Demoda Deneyin

Yeni derlenen `v0.34.1` çevrimdışı kurulum imajlarını (VGA konsol ve sanallaştırma için `.iso`, `115200 8N1` seri konsol donanımları için `.img`) [İndir sayfasından](/tr/download/) hemen edinebilir; mevcut kurulumlarınızı ise **Sistem -> Donanım Yazılımı** ekranından otomatik ZFS Önyükleme Ortamı (`bectl`) güvencesiyle kesintisiz yükseltebilirsiniz. Kurulum yapmadan arayüzü incelemek için [Canlı Demo](https://demo.stratumfw.com) ortamına göz atabilir veya [Özellikler sayfasında](/tr/product/features/#s2s-vpn) mimari detayları inceleyebilirsiniz.
