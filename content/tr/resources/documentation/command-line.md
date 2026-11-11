---
title: "Tanılama, Günlük Özeti ve Komut Satırı"
description: "Stratum Firewall 1s/24s Güvenlik Duvarı Günlük Özeti, Wireshark paket yakalama, ARP/Wake-on-LAN, Web Etkinliği, Stratum CLI komutları, pfctl ve ZFS bectl araçlarıyla ağ sorunlarını giderin."
lead: "Hem web panelinden hem SSH üzerinden tam hakimiyet — 1s/24s Güvenlik Duvarı Günlük Özeti grafikleri, Wireshark .pcap paket yakalama, ARP ve Wake-on-LAN, Stratum CLI ve FreeBSD araçları."
---

Stratum Firewall, günlük ağ sorunlarını komut satırına gerek kalmadan çözebileceğiniz kapsamlı web analiz ve tanılama araçlarının yanı sıra FreeBSD 15.1 üzerinde şeffaf CLI komutları sunar.

## 1. Web Paneli Tanılama Araçları (`Tanılama / Diagnostics` Menüsü)

* **Güvenlik Duvarı Günlüğü ve 1s/24s Özet (`Diagnostics → Firewall Log → Summary`, `v0.28.8`):**
  * Canlı günlükte sistem kuralları iç etiketler yerine anlaşılır isimlerle (*"IP blocklist: inbound (WAN)"*, *"Block bogon networks"*, *"Broadcast and multicast"*) gösterilir.
  * **Özet (`Summary`)** sekmesi son **1 saati** veya **24 saati** (döndürülmüş eski günlük dosyaları dahil) tarayarak **en çok trafik üreten kaynak IP'leri** (ülkesiyle), **en çok hedeflenen portları** (`SSH`, `RDP` gibi servis adıyla), **en çok görülen ülkeleri (GeoIP)**, **en çok eşleşen kuralları** ve **arayüz başına 5 dakikalık / saatlik grafikleri** sunar; herhangi bir IP veya porta tıklandığında canlı günlük doğrudan o filtreyle açılır.
* **Paket Yakalama (`Diagnostics → Packet Capture`, `v0.25.1`):** İstediğiniz arayüzdeki trafiği protokol, IP/alt ağ ve port alanlarıyla filtreleyip **Wireshark** için `.pcap` dosyası olarak indirin.
* **ARP Tablosu ve Wake-on-LAN (`Diagnostics → ARP Table`, `v0.25.1`):** Ağdaki tüm cihazları MAC adresi, DHCP/DNS ismi ve rastgele (mobil) MAC işaretiyle görün; tek tıkla **Wake-on-LAN** paketi gönderin.
* **Web Etkinliği ve DHCP Geçmişi (`Diagnostics → Web Activity`, `v0.20.0`):** İsteğe bağlı cihaz bazlı alan adı sorgu denetimi ve CSV dışa aktarımlı DHCP/ARP geçmişi. (Hiçbir gezinme kaydı tutmadan DNS filtre aşımını önlemek için `Servisler → DNS Çözümleyici` altındaki [Zorunlu DNS](/tr/resources/documentation/blocklists/) özelliğini kullanabilirsiniz.)
* **Aktif `pf` Durum Tablosu ve Ağ Araçları:** Aktif bağlantıları (`states`) filtreleyip sonlandırın ve Ping / Traceroute / DNS testleri çalıştırın.

## 2. Yerleşik Stratum Komut Satırı Araçları

SSH üzerinden veya konsol menüsündeki **`8) Shell`** seçeneğinden erişebileceğiniz yönetim komutları:

| Komut | Görevi |
| :--- | :--- |
| `stratum-menu` | Konsol kurulum ve kurtarma menüsünü açar — arayüz atama, IP verme, `admin` parola sıfırlama **ve telefon kaybında TOTP iki aşamalı girişi (2FA) kapatma (`v0.30.0`)** seçeneklerini içerir. |
| `stratum-config` | Merkezi yapılandırmayı doğrular, ön kontrollerden geçirir ve uygular veya geri alır. |
| `stratum-pfrefresh` | [Kural Zamanlamaları](/tr/resources/documentation/pf-rules/) için dakikada bir, WAN hat geçişi ve DHCP WAN ağ geçidi değişimlerinde ise anında çalışan hafif `pf.conf` yenileme aracıdır (`v0.29.0` – `v0.29.1`); Uygula (Apply) gerektirmeden kuralları günceller. |
| `stratum-update` | İmzalı güncellemeleri denetler ve ZFS Önyükleme Ortamı yedeği alarak kurar (`stratum-update run`). |
| `stratum-netextra` | Sanal IP'leri (VIP) ve statik rotaları çekirdek durumuyla senkronize eder. |
| `stratum-cert` | Web paneli TLS sertifikasını görüntüler, yeniler, yükler veya bir önceki sertifikayı geri yükler (`restore`). |
| `stratum-ipbl` / `stratum-dnsbl` | IP engelleme tablolarını ve Unbound RPZ DNS listelerini anında günceller. |

## 3. Temel FreeBSD 15.1 Komutları (`pfctl` ve `bectl`)

```bash
# Aktif pf filtre, 1:1 binat / NAT kurallarini, istatistikleri ve tablo boyutunu inceleyin
pfctl -sr
pfctl -sn
pfctl -si

# Her guncelleme oncesi otomatik olusturulan ZFS Onyukleme Ortamlarini listeleyin
bectl list
zfs list
```
