---
title: "Sürüm Notları"
seo_title: "Stratum Firewall Sürüm Notları (v0.30.3) — Tüm Yenilikler ve Değişiklik Geçmişi"
description: "Stratum Firewall v0.30.3 sürümüne kadar tüm yenilikler, değişiklikler ve düzeltmeler — OPNsense Unbound DNSBL geçişi, bölünmeyen DHCP havuzları, dummynet hız sınırı koruması, TOTP 2FA, Zorunlu DNS ve DHCP Çoklu WAN."
---

Stratum FW üzerindeki tüm önemli değişiklikler en yeniden eskiye doğru burada listelenir. Stratum FW kendisini imzalı güncelleme sunucusundan (**Sistem > Güncelleme**) veya çevrimdışı güncelleme dosyasından (`.sfu`) günceller; her güncelleme yeni bir ZFS önyükleme ortamı (Boot Environment) oluşturur, böylece önceki sürüme istenildiği zaman geri dönülebilir.

## 0.30.3 - 2026-11-11

### Düzeltildi
- **Kurulum İmajından Kurulan Sistemlerde Bant Genişliği Sınırları (`Güvenlik Duvarı > Hız Sınırlayıcı`):** İmajdan kurulan sistemlerde `dnctl` aracı ve `dummynet` başlangıç betikleri (`FreeBSD-ipfw`) eksikti; `pf` hız sınırı tanımlı ağların trafiğini `dummynet`'e yönlendirmeye devam ettiği için bu ağlarda DHCP dışındaki trafik düşüyordu. `FreeBSD-ipfw` artık hem kurulum imajına dahil edildi hem de paket bağımlılığı olarak eklendi (güncelleme yapan sistemlere otomatik kurulur).
- `dnctl` aracı veya `dummynet` çekirdek modülü eksik olduğunda, bant genişliği sınırı içeren bir yapılandırmanın uygulanması trafiği düşürecek `pf` kuralları yüklemek yerine açık gerekçesiyle reddedilir.

## 0.30.2 - 2026-11-11

### Eklendi
- **OPNsense'den İçe Aktarma (`Unbound DNS Engelleme Listeleri`):** OPNsense üzerindeki Unbound DNS engelleme listeleri (hem eski biçim hem de birden fazla liste içeren yeni biçim) artık otomatik olarak taşınır ve güvenlik duvarının DNS engelleme listesi ayarlarının yerini alır.
  - Katalogda aynı kaynağa sahip olan listeler (URL adresine göre, ayrıca *Steven Black* ve *AdGuard* kendi OPNsense kodlarıyla) otomatik etkinleştirilir; diğer `https://` listeleri özel liste (`custom list`) olarak eklenir.
  - İzin listesi (`allow list`), engelleme listesi (`block list`) ve joker (`wildcard`) alan adları olduğu gibi aktarılır.
  - Karşılığı bulunmayan OPNsense liste kodları, şifresiz `http://` listeleri ve düzenli ifade (`pattern`) kuralları önizleme raporunda listelenir.
  - OPNsense'de yalnızca belirli ağlar için seçilmiş bir liste varsa raporda belirtilir (Stratum'un DNS engelleme listeleri tüm ağları kapsar).

### Düzeltildi
- **OPNsense'den İçe Aktarma:** Yeni OPNsense biçimindeki DNS engelleme listelerinin ne içe aktarılması ne de raporda gösterilmesi sorunu giderildi.

## 0.30.1 - 2026-11-11

### Değiştirildi
- **DHCP:** Bir IP dağıtım aralığı artık OPNsense'de olduğu gibi arayüzün kendi IP adresini (ve istemcilere verilen ağ geçidi adresini) kapsayabilir; bu adresler hiçbir istemciye dağıtılmaz. Aralık, bu adreslerin etrafında iki ayrı havuz olarak yazıldığı için hiçbir IP bloğu kaybedilmez ve kontrol paneli havuz boyutunu bu adresleri düşerek hesaplar.
- **OPNsense'den İçe Aktarma:** Arayüz adresini kapsayan DHCP aralıkları artık yalnızca büyük parçasına kesilmek yerine tam kapsamıyla korunur; ağ adresinden (`network address`) başlayan veya yayın adresinde (`broadcast`) biten aralıklar ise otomatik olarak geçerli sınıra kaydırılır.

## 0.30.0 - 2026-11-11

### Eklendi
- **Hesabım (`My Account`) altında İki Aşamalı Giriş (`TOTP 2FA`):** Parola doğrulamasının ardından yönetim paneli kimlik doğrulama uygulamasındaki (`Google Authenticator`, `Microsoft Authenticator`, `1Password`, `Bitwarden` vb.) 6 haneli kodu ister.
  - Karekod (QR kod) okutarak veya gizli anahtarı elle girerek kurulur. Karekod yerleşik kütüphaneyle doğrudan tarayıcı içinde çizilir; gizli anahtar güvenlik duvarı dışına asla çıkmaz ve kurulum tamamlandıktan sonra bir daha gösterilmez.
  - Her kod yalnızca bir kez geçerlidir (tekrar kullanımı engellenir); ileri veya geri 30 saniyelik saat farkı kabul edilir.
  - Telefon kaybına karşı tek seferlik gösterilen **10 adet kurtarma kodu** üretilir; istenildiği zaman yeni kurtarma kodları oluşturulabilir. Kurtarma koduyla yapılan her giriş sistem bildirimi üretir.
  - İki aşamalı giriş etkinleştirildiğinde kullanıcının açık olan diğer tüm oturumları sonlandırılır.
  - Hatalı kod denemeleri giriş koruması (`sign-in protection`) sayacına dahil edilir; kod doğrulama ekranı 5 dakika sonunda zaman aşımına uğrar.
