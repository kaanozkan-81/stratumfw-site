---
title: "Sürüm Notları"
description: "Stratum FW'nin sürüm geçmişi - her sürümde yeni özellikler, değişiklikler ve düzeltmeler."
---

Stratum FW'deki tüm önemli değişiklikler burada, en yeniden eskiye doğru listelenmiştir. Stratum FW kendini imzalı güncelleme sunucusundan (Sistem > Güncelleme) veya çevrimdışı bir güncelleme dosyasından (`.sfu`) günceller; her güncelleme bir ZFS önyükleme ortamı oluşturur, böylece önceki sürüm her zaman tekrar önyüklenebilir.

## 0.13.0 - 2026-11-04

### Değişen
- **Yeni pano**:
  - Durum şeridi: ana bilgisayar adı, sürüm ve güncelleme durumu, her WAN bağlantısı adresi ve canlı indirme / yükleme hızıyla, çalışma süresi, son yapılandırma değişikliğinin zamanı, güvenlik duvarı durumu.
  - Temel rakamlar: 5 dakikalık eğriye sahip CPU, bellek, disk, sıcaklık, durum tablosu sınırına karşı bağlantılar ve son bir saatte engellenen paketler (eğriyle); renkler uyarı seviyelerinde değişir.
  - Her arayüz için ortalama ve tepe değerlerle son bir saatin trafiği.
  - Koruma: IP ve DNS karalisteleri, IP karalisteleri tarafından durdurulan paketler, başarısız girişlerden sonra engellenen adresler, çevrimiçi OpenVPN kullanıcıları ve WireGuard eşleri, son bir saatte en çok engelleyen kurallar.
  - Arayüzler (adı, durumu, bağlantı hızı, adresi ve canlı trafiği ile); çalışan / durdurulan özeti olan servisler.
  - Son bir saatin en çok engellenen kaynakları ve portları ile en son yapılandırma değişiklikleri (uygulandı, tutuldu, geri alındı, başarısız oldu).
  - Koyu ve açık temalarda ve telefonlarda çalışır.

## 0.12.6 - 2026-11-04

### Eklenen
- **Sistem > Ayarlar > Ağ**: her fiziksel portta donanım hızlandırmayı (LRO, TSO, checksum) kapatın - portları yeniden başlatmadan canlı olarak ayarlanır ve yeniden başlatmadan sonra korunur - her port için mevcut durum ile; CPU güç yönetimi (uyarlanabilir, maksimum performans, ekonomi); ARP günlüğü ve ARP giriş ömrü; IPv6 durumu.
- **Sistem > Ayarlar > Ayarlanabilirler (Tunables)**: güvenlik duvarında önemli olan çalışma zamanı sistem değerleri (sysctl), pfSense'in öneri olarak kullandığı değerler ve her birinin mevcut değeri, ayrıca kendi çalışma zamanı tunable'larınız. Değerler Uygula ile ayarlanır ve yeniden başlatmadan sonra korunur (/etc/sysctl.conf.local).
- Tüm Sistem > Ayarlar sekmeleri artık kullanılabilir.

### Değişen
- Mevcut bir /etc/sysctl.conf.local dosyası /etc/sysctl.conf.local.stratum-orig olarak saklanır; dosya artık Ayarlar > Tunables'tan yazılır.

## 0.12.5 - 2026-11-04

