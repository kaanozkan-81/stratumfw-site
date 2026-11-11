---
title: "Tanılama ve Komut Satırı"
description: "Stratum Firewall yerleşik Wireshark paket yakalama, ARP/Wake-on-LAN, Web Etkinliği, Stratum CLI komutları, pfctl ve ZFS bectl araçlarıyla ağ sorunlarını giderin."
lead: "Hem web panelinden hem SSH üzerinden tam hakimiyet — Wireshark .pcap paket yakalama, ARP ve Wake-on-LAN, Web Etkinliği, Stratum CLI ve FreeBSD araçları."
---

Stratum Firewall, günlük ağ sorunlarını komut satırına gerek kalmadan çözebileceğiniz kapsamlı web tanılama araçlarının yanı sıra FreeBSD 15.1 üzerinde şeffaf CLI komutları sunar.

## 1. Web Paneli Tanılama Araçları (`Tanılama / Diagnostics` Menüsü)

* **Paket Yakalama (`Diagnostics → Packet Capture`, `v0.25.1`):** İstediğiniz arayüzdeki trafiği protokol, IP/alt ağ ve port alanlarıyla filtreleyip **Wireshark** ile incelemek üzere `.pcap` dosyası olarak indirin (kendi yönetim ve SSH trafiğiniz varsayılan olarak hariç tutulur).
* **ARP Tablosu ve Wake-on-LAN (`Diagnostics → ARP Table`, `v0.25.1`):** Ağdaki tüm cihazları MAC adresi, DHCP/DNS ismi ve rastgele (mobil) MAC işaretiyle görün; uyku modundaki bilgisayarları tek tıkla **Wake-on-LAN** ile uyandırın.
* **Web Etkinliği ve DHCP Geçmişi (`Diagnostics → Web Activity`, `v0.20.0`):** Hangi cihazın (OpenVPN kullanıcıları ve WireGuard eş isimleri dahil) hangi alan adlarına eriştiğini inceleyin, DHCP/ARP geçmişini CSV olarak dışa aktarın ve isteğe bağlı **"Tüm DNS trafiğini güvenlik duvarına yönlendir"** kuralını etkinleştirin.
* **Canlı Günlükler ve `pf` Durum Tablosu:** Eşleşen kural isimleriyle canlı güvenlik duvarı günlüklerini izleyin, aktif bağlantıları (`states`) filtreleyip sonlandırın ve Ping / Traceroute / DNS testleri çalıştırın.

## 2. Yerleşik Stratum Komut Satırı Araçları

SSH üzerinden veya konsol menüsündeki **`8) Shell`** seçeneğinden erişebileceğiniz yönetim komutları:

| Komut | Görevi |
| :--- | :--- |
| `stratum-menu` | Konsol kurulum ve kurtarma menüsünü açar (arayüz atama, IP verme, admin şifre sıfırlama, WAN erişimi). |
| `stratum-config` | Merkezi yapılandırmayı doğrular, ön kontrollerden geçirir ve uygular veya geri alır. |
| `stratum-update` | İmzalı güncellemeleri denetler ve ZFS Önyükleme Ortamı yedeği alarak kurar (`stratum-update run`). |
| `stratum-netextra` | Sanal IP'leri (VIP) ve statik rotaları çekirdek durumuyla senkronize eder. |
| `stratum-cert` | Web paneli TLS sertifikasını görüntüler, yeniler, yükler veya bir önceki sertifikayı geri yükler (`restore`). |
| `stratum-ipbl` / `stratum-dnsbl` | IP engelleme tablolarını ve Unbound RPZ DNS listelerini anında günceller. |

## 3. Temel FreeBSD 15.1 Komutları (`pfctl` ve `bectl`)

```bash
# Aktif pf filtre/NAT kurallarini, istatistikleri ve tablo boyutunu inceleyin
pfctl -sr
pfctl -sn
pfctl -si

# Her guncelleme oncesi otomatik olusturulan ZFS Onyukleme Ortamlarini listeleyin
bectl list
zfs list
```
