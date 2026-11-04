---
title: "Sürüm Notları"
description: "Stratum FW'nin sürüm geçmişi - her sürümde yeni özellikler, değişiklikler ve düzeltmeler."
---

Stratum FW'deki tüm önemli değişiklikler burada, en yeniden eskiye doğru listelenmiştir. Stratum FW kendini imzalı güncelleme sunucusundan (Sistem > Güncelleme) veya çevrimdışı bir güncelleme dosyasından (`.sfu`) günceller; her güncelleme bir ZFS önyükleme ortamı oluşturur, böylece önceki sürüm her zaman tekrar önyüklenebilir.

## 0.12.2 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar > Güvenlik Duvarı ve NAT**: Engellenen bağlantılar sessizce düşürülür veya reddedilir; varsayılan bloğun ve yayın gürültüsünün günlüğe kaydedilmesi. Bogon ve özel ağ engelleme, WAN adresinde ping. Kurulu bellek ve mevcut kullanım için bir öneriyle durum tablosu boyutu, boşta bağlantı zaman aşımları, SYN sel koruması. Paket normalleştirme: rastgele IP kimliği, parçalanma bayrağını temizleme. **NAT yansıması**: yerel cihazlar genel adres üzerinden bir port yönlendirmesine ulaşır. Otomatik giden NAT için statik kaynak portları. Varsayılanlar, önceki sürümlerin oluşturulan pf yapılandırmasını değiştirmez.

### Değişen
- Panelin kendi başına kullandığı ayarlar (oturum zaman aşımı, giriş bildirimi, giriş koruması) artık "Değişiklikleri uygula" çubuğunu göstermiyor ve Vazgeç bunları tutuyor.

## 0.12.1 - 2026-11-03

### Düzeltilen
- Uygula'ya tıklamak aynı uygulamayı iki kez başlatabiliyordu: uygulama çubuğunun periyodik yenilemesi, ilk uygulama hala çalışırken etkin bir Uygula düğmesini yeniden çiziyordu. Çubuk artık işlem bitene kadar meşgul kalıyor ve aynı anda yalnızca bir uygulama, onaylama veya geri alma çalışabiliyor.
- Yönetim portundaki (örneğin 443) bir WAN port yönlendirmesi artık herhangi bir yapılandırmanın kaydedilmesini engellemiyor.

## 0.12.0 - 2026-11-03

### Eklenen
- **Sistem > Ayarlar** sekmelerle, Sistem > Genel'in yerini aldı.
- **Genel**: ana bilgisayar adı, yerel alan adı, saat dilimi ve giriş sayfasında gösterilen bir giriş bildirimi.
- **Yönetici Erişimi**: Web paneli portu, oturum zaman aşımı. Güvenlik duvarını kimlerin yönetebileceği: tüm dahili ağlar veya yalnızca seçili arayüzler; LAN'da anti-lockout kuralı. WAN'dan erişim, izin verilen kaynak adreslerinin listesiyle. Giriş koruması: ayarlanabilir denemeler, süre ve engelleme süresi, engellenen adreslerin listesi ve Engeli Kaldır düğmesi. SSH: açık veya kapalı, port, parola veya yalnızca anahtarla giriş, root'un yetkili genel anahtarları. Yeni ayarlar bağlı olduğunuz adresi kilitlerse kaydetme reddedilir.
- Konsol menüsü 6) **Yönetim erişimi**: mevcut ayarları gösterir, WAN erişimini açar veya kapatır ve yönetim erişimini varsayılanlara sıfırlar.

### Değişen
- Anti-lockout ve WAN yönetim kuralları, yapılandırılmış panel ve SSH portlarını takip eder.
- Root'un mevcut SSH anahtarları, güncellemeden sonraki ilk kaydetmede panel tarafından devralınır.

## 0.11.0 - 2026-11-03