### Eklenen
- **Zamanlanmış otomatik karaliste güncellemeleri** (Güvenlik Duvarı > Karalisteler, IP ve DNSBL): her saat, her 6 veya 12 saatte bir, seçilen bir saatte günlük, seçilen bir günde haftalık veya kapalı. Sayfa bir sonraki otomatik güncellemeyi gösterir. Kaçırılan bir güncelleme (güvenlik duvarı kapatıldığında) bir sonraki saat çalışır ve başarısız bir güncellemeden sonra bir sonraki saat tekrar dener. Zamanlama değişiklikleri Uygula olmadan anında geçerli olur. Varsayılanlar önceki ritmi korur (IP listeleri saatlik, DNS listeleri günlük 03:00'te).

### Düzeltilen
- IP karalistelerinin güncelleme günlüğü bir güncelleme çalışırken gizli kalıyordu (sayfadaki iki öğe aynı kimliği paylaşıyordu).

## 0.12.4 - 2026-11-04

### Eklenen
- **Sistem > Ayarlar > Bildirimler**: Güvenlik duvarından **Telegram** (bot) ve **e-posta** (STARTTLS veya TLS ile SMTP) ile mesajlar; her biri sayfadaki değerleri kullanan bir test düğmesine sahiptir.
  - Kapatılabilen olaylar: yazılım güncellemesi mevcut, yapılandırmanın uygulanması başarısız oldu, zamanında onaylanmadığı için değişiklikler otomatik olarak geri alındı, WAN bağlantısı kesildi / geri geldi / yeni adres, başarısız girişlerden sonra bir adres engellendi, panel sertifikasının süresi doluyor, IP veya DNS karaliste indirmeleri başarısız, disk neredeyse dolu ve CPU sıcaklığı yüksek (ayarlanabilir eşikler).
  - Başarısız uygulamalar, otomatik geri almalar ve engellenen girişler anında bildirilir; geri kalanı her 5 dakikada bir kontrol edilir. Aynı sorun yalnızca bir aradan sonra tekrarlanır.
  - Bot token'ı ve posta şifresi yalnızca güvenlik duvarında (yalnızca root dosyaları) saklanır ve bir daha asla gösterilmez. Bildirim ayarları Uygula olmadan anında geçerli olur.

## 0.12.3 - 2026-11-04

### Eklenen
- **Sistem > Ayarlar > Günlükleme**:
  - Güvenlik duvarı günlüğü rotasyonu: dosya boyutu ve saklanan eski dosya sayısı; güvenlik duvarı, sistem, DHCP/DNS ve giriş günlüklerinin mevcut boyutu ve günlükler için boş alan.
  - **Uzak syslog**: en fazla iki sunucu (örneğin bir NAS), her biri kendi günlük seçimine sahip: sistem, girişler (panel, SSH, VPN), güvenlik duvarı günlüğü (paket başına okunabilir bir satır, `filterlog` etiketi) ve DHCP/DNS. Sistemin syslogd'si tarafından UDP üzerinden gönderilir; yerel günlükler olduğu gibi kalır.
  - **Test mesajı gönder**, herhangi bir şey uygulanmadan önce bir sunucunun syslog aldığını kontrol eder.

### Değişen
- Güvenlik duvarı ve DHCP/DNS günlüklerinin rotasyonu ayarlardan oluşturulur (varsayılanlar değişmez).
- Yazılım güncellemeleri güvenlik duvarı günlüğünü ve trafik toplayıcılarını yeniden başlatarak yeni sürümü anında çalıştırır.

## 0.12.2 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar > Güvenlik Duvarı ve NAT**: Engellenen bağlantılar sessizce düşürülür veya reddedilir; varsayılan bloğun ve yayın gürültüsünün günlüğe kaydedilmesi. Bogon ve özel ağ engelleme, WAN adresinde ping. Kurulu bellek ve mevcut kullanım için bir öneriyle durum tablosu boyutu, boşta bağlantı zaman aşımları, SYN sel koruması. Paket normalleştirme: rastgele IP kimliği, parçalanma bayrağını temizleme. **NAT yansıması**: yerel cihazlar genel adres üzerinden bir port yönlendirmesine ulaşır. Otomatik giden NAT için statik kaynak portları. Varsayılanlar, önceki sürümlerin oluşturulan pf yapılandırmasını değiştirmez.

### Değişen
- Panelin kendi başına kullandığı ayarlar artık "Değişiklikleri uygula" çubuğunu göstermiyor ve Vazgeç bunları tutuyor.

## 0.12.1 - 2026-11-03

### Düzeltilen
- Uygula'ya tıklamak aynı uygulamayı iki kez başlatabiliyordu: uygulama çubuğunun periyodik yenilemesi, ilk uygulama hala çalışırken etkin bir Uygula düğmesini yeniden çiziyordu. Çubuk artık işlem bitene kadar meşgul kalıyor ve aynı anda yalnızca bir uygulama, onaylama veya geri alma çalışabiliyor.
- Yönetim portundaki (örneğin 443) bir WAN port yönlendirmesi artık herhangi bir yapılandırmanın kaydedilmesini engellemiyor.

## 0.12.0 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar** sekmelerle, Sistem > Genel'in yerini aldı.
- **Genel**: ana bilgisayar adı, yerel alan adı, saat dilimi ve giriş sayfasında gösterilen bir giriş bildirimi.
- **Yönetici Erişimi**: Web paneli portu, oturum zaman aşımı. Güvenlik duvarını kimlerin yönetebileceği. WAN'dan erişim, izin verilen kaynak adreslerinin listesiyle. Giriş koruması. SSH: açık veya kapalı, port, parola veya yalnızca anahtarla giriş. Yeni ayarlar bağlı olduğunuz adresi kilitlerse kaydetme reddedilir.
- Konsol menüsü 6) **Yönetim erişimi**: mevcut ayarları gösterir, WAN erişimini açar veya kapatır.

