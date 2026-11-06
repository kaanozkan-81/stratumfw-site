---
title: "Dosya ve Yol Referansı"
lead: "Yapılandırmanın nerede yaşadığı, günlüklerin nerede saklandığı ve her dosyanın ne yaptığı."
---

Stratum Firewall'ın temel tasarım ilkelerinden biri **şeffaflıktır**. Yapılandırmaları ikili veritabanlarında saklamıyoruz. Her şey inceleyebileceğiniz, yedekleyebileceğiniz veya sürüm kontrolüne alabileceğiniz düz metin dosyalarında saklanır.

## Temel Dizinler

| Yol | Açıklama |
|---|---|
| `/usr/local/etc/stratum/` | Ana yapılandırma dizini. |
| `/usr/local/etc/stratum/config.json` | Tüm güvenlik duvarı ayarları için tek doğruluk kaynağı. |
| `/usr/local/etc/stratum/wireguard/` | WireGuard anahtarlarını ve oluşturulan `wg0.conf` dosyalarını saklar. |
| `/usr/local/etc/stratum/blocklists/` | `pf` tabloları için indirilen IP akışlarını saklar. |
| `/opt/stratum/` | Panelin ve Python sanal ortamının kurulum dizini. |

## Oluşturulan Yapılandırma Dosyaları

Panelde **Uygula**'ya tıkladığınızda, Python motoru `config.json` dosyanızı yerel FreeBSD dosyalarına dönüştürür. **Bu dosyaları doğrudan düzenlemeyin**, çünkü üzerine yazılacaklardır.

| Yol | Oluşturan | Açıklama |
|---|---|---|
| `/etc/pf.conf` | `generators.py` | Ana güvenlik duvarı kuralları, NAT ve yönlendirme. |
| `/usr/local/etc/wireguard/wg0.conf` | `generators.py` | WireGuard sunucu arayüzü yapılandırması. |
| `/usr/local/etc/unbound/unbound.conf` | `generators.py` | DNSBL bölgeleri dahil DNS çözümleyici yapılandırması. |
| `/etc/rc.conf` | `generators.py` | FreeBSD sistem başlangıç yapılandırması. |

## Günlük Dosyaları

Günlükler hata ayıklama için gereklidir. Varsayılan olarak Stratum, FreeBSD'nin yerel günlük mekanizmalarını kullanır.

Panel ve kimlik doğrulama günlükleri:

~~
cat /var/log/auth.log
~~

*(Başarılı ve başarısız giriş denemelerini ve API eylemlerini içerir).*

Güvenlik duvarı günlükleri — engellenen paketler `pflog0`'a kaydedilir:

~~
tcpdump -n -e -ttt -i pflog0
~~

Sistem mesajları:

~~
cat /var/log/messages
~~

## Yedekleme

Tüm dosya sistemini yedeklemeniz gerekmez. **`/usr/local/etc/stratum/`** dizinini yedeklemek, `config.json` dosyanızı ve özel anahtarlarınızı içerdiği için tüm güvenlik duvarı durumunuzu korumak için yeterlidir.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>REST API</h3>
    <p>Yapılandırma yönetimini API üzerinden otomatikleştirin.</p>
    <a href="/tr/resources/documentation/rest-api/" class="card-link">API Referansı →</a>
  </div>
  <div class="card">
    <h3>Kuruluma Dön</h3>
    <p>Kurulum adımlarını gözden geçirin.</p>
    <a href="/tr/resources/documentation/building-from-source/" class="card-link">Rehberi Okuyun →</a>
  </div>
</div>
