---
title: "OPNsense'ten Geçiş"
seo_title: "12 OPNsense'ten Geçiş (Migrating from OPNsense) — Stratum FW Kullanım Kılavuzu"
description: "OPNsense config.xml dosyasını içe aktarma: taşınan ve taşınmayan ayarlar, ağ portlarını eşleştirme, önizleme ve dönüşüm raporu, kullanıcıların mevcut profillerini bozmadan OpenVPN'i devralma ve kesintisiz canlı geçiş planı."
weight: 1201
chapter_num: "12"
url: "/tr/manual/migrating-from-opnsense/"
aliases:
  - "/tr/manual/12-01-migrating-from-opnsense/"
  - "/tr/manual/opnsense-migration/"
translationKey: "manual-12-01-migrating-from-opnsense"
---

**System > Backup > Import from OPNsense** aracı bir OPNsense yapılandırmasını doğrudan Stratum FW yapılandırmasına dönüştürür: eski güvenlik duvarının arayüzleri, kuralları, NAT tanımları, takma adları, DHCP ve DNS ayarları yeni sistemde kullanıma hazır hale gelir. OpenVPN sunucusu da sertifikalarıyla birlikte eksiksiz devralınabildiği için **kullanıcıların bilgisayarlarındaki mevcut VPN profilleri hiçbir değişiklik yapılmadan bağlanmaya devam eder**.