### Değişen
- Anti-lockout ve WAN yönetim kuralları, yapılandırılmış panel ve SSH portlarını takip eder.

## 0.11.0 - 2026-11-03

### Eklenen
- **Sertifikalar** (Sistem > Sertifikalar): web panelinin TLS sertifikasını yönetin. Aktif sertifikayı görüntüleyin. Kendi sertifikanızı yükleyin. Yeni bir kendinden imzalı sertifika oluşturun. Önceki sertifika saklanır ve geri yüklenebilir.
- CA tarafından verilen panel sertifikasının süresi 21 gün içinde dolduğunda yöneticiler için uyarı.
- `stratum-cert` komutu.

## 0.10.0 - 2026-11-03

### Güvenlik
- **IPv6 güvenlik temeli**: IPv6 henüz desteklenmiyor ve artık kapalı kalması garanti ediliyor — her IPv6 paketi herhangi bir geçiş kuralından önce düşürülür.

### Eklenen
- IPv6 tasarım ve sürüm planı.

## 0.9.1 - 2026-11-03

### Değişen
- Bir Uygula başarısız olduğunda, uygulama çubuğu artık nedenini söylüyor.
- Güncel bir sistemde `stratum-update run` bir hata yerine "kurulacak bir şey yok" bildirir.

## 0.9.0 - 2026-11-03

### Eklenen
- **IP karalisteleri** (Güvenlik Duvarı > Karalisteler > IP): bilinen kötü adresleri pf ile engelleyin.
  - pfBlockerNG'den türetilmiş 63 bakımlı IPv4 akışı kataloğu.
  - Yön: internetten gelen, yerel ağlardan giden veya her ikisi.
  - Kendi izin ve engelleme listeleri ve özel liste URL'leri.
  - Saatlik güncellemeler Uygula olmadan, canlı ilerleme çubuğu.
  - Güvenlik: özel ve ayrılmış aralıklar ve /8'den büyük ağlar asla engellenmez.
- **Kurulum medyası iki varyantta**.

## 0.8.0 - 2026-11-03

### Eklenen
- **WireGuard** (VPN > WireGuard): FreeBSD'ye yerleşik WireGuard ile uzaktan erişim.
  - Sunucu ayarları: port, dinleme WAN'ı, tünel ağı, MTU, harici adres.
  - Eşlerin hangi LAN ve VLAN'lara erişebileceğini tam olarak seçin.
  - Güvenlik duvarında oluşturulan anahtar çiftleri ve ön paylaşılan anahtarlarla eşler.
  - Eş başına canlı durum.
- Özel anahtarlar yapılandırma dosyasının dışında tutulur.

## 0.7.3 - 2026-11-02

### Eklenen
- **OpenVPN gelişmiş ayarları**.
- **Kullanıcı başına sabit VPN adresi** (Sistem > Kullanıcılar).
- Bağlantı ayarları değiştiğinde panel size bildirir.