### Eklenen
- **Sertifikalar** (Sistem > Sertifikalar): web panelinin TLS sertifikasını yönetin. Aktif sertifikayı görüntüleyin: adlar, veren, kalan gün sayısıyla son kullanma tarihi, anahtar türü ve SHA-256 parmak izi. Kendi sertifikanızı ara sertifikalarla birlikte yükleyin. Yeni bir kendinden imzalı sertifika oluşturun. Önceki sertifika saklanır ve sayfadan veya `stratum-cert restore` ile geri yüklenebilir.
- CA tarafından verilen panel sertifikasının süresi 21 gün içinde dolduğunda yöneticiler için uyarı.
- `stratum-cert` komutu: `show`, `selfsigned`, `install`, `restore`.

## 0.10.0 - 2026-11-03

### Güvenlik
- **IPv6 güvenlik temeli**: IPv6 henüz desteklenmiyor ve artık kapalı kalması garanti ediliyor — her IPv6 paketi herhangi bir geçiş kuralından önce düşürülür, IPv6 tüm arayüzlerde devre dışı kalır ve asla iletilmez.

### Eklenen
- IPv6 tasarım ve sürüm planı (`docs/ipv6.md`).

## 0.9.1 - 2026-11-03

### Değişen
- Bir Uygula başarısız olduğunda, uygulama çubuğu artık nedenini söylüyor: her başarısız kontrol, mesajıyla birlikte, Sistem Günlükleri > Yapılandırma altında tam çıktıya bir bağlantıyla. Başarısızlıklar sistem günlüğüne de yazılır.
- Güncel bir sistemde `stratum-update run` bir hata yerine "kurulacak bir şey yok" bildirir.

## 0.9.0 - 2026-11-03

### Eklenen
- **IP karalisteleri** (Güvenlik Duvarı > Karalisteler > IP): bilinen kötü adresleri pf ile engelleyin.
  - pfBlockerNG'den türetilmiş 63 bakımlı IPv4 akışı kataloğu; Spamhaus DROP, Abuse.ch Feodo Tracker ve Emerging Threats önerilir ve önceden seçilidir.
  - Yön: internetten gelen, yerel ağlardan giden veya her ikisi.
  - Kendi izin ve engelleme listeleri ve özel liste URL'leri.
  - Saatlik güncellemeler Uygula olmadan, canlı ilerleme çubuğu ve güncelleme günlüğü, engellenen paket sayaçları ve "Bir adresi kontrol et".
  - Güvenlik: özel ve ayrılmış aralıklar ve /8'den büyük ağlar asla engellenmez; güvenlik duvarının DNS, güncelleme ve paket sunucuları her zaman izinlidir.
- **Kurulum medyası iki varyantta**: `stratum-fw-v0.9.0-vga.iso` ve `stratum-fw-v0.9.0-serial.iso`.

## 0.8.0 - 2026-11-03

### Eklenen
- **WireGuard** (VPN > WireGuard): FreeBSD'ye yerleşik WireGuard ile uzaktan erişim - ekstra paket yok.
  - Sunucu ayarları: port, dinleme WAN'ı, tünel ağı, MTU, harici adres.
  - Eşlerin hangi LAN ve VLAN'lara erişebileceğini tam olarak seçin, artı ek ağlar.
  - Güvenlik duvarında oluşturulan anahtar çiftleri ve ön paylaşılan anahtarlarla eşler: yapılandırma QR kod olarak gösterilir veya `.conf` dosyası olarak indirilir.
  - Eş başına canlı durum: bağlı, uç nokta, son el sıkışma, alınan ve gönderilen veriler.
- Özel anahtarlar yapılandırma dosyasının dışında tutulur.

## 0.7.3 - 2026-11-02

### Eklenen
- **OpenVPN gelişmiş ayarları**: UDP veya TCP, dinleme WAN'ı, VPN istemcilerinin hangi LAN ve VLAN'lara erişebileceği, istemciden istemciye trafik, kullanıcı başına birkaç cihaz, maksimum bağlantı, DNS sunucuları ve alan adları, veri şifreleri, kimlik doğrulama özeti, minimum TLS sürümü, anahtar yeniden anlaşması, keepalive, MTU/MSS, günlük seviyesi ve yalnızca sertifikayla giriş.
- **Kullanıcı başına sabit VPN adresi** (Sistem > Kullanıcılar).
- Bağlantı ayarları değiştiğinde ve kullanıcıların profillerinin yeniden indirilmesi gerektiğinde panel size bildirir.