- **Kullanıcılar (`Users`):** Kullanıcı listesinde `2FA` rozeti gösterilir; yöneticiler telefonunu kaybeden bir kullanıcının iki aşamalı girişini kapatabilir.
- **Konsol Menüsü:** Parola sıfırlama adımı artık iki aşamalı girişi kapatma seçeneğini de sunar.

## 0.29.2 - 2026-11-11

### Eklendi
- **Servisler > DNS Çözümleyici > DNS'i Güvenlik Duvarından Zorla (`Force DNS through the firewall`):** Seçilen dahili ağlardaki cihazlar hangi DNS sunucusunu sorgularsa sorgulasın (örneğin elle girilmiş `8.8.8.8`), tüm DNS sorguları güvenlik duvarının kendi çözümleyicisine yönlendirilir; böylece DNS engelleme listeleri (DNSBL) ve Web Filtresi aşılması imkansız hale gelir. Bu ağlarda şifreli DNS over TLS (`DoT`) de otomatik engellenir.
  - **İstisna Takma Adı (`Exception alias`):** Örneğin bir Active Directory etki alanı denetleyicisi (Domain Controller) gibi belirli sunuculara doğrudan erişimin açık kalmasını sağlar.
  - **Web Etkinliği (`Web Activity`) özelliğinden bağımsızdır:** Hiçbir gezinme kaydı tutulmaz.
- **OPNsense'den İçe Aktarma:**
  - Dahili ağların DNS trafiğini güvenlik duvarına yönlendiren port yönlendirme kuralları, ters çevrilmiş (`negated`) hedef takma adı istisna listesi olacak şekilde otomatik olarak **Zorunlu DNS (`Force DNS`)** ayarına dönüştürülür.
  - IP engelleme kataloğundaki listelerden birini indiren URL tablosu takma adları (örneğin `CINS Army` veya `blocklist.de`, eski alan adlarıyla tanımlanmış olsalar bile) artık boş aktarılmak yerine ilgili IP engelleme listesini gelen ve giden yönlerde otomatik etkinleştirir.

## 0.29.1 - 2026-11-11

### Eklendi
- **Ek WAN Arayüzlerinde DHCP Desteği:** İkinci (veya üçüncü) internet bağlantısı artık kablo modem veya 4G/5G LTE yönlendirici gibi cihazlardan **DHCP** ile dinamik IP adresi alabilir.
  - Ek bir DHCP WAN hattı varsayılan rotayı veya sistem DNS sunucularını tek başına üzerine almaz; varsayılan rotayı hangi hattın taşıyacağına her zaman ağ geçidi izleyicisi (**Arayüzler > Ağ Geçitleri**, yedeklilik/failover) karar verir.
  - DHCP kiralamasıyla alınan ağ geçidi; hat yedekleme (`failover`), o hattan gelen bağlantılara dönüş (`reply-to`) ve politika tabanlı yönlendirme (`policy routing`) için kullanılır. Servis sağlayıcı farklı bir ağ geçidi verdiğinde güvenlik duvarı kuralları **Uygula (`Apply`)** gerektirmeden saniyeler içinde yeni adresi takip eder.
- **Politika Yönlendirmesi (`Policy routing`):** Bir kuralın çıkış ağ geçidi olarak DHCP tabanlı ek WAN seçilebilir; hat IP kiralayana kadar kural normal yönlendirmeyi kullanır.
- **OPNsense'den İçe Aktarma:** DHCP kullanan `WAN2` arayüzleri artık uyarı üretmeden doğrudan içe aktarılır.

## 0.29.0 - 2026-11-11

### Eklendi
- **Güvenlik Duvarı > Zamanlamalar (`Firewall > Schedules`):** Güvenlik duvarı kurallarında kullanılabilen `"Pzt-Cum 08:00-18:00; Cmt 09:00-13:00"` gibi adlandırılmış zaman aralıkları. pfSense'de olduğu gibi zamanlamaya bağlı bir kural yalnızca tanımlı saat dilimleri içinde geçerlidir.
  - Tanımlı saatlerin dışında izin (`pass`) kuralı artık trafiğe izin vermez, engelleme (`block`) kuralı ise engellemez; kararı alt sıradaki kurallar verir.
  - Bir zamanlama süresi sona erdiğinde, ilgili izin kurallarının açtığı mevcut bağlantılar kapatılır.
  - Kurallar **Uygula (`Apply`)** işlemine gerek kalmadan saat dilimine göre bir dakika içinde otomatik devreye girer veya çıkar. Saatler güvenlik duvarının yerel saat dilimini kullanır; gece yarısını aşan aralıklar iki parça halinde yazılır.
  - Zamanlama listesinde her planın şu an aktif olup olmadığı, bir sonraki başlangıç veya bitiş zamanı ve kaç kural tarafından kullanıldığı gösterilir. Zamanlamanın adını değiştirmek bağlı kuralları korur; kullanımda olan bir zamanlama silinemez.
  - Kural sayfalarında her kural için **Zamanlama (`Schedule`)** seçimi ve kural listesinde durum rozeti (aktifken yeşil) yer alır.
- **OPNsense'den İçe Aktarma:** Zamanlamaya sahip kurallar gün boyu yanlışlıkla açık kalmaması için kapalı (`disabled`) olarak içe aktarılır ve aktarım raporunda listelenir.

