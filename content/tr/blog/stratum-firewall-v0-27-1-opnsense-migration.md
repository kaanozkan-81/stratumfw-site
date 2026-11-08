---
title: "Stratum Firewall v0.27.1: Kesintisiz OPNsense ve OpenVPN Göçü, Sanal IP'ler, Ülke Takma Adları ve Tanılama Araçları"
date: 2026-11-08
author: "Stratum Security"
description: "Stratum Firewall v0.27.1 sürümüyle OPNsense yapılandırmalarını ve OpenVPN sunucusunu mevcut istemci profillerini bozmadan içe aktarın; Sanal IP (VIP), Statik Rota, Ülke (GeoIP) takma adları ve Wireshark paket yakalama özelliklerini keşfedin."
lead: "v0.24.3'ten v0.27.1'e uzanan güncelleme serisiyle Stratum Firewall; kesintisiz OPNsense ve OpenVPN göçü, Sanal IP yönetimi, lisans gerektirmeyen ülke bazlı filtreleme ve gelişmiş ağ tanılama araçlarına kavuştu."
tags: ["stratum firewall", "stratum fw", "freebsd", "opnsense", "openvpn", "sanal ip", "geoip", "suricata"]
---

[Stratum Firewall TR](https://stratumfw.com/tr/) için yayınladığımız **v0.27.1** sürümü, mevcut bir güvenlik duvarından geçiş sürecini zahmetsiz hale getiren ve günlük ağ operasyonlarını hızlandıran kritik yetenekleri bir araya getiriyor. [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) sayfasında detaylandırılan `v0.24.3` ile `v0.27.1` arasındaki geliştirmeler; OPNsense üzerinden tam kapsamlı yapılandırma ve OpenVPN göçünden Sanal IP (Virtual IP) yönetimine, ülke bazlı `pf` tablolarından Wireshark paket yakalama araçlarına kadar uzanıyor.

### 1. Kesintisiz OPNsense ve OpenVPN Göçü (v0.25.0 & v0.27.1)

Bir güvenlik duvarını değiştirmenin en zor kısmı, yıllar içinde birikmiş kuralları ve son kullanıcıların cihazlarındaki VPN profillerini yeniden dağıtmaktır. **Sistem > Yedekleme > OPNsense'den İçe Aktar** aracı, bir OPNsense `config.xml` dosyasını okuyarak [Stratum Firewall TR](https://stratumfw.com/tr/) üzerinde bekleyen yapılandırmaya dönüştürür:

* **Kapsamlı Kural ve Ağ Taşıma:** Arayüzler, VLAN'lar, ağ geçitleri, arayüz grupları, iç içe geçmiş takma adlar (aliases), güvenlik duvarı kuralları, erişim sınırları korunmuş port yönlendirmeleri, giden NAT, DHCP rezervasyonları (`dnsmasq` ve `ISC dhcpd`) ve DNS geçersiz kılmaları eksiksiz aktarılır ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).

* **Kaydetmeden Önce Önizleme ve Raporlama:** Herhangi bir değişiklik uygulanmadan önce kaynak cihazdaki ağ portlarını mevcut donanımınızın portlarıyla eşleştirebilir; nelerin aktarıldığını veya LAN/WAN çakışmalarını detaylı uyumluluk raporunda görebilirsiniz ([Mimari](https://stratumfw.com/tr/product/architecture/)).

* **OpenVPN Sunucusunu Olduğu Gibi Devralma (`v0.27.1`):** *"OpenVPN'i de içe aktar"* seçeneği işaretlendiğinde; Sertifika Otoritesi (CA), sunucu sertifikası, `tls-crypt` / `tls-auth` anahtarı, tüm kullanıcı sertifikaları ve kullanıcıların mevcut OPNsense parola özetleri (PHP `$2y$` bcrypt ve `argon2id` dahil) devralınır. Böylece kullanıcıların bilgisayarlarındaki veya telefonlarındaki mevcut `.ovpn` profilleri **hiçbir değişiklik yapılmadan** doğrudan yeni güvenlik duvarına bağlanmaya devam eder ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).

* **Çift WAN OpenVPN ve İstemci Güvenliği:** İki WAN bağlantılı sistemlerde ikinci WAN'a gelen OpenVPN bağlantılarının aynı WAN üzerinden yanıtlanması (`multihome` ve `pf` yönlendirmesi), çoklu `remote` adres satırları ve Windows istemcileri için `block-outside-dns` desteği eklendi ([Özellikler](https://stratumfw.com/tr/product/features/)).

### 2. Sanal IP'ler (Virtual IPs) ve Statik Rotalar (v0.27.0)

Birden fazla genel (public) IP adresine sahip kurumsal hatlar ve çoklu yönlendirici bulunan topolojiler için **Arayüzler > Sanal IP'ler** ve **Arayüzler > Rotalar** modülleri devreye alındı ([Özellikler](https://stratumfw.com/tr/product/features/)):

* **Esnek Sanal IP (VIP) Yönetimi:** `/29` gibi bir WAN bloğundaki ek genel IP adreslerini tek bir arayüze tanımlayabilir, her genel IP adresini farklı bir iç sunucuya port yönlendirebilir (NAT yansıması dahil) ve giden NAT (Outbound NAT) kurallarında kaynak adresi olarak seçebilirsiniz ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).

* **Statik Rotalar ve Otomatik NAT:** Dahili bir arayüzün arkasındaki başka bir yönlendirici üzerinden ulaşılan alt ağlara statik rotalar tanımlayabilirsiniz; bu ağlar tıpkı doğrudan bağlı arayüz ağları gibi internete otomatik olarak NAT'lanır.

* **`stratum-netextra` ile Temiz Durum Yönetimi:** Sanal IP'ler ve statik rotalar FreeBSD `rc.conf` dosyasını kirletmez; `stratum-netextra` servisi açılışta, her Uygula (Apply) ve geri alma (rollback) işleminde ve arayüz yeniden başlatmalarında yalnızca eksik kayıtları ekleyip kaldırarak çekirdek durumunu yapılandırmayla senkronize tutar ([Mimari](https://stratumfw.com/tr/product/architecture/)).

### 3. Dinamik Hostname ve Ücretsiz Ülke (GeoIP) Takma Adları (v0.26.0 & v0.26.1)

**Güvenlik Duvarı > Takma Adlar (Aliases)** bölümü, IP ve port listelerinin ötesine geçerek iki yeni dinamik `pf` tablo türüne kavuştu ([Özellikler](https://stratumfw.com/tr/product/features/)):

* **Ana Bilgisayar Adı (Hostname) Takma Adları:** `update.example.com` gibi alan adlarını içeren takma adlar her 5 dakikada bir güvenlik duvarının kendi DNS çözümleyicisi üzerinden sorgulanır. CDN arkasındaki servislerin IP dönüşümlerinde bağlantı kopmaması için çözümlenen adresler son görüldükleri andan itibaren 1 saat boyunca tabloda tutulur ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).

* **Lisans Anahtarı Gerektirmeyen Ülke (GeoIP) Takma Adları:** Beş bölgesel internet kayıt kuruluşundan (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC) haftalık olarak indirilen ülke IP bloklarıyla *"bu port yönlendirmeye yalnızca Türkiye erişebilsin"* veya *"şu ülkelerden gelen trafiği engelle"* gibi kuralları saniyeler içinde oluşturabilirsiniz. Her iki takma ad türü de `pf` çalışırken Uygula (Apply) gerektirmeden canlı olarak güncellenir.

### 4. Paket Yakalama (.pcap), ARP Tablosu, Wake-on-LAN ve Güç Yönetimi (v0.25.1)

Ağ yöneticilerinin sorun giderme süreçlerini hızlandırmak için **Tanılama (Diagnostics)** ve **Sistem (System)** menülerine yeni araçlar eklendi ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)):