### Değişen
- OpenVPN sayfasındaki VPN erişim onay kutuları hemen etkili olur.

## 0.7.2 - 2026-11-01

### Değişen
- Sistem > Kullanıcılar: OpenVPN erişimi kendi VPN sütununda gösterilir.

## 0.7.1 - 2026-11-01

### Eklenen
- **Yalnızca VPN hesapları**: OpenVPN erişimi olan ancak web paneli oturum açma özelliği olmayan kullanıcılar.
- Kullanıcı listesi: son VPN oturum açma ve başarısız VPN parola denemeleri; `.ovpn` profilini listeden indirin.

### Değişen
- VPN erişimini kaldırmak, bir kullanıcıyı devre dışı bırakmak veya silmek, kullanıcının sertifikasını hemen iptal eder.
- OpenVPN profilleri artık kullanımdan kaldırılmış `persist-key` seçeneğini kullanmıyor.

## 0.7.0 - 2026-11-01

### Eklenen
- **OpenVPN uzaktan erişim** (VPN > OpenVPN).
  - Yerleşik sertifika otoritesi (eliptik eğri anahtarları, iptal listesi, tls-crypt).
  - Kullanıcının sertifikası ve parolasıyla giriş.
  - Kullanıcı başına `.ovpn` profilleri, sertifika iptali, bağlı istemcilerin listesi.
  - VPN istemcileri için güvenlik duvarı ve NAT kuralları ve DNS otomatik olarak oluşturulur.

## 0.6.13 - 0.6.19 - 2026-10-31

### Eklenen
- **Unbound DNS çözümleyici** (Servisler > DNS): DNSSEC doğrulamalı, yönlendirici veya tam özyineleme modu.
- **DNS karalisteleri** (Güvenlik Duvarı > Karalisteler > DNSBL), pfBlockerNG tarzı: pfBlockerNG'den türetilmiş katalog artı HaGeZi listeleri, önerilen varsayılanlar, kendi listeleriniz, "Bir alan adı kontrol et", gece güncellemeleri ve yüzde çubuğu, geçerli adım ve canlı günlük içeren bir güncelleme ekranı.
- **Pano**: sürümün yanında güncelleme rozeti, canlı çalışma süresi, CPU sıcaklığı, PPPoE oturum adresi ve son bir saatin trafik grafiği.

### Düzeltilen
- DNS karalisteleri: liste yorumlarındaki bağlantılar artık giriş olarak işlenmiyor; liste kaynakları, güncelleme sunucusu ve paylaşılan barındırma platformları asla engellenmez.
- 0.6.15'te DNS karalisteleri etkinken yapılandırmanın uygulanması başarısız oluyordu.

## 0.6.6 - 0.6.12 - 2026-10-31

### Eklenen
- **Çevrimdışı güncellemeler**: imzalı bir güncelleme dosyası (`.sfu`) indirin ve Sistem > Güncelleme'ye yükleyin.
- Her güncellemeden sonra en yeni 5 önyükleme ortamı saklanır.

### Düzeltilen
- Önceki bir önyükleme ortamını önyükleme çalışır.
- Yeni kurulumlar sürümlerini doğru bildirir.
- Çevrimiçi kontrol internet olmadan hızlı bir şekilde başarısız olur.

## 0.6.1 - 0.6.5 - 2026-10-31

### Eklenen
- Güncelleme ilerlemesi: yüzde çubuğu, zamanlarla adım kontrol listesi, devam eden işlem bandı ve "yeniden başlatılıyor" sayfası.
- Sistem uyarıları (IP yönlendirme kapalı, güncelleme mevcut) ve parola değişikliği ile kendi hesap sayfası.

