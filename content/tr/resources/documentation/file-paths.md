---
title: "Dosya ve Yol Referansı"
description: "Stratum Firewall yapılandırma dosyaları, izole gizli anahtarlar, üretilen FreeBSD servis dosyaları, günlükler ve ZFS önyükleme ortamları referansı."
lead: "Şeffaf mimari — tekil yapılandırma dosyasının, kriptografik anahtarların, üretilen FreeBSD servis ayarlarının ve günlüklerin nasıl organize edildiğini inceleyin."
---

Stratum Firewall ayarlarınızı kapalı ikili (binary) veritabanlarına hapsetmez. Tüm sistem niyeti denetlenebilir tek bir JSON dosyasında tutulurken, hassas anahtarlar ve parolalar ZFS havuzu üzerinde yalnızca `root` erişimli ayrı dosyalarda izole edilir.

## 1. Tek Doğruluk Kaynağı ve İzole Gizli Anahtarlar

| Konum / Bileşen | Açıklama |
| :--- | :--- |
| `config.json` | Arayüzler, kurallar, NAT, takma adlar, servisler ve sistem ayarları için tek doğruluk kaynağıdır; değişiklik geçmişi ve fark (diff) karşılaştırmasıyla saklanır. |
| **`root`-Özel Gizli Dosyalar (`0600`)** | Güvenlik gereği `config.json` dışında tutulur: kullanıcı parola özetleri, WireGuard özel/PSK anahtarları, OpenVPN CA/sunucu/kullanıcı anahtarları, PPPoE şifreleri, Telegram/SMTP parolaları, SNMP topluluk adı, SFTP yedek şifresi ve özetlenmiş MCP anahtarları. Şifreli `.stbk` yedeği alındığında pakete dahil edilebilir. |

## 2. Üretilen FreeBSD Yapılandırma Dosyaları (`stratum-config`)

Panelde **Uygula (Apply)** tıklandığında sistem dosyaları `config.json` üzerinden otomatik üretilir ve ön kontrolden geçirilir (üzerine yazılacağı için bu dosyaları elle düzenlemeyin):

| Hedef Yapılandırma | Alt Sistem / Servis | Notlar |
| :--- | :--- | :--- |
| `/etc/pf.conf` ve tablo dosyaları | FreeBSD `pf` &amp; `dummynet` | Güvenlik duvarı kuralları, NAT, politika yönlendirme, hız sınırı boruları, IP tehdit listeleri ve GeoIP/Hostname tabloları. |
| `/etc/rc.conf` | FreeBSD `rc(8)` | Arayüzler, VLAN'lar ve servis başlangıçları (Sanal IP'ler ve statik rotalar `rc.conf`'u kirletmeden `stratum-netextra` tarafından yönetilir). |
| `/etc/sysctl.conf.local` | Çekirdek Ayarları (`Tunables`) | **Sistem → Ayarlar → Tunables** ekranından yönetilir; önceki dosya `.stratum-orig` olarak korunur. |
| Unbound &amp; `dnsmasq` | DNS, DNSBL &amp; DHCP | DNSSEC kök çapası, RPZ DNSBL bölgeleri, 55 kategorili Web Filtresi ve DHCP rezervasyonları. |
| Suricata (`netmap` / `pcap`) | Saldırı Tespit / IPS | İzleme ve engelleme süreçleri ile ET Open kural kategorileri. |
| OpenVPN &amp; `wg0` | Uzaktan Erişim VPN | Sunucu yapılandırması, iptal listesi (`CRL`), `multihome` ve çekirdek `if_wg` eşleri. |
| `nginx` &amp; `sshd` | Yönetim Katmanı | Uygulanmadan önce her zaman `nginx -t` ve `sshd -t` ile doğrulanır. |

## 3. Günlükler, Uzak Syslog ve ZFS Önyükleme Ortamları

* **Sistem ve Güvenlik Günlükleri (`/var/log/`):** **Sistem → Ayarlar → Günlükleme (`Logging`)** altından dosya boyutu, saklama adedi ve en fazla 2 sunucuya kadar **Uzak Syslog (`Remote Syslog`)** aktarımıyla yönetilir.
* **Engelleme ve Web Etkinliği Veritabanları:** Cihaz üzerinde yalnızca `root` erişimli SQLite veritabanlarında saklanır (Birleşik Engelleme Raporları için `7 gün`, Web Etkinliği için `1–730 gün`, `.pcap` paket yakalama dosyaları için en fazla `24 saat`).
* **ZFS Önyükleme Ortamları (`zroot/ROOT/*`):** Her çevrimiçi veya çevrimdışı (`.sfu`) güncellemede aktif kök veri kümesi önyüklenebilir yeni bir ZFS ortamına kopyalanır ve en yeni 5 ortam otomatik korunur.