* **Wireshark için Paket Yakalama (`Diagnostics > Packet Capture`):** Seçilen arayüzdeki trafiği protokol, IP/ağ ve port filtreleriyle `.pcap` dosyasına kaydedin. Yöneticinin kendi web paneli ve SSH trafiği varsayılan olarak hariç tutulur; böylece temiz bir analiz çıktısı elde edilir.

* **Gelişmiş ARP Tablosu ve Wake-on-LAN (`Diagnostics > ARP Table`):** Ağdaki tüm cihazları MAC adresi, arayüz, DHCP/DNS ismi ve telefon/dizüstü bilgisayarların rastgele (gizli) MAC işaretiyle görüntüleyin; tek tıkla **Wake-on-LAN** paketi göndererek uyku modundaki cihazları uyandırın.

* **Uzaktan Güvenli Fabrika Sıfırlaması (`System > Power`):** Uzaktaki bir güvenlik duvarını fabrika ayarlarına sıfırlarken o anda kullanımda olan fiziksel port atamaları korunur (WAN DHCP, LAN `192.168.1.1/24` DHCP açık); böylece cihaz sıfırlama sonrasında da erişilebilir kalır.

### 5. Arayüz Bazlı Çift Motorlu Suricata IPS/IDS (v0.24.3 & v0.24.4)

**Servisler > Saldırı Tespit Sistemi (Intrusion Detection)** altında pfSense'de olduğu gibi her arayüz için bağımsız **İzle (Watch)** veya **Engelle (Block)** modu seçilebilir ([Özellikler](https://stratumfw.com/tr/product/features/)). Örneğin WAN arayüzünü yalnızca uyarı üretecek şekilde izlerken yerel VLAN ağlarında aktif engelleme (IPS) çalıştırabilirsiniz; sistem bu durumda engelleme yapılan portlar için `netmap`, izleme portları için `pcap` tabanlı iki ayrı denetimli Suricata sürecini otomatik yönetir ve kural kategorisi değişikliklerini FreeBSD `SIGUSR2` sinyaliyle kesintisiz uygular ([Sürüm Notları](https://stratumfw.com/tr/resources/changelog/)).

### Hemen Güncelleyin veya İndirin

Mevcut bir [Stratum Firewall TR](https://stratumfw.com/tr/) kurulumunuz varsa ISO dosyasını yeniden indirmenize gerek kalmadan doğrudan yönetim panelinizdeki **Sistem > Güncelleme** menüsünden imzalı `v0.27.1` güncellemesini (otomatik ZFS önyükleme ortamı yedeğiyle) uygulayabilirsiniz. Yeni kurulumlar için [İndir](https://stratumfw.com/tr/download/) sayfasından güncel VGA veya Seri konsol kurulum imajlarını edinebilirsiniz.
