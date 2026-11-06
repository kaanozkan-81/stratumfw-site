---
title: "Sürüm Notları"
description: "Stratum FW sürüm geçmişi - her sürümdeki yeni özellikler, değişiklikler ve düzeltmeler."
---

Stratum FW'deki tüm önemli değişiklikler burada, en yeniden eskiye doğru listelenmiştir. Stratum FW kendini imzalı güncelleme sunucusundan (System > Update) veya çevrimdışı bir güncelleme dosyasından (`.sfu`) günceller; her güncelleme bir ZFS önyükleme ortamı oluşturur, böylece önceki sürüm herhangi bir zamanda yeniden başlatılabilir.

## 0.24.0 - 2026-11-05

### Eklendi
- **Web Filtresi** (Firewall > Web Filter), güvenlik duvarının DNS çözümleyicisi (Unbound RPZ) üzerinden site kategorilerini engeller ve ağ başına farklı kurallar uygular.
  - **Politikalar:** Bir politika, bir veya daha fazla ağa engellenen kategorilerden oluşan bir set atar. VPN istemci ağları da politika ağı olabilir ve her ağ yalnızca bir politikaya bağlıdır.
  - **Kategoriler:** Université Toulouse Capitole'ün UT1 kara listelerinden (CC BY-SA 4.0, günlük güncellenir) ve Block List Project'ten alınan gruplar halinde 55 kategori (yetişkin içerik, güvenlik tehditleri, filtreyi atlatma yolları, sosyal ağlar, eğlence, alışveriş ve para, indirmeler ve daha fazlası). Yetişkin içerik kompakt ve çok büyük tam liste olarak gelir; politika sayfası seçilen kategorilerin çözümleyici belleğinde ne kadar yer kaplayacağını tahmin eder.
  - **Güvenli Arama (Safe Search):** Her politika, Google (google.com.tr ve diğer ülke alan adları dahil), Bing ve DuckDuckGo üzerinde güvenli sonuçları ve YouTube'da kısıtlı modu zorunlu kılabilir.
  - **İstisnalar:** Listelenen siteler web filtresi tarafından asla engellenmez ve Apply gerektirmeden anında etkili olur. DNS kara listeleri onlar için hâlâ geçerlidir.
  - **Bir siteyi kontrol et:** Bir adın hangi kategorilerde olduğunu ve hangi politikaların onu engellediğini gösterir.
  - **Engelleme raporu:** Bugün, 7 veya 30 gün için kategori, site ve cihaz başına engellemeler. Web Etkinliği gerektirir; toplayıcısı artık web filtresi ve DNS kara liste engellemelerini de sayar. Raporu yalnızca yöneticiler görür ve görüntüleme günlüğe kaydedilir.
  - **Güncellemeler:** Kategori listeleri zamanlamaya göre (varsayılan olarak günlük) ve talep üzerine güncellenir; başarısız bir indirme önceki listeyi korur.

## 0.23.0 - 2026-11-05

### Değiştirildi
- **Yeni kontrol paneli:** Her panel taşınabilir ve yeniden boyutlandırılabilir. Özelleştir, bir paneli başlığından sürükleyip 12 sütunlu bir ızgarada kenarlarından veya köşesinden boyutlandırmanıza olanak tanır. Her kullanıcının kendi düzeni vardır, Apply olmadan anında kaydedilir ve Düzeni sıfırla varsayılana döndürür.
- **Kontrol Paneli > Panel ekle** (tam sayfa) her paneli listeler, eklemek veya kaldırmak için. Paneller:
  - Sistem, WAN, CPU, Bellek, Disk, Sıcaklık, Bağlantılar, Engellenenler
  - Trafik, Koruma, Arayüzler, Servisler, DHCP, VPN bağlantıları
  - En çok engellenen kaynaklar, En çok engellenen portlar, Son değişiklikler
  - Yeni: Saldırı tespiti, Ağ geçitleri (gecikme, kayıp, yedekleme), Bant genişliği sınırları (sınır ve mevcut hız), Yedekleme ve Web Etkinliği. Yedekleme ve Web Etkinliği yalnızca yöneticiler içindir ve kontrol panelinde Web Etkinliğini görüntülemek günlüğe kaydedilir.
- Panel içerikleri panel boyutuna uyum sağlar: VPN bağlantıları gibi uzun listeler panel içinde kaydırılır ve trafik grafiği panelini doldurur.
- **Her sayfanın sağ üst köşesinde bildirimler:** okunmamış sayısını gösteren bir zil, açılır menüde en son olaylar ve yeni bir olay geldiğinde kısa bir açılır pencere. Uyarılar panelinin yerini alırlar.
- Kontrol panelinde **Güncelleme kontrolü simgesi**: kontrol ederken döner, tıklama hemen kontrol eder ve sonucu gösterir.
- DHCP ve VPN panelleri ayrıdır. Telefonlarda kontrol paneli tek sütun gösterir.
- gridstack 12.6.0 (MIT) panel ile birlikte gelir. İnternetten hiçbir şey yüklenmez.

## 0.22.0 - 2026-11-05

