---
title: "Sürüm Notları"
description: "Stratum FW'nin sürüm geçmişi - her sürümde yeni özellikler, değişiklikler ve düzeltmeler."
---



Stratum FW'deki tüm önemli değişiklikler burada, en yeniden eskiye doğru listelenmiştir. Stratum FW kendini imzalı güncelleme sunucusundan (Sistem > Güncelleme) veya çevrimdışı bir güncelleme dosyasından (`.sfu`) günceller; her güncelleme bir ZFS önyükleme ortamı oluşturur, böylece önceki sürüm her zaman tekrar önyüklenebilir.



## 0.12.2 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar > Güvenlik Duvarı ve NAT**: Engellenen bağlantılar sessizce düşürülür veya reddedilir; varsayılan bloğun ve yayın gürültüsünün günlüğe kaydedilmesi. Bogon ve özel ağ engelleme, WAN adresinde ping. Durum tablosu boyutu, boşta bağlantı zaman aşımları, SYN sel koruması. Paket normalleştirme. **NAT yansıması**. Otomatik giden NAT için statik kaynak portları.

### Değişen
- Panelin kendi başına kullandığı ayarlar artık "Değişiklikleri uygula" çubuğunu göstermiyor.

## 0.12.1 - 2026-11-03

### Düzeltilen
- Uygula'ya tıklamak aynı uygulamayı iki kez başlatabiliyordu. Çubuk artık işlem bitene kadar meşgul kalıyor.

## 0.12.0 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar** sekmelerle, Sistem > Genel'in yerini aldı.
- **Genel**: ana bilgisayar adı, yerel alan adı, saat dilimi ve giriş bildirimi.
- **Yönetici Erişimi**: Web paneli portu, oturum zaman aşımı, güvenlik duvarını kimlerin yönetebileceği, WAN'dan erişim, giriş koruması, SSH ve anahtarlar.

## 0.11.0 - 2026-11-03

### Eklenen
- **Sertifikalar** (Sistem > Sertifikalar): web panelinin TLS sertifikasını yönetin.
- CA tarafından verilen panel sertifikasının süresi 21 gün içinde dolduğunda yöneticiler için uyarı.

## 0.10.0 - 2026-11-03

### Güvenlik
- **IPv6 güvenlik temeli**: IPv6 henüz desteklenmiyor ve artık kapalı kalması garanti ediliyor.

## 0.9.0 - 2026-11-03

### Eklenen
- **IP karalisteleri** (Güvenlik Duvarı > Karalisteler > IP): bilinen kötü adresleri pf ile engelleyin. pfBlockerNG'den türetilmiş 63 bakımlı IPv4 akışı kataloğu. Saatlik güncellemeler.
- **Kurulum medyası iki varyantta**: `stratum-fw-v0.9.0-vga.iso` ve `stratum-fw-v0.9.0-serial.iso`.

## 0.8.0 - 2026-11-03

### Eklenen
- **WireGuard** (VPN > WireGuard): FreeBSD'ye yerleşik WireGuard ile uzaktan erişim.
  - Sunucu ayarları: port, dinleme WAN'ı, tünel ağı, MTU, harici adres.
  - Eşler (cihaz başına bir tane) güvenlik duvarında oluşturulan anahtar çiftleri ve ön paylaşılan anahtarlar.
  - Eş başına canlı durum: bağlı, uç nokta, son el sıkışma, alınan ve gönderilen veriler.

## 0.7.0 - 2026-11-01

### Eklenen
- **OpenVPN uzaktan erişim** (VPN > OpenVPN).
  - Yerleşik sertifika otoritesi (eliptik eğri anahtarları, iptal listesi, tls-crypt).
  - Kullanıcı başına `.ovpn` profilleri.

## 0.6.0 - 2026-10-31

### Eklenen
- **Kullanıcılar ve roller** (Sistem > Kullanıcılar): yönetici / operatör / salt okunur roller.
- **Yedekleme ve geri yükleme**.
- **İmzalı yazılım güncellemeleri**.

## 0.5.0 - 2026-10-30

### Eklenen
- **Güvenlik Duvarı Günlüğü**, **Sistem Günlükleri**, **Ping, Traceroute ve DNS araması**.
- **Her arayüz için trafik grafikleri**.

## 0.4.0 - 2026-10-30

### Eklenen
- **DHCP Sunucusu**, **DNS Çözümleyici**, **NTP**.

## 0.3.0 - 2026-10-30

### Eklenen
- **Arayüz atamaları** (WAN, LAN, OPT).
- **PPPoE** ve **VLAN'lar** (802.1Q).

## 0.2.0 - 2026-10-30

### Eklenen
- **Güvenlik duvarı kuralları**, **Takma adlar**, **NAT**.
- **Güvenli uygulama**: otomatik geri alma.

## 0.1.0 - 2026-10-29

İlk sürüm.
- Web panelli (HTTPS) ve panelli FreeBSD tabanlı güvenlik duvarı.
- Konsol menüsü ve ilk önyükleme kurulum sihirbazı.
- Kurulum ISO'su: BIOS ve UEFI için GPT, ZFS, tamamen çevrimdışı kurulum.