![OPNsense'ten İçe Aktarma](/images/manual/opnimport.png)

## Neler içe aktarılır?

| İçe aktarılanlar | Notlar |
| :--- | :--- |
| Sistem adı, alan adı, saat dilimi, DNS ve zaman sunucuları | |
| Arayüzler ve VLAN'lar | Bu sistemin fiziksel portlarına eşleştirilir (aşağıda). Statik, DHCP ve **PPPoE** (kullanıcı adı ve parolasıyla birlikte). |
| Ağ geçitleri (Gateways) | Öncelikler (yedeklilik sırası), izleme adresleri ve varsayılan ağ geçidi. |
| Sanal IP'ler (VIP), statik rotalar | |
| Arayüz grupları | |
| Takma adlar (Aliases) | Tekil adresler, ağlar, portlar; **alan adları** FQDN takma adına, **GeoIP** tanımları ülke takma adına dönüşür. Bilinen bir engelleme listesiyle eşleşen URL tabloları ilgili **IP engelleme listesini** açar; diğer harici tablolar boş olarak aktarılır. |
| Güvenlik duvarı kuralları | Hem yeni *Firewall > Rules* modeli hem klasik kurallar; ağ geçidi seçili kurallar politika tabanlı yönlendirmeye dönüşür. |
| Port yönlendirmeler ve giden NAT | Aşağıdaki *Port yönlendirmeler* notuna bakınız. Güvenlik duvarına yapılan DNS yönlendirmeleri **Force DNS** ayarına dönüşür. |
| DHCP | Dağıtım aralıkları ve statik MAC eşlemeleri (`dnsmasq` ve ISC DHCP). |
| DNS | Host ve domain override kayıtları; DNS engelleme listeleriyle birlikte Unbound. |
| Bant genişliği sınırları | Arayüz başına Shaper hız sınırları. |
| Bogon ve özel ağ engellemesi, NAT yansıması | |
| **OpenVPN** (isteğe bağlı) | Sertifika otoritesi (CA), sunucu sertifikası, TLS anahtarı, kullanıcı sertifikaları ve parolalarıyla birlikte VPN kullanıcıları. |

**İçe aktarılmayanlar** - geçiş sonrasında yeniden tanımlayın: web paneli yöneticileri (VPN kullanıcıları OpenVPN ile birlikte gelir), **IPsec**, **WireGuard**, Captive Portal, port birleştirme (LAGG), köprüler (bridge) ve üçüncü taraf eklentiler (plugins).

**Bu güvenlik duvarının kendi sistem ayarları korunur**: mevcut panel kullanıcıları, yönetim erişimi, bildirimler, yedekleme ayarları, saldırı tespit, Web Filtresi ve diğer servisler silinmez.

Yüklenen `config.xml` dosyası yalnızca okunur; diske kaydedilmez ve önizleme raporunda gizli parola/anahtar gösterilmez.

## Başlamadan önce

- Yeni donanıma Stratum FW kurun ve ilk açılış sihirbazını tamamlayın ([**Kurulum**](/tr/manual/installation/), [**İlk Yapılandırma**](/tr/manual/initial-configuration/)). Cihazı şimdilik yalnızca kendi bilgisayarınıza bağlayın - henüz canlı üretim ağına takmayın.
- OPNsense üzerinde **System > Configuration > Backups > Download configuration** adımıyla **şifrelenmemiş** `config.xml` dosyasını indirin. Dosya gizli anahtarları içerdiği için güvenli tutun.
- Hangi OPNsense portunun (`igb0`, `igb1` ...) hangi hattı taşıdığını not edin.

## Adım adım içe aktarma

### 1. Dosyayı okutma

**Choose file** ile `config.xml` dosyasını seçip **Read the file** düğmesine tıklayın.

### 2. Ağ portlarını eşleştirme (Map the network ports)

**Network ports** tablosu her OPNsense portunu, o portun neleri taşıdığını (arayüzler, VLAN'lar) ve bu cihazdaki karşılığını (**Port here**) listeler. Birebir aynı donanımda port adları genellikle kendiliğinden örtüşür; farklı bir donanıma geçiyorsanız o kablonun takılacağı yeni portu seçin.

Eşleştirilmeden boş bırakılan bir port - üzerindeki arayüzler, VLAN'lar ve kurallarla birlikte - içe aktarılmaz.

Eşleştirmeyi değiştirdikten sonra **Update the preview** düğmesine tıklayın.

### 3. Önizlemeyi inceleme

Önizleme ekranı şunları gösterir:

- **Interfaces** - her arayüzün portu, türü ve IP adresi;
- **Changed while importing** - OPNsense'ten farklı biçimde dönüştürülen her öğe ve nedeni: yeniden adlandırılan takma adlar, kapalı olarak aktarılan kurallar, kaynak eklenen port yönlendirmeler, taşınmayan zamanlamalar ...;
- **Not imported** - dışarıda bırakılan öğeler ve nedenleri;
- en üstte önemli **uyarılar** - örneğin *LAN adresi ... olarak değişiyor* (Apply sonrasında yeni LAN ağından bağlanın) veya *Bu güvenlik duvarının mevcut WAN adresi içe aktarılan iç ağların içinde kalıyor* (Apply öncesinde WAN kablosunu gerçek hatta takın, aksi halde erişim kesilir).

**Changed while importing** listesini dikkatle okuyun - geçiş sonrasında kontrol edeceğiniz noktaların tam listesidir.

### 4. OpenVPN devralma

Sunucuyu eksiksiz devralmak için **Also import OpenVPN** kutusunu işaretleyin: aynı CA, sunucu sertifikası, TLS anahtarı, kullanıcı sertifikaları ve parolaları aktarılır. Böylece Stratum FW eski güvenlik duvarının dış adresini aldığında kullanıcıların mevcut profilleri hiçbir değişiklik gerekmeden bağlanır.

- Sertifikalar hemen yazılır (bu güvenlik duvarının önceki sertifikaları ayrı bir klasörde yedeklenir); sunucu ayarları ise Apply işlemini bekler.
- Henüz burada bulunmayan VPN kullanıcıları OPNsense parolalarıyla **yalnızca VPN (VPN-only)** hesabı olarak oluşturulur. Burada zaten var olan kullanıcıların mevcut parolaları korunur.
- İptal edilmiş sertifika listeleri (CRL) taşınmaz: OPNsense'te iptal edilmiş sertifikalar varsa geçiş sonrasında [**VPN > OpenVPN**](/tr/manual/vpn-openvpn/) altından tekrar iptal edin. Devralınamayan eski özet biçimindeki parolalar raporda listelenir - bunları [**System > Users**](/tr/manual/system-users/) sayfasından yeniden belirleyin.

### 5. İçe aktarma ve uygulama

**Replace the interfaces, rules, NAT, aliases, DHCP and DNS settings with the imported ones** kutusunu işaretleyip **Import** düğmesine tıklayın.

Sonuç **bekleyen yapılandırma (pending configuration)** olarak yüklenir: siz **Apply** diyene kadar hiçbir şey değişmez ve Apply sonrasında da geri alma güvencesi çalışır. İçe aktarma öncesindeki yapılandırma da geçmişte ([**System > Backup**](/tr/manual/system-backup/)) saklanır.

## Kurallar nasıl dönüştürülür?

**Port yönlendirmeler.** OPNsense'te bir port yönlendirme çoğunlukla *ayrı* bir WAN güvenlik duvarı kuralıyla içeri alınır. Stratum FW'de ise port yönlendirme kendi izinli kaynağından gelen trafiği doğrudan geçirir; bu nedenle her port yönlendirmenin kaynak alanı ilgili trafiğe izin veren OPNsense WAN kurallarından otomatik alınır. Hiçbir WAN kuralının izin vermediği bir yönlendirme güvenlik amacıyla **kapalı (switched off)** olarak aktarılır.

**Taşınamayan takma adlar** boş takma ad olarak oluşturulur: bunları kullanan engelleme kuralları hiçbir şeyi engellemez, izin kuralları ise hiçbir şeye izin vermez - böylece kurallar güvenli anlamını korur.

**İfade edilemeyen kurallar** - IPv6, *out* (çıkış) yönlü kurallar, kaynak port kısıtlamaları, tek kuralda birden çok arayüz veya bilinmeyen protokoller - atlanır ve raporda tek tek listelenir. OpenVPN ve WireGuard arayüz kuralları da atlanır; VPN istemcilerinin erişebileceği ağlar [**VPN > OpenVPN**](/tr/manual/vpn-openvpn/) altından seçilir.

**Zamanlamalı (schedule) kurallar** kapalı olarak aktarılır: zamanlamayı [**Firewall > Schedules**](/tr/manual/firewall-schedules/) sayfasından oluşturup kuralda seçtikten sonra kuralı etkinleştirin.

## Çalışan güvenlik duvarını kesintisiz değiştirme planı

Kesintiyi birkaç dakikayla sınırlayan ve geri dönüş yolunu her an açık tutan geçiş planı:

1. Yeni güvenlik duvarını masada çevrimdışı **hazırlayın**: içe aktarın, Apply ve **Keep changes** yapın. **Interfaces**, **Firewall > Rules**, **NAT**, **Services > DHCP Server** ve **VPN > OpenVPN** sayfalarını eski cihazla ve *Changed while importing* listesiyle karşılaştırın.
2. Taşınmayan bileşenleri ekleyin: IPsec ve WireGuard tünelleri, Captive Portal ve diğer panel yöneticileri.
3. Yeni güvenlik duvarının **tam bir yedeğini indirin**.
4. Sakin bir saatte **ağ kablolarını** eşleştirdiğiniz port sırasıyla eski güvenlik duvarından yeni cihaza aktarın.
5. Servis sağlayıcı modemi veya üst yönlendirici eski cihazın MAC adresini kilitlemişse modemi yeniden başlatın ya da yeni MAC adresini öğrenmesini bekleyin.
6. Test edin: her alt ağdan internet erişimi, DHCP, DNS, dışarıdan port yönlendirmeler, mevcut `.ovpn` profiliyle bir VPN bağlantısı ve kontrol panelindeki ağ geçitleri.
7. **Geri dönüş güvencesi:** eski güvenlik duvarını birkaç gün olduğu gibi saklayın. Beklenmedik bir durumda kabloları eski cihaza geri takmak saniyeler içinde eski duruma döndürür.

Geçişin hemen ardından geçmesi gereken meşru trafiğin engellenip engellenmediğini görmek için [**Diagnostics > Firewall Log**](/tr/manual/diagnostics-firewall-log/) ekranını izleyin.