### Düzeltilen
- IP yönlendirme, uygulama, geri alma ve hizmet başlatmalarından sonra her zaman yapılandırmayı takip eder.
- Web paneli bir güncellemeden sonra kendi kendine yeniden başlar.

## 0.6.0 - 2026-10-31

### Eklenen
- **Kullanıcılar ve roller** (Sistem > Kullanıcılar): birkaç hesap, yönetici / operatör / salt okunur roller, etkinleştirme ve devre dışı bırakma, son oturum açma ve başarısız denemeler.
- **Genel ayarlar** (Sistem > Genel): ana bilgisayar adı, yerel alan adı, boşta oturum zaman aşımı.
- **Açık tema** ve sistemi takip etme modu.
- **Yedekleme ve geri yükleme** (Sistem > Yedekleme): yapılandırmayı indirin, doğrulanmış geri yükleme, yapılandırma geçmişi.
- **İmzalı yazılım güncellemeleri** (Sistem > Güncelleme): imza kontrollü depo, her güncellemeden önce yedek ve yeni bir ZFS önyükleme ortamı, canlı ilerleme, geri alma.
- Markalı önyükleme menüsü.

## 0.5.0 - 2026-10-30

### Eklenen
- **Güvenlik Duvarı Günlüğü**: filtreler ve kural adlarıyla canlı görünüm.
- **Sistem Günlükleri**: sistem, DHCP/DNS, PPPoE, NTP, kimlik doğrulama, panel ve yapılandırma günlükleri.
- **Ping, Traceroute ve DNS araması** ve **pf durum tablosu**.
- **Her arayüz için trafik grafikleri**.

### Değişen
- Yayın ve çoklu yayın gürültüsü günlüğü doldurmadan engellenir.

## 0.4.0 - 2026-10-30

### Eklenen
- **DHCP Sunucusu**: arayüz başına ayarlar, statik eşlemeler, etkin kiralama.
- **DNS Çözümleyici**: yerel alan adı, üst akış sunucuları, ana bilgisayar ve alan adı geçersiz kılmaları.
- **NTP**: zaman sunucuları, saat dilimi.
- Konsol kurulum sihirbazında saat dilimi sorusu.

## 0.3.0 - 2026-10-30

### Eklenen
- **Arayüz atamaları**: portları WAN, LAN ve ek (OPT) arayüzlere eşleyin.
- Arayüz ayarları: statik, DHCP veya hiçbiri, ağ geçidi, MTU.
- Her WAN tipi arayüzde **PPPoE**.
- **VLAN'lar** (802.1Q), WAN veya LAN olarak atanabilir.

## 0.2.0 - 2026-10-30

### Eklenen
- **Güvenlik duvarı kuralları**: sistem kuralları gösterilir, isabet sayaçları, sürükle-bırak sırası, açma/kapama anahtarı.
- **Takma adlar**: ana bilgisayarlar, ağlar ve portlar için.
- **NAT**: port yönlendirmeleri ve giden NAT modları.
- Anti-lockout kuralı, bogon ve özel ağ engelleme.
- **Güvenli uygulama**: her değişiklik, zamanında onaylanmazsa otomatik geri alma ile uygulanır.

## 0.1.1 - 2026-10-30

### Değişen
- WAN'dan yönetim erişimi varsayılan olarak kapalıdır.
- Çakışan arayüz ağları reddedilir.

### Düzeltilen
- UEFI sistemlerde konsol seçimi; hem UEFI hem de BIOS ile seri konsol.

## 0.1.0 - 2026-10-29

İlk sürüm.

- Web panelli (HTTPS) ve panelli FreeBSD tabanlı güvenlik duvarı.
- Otomatik geri alma ile uygulanan tek bir merkezi yapılandırma dosyası.
- Konsol menüsü ve ilk önyükleme kurulum sihirbazı.
- Kurulum ISO'su: BIOS ve UEFI için GPT, ZFS, tamamen çevrimdışı kurulum.