### Değiştirildi
- Politika yönlendirmesi (Policy Routing) ve zamanlamalar; çalışan `pf.conf` dosyası yalnızca yapılandırmanın ürettiği çıktıyla birebir aynı olduğunda `pf.conf` dosyasını **Uygula** gerektirmeden yenileyen tek bir ortak mekanizmayı (`stratum-pfrefresh`, dakikada bir çalışır; bir ağ geçidi düştüğünde ağ geçidi izleyicisi anında tetikler) paylaşır.

## 0.28.9 - 2026-11-11

### Eklendi
- **Güvenlik Duvarı > NAT > 1:1:** Dahili bir sunucuya (veya alt ağa) çift yönlü olarak kendine ait bir genel (public) IP adresi (veya aynı boyutta genel ağ bloğu) tahsis eder. İçerideki sunucu internete bu genel adresle çıkar ve bu genel adrese gelen trafik doğrudan o sunucuya ulaşır.
  - Tekil bir genel IP adresi WAN üzerinde tanımlı bir Sanal IP (Virtual IP) olmalıdır; bir ağ bloğu ise servis sağlayıcı tarafından güvenlik duvarına yönlendirilmiş olmalıdır. Güvenlik duvarına erişimin kaybedilmemesi için WAN arayüzünün kendi ana IP adresi 1:1 NAT için reddedilir.
  - pfSense'de olduğu gibi 1:1 NAT tek başına dışarıdan port açmaz: gelen trafik için WAN üzerinde dahili adrese izin veren bir `pass` kuralı tanımlanmalıdır. Liste ekranı her sunucuya kaç WAN kuralının izin verdiğini (hiç yoksa `"kapalı / closed"`) gösterir.
  - İlgili portlarda Port Yönlendirme (`Port Forward`) kuralları önceliklidir; 1:1 NAT ise giden NAT (`Outbound NAT`) kurallarından önce değerlendirilir.
- **OPNsense'den İçe Aktarma:** 1:1 NAT kayıtları içe aktarım raporunda bilgi amaçlı listelenir (otomatik taşınmaz).

## 0.28.8 - 2026-11-11

### Eklendi
- **Tanılama > Güvenlik Duvarı Günlüğü > Özet (`Diagnostics > Firewall Log > Summary`):** Canlı günlük sekmesinin yanında yeni analiz sekmesi.
  - Son 1 saati veya 24 saati kapsar; en çok trafik üreten kaynak IP'leri (ülke bilgisiyle), en çok hedeflenen portları (`SSH` veya `RDP` gibi servis adlarıyla), ilk sıradaki ülkeleri, en çok eşleşen kuralları, hedef adresleri ve arayüz bazlı paket sayılarını 5 dakikalık veya saatlik grafiklerle gösterir.
  - Engellenen veya izin verilen (günlüğe kaydedilen) paketlere ve arayüze göre filtrelenebilir. Herhangi bir IP adresine veya porta tıklandığında o değere filtrelenmiş canlı günlük ekranı açılır.
  - Seçilen zaman aralığı gerektirdiğinde döndürülmüş (rotated) eski günlük dosyaları da otomatik okunur; günlük geçmişi seçilen sürenin tamamını kapsamıyorsa sayfada belirtilir.
  - Ülke eşleştirmesi ülke takma adlarının (GeoIP) verisini kullanır; bu veri henüz indirilmemişse özet ekranı ülke sütunu olmadan çalışmaya devam eder.

### Değiştirildi
- **Güvenlik Duvarı Günlüğü:** Sistemin kendi dahili kuralları artık iç etiketler yerine okunabilir adlarla (`"IP blocklist: inbound (WAN)"`, `"Block bogon networks"`, `"Broadcast and multicast"`) gösterilir.

## 0.28.7 - 2026-11-11

### Düzeltildi
- **Uygula (`Apply`):** Her değişiklik uygulandığında görünen gereksiz `"rc.conf remove dummynet_enable"`, `"dnctl_*"` ve `"bsnmpd_*"` satırları giderildi. Bu değişkenler FreeBSD'nin `/etc/defaults/rc.conf` dosyasında varsayılan değere sahip olduğu için sistem bunları kaldırılacak ayar sanıyordu; artık yalnızca `/etc/rc.conf` içinde gerçekten tanımlı olan değişkenler temizlenir.

## 0.28.6 - 2026-11-11

### Eklendi
- **Açılış ve Kapanış Melodileri:** **Sistem > Ayarlar > Genel (`System > Settings > General`)** altında her birinin kendine özgü kapanış versiyonu bulunan melodi seçenekleri eklendi: `Stratum` (varsayılan), `Klasik bip (Classic beep)`, `Çan (Westminster)`, `Neşeye Övgü (Ode to Joy)`, `Für Elise (Beethoven)` ve `Arcade`. Kaydetmeden önce **Test** düğmesiyle seçilen melodi güvenlik duvarı üzerinde dinlenebilir.

## 0.28.5 - 2026-11-11

### Eklendi
- pfSense ve OPNsense'de olduğu gibi donanım hoparlörü (PC speaker) üzerinden **açılış ve kapanış sesi**: güvenlik duvarı tamamen açıldığında yükselen kısa bir melodi, kapatılırken veya yeniden başlatılırken (`Sistem > Güç` dahil) alçalan bir melodi çalar.
  - **Sistem > Ayarlar > Genel:** `"Açılış ve kapanışta ses çal"` seçeneği (varsayılan açık) ve **Test** düğmesi eklendi.
  - Sanal makineler gibi dahili hoparlörü bulunmayan sistemler sessiz kalır.