### Değişen
- OpenVPN sayfasındaki VPN erişim onay kutuları hemen etkili olur.

## 0.7.2 - 2026-11-01

### Değişen
- Sistem > Kullanıcılar: OpenVPN erişimi kendi VPN sütununda gösterilir.

## 0.7.1 - 2026-11-01

### Eklenen
- **Yalnızca VPN hesapları**.
- Kullanıcı listesi: son VPN oturum açma ve başarısız denemeler.

### Değişen
- VPN erişimini kaldırmak, bir kullanıcıyı devre dışı bırakmak veya silmek, kullanıcının sertifikasını hemen iptal eder.

## 0.7.0 - 2026-11-01

### Eklenen
- **OpenVPN uzaktan erişim** (VPN > OpenVPN).
  - Yerleşik sertifika otoritesi.
  - Kullanıcının sertifikası ve parolasıyla giriş.
  - Kullanıcı başına `.ovpn` profilleri.
  - VPN istemcileri için güvenlik duvarı ve NAT kuralları ve DNS otomatik olarak oluşturulur.

## 0.6.13 - 0.6.19 - 2026-10-31

### Eklenen
- **Unbound DNS çözümleyici** (Servisler > DNS).
- **DNS karalisteleri** (Güvenlik Duvarı > Karalisteler > DNSBL).
- **Pano**: güncelleme rozeti, canlı çalışma süresi, CPU sıcaklığı.

### Düzeltilen
- DNS karalisteleri: liste yorumlarındaki bağlantılar artık giriş olarak işlenmiyor.

## 0.6.6 - 0.6.12 - 2026-10-31

### Eklenen
- **Çevrimdışı güncellemeler**.
- Her güncellemeden sonra en yeni 5 önyükleme ortamı saklanır.

### Düzeltilen
- Önceki bir önyükleme ortamını önyükleme çalışır.

## 0.6.1 - 0.6.5 - 2026-10-31

### Eklenen
- Güncelleme ilerlemesi.
- Sistem uyarıları.

### Düzeltilen
- IP yönlendirme, uygulama ve geri almalardan sonra her zaman yapılandırmayı takip eder.

## 0.6.0 - 2026-10-31

### Eklenen
- **Kullanıcılar ve roller** (Sistem > Kullanıcılar).
- **Genel ayarlar** (Sistem > Genel).
- **Açık tema** ve sistemi takip etme modu.
- **Yedekleme ve geri yükleme**.
- **İmzalı yazılım güncellemeleri**.
- Markalı önyükleme menüsü.

## 0.5.0 - 2026-10-30

### Eklenen
- **Güvenlik Duvarı Günlüğü**.
- **Sistem Günlükleri**.
- **Ping, Traceroute ve DNS araması** ve **pf durum tablosu**.
- **Her arayüz için trafik grafikleri**.

### Değişen
- Yayın ve çoklu yayın gürültüsü günlüğü doldurmadan engellenir.

## 0.4.0 - 2026-10-30

### Eklenen
- **DHCP Sunucusu**.
- **DNS Çözümleyici**.
- **NTP**.

## 0.3.0 - 2026-10-30

### Eklenen
- **Arayüz atamaları**.
- Arayüz ayarları.
- **PPPoE**.
- **VLAN'lar** (802.1Q).

## 0.2.0 - 2026-10-30

### Eklenen
- **Güvenlik duvarı kuralları**.
- **Takma adlar**.
- **NAT**.
- **Güvenli uygulama**.

## 0.1.1 - 2026-10-30

### Değişen
- WAN'dan yönetim erişimi varsayılan olarak kapalıdır.

### Düzeltilen
- UEFI sistemlerde konsol seçimi.

## 0.1.0 - 2026-10-29

İlk sürüm.

- Web panelli (HTTPS) ve panelli FreeBSD tabanlı güvenlik duvarı.
- Konsol menüsü ve ilk önyükleme kurulum sihirbazı.
- Kurulum ISO'su.