### Eklendi
- **Şifrelenmiş yedekler:** System > Backup, indirilen yedeği bir parola ile şifreleyebilir (AES-256-GCM, anahtar scrypt'ten). Şifrelenmiş bir yedek (.stbk), yapılandırma dışında tutulan **sertifikaları ve sırları** da taşıyabilir: OpenVPN CA ve kullanıcı sertifikaları, WireGuard anahtarları, bildirim sırları, SNMP topluluğu, MCP belirteçleri ve panel sertifikası. Yeni bir güvenlik duvarında geri yüklemek, çalışan VPN profillerini geri getirir.
- Geri yükleme, şifrelenmiş bir dosyanın parolasını ister ve sertifikaları ve sırları geri koyabilir. Dosyalar yalnızca bilinen yerlerine yazılır.
- **Sunucuya otomatik yedekleme:** şifrelenmiş bir yedek, SFTP (NAS, Linux veya Windows OpenSSH sunucusu) üzerinden zamanlamaya göre yüklenir.
  - Güvenlik duvarı kendi SSH anahtarını kullanır ve sunucunun ana makine anahtarı ilk bağlantıda kaydedilir.
  - Sunucuda bu güvenlik duvarının yalnızca en yeni N yedeği tutulur.
  - Bağlantıyı test et ve Şimdi yedekle düğmeleri ve son yedekleme durumu.
  - Başarısız bir yedekleme bir sonraki saatte yeniden denenir ve Bildirimler aracılığıyla raporlanır.
  - Apply olmadan etkili olur. Parola yalnızca root tarafından okunabilen bir dosyada kalır ve bir daha asla gösterilmez.
- stratum-fw paketi artık py312-cryptography'ye bağımlıdır.

## 0.21.1 - 2026-11-05

### Değiştirildi
- Saldırı Tespiti: her arayüz artık pfSense'te olduğu gibi kendi başına **Kapalı**, **İzle** veya **Engelle** şeklindedir. Örneğin, WAN yalnızca uyarı verebilirken yerel ağlar engelleyebilir.
- En az bir engelleme arayüzü ile, bir Suricata süreci tüm izlenen portları netmap üzerinden denetler, böylece kurallar yalnızca bir kez yüklenir: IPS modunda portları engelleme, tap modunda yalnızca izleme portları (her paket geçer). Aynı porttaki VLAN'lar modunu paylaşır.
- Uyarı listesi, bir paketi yalnızca bir engelleme arayüzünde Engellendi olarak işaretler ve netmap portlarını arayüzlerine göre adlandırır.
- 0.21.0 ile kaydedilen ayarlar devralınır: "Algıla ve engelle", izlenen her arayüzde Engelle olur.

## 0.21.0 - 2026-11-05

### Eklendi
- Services > Intrusion Detection'da **Satır içi engelleme (IPS):** "Algıla ve engelle" modu, Suricata'yı paketlerin yoluna koyar (OPNsense'te olduğu gibi ana bilgisayar yığını ile netmap). **Engelle** olarak işaretlenen kategorilerin kuralları, eşleştikleri şeyleri düşürür; diğer tüm kategoriler hâlâ yalnızca uyarı verir. Engellenen paketler, uyarı listesinde bir "Engellendi" rozeti gösterir.
- Engelleme kategorileri yeniden başlatma olmadan anında etkili olur. Modu değiştirmek Apply gerektirir ve güvenlik duvarına erişilemez hale gelirse olağan otomatik geri alma ile birlikte.
- Engelleme modunda, VLAN ve PPPoE arayüzleri fiziksel portlarında denetlenir ve donanım hızlandırma (TSO, LRO, sağlama toplamları) kapatılır. Suricata durursa, portlar normal iletmeye geri döner (fail open).
- Apply, herhangi bir şey değişmeden önce netmap'in mevcut olduğunu kontrol eder.

## 0.20.3 - 2026-11-05

### Değiştirildi
- Web Etkinliği: bir cihaz satırında herhangi bir yere tıklamak, o cihazın kullandığı siteleri açar ve bir site satırına tıklamak, onu kullanan cihazları açar. Ctrl / Cmd + tıklama sayfayı yeni bir sekmede açar.

## 0.20.2 - 2026-11-05

### Değiştirildi
- Web Etkinliği VPN istemcilerini adlandırır: bir OpenVPN adresi kullanıcıyı ("OpenVPN: ad") ve bir WireGuard adresi eşi ("WireGuard: ad") gösterir. Her ikisi de DHCP geçmişinde tutulur, böylece zaman içinde farklı kullanıcılar tarafından kullanılan bir VPN adresi ayrı kalır.
- Cihaz sayısı başlığın yanındadır.

## 0.20.1 - 2026-11-05

### Düzeltildi
- Web Etkinliği hiçbir şey kaydetmiyordu: syslogd yalnızca zaten var olan günlük dosyalarına yazar, bu yüzden DNS sorgu günlüğü hiç başlatılmadı. Apply ve toplayıcı artık syslogd yeniden yüklenmeden önce dosyayı (yalnızca root) oluşturur.

## 0.20.0 - 2026-11-05

### Eklendi
- **Web Etkinliği** (Diagnostics > Web Activity), güvenlik duvarının Unbound çözümleyicisinin yanıtladığı DNS sorgularından hangi cihazın hangi sitelere baktığını gösterir. Her cihaz, gün ve ad, bir sayı ve ilk ve son zaman ile tutulur.
- Cihazlar sekmesi her adresi ana bilgisayar adı ve MAC adresiyle listeler ve cihazın kullandığı sitelerle bir cihaz sayfası açar. Siteler sekmesi her siteyi onu kullanan cihazlarla listeler. Her iki sekme de döneme (bugünden bir yıla kadar), ağa ve arama metnine göre filtreler.
- **DHCP geçmişi**, hangi cihazın (MAC adresi, ana bilgisayar adı) her adresi ne zaman tuttuğunu kaydeder. DHCP kiralamalarını ve ARP tablosunu okur, böylece sabit adresli cihazlar da dahil edilir. Geçmiş CSV olarak dışa aktarılabilir.
- **Tüm DNS'i güvenlik duvarına gönder** (isteğe bağlı): pf, DNS'i iç ağlardan güvenlik duvarına yönlendirir, 8.8.8.8 ve benzerlerine yönelik sorgular dahil. Ayrıca DNS over TLS'i (port 853) engeller ve Firefox'a kendi DNS over HTTPS'ini kullanmamasını söyler.
- Kayıtlar güvenlik duvarında yalnızca root tarafından okunabilen bir veritabanında 1 ila 730 gün (varsayılan olarak 30) tutulur. Sayfalar yalnızca yöneticilere açıktır ve verilere her bakış sistem günlüğüne yazılır.
- stratum-fw paketi artık py312-sqlite3'e bağımlıdır.

## 0.19.0 - 2026-11-05

### Eklendi
- LibreNMS, Zabbix, PRTG, Observium veya Cacti gibi izleme sistemleri için **SNMP aracısı** (System > Settings > SNMP): SNMP v1 / v2c, salt okunur, arayüzler ve 64-bit trafik sayaçları (IF-MIB), CPU, bellek, diskler ve işlemler (HOST-RESOURCES) ve pf istatistikleri ile. FreeBSD'nin kendi bsnmpd'sini kullanır.
- Yalnızca seçilen iç arayüzlerin adreslerinde yanıt verir, asla bir WAN'da yanıt vermez ve pf sorguların yalnızca izin verilen adreslerden girmesine izin verir. Topluluk yalnızca root tarafından saklanır ve bir daha asla gösterilmez; SNMP üzerinden hiçbir şey değiştirilemez.
- stratum-fw paketi artık FreeBSD-bsnmp'ye bağımlıdır, böylece güncelleme aracıyı yükler.

## 0.18.0 - 2026-11-05

### Eklendi
- **Bant genişliği sınırları** (Firewall > Shaper): her iç ağ için bir indirme ve bir yükleme sınırı; tüm ağ tarafından paylaşılır (örneğin ofis VLAN'ı için 255 Mbit/s) veya her cihaz için ayrı ayrı (örneğin misafir telefonu başına 20 Mbit/s). Ondalık değerlere izin verilir; boş bir alan o yönde sınır olmadığı anlamına gelir.
- Sınırlar, pf'den doğrudan FreeBSD'nin dummynet'ini kullanır (ikinci bir güvenlik duvarı yok). Borular her Apply'da pf'den önce ve önyüklemede FreeBSD tarafından yüklenir; korunmayan bir Apply bunları tekrar kaldırır.

## 0.17.0 - 2026-11-05

### Eklendi
- **Arayüz grupları** (Interfaces > Groups): örneğin tüm VLAN'lar veya tüm WAN'lar gibi bir arayüz grubu, Firewall > Rules'ta kendi sekmesini alır. Kuralları her üye arayüze uygulanır ve pfSense ve OPNsense'te olduğu gibi her arayüzün kendi kurallarından önce değerlendirilir. WAN üyeleri reply-to'larını korur.
- Gruplar yeniden adlandırılabilir (kuralları onlarla birlikte taşınır) ve kuralları kalmadığında silinebilir. Bir arayüz kaldırıldığında, gruplarından ayrılır.
- MCP aracı firewall_rules, grup kurallarını ve uygulandıkları arayüzleri listeler.

## 0.16.1 - 2026-11-05

### Eklendi
- **WAN yedeklemesi (Failover)** (Interfaces > Gateways, iki veya daha fazla WAN ile gösterilir): varsayılan rotayı taşıyan ağ geçidi düştüğünde, varsayılan rota anında açık olan en iyi ağ geçidine (en düşük öncelik numarası) taşınır. Tercih edilen ağ geçidi, yalnızca belirli bir süre (varsayılan olarak 60 sn) çevrimiçi olduktan sonra tekrar devralır, böylece dalgalanan bir hat ileri geri geçiş yapmaz. Başarısız ağ geçidinin bağlantıları, yenisinden hemen yeniden açılmaları için kapatılabilir. Her geçiş günlüğe kaydedilir ve raporlanır.
- İki veya daha fazla WAN ile, bir WAN'a gelen bağlantılara verilen yanıtlar (port yönlendirmeleri, yönetim, geçiş kuralları), varsayılan rota ne olursa olsun o aynı WAN'dan çıkar (reply-to). Statik WAN'lar ağ geçitlerini, PPPoE WAN'lar bağlantılarını kullanır. Tek WAN'lı sistemler tamamen aynı güvenlik duvarı kurallarını korur.

### Düzeltildi
- 0.16.0'a güncellemeden sonra ağ geçidi izleyicisi yalnızca bir sonraki Apply veya yeniden başlatma ile başladı, çünkü önceki sürümün güncelleme aracı onu bilmiyordu. Panel artık etkinleştirildiğinde ancak çalışmadığında onu başlatır.

## 0.16.0 - 2026-11-05

### Eklendi
- **Ağ geçitleri** (Interfaces > Gateways): her WAN tipi arayüzün canlı durumu (çevrimiçi, uyarı, kapalı),
  gecikme, paket kaybı ve son bir saati küçük bir grafik olarak gösteren bir ağ geçidi vardır.
- Yeni bir ağ geçidi izleme servisi, kimse paneli açık tutmasa bile her ağ geçidini WAN'ın kendi adresinden
  saniyede bir yoklar. Son 30 saniyedeki paket kaybı veya gecikme sınırlarına ulaştığında (varsayılan olarak
  %20 ve 500 ms, ağ geçidi başına ayarlanır) ya da arayüzünde bağlantı veya adres olmadığında bir ağ geçidi
  kapalı sayılır; sınırın yarısı uyarı gösterir.
- Ağ geçidi başına: bir açıklama, bir izleme adresi (ağ geçidi ping'lere yanıt vermeyen sağlayıcılar için;
  her zaman o ağ geçidi üzerinden erişilir), sınırlar ve 0.16.1'de gelecek WAN yedeklemesi için bir öncelik.
- Bağlantısı açıkken kapanan bir ağ geçidi ve geri dönüşü bildirimlerle raporlanır.

### Değiştirildi
- Kontrol panelinin WAN paneli ve MCP aracı wan_status artık ağ geçidi izleyicisini kullanır (panelden her 30
  saniyede üç kez yerine saniyede bir yoklama).

## 0.15.3 - 2026-11-05

### Düzeltildi
- Bir WAN arayüzünde saldırı tespiti neredeyse hiç uyarı vermiyordu: güvenlik duvarının genel adresi korunan
  ağların (HOME_NET) bir parçası değildi, bu yüzden internetten ona gelen trafik çoğu kuralla eşleşmiyordu.
  WAN adresleri artık dahil edildi. Bir PPPoE veya DHCP adresi değiştiğinde, yeni adres beş dakika içinde
  devralınır ve Suricata yeniden başlar; bir WAN yeniden bağlanırken hiçbir şey değişmez.

## 0.15.2 - 2026-11-05

### Eklendi
- Kontrol paneli: Koruma kartı son 24 saatteki saldırı uyarılarını (yüksek önem dereceliler kırmızı) ve en sık
  görülen üç kuralı gösterir, Uyarılar sayfasına bağlantı verir.
- Bildirimler: yüksek önem dereceli saldırı uyarıları için e-posta veya Telegram ile yeni bir olay. Her kural
  en fazla 6 saatte bir raporlanır, devre dışı kurallar asla. Diğer olaylar gibi System > Settings >
  Notifications'tan kapatılabilir ve uyarılar kontrol panelinin Uyarılar kartında görünür.
- MCP: yeni bir araç olan ids_alerts, yapay zeka asistanlarına saldırı tespiti durumunu, son 24 saati sayılarla,
  en sık görülen kuralları ve en son uyarıları önem derecesine veya metne göre filtreleyerek sunar.

## 0.15.1 - 2026-11-05

### Düzeltildi
- Saldırı tespiti, imajdan kurulan sistemlerde başlatılamıyordu: Suricata, minimal temel sistemde bulunmayan
  netmap kütüphanesine ihtiyaç duyar. Paket artık buna (FreeBSD-netmap) bağımlıdır, böylece güncelleme onu
  kurar ve yeni kurulumlar onu içerir. Ön kontrol Apply'ı durdurmuştu, bu yüzden hiçbir sistem değiştirilmedi.

## 0.15.0 - 2026-11-05

### Eklendi
- **Saldırı tespiti** (Services > Intrusion Detection) IDS modunda Suricata 8 ile: seçilen arayüzlerin trafiği
  ET Open kurallarıyla karşılaştırılır ve eve bağlanan kötü amaçlı yazılımlar, istismarlar, taramalar, kimlik
  avı, kripto madencileri ve daha fazlası için uyarılar oluşturulur. Henüz hiçbir şey engellenmez; engelleme
  (IPS), uyarılar ağa hangi kuralların uyduğunu gösterdikten sonra gelir.
- Üç grupta kural kategorileri, önerilen bir seçimle (tehditler açık, gürültülü politika ve bilgi kuralları
  kapalı) ve her kategorideki kural sayısı.
- Engelleme listeleri gibi, zamanlamaya göre otomatik kural güncellemeleri (varsayılan olarak günlük). Yeni bir
  kural kümesi yalnızca değiştiğinde indirilir ve kullanılmadan önce Suricata ile test edilir. Başarısız bir
  indirme veya test önceki kuralları korur.
- Uyarılar sayfası: en yeniden eskiye, önem derecesi, kural, kaynak, hedef ve arayüz ile, bir arama ve bir önem
  filtresiyle. Normal trafikte uyarı veren bir kural tek tıkla devre dışı bırakılabilir ve Ayarlar sekmesinde
  yeniden etkinleştirilebilir; değişiklik hemen, Apply olmadan ve tespitte bir boşluk olmadan yürürlüğe girer.
- Suricata denetimli çalışır ve durursa yeniden başlatılır. Bellek sınırları kurulu belleği izler. Günlükleri
  diğer günlüklerle birlikte döndürülür ve başarısız kural indirmeleri bildirimlerle raporlanır.

### Değiştirildi
- MCP adresini bir web tarayıcısında açmak artık boş bir hata sayfası yerine kısa bir açıklama gösterir.
- stratum-fw paketi artık Suricata'ya bağımlıdır.

## 0.14.0 - 2026-11-04

### Eklendi
- **MCP sunucusu** (Model Context Protocol, System > Settings > MCP): Claude, ChatGPT, Gemini veya DeepSeek gibi yapay zeka asistanları (bir MCP istemcisi aracılığıyla) güvenlik duvarının canlı durumunu okuyabilir ve "son bir saatte neler engellendi?" veya "misafir ağında hangi cihazlar var?" gibi soruları yanıtlayabilir. 14 salt okunur araç: sistem durumu, arayüzler, WAN durumu, servisler, güvenlik duvarı günlüğü, engellenen trafik özeti, güvenlik duvarı kuralları, NAT kuralları, DHCP kiralamaları, VPN oturumları, uyarılar, yapılandırma değişiklikleri, engelleme listeleri ve trafik istatistikleri. MCP üzerinden hiçbir şey değiştirilemez.
- MCP için erişim belirteçleri; her asistan veya bilgisayar için bir tane, isteğe bağlı olarak belirli adreslerle sınırlandırılabilir. Bir belirteç yalnızca bir kez gösterilir ve yalnızca karma olarak saklanır; son kullanım, adres ve çağrı sayısı gösterilir ve belirteç herhangi bir zamanda iptal edilebilir. Yanlış belirteçler başarısız oturum açma olarak sayılır ve adresi engeller.
- Her araç kapatılabilir, böylece verileri hiçbir zaman bir yapay zeka sağlayıcısına gönderilmez. Parolalar, anahtarlar, belirteçler ve VPN yapılandırmaları asla döndürülmez.
- Panel sertifikası, ona güvenmesi gereken MCP istemcileri için indirilebilir.

## 0.13.2 - 2026-11-04

### Eklendi
- Kontrol paneli **Uyarılar**: en son olaylar (WAN bağlantısı kesildi veya yeniden kuruldu, yeni WAN adresi, engellenen oturum açmalar, başarısız uygulamalar, otomatik geri almalar, mevcut güncellemeler, disk dolu, yüksek sıcaklık, sertifika ve engelleme listesi sorunları) zamanıyla ve her birinin e-posta veya Telegram ile gönderilip gönderilmediğiyle birlikte. Olaylar artık bir bildirim kanalı ayarlanmamış olsa bile kaydedilir; son 100 olay tutulur.
- Kontrol paneli **DHCP**: ağ başına, etkin kiralaması olan cihazlar, adres havuzunun ne kadarının kullanımda olduğu (%75'ten itibaren sarı, %90'dan itibaren kırmızı) ve statik eşleme sayısı.

## 0.13.1 - 2026-11-04

### Eklendi
- Kontrol paneli **WAN paneli**: bağlantı türü, ağ geçidi, ağ geçidine gecikme ve paket kaybı (her 30 saniyede ölçülür, eğri ile), bağlantının ne kadar süredir açık olduğu, bugünkü indirme ve yükleme ve canlı hız; durum şeridinde internete (1.1.1.1) gecikme.
- Kontrol paneli **VPN bağlantıları**: bağlı her OpenVPN kullanıcısı ve etkin WireGuard eşi, bağlandığı adres, VPN adresi, bağlantı süresi veya son el sıkışma ve alınan/gönderilen verilerle birlikte.
- Kontrol panelini açmak, pfSense gibi yazılım güncellemelerini denetler (en fazla 5 dakikada bir) ve sonucu hazır olur olmaz gösterir.

### Düzeltildi
- Güvenlik duvarı günlüğü: FreeBSD 15'te günlüğe kaydedilen her pakete yanlış kuralın adı veriliyordu (çoğunlukla "ipv6-off"), çünkü pf artık kural numaralarını "3..1/0" olarak yazıyor. Güvenlik Duvarı Günlüğü sayfası, kontrol paneli ve uzak syslog artık doğru kuralı gösteriyor; güncellemeden önce kaydedilen girişler eski adlarını koruyor.
- Kontrol paneli: CPU kutucuğu artık trafik grafiklerini açmıyor; grafiğin altındaki canlı trafik, arayüz listesiyle aynı ölçümü kullanıyor.

## 0.13.0 - 2026-11-04

### Değiştirildi
- **Yeni kontrol paneli**:
  - Durum şeridi: ana bilgisayar adı, sürüm ve güncelleme durumu, adresi ve canlı indirme/yükleme ile birlikte her WAN bağlantısı, çalışma süresi, son yapılandırma değişikliğinin zamanı, güvenlik duvarı durumu.
  - Temel göstergeler: 5 dakikalık eğri ile CPU, bellek, disk, sıcaklık, durum tablosu sınırına göre bağlantılar ve eğri ile son bir saatte engellenen paketler; renkler uyarı seviyelerinde değişir.
  - Arayüz başına son bir saatteki trafik, ortalama ve tepe değerlerle.
  - Koruma: IP ve DNS engelleme listeleri, IP engelleme listeleri tarafından durdurulan paketler, başarısız oturum açmalardan sonra engellenen adresler, çevrimiçi OpenVPN kullanıcıları ve WireGuard eşleri, son bir saatte en çok engelleyen kurallar.
  - Adı olarak açıklamasıyla birlikte arayüzler, durum, bağlantı hızı, adres ve canlı trafik; çalışıyor/durdu özetiyle servisler.
  - Son bir saatteki en çok engellenen kaynaklar ve portlar ile en son yapılandırma değişiklikleri (uygulandı, korundu, geri alındı, başarısız).
  - Koyu ve açık temalarda ve telefonlarda çalışır.

## 0.12.6 - 2026-11-04

### Eklendi
- **System > Settings > Networking**: her fiziksel portta donanım hızlandırmayı (LRO, TSO, sağlama toplamı) kapatın - portları yeniden başlatmadan canlı olarak ayarlanır ve yeniden başlatmadan sonra korunur - port başına mevcut durum ile; CPU güç yönetimi (uyarlanabilir, maksimum performans, ekonomi); ARP günlüğü ve ARP girişi ömrü; IPv6 durumu.
- **System > Settings > Tunables**: bir güvenlik duvarında önemli olan çalışma zamanı sistem değerleri (sysctl), pfSense'in öneri olarak kullandığı değerler ve her birinin mevcut değeri ile birlikte, ayrıca kendi çalışma zamanı tunable'larınız. Değerler Apply ile ayarlanır ve yeniden başlatmadan sonra korunur (/etc/sysctl.conf.local); varsayılana geri ayarlanan bir değer önceki değerini alır. Stratum FW'nin kendi yönettiği değerler burada değiştirilemez.
- Tüm System > Settings sekmeleri artık kullanılabilir.

### Değiştirildi
- Mevcut bir /etc/sysctl.conf.local, /etc/sysctl.conf.local.stratum-orig olarak korunur; dosya artık Settings > Tunables'tan yazılır.

## 0.12.5 - 2026-11-04

### Eklendi
- **Zamanlamaya göre otomatik engelleme listesi güncellemeleri** (Firewall > Blocklists, IP ve DNSBL): her saat, her 6 veya 12 saatte bir, seçilen saatte günlük, seçilen günde haftalık veya kapalı. Sayfa bir sonraki otomatik güncellemeyi gösterir. Kaçırılan bir güncelleme (güvenlik duvarı kapalıyken) bir sonraki saatte çalışır ve başarısız bir güncellemeden sonra bir sonraki saat yeniden dener. Zamanlama değişiklikleri hemen, Apply olmadan yürürlüğe girer. Varsayılanlar önceki ritmi korur (IP listeleri saatlik, DNS listeleri günlük 03:00'te).

### Düzeltildi
- IP engelleme listelerinin güncelleme günlüğü, bir güncelleme çalışırken gizli kalıyordu (sayfadaki iki öğe aynı kimliği paylaşıyordu).

## 0.12.4 - 2026-11-04

### Eklendi
- **System > Settings > Notifications**: güvenlik duvarından **Telegram** (bot) ve **e-posta** (STARTTLS veya TLS ile SMTP) ile mesajlar, her biri sayfadaki değerleri kullanan bir test düğmesiyle.
  - Her biri kapatılabilen olaylar: yazılım güncellemesi mevcut, yapılandırma uygulanamadı, zamanında korunmadığı için değişiklikler otomatik olarak geri alındı, WAN bağlantısı kesildi / yeniden kuruldu / yeni adres, başarısız oturum açmalardan sonra bir adres engellendi, panel sertifikası sona eriyor, IP veya DNS engelleme listesi indirmeleri başarısız, disk neredeyse dolu ve CPU sıcaklığı yüksek (ayarlanabilir eşikler).
  - Başarısız uygulamalar, otomatik geri almalar ve engellenen oturum açmalar hemen bildirilir; geri kalanı her 5 dakikada bir denetlenir. Aynı sorun yalnızca bir aradan sonra yinelenir.
  - Bot belirteci ve posta parolası yalnızca güvenlik duvarında saklanır (yalnızca root tarafından okunabilen dosyalar) ve bir daha asla gösterilmez. Bildirim ayarları hemen, Apply olmadan yürürlüğe girer.

## 0.12.3 - 2026-11-04

### Eklendi
- **System > Settings > Logging**:
  - Güvenlik duvarı günlüğü rotasyonu: dosya boyutu ve tutulan eski dosya sayısı; güvenlik duvarı, sistem, DHCP/DNS ve oturum açma günlüklerinin mevcut boyutu ve günlükler için boş alan.
  - **Uzak syslog**: en fazla iki sunucu (örneğin bir NAS), her biri kendi günlük seçimiyle: sistem, oturum açmalar (panel, SSH, VPN), güvenlik duvarı günlüğü (paket başına bir okunabilir satır, `filterlog` etiketi) ve DHCP/DNS. Sistemin syslogd'si tarafından UDP üzerinden gönderilir; yerel günlükler olduğu gibi kalır.
  - **Test mesajı gönder**, herhangi bir şey uygulanmadan önce bir sunucunun syslog aldığını doğrular.

### Değiştirildi
- Güvenlik duvarı ve DHCP/DNS günlüklerinin rotasyonu ayarlardan üretilir (varsayılanlar değişmemiştir).
- Yazılım güncellemeleri güvenlik duvarı günlüğünü ve trafik toplayıcılarını yeniden başlatır, böylece yeni sürümü hemen çalıştırırlar.

## 0.12.2 - 2026-11-03

### Eklendi
- **System > Settings > Firewall & NAT**:
  - Engellenen bağlantılar sessizce düşürülür veya reddedilir; varsayılan bloğun ve yayın gürültüsünün günlüğe kaydedilmesi.
  - Bogon ve özel ağ engelleme ve WAN adresinde ping (şimdiye kadar yalnızca yapılandırma dosyasında).
  - Durum tablosu boyutu, kurulu bellek ve mevcut kullanım için bir öneriyle, boşta bağlantı zaman aşımları (normal, yüksek gecikme, muhafazakar, agresif), SYN sel koruması.
  - Paket normalleştirme: rastgele IP kimliği, parçalama-bayrak temizleme.
  - **NAT yansıması**: yerel cihazlar genel adres üzerinden bir port yönlendirmesine ulaşır. Yönlendirilen trafik hâlâ cihazın kendi arayüzünün kuralları tarafından filtrelenir.
  - Otomatik giden NAT için statik kaynak portları.
  - Varsayılanlar, önceki sürümlerin üretilen pf yapılandırmasını değiştirmeden korur.

### Değiştirildi
- Panelin kendi başına kullandığı ayarlar (oturum zaman aşımı, oturum açma bildirimi, oturum açma koruması) artık "Apply changes" çubuğunu göstermiyor ve Discard bunları korur.

## 0.12.1 - 2026-11-03

### Düzeltildi
- Apply'a tıklamak aynı uygulamayı iki kez başlatabiliyordu: apply çubuğunun periyodik yenilemesi, ilk uygulama hâlâ çalışırken etkin bir Apply düğmesini yeniden çiziyordu ve ikincisi Keep'ten hemen sonra tekrar çalışıyor, aynı değişiklikleri ikinci kez korumayı istiyordu. Çubuk artık işlem bitene kadar meşgul kalır ve aynı anda yalnızca bir apply, onaylama veya geri alma çalışabilir (panel, konsol ve geri alma zamanlayıcısı).
- Yönetim portunda (örneğin 443) bir WAN port yönlendirmesi artık herhangi bir yapılandırmanın kaydedilmesini engellemez. Yönlendirilen port WAN yönetim kuralından çıkarılır (pf onu zaten yönlendirilen ana bilgisayara gönderir) ve Admin Access bir bildirim gösterir.

## 0.12.0 - 2026-11-03

### Eklendi
- **System > Settings** sekmelerle, System > General'in yerini alır. Daha fazla sekme (Firewall & NAT, Logging, Notifications, Networking, Tunables) sonraki sürümlerde gelir.
- **General**: ana bilgisayar adı, yerel etki alanı, saat dilimi ve oturum açma sayfasında gösterilen bir oturum açma bildirimi (örneğin bir yasal bildirim).
- **Admin Access**:
  - Web panel portu ve HTTP'den HTTPS'ye yönlendirme, oturum zaman aşımı.
  - Güvenlik duvarını kimlerin yönetebileceği: tüm iç ağlar (güvenlik duvarı kuralları karar verir) veya yalnızca seçili arayüzler; LAN'da kilitlenme önleyici kural.
  - WAN'dan erişim (şimdiye kadar yalnızca konsol menüsünde), izin verilen kaynak adreslerinin listesiyle.
  - Ayarlanabilir denemeler, süre ve engelleme süresiyle oturum açma koruması, engellenen adreslerin listesi ve bir Unblock düğmesi.
  - SSH: açık veya kapalı, port, parola veya yalnızca anahtarla oturum açma, root'un yetkili ortak anahtarları.
  - Yeni ayarlar bağlı olduğunuz adresi kilitleyecekse kaydetme reddedilir. nginx ve sshd, Apply'dan önce yeni ayarları kontrol eder; panel başka bir porta taşındığında, apply çubuğu yeni adrese bağlantı verir.
- Konsol menüsü 6) **Management access**: mevcut ayarları gösterir, WAN erişimini açar veya kapatır ve yönetim erişimini varsayılanlara sıfırlar.

### Değiştirildi
- Kilitlenme önleyici ve WAN yönetim kuralları, yapılandırılmış panel ve SSH portlarını izler.
- Root'un mevcut SSH anahtarları, güncellemeden sonraki ilk kaydetmede panel tarafından devralınır.

## 0.11.0 - 2026-11-03

### Eklendi
- **Sertifikalar** (System > Certificates): web panelinin TLS sertifikasını yönetin.
  - Etkin sertifikayı görüntüleyin: adlar, veren, kalan gün sayısıyla sona erme tarihi, anahtar türü ve tarayıcıyla karşılaştırmak için SHA-256 parmak izi.
  - Kendi sertifikanızı yükleyin (örneğin bir şirket CA'sından), ara sertifikalarla birlikte aynı dosyada. Kullanımdan önce kontrol edilir: anahtar eşleşmeli ve parola içermemeli, sertifika şu anda geçerli olmalı ve nginx onu kabul etmeli.
  - Panelin açıldığı ana bilgisayar adları ve adresler için yeni bir kendinden imzalı sertifika oluşturun.
  - Önceki sertifika saklanır ve sayfadan veya konsolda `stratum-cert restore` ile geri yüklenebilir. Yeni bir sertifika hemen, Apply olmadan ve açık bağlantıları düşürmeden yürürlüğe girer.
- Bir CA tarafından verilen panel sertifikasının 21 gün içinde sona ermesi durumunda yöneticiler için uyarı.
- `stratum-cert` komutu: `show`, `selfsigned`, `install`, `restore`.

## 0.10.0 - 2026-11-03

### Güvenlik
- **IPv6 güvenlik temeli**: IPv6 henüz desteklenmiyor ve artık kapalı kalması garanti altına alındı - her IPv6 paketi herhangi bir pass kuralından önce düşürülür (giriş ve çıkış, günlüğe kaydedilmez), IPv6 tüm arayüzlerde devre dışı kalır ve asla iletilmez. IPv6 sunmaya başlayan bir ISS artık IPv4 kuralları etrafında bir yol açamaz.

### Eklendi
- IPv6 tasarımı ve sürüm planı (`docs/ipv6.md`): önce DHCPv6 önek delegasyonu ile WAN IPv6, ardından iç ağlar ve güvenlik duvarı kuralları, servisler ve VPN'ler için IPv6.

## 0.9.1 - 2026-11-03

### Değiştirildi
- Bir Apply başarısız olduğunda, apply çubuğu artık nedenini söyler: her başarısız kontrol mesajıyla birlikte (örneğin bir pf sözdizimi hatası), System Logs > Configuration altındaki tam çıktıya bir bağlantı ile. Başarısızlıklar sistem günlüğüne de yazılır.
- Güncel bir sistemde `stratum-update run` bir hata yerine "nothing to install" bildirir.

## 0.9.0 - 2026-11-03

### Eklendi
- **IP engelleme listeleri** (Firewall > Blocklists > IP): pf ile bilinen-kötü adresleri engelleyin.
  - pfBlockerNG'den türetilen 63 bakımlı IPv4 beslemesi kataloğu (PRI1-PRI5, Scanners, TOR, Mail, DoH, VPN, Proxy ve daha fazlası); Spamhaus DROP, Abuse.ch Feodo Tracker ve Emerging Threats önerilir ve önceden seçilidir.
  - Yön: internetten gelen, yerel ağlardan giden veya her ikisi. Gelen bağlantılar düşürülür; gidenler reddedilir, böylece cihazlar hızla başarısız olur.
  - Kendi izin ve engelleme listeleriniz ve özel liste URL'leri. Engellenen bir ağ içindeki izin verilen bir adres erişilebilir kalır.
  - Apply olmadan saatlik güncellemeler (pf tablosu canlı olarak değiştirilir), canlı ilerleme çubuğu ve güncelleme günlüğü, engellenen paket sayaçları ve bir IP'nin hangi listede olduğunu görmek için "Check an address".
  - Güvenlik: özel ve ayrılmış aralıklar ve /8'den büyük ağlar, bir liste ne içerirse içersin asla engellenmez; güvenlik duvarının DNS, güncelleme ve paket sunucularına her zaman izin verilir.
- **İki varyantta kurulum ortamı**: `stratum-fw-v0.9.0-vga.iso` (monitör ve klavye) ve `stratum-fw-v0.9.0-serial.iso` (seri konsol, 115200 8N1). Her ikisi de CD ve USB çubukları için hibrit görüntülerdir ve kurulu sistem, kurulduğu ortamın konsolunu korur.

## 0.8.0 - 2026-11-03

### Eklendi
- **WireGuard** (VPN > WireGuard): FreeBSD'ye yerleşik WireGuard ile uzaktan erişim - ekstra paket yok.
  - Sunucu ayarları: port, dinleme WAN'ı, tünel ağı, MTU, harici adres (otomatik algılanır).
  - Eşlerin hangi LAN'lara ve VLAN'lara ulaşabileceğini tam olarak seçin, ayrıca ekstra ağlar; isteğe bağlı tam tünel ve eşler arası trafik.
  - Güvenlik duvarında oluşturulan anahtar çiftleri ve ön paylaşımlı anahtarlarla eşler (cihaz başına bir tane): yapılandırma WireGuard uygulaması için QR kodu olarak gösterilir veya `.conf` dosyası olarak indirilir. Kendi anahtarlarına sahip cihazlar da desteklenir.
  - Eş başına canlı durum: bağlı, uç nokta, son el sıkışma, alınan ve gönderilen veriler.
  - Bir eş eklemek veya kaldırmak diğerlerinin bağlantısını kesmez.
- Özel anahtarlar yapılandırma dosyasının dışında tutulur (yalnızca root tarafından okunabilen dosyalar); eş yapılandırmaları yalnızca yöneticilere gösterilir.

## 0.7.3 - 2026-11-02

### Eklendi
- **OpenVPN gelişmiş ayarları**: UDP veya TCP, dinleme WAN'ı, VPN istemcilerinin hangi LAN'lara ve VLAN'lara ulaşabileceği (arayüz başına) ve ekstra ağlar, istemciden istemciye trafik, kullanıcı başına birkaç cihaz, maksimum bağlantılar, istemciler için DNS sunucuları ve etki alanları, veri şifreleri, kimlik doğrulama özeti, minimum TLS sürümü, anahtar yeniden anlaşması, keepalive, MTU/MSS, günlük düzeyi ve yalnızca sertifikayla oturum açma.
- **Kullanıcı başına sabit VPN adresi** (System > Users).
- Panel, bağlantı ayarları değiştiğinde ve kullanıcıların profillerinin yeniden indirilmesi gerektiğinde sizi bilgilendirir.

### Değiştirildi
- OpenVPN sayfasındaki VPN erişim onay kutuları hemen etkili olur; Kaydet düğmesi ayarların altındadır.

## 0.7.2 - 2026-11-01

### Değiştirildi
- System > Users: OpenVPN erişimi kendi VPN sütununda gösterilir.

## 0.7.1 - 2026-11-01

### Eklendi
- **Yalnızca VPN hesapları**: web paneline oturum açma olmadan OpenVPN erişimi olan kullanıcılar, System > Users'ta yönetilir.
- Kullanıcı listesi: son VPN oturum açması ve başarısız VPN parola denemeleri; `.ovpn` profilini listeden indirin.

### Değiştirildi
- VPN erişimini kaldırmak, bir kullanıcıyı devre dışı bırakmak veya silmek kullanıcının sertifikasını hemen iptal eder.
- OpenVPN profilleri artık kullanımdan kaldırılan `persist-key` seçeneğini kullanmıyor (OpenVPN 2.7).

## 0.7.0 - 2026-11-01

### Eklendi
- **OpenVPN uzaktan erişim** (VPN > OpenVPN).
  - Yerleşik sertifika yetkilisi (eliptik eğri anahtarları, iptal listesi, tls-crypt).
  - Kullanıcının sertifikası ve parolasıyla oturum açma; sertifika aynı kullanıcıya ait olmalı.
  - Her şeyin gömülü olduğu kullanıcı başına `.ovpn` profilleri (yalnızca yöneticiler), sertifika iptali, bağlı istemcilerin listesi.
  - VPN istemcileri için güvenlik duvarı ve NAT kuralları ve DNS otomatik olarak oluşturulur.

## 0.6.13 - 0.6.19 - 2026-10-31

### Eklendi
- **Unbound DNS çözümleyici** (Services > DNS): DNSSEC ile doğrulayıcı çözümleyici, yönlendirici veya tam özyineleme modu; DHCP istemci adları çözülmeye devam eder. dnsmasq kullanılabilir kalır ve aralarındaki geçiş her iki yönde de güvenlidir.
- **DNS engelleme listeleri** (Firewall > Blocklists > DNSBL), pfBlockerNG tarzı: pfBlockerNG'den türetilen katalog artı HaGeZi listeleri, önerilen varsayılanlar, kendi izin ve engelleme listeleriniz, özel listeler, "Check a domain", gece güncellemeleri ve yüzde çubuğu, mevcut adım ve canlı günlük içeren bir güncelleme ekranı.
- **Kontrol paneli**: sürümün yanında güncelleme rozeti, canlı çalışma süresi, CPU sıcaklığı, PPPoE oturum adresi ve son bir saat için bir trafik grafiği.

### Düzeltildi
- DNS engelleme listeleri: liste yorumlarındaki bağlantılar artık giriş olarak değerlendirilmiyor (github.com gibi popüler siteler bazı listeler tarafından engelleniyordu); liste kaynakları, güncelleme sunucusu ve paylaşımlı barındırma platformları asla engellenmez.
- DNS engelleme listeleri etkinleştirilmiş yapılandırmanın uygulanması 0.6.15'te başarısız oldu.
- DNSSEC kök güven çapası, Unbound yapılandırması kontrol edilmeden önce oluşturulur.

## 0.6.6 - 0.6.12 - 2026-10-31

### Eklendi
- **Çevrimdışı güncellemeler**: imzalı bir güncelleme dosyası (`.sfu`) indirin ve System > Update'te yükleyin - ilerleme çubuğuyla imza ve sağlama toplamı doğrulaması, ardından çevrimiçi güncellemeyle aynı kurulum adımları.
- Her güncellemeden sonra en yeni 5 önyükleme ortamı tutulur; daha eskileri otomatik olarak kaldırılır.

### Düzeltildi
- Daha önceki bir önyükleme ortamını başlatmak çalışır (yükleyici ve güncelleyici doğru ZFS özelliklerini ayarlar; mevcut kurulumlar otomatik olarak onarılır).
- Yeni kurulumlar sürümlerini doğru bildirir (paket veritabanı şeması), böylece güncellemeler sunulur.
- Çevrimiçi denetim internet olmadan hızlı başarısız olur ve kesintiye uğramış bir paket indirmesi artık güncellemeyi askıda bırakamaz.

## 0.6.1 - 0.6.5 - 2026-10-31

### Eklendi
- Güncelleme ilerlemesi: yüzde çubuğu, zamanlarla adım kontrol listesi, devam eden başlık ve panel yeniden başlarken bir "restarting" sayfası.
- Sistem uyarıları (IP yönlendirme kapalı, güncelleme mevcut) ve parola değişikliğiyle kendi hesap sayfanız.

### Düzeltildi
- IP yönlendirme, uygulama, geri alma ve servis başlatmalarından sonra her zaman yapılandırmayı izler.
- Web paneli bir güncellemeden sonra kendiliğinden yeniden başlar.

## 0.6.0 - 2026-10-31

### Eklendi
- **Kullanıcılar ve roller** (System > Users): birden çok hesap, yönetici / operatör / salt okunur rolleri, etkinleştirme ve devre dışı bırakma, son oturum açma ve başarısız denemeler; bir parola değiştiğinde veya bir hesap devre dışı bırakıldığında oturumlar sona erer.
- **Genel ayarlar** (System > General): ana bilgisayar adı, yerel etki alanı, boşta oturum zaman aşımı.
- **Açık tema** ve sistemi takip et modu, kullanıcı başına seçilir.
- **Yedekleme ve geri yükleme** (System > Backup): yapılandırmayı indirin (isteğe bağlı olarak kullanıcılarla birlikte), bekleyen değişiklikler olarak doğrulanmış geri yükleme, farklılıklarla yapılandırma geçmişi.
- **İmzalı yazılım güncellemeleri** (System > Update) update.stratumfw.com'dan: imza denetimli depo, her güncellemeden önce bir yedek ve yeni bir ZFS önyükleme ortamı, canlı ilerleme, önceki önyükleme ortamına geri alma, günlük güncelleme denetimi.
- Markalı önyükleme menüsü.

## 0.5.0 - 2026-10-30

### Eklendi
- **Güvenlik Duvarı Günlüğü**: filtreler ve kural adlarıyla canlı görünüm.
- **Sistem Günlükleri**: sistem, DHCP/DNS, PPPoE, NTP, kimlik doğrulama, panel ve yapılandırma günlükleri.
- **Ping, Traceroute ve DNS araması** ve durumları sonlandırma seçeneğiyle **pf durum tablosu**.
- Her arayüz için **Trafik Grafikleri** (son bir saat, 24 saat, 30 gün).

### Değiştirildi
- Yayın ve çoklu yayın gürültüsü, günlüğü doldurmadan engellenir (günlüğe kaydetme açılabilir).

## 0.4.0 - 2026-10-30

### Eklendi
- **DHCP Sunucusu**: arayüz başına ayarlar, statik eşlemeler, "statik yap" ile etkin kiralamalar.
- **DNS Çözümleyici**: yerel etki alanı, üst akış sunucuları, ana bilgisayar ve etki alanı geçersiz kılmaları.
- **NTP**: zaman sunucuları, saat dilimi, iç ağlar için isteğe bağlı zaman servisi.
- Konsol kurulum sihirbazında saat dilimi sorusu.

## 0.3.0 - 2026-10-30

### Eklendi
- **Arayüz atamaları**: portları WAN, LAN ve ek (OPT) arayüzlere eşleyin; OPT arayüzleri WAN veya LAN türü olabilir.
- Arayüz ayarları: statik, DHCP veya hiçbiri, ağ geçidi, MTU.
- Her WAN tipi arayüzde **PPPoE** (parola bir daha asla gösterilmez).
- **VLAN'lar** (802.1Q), WAN veya LAN olarak atanabilir.

### Değiştirildi
- Bir arayüzün açıklaması her yerde adı olarak gösterilir, rolü altında.

## 0.2.0 - 2026-10-30

### Eklendi
- Sistem kuralları gösterilen, isabet sayaçları, sürükle-bırak sırası, açma/kapama anahtarıyla arayüz başına **güvenlik duvarı kuralları**.
- Ana bilgisayarlar, ağlar ve portlar için **takma adlar**.
- **NAT**: port yönlendirmeleri (otomatik pass kuralları ve isabet sayaçlarıyla) ve manuel kurallarla giden NAT modları.
- WAN'da kilitlenme önleyici kural, bogon ve özel ağ engelleme.
- **Güvenli uygulama**: her değişiklik, zamanında onaylanmadıkça otomatik geri almayla uygulanır.

## 0.1.1 - 2026-10-30

### Değiştirildi
- WAN'dan yönetim erişimi varsayılan olarak kapalıdır (kurulum sihirbazında veya konsol menüsünde açılabilir).
- Çakışan arayüz ağları reddedilir.

### Düzeltildi
- UEFI sistemlerde konsol seçimi; hem UEFI hem de BIOS ile seri konsol.

## 0.1.0 - 2026-10-29

İlk sürüm.

- Web paneli (HTTPS) ve kontrol paneli ile FreeBSD tabanlı güvenlik duvarı.
- Otomatik geri almayla uygulanan tek merkezi yapılandırma dosyası.
- Konsol menüsü ve ilk önyükleme kurulum sihirbazı.
- Yükleyici ISO'su: BIOS ve UEFI için GPT, ZFS, tamamen çevrimdışı kurulum.