## 0.28.4 - 2026-11-11

### Eklendi
- **Sistem > Ayarlar > Güvenlik Duvarı ve NAT:** Varsayılan engelleme günlüğü seçeneğinin altına **"WAN üzerinde de (`Also on the WAN`)"** ayarı eklendi. Kapatıldığında WAN arayüzlerindeki varsayılan kurala takılan paketler (internet taramaları ve kapanmış bağlantıların gecikmiş yanıtları) artık günlüğü şişirmez; yerel ağ ve VPN arayüzlerindeki engellemeler kaydedilmeye devam eder. Mevcut davranışı koruma amacıyla varsayılan olarak açıktır.

### Değiştirildi
- **Engelleme Listesi Raporu:** Engelleme anından sonra güncellenen listelerden çıkmış olan adreslere ait eski kayıtlar artık `"Artık listede değil (No longer in a list)"` olarak işaretlenir.

## 0.28.3 - 2026-11-11

### Düzeltildi
- Engelleme listesi (Blocklist) raporu: sorgulanan alan adından önce eşleşen kuralı (`*.example.com.`) ve `rpz-nxdomain` eylemini kaydeden Unbound 1.26 ile engellenen DNS sorguları sayılmıyordu. Sorgulanan alan adı artık doğru şekilde okunuyor. Güncellemeden sonra DNS günlüğü baştan bir kez daha okunarak son günlerdeki engellemelerin raporda görünmesi sağlanır; IP engellemeleri iki kez sayılmaz.
- Web Etkinliği (Web Activity): aynı nedenle cihaz başına engellenen sorgular sayılmıyordu; düzeltildi.
- Yalnızca engelleme eylemleri engelleme olarak sayılır; izin verilen bir alan adı (`rpz-passthru`) engelleme olarak değerlendirilmez.

## 0.28.2 - 2026-11-11

### Eklendi
- Güvenlik Duvarı > Engelleme Listeleri > Engellenenler (Firewall > Blocklists > Blocked): pfBlockerNG raporlarına benzer şekilde, engelleme listelerinin neleri durdurduğunu gösteren kapsamlı rapor.
  - DNS engelleme listelerini, Web Filtresini ve IP engelleme listelerini kapsar.
  - Kaç adet DNS sorgusunun ve IP paketinin (gelen ve giden) engellendiğini ve saatlik veya günlük engelleme grafiğini gösterir.
  - Engellemeyi hangi listenin yaptığını, en çok engellenen alan adlarını ve IP adreslerini ve en fazla engelleme yaşanan cihazları listeler.
  - Engellenen her sorgu ve paket; türe, listeye, alan adına, IP adresine veya cihaza göre aranabilir ve filtrelenebilir.
  - Zaman aralıkları: son 1 saat, 24 saat veya 7 gün. Kayıtlar 7 gün boyunca saklanır.
  - Yalnızca yöneticiler erişebilir; Web Etkinliği'nde olduğu gibi her görüntüleme sistem günlüğüne kaydedilir. Rapor temizlenebilir.
- Kontrol Paneli (Dashboard) "Engelleme Listeleri (Blocklists)" paneli: son 24 saatteki DNS ve IP engellemelerini ve liste bazlı sayıları gösterir. Yöneticiler en çok engellenen alan adlarını ve adresleri de görür. Kontrol Paneli > Paneller menüsünden eklenebilir.
- Rapor verileri her dakika Unbound günlüğünden ve güvenlik duvarı günlüğünden toplanır. Her engelleme, önce kendi özel engelleme listenizden başlayarak ilgili alan adını veya adresi içeren listeyle eşleştirilir.

### Değiştirildi
- Unbound artık yalnızca Web Etkinliği açıkken değil, herhangi bir DNS engelleme listesi veya Web Filtresi etkin olduğunda da engellenen sorguları kendi dosyasına yazar. Web Etkinliği kapalıysa bu dosyaya yalnızca engellenen sorgular kaydedilir. Güncellemeden sonra bir sonraki Uygula (Apply) işleminde devreye girer.
- IP engellemeleri yalnızca IP engelleme listelerinde "Engellenen paketleri günlüğe kaydet" seçeneği açıkken sayılır (varsayılan olarak açıktır).

## 0.28.1 - 2026-11-11

### Eklendi
- Servisler > Servis Durumu (Services > Service Status): güvenlik duvarındaki tüm servisler; durumu (çalışıyor, durduruldu, kapalı), çalışma süresi, bellek kullanımı ve süreç kimliği (PID) ile tek listede görüntülenir. Yöneticiler servisleri doğrudan buradan başlatabilir, durdurabilir ve yeniden başlatabilir.
  - Durdurma işlemi onay için ikinci bir tıklama ister. Durdurulan bir servis tekrar başlatılana, kendisini değiştiren bir sonraki Uygula (Apply) işlemine veya sistem yeniden başlatılana kadar durdurulmuş kalır.
  - Kendi ayarlarında kapalı olan bir servis buradan başlatılamaz; bunun yerine doğrudan ilgili ayar sayfasına bağlantı verilir.
  - Paket filtresi (`pf`), web sunucusu, web paneli, SSH, sistem günlüğü ve zamanlayıcı yalnızca yeniden başlatılabilir. Paket filtresi yeniden başlatıldığında kurallar yeniden yüklenir ve IP engelleme listesi tablosu tekrar doldurulur.
  - Diğer kullanıcı rolleri listeyi görebilir ancak değişiklik yapamaz.

## 0.28.0 - 2026-11-11

### Eklendi
- Bildirimler (sağ üstteki zil simgesi): her kullanıcı tek bir bildirimi veya tümünü okundu olarak işaretleyebilir, tek tek silebilir ya da listeyi tamamen temizleyebilir. Bu işlemler kullanıcıya özeldir; diğer kullanıcıların bildirim listeleri ve olay geçmişi etkilenmez. Temizleme sonrasında gelen yeni olaylar tekrar görünür.
- Bir değişiklik kaydedildikten sonra OPNsense ve pfSense'de olduğu gibi sayfadaki Kaydet (Save) düğmesinin yanında doğrudan bir "Değişiklikleri uygula (Apply changes)" düğmesi belirir. Yalnızca yapılandırma gerçekten değiştiğinde görünür; Wake-on-LAN veya paket yakalama gibi anlık eylemlerden sonra çıkmaz.

### Değiştirildi
- Değişiklikleri uygula / Değişiklikleri koru çubuğu sayfa aşağı kaydırılırken üst çubuğun altında görünür kalmaya devam eder ve kaydetme sonrasında kısaca parlayarak dikkat çeker. Dar ekranlarda sayfanın üstünde sabit kalır.
- Bildirim listesini açmak artık tüm bildirimleri otomatik olarak okundu işaretlemez; bunun için "Tümünü okundu işaretle" düğmesini kullanabilir veya bildirimin üzerine tıklayabilirsiniz.

## 0.27.2 - 2026-11-11

### Eklendi
- Güvenlik duvarı kuralları, politika tabanlı yönlendirme (Policy Routing): dahili bir arayüzdeki (veya dahili arayüz grubundaki) bir izin (pass) kuralı, trafiğini seçilen belirli bir WAN üzerinden gönderebilir (örneğin misafir ağını ikinci internet sağlayıcısı üzerinden çıkarmak).
  - Statik ağ geçidine sahip veya PPPoE kullanan WAN bağlantılarıyla çalışır.
  - Kural kapsamındaki trafiğin kendi ağlarınıza (yerel ağlar, VPN ağları, statik rotalı ağlar) ve doğrudan güvenlik duvarının kendisine giden kısmı normal yönlendirmeyi korur; böylece yerel erişim yanlışlıkla internet sağlayıcısına gönderilmez.
  - Seçilen ağ geçidi çökerse (down): trafik, ağ geçidi geri gelene kadar varsayılan rotayı (yedek hat / failover dahil) kullanır veya tercih edilirse yalnızca o ağ geçidinde kalır. Ağ geçidi izleyicisi kuralları saniyeler içinde yeni hatta geçirir; açık bağlantılar korunur.
  - Kural listesinde her kuralın kullandığı ağ geçidi doğrudan gösterilir.
- OPNsense'den içe aktarma: kural ağ geçitleri eşleşen WAN arayüzüne taşınır. Ağ geçidi grupları ve WAN kurallarındaki ağ geçitleri uyumluluk raporunda belirtilir ve varsayılan rotayı kullanır.

## 0.27.1 - 2026-11-07

### Eklendi
- OPNsense'den içe aktarma, "OpenVPN'i de içe aktar" seçeneği: OpenVPN sunucusu eksiksiz olarak devralınır; böylece kullanıcıların bilgisayarlarındaki mevcut profiller hiçbir değişiklik gerektirmeden bağlanmaya devam eder. OPNsense tarzı bir profille gerçek bağlantı üzerinden test edildi.
  - Devralınan öğeler: özel anahtarıyla birlikte sertifika otoritesi (CA), sunucu sertifikası, `tls-crypt` veya `tls-auth` anahtarı, her kullanıcının sertifikası ve OPNsense parolalarıyla birlikte VPN kullanıcıları.
  - VPN kullanıcıları yalnızca VPN erişimine sahip (VPN-only) hesaplara dönüştürülür.
  - Ayarlar da eksiksiz taşınır: port, protokol, tünel ağı, şifreleme algoritmaları (ciphers), özet (digest), DNS, tam tünel (full tunnel) ve `block-outside-dns`.
  - Bu güvenlik duvarının önceki OpenVPN sertifikaları ayrı bir klasörde saklanır.
  - Sistemde zaten mevcut olan bir kullanıcı, burada ayarlanmış olan parolasını korur.
- OpenVPN kullanıcıları diğer sistemlerden gelen parola özetleriyle (hash) oturum açabilir: PHP'nin yazdığı biçimiyle bcrypt (`$2y$`) ve argon2id. Bir argon2id parolası ilk oturum açmada bcrypt olarak kaydedilir.
- OpenVPN, istemciler için birden fazla adres desteği: yeni profiller her adres için sırayla denenen bir `remote` satırı alır.
- OpenVPN, Windows istemcileri yalnızca VPN'in DNS sunucularını kullanacak şekilde sınırlandırılabilir (`block-outside-dns`).
- OpenVPN, başka sistemler tarafından üretilmiş sertifikalar için kullanıcı sertifikalarındaki "TLS client" denetimi kapatılabilir.

### Düzeltildi
- İki WAN bağlantılı OpenVPN: ikinci WAN adresine bağlanan bir istemci yanıtlarını birinci WAN üzerinden alıyor ve bağlantı kuramıyordu. Sunucu artık istemcinin kullandığı adresten yanıt veriyor (`multihome`) ve `pf` yanıtları geldikleri WAN üzerinden geri gönderiyor.
- OpenVPN profilleri sabit bir isim yerine sunucunun gerçek sertifika adını denetler; bu durum başka bir sistemden devralınan sunucu sertifikalarında kritik önem taşır.

## 0.27.0 - 2026-11-07

### Eklendi
- Arayüzler > Sanal IP'ler (Virtual IPs): bir arayüz üzerinde ek adresler; örneğin bir WAN bloğundaki (`/29`) ek genel (public) IP adresleri.
  - Bir port yönlendirme kuralı hedef adres olarak sanal IP seçebilir; böylece her genel adres farklı bir sunucuya yönlendirilebilir. NAT yansıması (NAT reflection) da bunu takip eder.
  - Giden NAT (Outbound NAT) trafiği bir sanal IP adresine dönüştürebilir.
  - Arayüzün kendi ağı içindeki bir adres, FreeBSD'nin gerektirdiği şekilde `/32` olarak eklenir.
- Arayüzler > Rotalar (Routes): doğrudan bağlı bir ağ üzerindeki ağ geçidi (gateway) aracılığıyla başka bir yönlendiricinin arkasındaki ağlara statik rotalar.
  - Dahili bir arayüzdeki yönlendirici üzerinden ulaşılan ağlar, tıpkı arayüz ağları gibi internete otomatik olarak NAT'lanır.
- Sanal IP'ler ve rotalar `rc.conf` dosyasına yazılmaz. `stratum-netextra` sistemi uygulanan listeyle uyumlu tutar: eksik olanları ekler ve yalnızca kendi eklediklerini kaldırır.
  - Sistem açılışında, her Uygulama (Apply) ve geri alma (rollback) sonrasında ve ayrıca takma adları (alias) ile rotaları düşüren arayüz ve yönlendirme yeniden başlatmalarından sonra çalışır.
  - Her kayıt güvenlik duvarı üzerinde aktif olup olmadığını gösterir.
- OPNsense'den içe aktarma: IP takma adı (IP alias) sanal IP'leri, statik rotalar (isimle çözümlenen ağ geçitleri) ve sanal IP'ye yapılan port yönlendirmeleri taşınır. CARP sanal IP'leri desteklenmiyor olarak listelenir.

## 0.26.1 - 2026-11-07

### Düzeltildi
- Ana bilgisayar adı (host name) ve ülke takma adları (aliases): Uygula (Apply) veya "Şimdi güncelle" (Update now) ile başlatılan güncelleme, o anda 5 dakikalık ana bilgisayar adı güncellemesi çalışıyorsa iz bırakmadan atlanabiliyordu ve yeni bir ülke takma adı bir sonraki günlük çalışmaya kadar boş kalıyordu. Bu güncellemeler artık çalışan işlemin bitmesini bekliyor. Yalnızca 5 dakikalık periyodik çalışma başka bir güncelleme meşgulken atlamaya devam eder.

## 0.26.0 - 2026-11-07

### Eklendi
- Güvenlik Duvarı > Takma Adlar (Aliases), Ana bilgisayar adları (Host names): `update.example.com` gibi ana bilgisayar adlarından oluşan ve aynı zamanda IP adresleri ile ağları da barındırabilen takma adlar.
  - Güvenlik duvarı isimleri her 5 dakikada bir kendi DNS'i üzerinden çözümler; böylece istemcilerle aynı adresleri görür.
  - CDN'ler adreslerini döndürdüğü için bir adres son görüldüğü andan itibaren bir saat boyunca takma ad içinde kalır.
  - Çözümlenemeyen bir isim son adreslerini korur ve sayfa bu durumu gösterir.
- Güvenlik Duvarı > Takma Adlar (Aliases), Ülkeler (Countries): aranabilir bir listeden seçilen ülkelerin tüm adres blokları. Tipik kullanım: "bu port yönlendirmeye yalnızca Türkiye erişebilsin" veya "şu ülkeleri engelle".
  - Veriler beş bölgesel internet kayıt kuruluşundan (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC) alınır. Ücretsizdir ve lisans anahtarı gerektirmez.
  - Haftalık olarak indirilir. Başarısız bir indirme önceki verileri korur.
- Her iki takma ad türü de dosyadan yüklenen `pf` tablolarıdır. Her `pf` yeniden yüklemesi son adresleri okur ve güncellemeler `pf` çalışırken Uygula (Apply) gerektirmeden tabloyu canlı olarak değiştirir.
- Takma ad listesi bu takma adların her birinin kaç adres tuttuğunu ve en son ne zaman güncellendiğini gösterir. "Şimdi güncelle" (Update now) düğmesi hepsini tek seferde yeniler.
- OPNsense'den içe aktarma: ana bilgisayar adları içeren takma adlar ana bilgisayar adı takma adına, GeoIP takma adları ise ülke takma adına dönüştürülür. Artık boş olarak içe aktarılmazlar.

### Düzeltildi
- Takma ad sayfası sekmeleri artık telefon ekranlarında taşma yapmıyor.

## 0.25.1 - 2026-11-07

### Eklendi
- Sistem > Güç (Power):
  - Onay adımıyla birlikte yeniden başlatma ve kapatma. Yeniden başlatma sonrasında sayfa kendiliğinden tekrar bağlanır.
  - Fabrika ayarlarına sıfırlama. Sıfırlama işlemi o anda kullanımda olan portları korur; böylece uzaktaki bir güvenlik duvarı erişilebilir şekilde geri döner: WAN DHCP ile, LAN `192.168.1.1/24` DHCP açık olarak, yönetim paneli yalnızca dahili ağlardan erişilebilir biçimde.
  - Sıfırlama seçenekleri: panel kullanıcılarını koru veya yeni bir "admin" parolasıyla sıfırdan başla; sertifikaları, anahtarları ve gizli bilgileri sil; günlükleri ve istatistikleri sil.
  - Sıfırlama işlemi ana bilgisayar adı (hostname) yazılarak onaylanır. Geri alma zamanlayıcısı olmadan uygulanır ve güvenlik duvarı yeniden başlar.
  - Yalnızca yöneticiler kullanabilir; her işlem günlüğe kaydedilir.
- Tanılama > ARP Tablosu (ARP Table):
  - Güvenlik duvarının bildiği her adres; MAC adresi, arayüz, geçerlilik süresi ve ismiyle (DHCP rezervasyonları, kiralamaları ve DNS geçersiz kılmalarından) listelenir.
  - Telefonların ve dizüstü bilgisayarların özel (rastgele) MAC adresleri işaretlenir.
  - Bir kayıt silinebilir.
- ARP Tablosu sayfasında Wake-on-LAN: bir cihazı MAC adresiyle veya DHCP rezervasyonu olan cihazlar arasından tek tıkla uyandırın. Operatörler de kullanabilir.
- Tanılama > Paket Yakalama (Packet Capture): bir arayüzü Wireshark için `.pcap` dosyasına kaydedin.
  - Filtre serbest metin yerine alanlardan (protokol, ana bilgisayar veya ağ, port) oluşturulur.
  - Süre, boyut ve paket sayısı sınırları; aynı anda tek bir yakalama işlemi.
  - Yöneticinin kendi panel ve SSH trafiği varsayılan olarak hariç tutulur.
  - Dosya yalnızca root erişimine açık bir klasörde en fazla 24 saat saklanır.
  - Yalnızca yöneticiler kullanabilir; başlatma, durdurma ve indirme işlemleri günlüğe kaydedilir.

### Düzeltildi
- Durumlar (States) ve ARP Tablosu sayfalarındaki arama kutularında stil eksikliği giderildi.

## 0.25.0 - 2026-11-07

### Eklendi
- Sistem > Yedekleme > OPNsense'den İçe Aktar: bir OPNsense `config.xml` dosyası bekleyen yapılandırma haline gelir. İçe aktarma işlemi arayüzleri, VLAN'ları, ağ geçitlerini ve varsayılan ağ geçidini, arayüz gruplarını, takma adları (iç içe takma adlar çözümlenerek), güvenlik duvarı kurallarını (güncel ve eski kural biçimi), port yönlendirmeleri, giden NAT kurallarını, DHCP aralıklarını ve rezervasyonlarını (`dnsmasq` ve `ISC dhcpd`), DNS ana bilgisayar ve alan adı geçersiz kılmalarını, bant genişliği sınırlarını, ana bilgisayar adını, alan adını, saat dilimini ve DNS ile zaman sunucularını taşır.
- Herhangi bir şey kaydedilmeden önce içe aktarma bir önizleme gösterir:
  - OPNsense cihazının ağ portları bu sistemin portlarıyla eşleştirilir ve istenen port dışarıda bırakılabilir;
  - Bir rapor nelerin içe aktarıldığını, aktarım sırasında nelerin değiştiğini ve nelerin hangi nedenle içe aktarılamadığını listeler.
- Port yönlendirmeleri erişim sınırlarını korur. OPNsense'de bir port yönlendirme genellikle ayrı WAN kurallarıyla izinlendirilir. Stratum'da port yönlendirme kendi trafiğine doğrudan izin verdiği için kaynak sınırı bu WAN kurallarından alınır ve birden fazla kural izin veriyorsa birleşik bir takma ad oluşturulur. Hiçbir kuralın izin vermediği bir yönlendirme kapalı olarak içe aktarılır ve asla herkese açılmaz.
- Taşınamayan takma adlar (URL tabloları) boş olarak içe aktarılır; böylece bunları kullanan engelleme kuralları hiçbir şeyi engellemez ve izin kuralları hiçbir şeye izin vermez. Bunlar raporda listelenir.
- İçe aktarma işlemi LAN adresi değiştiğinde ve içe aktarılan bir ağ güvenlik duvarının mevcut WAN adresini içerdiğinde (uygulandığında bağlantıyı keseceği için) uyarı verir.
- Dosya yalnızca okunur; asla saklanmaz ve raporda hiçbir gizli bilgi yer almaz. Kullanıcılar, sertifikalar, IPsec, WireGuard ve eklentiler içe aktarılmadı olarak listelenir. İşlem yalnızca yöneticilere açıktır ve günlüğe kaydedilir.

### Düzeltildi
- Geniş tablolara sahip form sayfaları artık telefonlarda yana doğru kaymıyor.

## 0.24.4 - 2026-11-07

### Eklendi
- Saldırı Tespit Sistemi (Intrusion Detection): İzleme (Watch) ve Engelleme (Block) modları artık pfSense'de olduğu gibi arayüz bazında ayrı ayrı ayarlanabilir. Örneğin WAN yalnızca izlenirken yerel ağlar engelleme yapabilir. İkisini birlikte kullanmak iki Suricata motoru çalıştırır: engelleme yapılan arayüzler için `netmap` üzerinde çalışan bir motor ve yalnızca izlenen arayüzler için kopyaları (`pcap`) okuyan bir motor. Suricata süreç başına tek bir yakalama yöntemine izin verir. Her iki motor da aynı kuralları yükler ve her biri bağımsız olarak denetlenip yeniden başlatılır. İkisinin uyarıları ve istatistikleri birlikte gösterilir; günlük döndürme ve WAN adresi güncellemesi her ikisini de kapsar.
- İki motor kullanıldığında kurallar iki kez yüklendiği için bellek kullanımı yaklaşık iki katına çıkar. Her motora daha küçük bellek sınırları verilir ve ayarlar sayfası bunu belirtir. Uygula (Apply) işlemi her iki yapılandırmayı da kullanmadan önce Suricata ile denetler.

### Düzeltildi
- Yapay zeka asistanı (MCP): saldırı tespit aracı her zaman "yalnızca tespit" (detection only) bildiriyordu. Artık engelleme yapan arayüzleri ve kategorileri de listeliyor.

## 0.24.3 - 2026-11-07

### Düzeltildi
- Saldırı Tespit Sistemi: kural kategorilerinde yapılan değişiklikler (açık, kapalı, Engelle) çalışan Suricata'ya yeniden başlatılana kadar ulaşmıyordu. Canlı kural yenileme işlemi Linux'ta `SIGUSR2` olan ancak FreeBSD'de `SIGSYS` anlamına gelen 12 sinyalini gönderiyordu. Artık doğrudan `SIGUSR2` kullanıyor.
- Saldırı Tespit Sistemi: bazı arayüzlerde Engelle (Block) ile diğerlerinde İzle (Watch) seçilmesi net bir mesajla reddediliyordu. Suricata tek bir `netmap` sürecinde engelleme ve yalnızca izleme portlarına birlikte izin vermediği için başlatılamıyor ve trafik akmaya devam ederken hiçbir denetim yapılmıyordu.
- Kontrol Paneli (Dashboard): kablosu takılı olmayan bir arayüz "Açık" (Up) olarak görünüyordu; artık "Kablo yok" (No cable) olarak gösteriliyor.

### Eklendi
- Saldırı Tespit Sistemi: ET Open "Policy" (bir ağın izin vermeyebileceği yazılım, araç ve servisler) kategorisi varsayılan olarak kapalı biçimde eklendi.

## 0.24.2 - 2026-11-06

### Düzeltildi
- Saldırı Tespiti, engelleme modu (IPS): Suricata, başlangıcını görmediği her bağlantıyı düşürüyordu. Bunlar,
  kendisi başlamadan önce açılmış bağlantılar ve telefonların açık tuttuğu anlık bildirim kanalları gibi akış
  zaman aşımından daha uzun süre boşta kalan uzun ömürlü bağlantılardı. Engelleme ağındaki telefonlar
  bağlantılarını kaybetti ve Wi-Fi'den düştü. Suricata artık bu tür bağlantıları (akış ortasında) yakalıyor
  ve herhangi bir istisnada (`exception-policy: ignore`) akışı düşürmek yerine trafiğin geçmesine izin
  veriyor. Yalnızca engelleme kurallarıyla eşleşenler düşürülür.

## 0.24.1 - 2026-11-05

### Düzeltildi
- Web Filtresi: kategori listeleri ilk Apply'dan sonra indirilmiyordu, bu yüzden politikalar bir sonraki
  zamanlanmış güncellemeye kadar hiçbir şeyi engellemiyordu. İndirme artık DNS engelleme listelerinde olduğu
  gibi kaydedilen yapılandırmayı takip ediyor ve Unbound yalnızca yüklediği bölgeleri yeniden yüklüyor.

## 0.24.0 - 2026-11-05

### Eklendi
- **Web Filtresi** (Firewall > Web Filter), güvenlik duvarının DNS çözümleyicisi (Unbound RPZ) üzerinden site
  kategorilerini engeller ve ağ başına farklı kurallar uygular.
  - **Politikalar:** Bir politika, bir veya daha fazla ağa engellenen kategorilerden oluşan bir set atar. VPN
    istemci ağları da politika ağı olabilir ve her ağ yalnızca bir politikaya bağlıdır.
  - **Kategoriler:** Université Toulouse Capitole'ün UT1 kara listelerinden (CC BY-SA 4.0, günlük güncellenir)
    ve Block List Project'ten alınan gruplar halinde 55 kategori (yetişkin içerik, güvenlik tehditleri,
    filtreyi atlatma yolları, sosyal ağlar, eğlence, alışveriş ve para, indirmeler ve daha fazlası).
    Yetişkin içerik kompakt ve çok büyük tam liste olarak gelir; politika sayfası seçilen kategorilerin
    çözümleyici belleğinde ne kadar yer kaplayacağını tahmin eder.
  - **Güvenli Arama (Safe Search):** Her politika, Google (google.com.tr ve diğer ülke alan adları dahil),
    Bing ve DuckDuckGo üzerinde güvenli sonuçları ve YouTube'da kısıtlı modu zorunlu kılabilir.
  - **İstisnalar:** Listelenen siteler web filtresi tarafından asla engellenmez ve Apply gerektirmeden anında
    etkili olur. DNS kara listeleri onlar için hâlâ geçerlidir.
  - **Bir siteyi kontrol et:** Bir adın hangi kategorilerde olduğunu ve hangi politikaların onu engellediğini
    gösterir.
  - **Engelleme raporu:** Bugün, 7 veya 30 gün için kategori, site ve cihaz başına engellemeler. Web Etkinliği
    gerektirir; toplayıcısı artık web filtresi ve DNS kara liste engellemelerini de sayar. Raporu yalnızca
    yöneticiler görür ve görüntüleme günlüğe kaydedilir.
  - **Güncellemeler:** Kategori listeleri zamanlamaya göre (varsayılan olarak günlük) ve talep üzerine
    güncellenir; başarısız bir indirme önceki listeyi korur.

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
