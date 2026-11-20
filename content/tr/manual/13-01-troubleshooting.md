---
title: "Sorun Giderme"
seo_title: "13 Sorun Giderme (Troubleshooting) — Stratum FW Kullanım Kılavuzu"
description: "Web paneline ulaşılamadığında, internet veya DNS çalışmadığında, değişiklik geri alındığında, port yönlendirme veya VPN bağlanmadığında, bir site engellendiğinde ya da ağ yavaşladığında kontrol edilecek sayfalar ve komutlar."
weight: 1301
chapter_num: "13"
url: "/tr/manual/troubleshooting/"
aliases:
  - "/tr/manual/13-01-troubleshooting/"
translationKey: "manual-13-01-troubleshooting"
---

Karşılaşılan sorunların büyük çoğunluğu şu dört ekranda bulunur:

| Ekran | Neyi gösterir? |
| :--- | :--- |
| [**Dashboard**](/tr/manual/dashboard/) | Ağ geçitleri, servisler, arayüzler ve uyarılar - genel bakış |
| [**Diagnostics > Firewall Log**](/tr/manual/diagnostics-firewall-log/) | Hangi paketin hangi kural tarafından engellendiği |
| [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) | Servislerin bildirdiği iletiler; her Apply işlemi için **Configuration** sekmesi |
| [**Diagnostics > Ping / Traceroute / DNS**](/tr/manual/diagnostics-tools-states-arp/) | Güvenlik duvarının kendisinin nereye ulaşabildiği |

[**Yapay Zeka Asistanı**](/tr/manual/diagnostics-ai-assistant/) bu ekranları sizin yerinize tarayıp özetleyebilir: *"192.168.1.50 neden internete çıkamıyor?"*

## Web paneline ulaşılamıyor

| Kontrol | Nasıl yapılır? |
| :--- | :--- |
| Doğru adres ve port mu? | [Konsol menüsünün](/tr/manual/console-menu/) üst başlığı panel adreslerini (`https://...`) gösterir. [**Admin Access**](/tr/manual/system-settings-general-access/) altından değiştirilmediyse port `443`'tür. |
| Doğru ağdan mı bağlanıyorsunuz? | Panel iç ağlardan yanıt verir; WAN'dan yalnızca açıkça izin verildiyse açılır. **Admin Access** altında *Only the selected interfaces* seçiliyse diğer iç ağlar da reddedilir. |
| Hatalı giriş denemeleri sonrası engellendiniz mi? | Engelleme süresinin dolmasını bekleyin (varsayılan 15 dakika) ya da farklı bir IP adresinden deneyin. |
| Az önce yaptığınız değişiklik mi bağlantıyı kesti? | Bekleyin: onaylanmayan değişiklikler süre sonunda kendiliğinden geri alınır. Ya da konsol menüsünde **7) Pending change** > **n** seçin. |
| Panel servisi mi durdu? | Konsol menüsünde **4) Restart web panel**. |
| Yönetim erişimi ayarları mı hatalı? | Konsol menüsünde **6) Management access** > **2)** portu, ağları, *anti-lockout* kuralını ve SSH'ı varsayılana döndürür. |
| Parola unutuldu veya 2FA telefonu kayboldu mu? | Konsol menüsünde **3) Reset admin password** (iki aşamalı doğrulamayı kapatmayı da teklif eder). |
| Sertifika değişikliği sonrası tarayıcı bağlantıyı reddediyor mu? | Konsol **8) Shell**: `stratum-cert restore`. |

## İnternet erişimi yok

Güvenlik duvarından dışarıya ve istemciye doğru sırasıyla kontrol edin:

1. **Dashboard > Gateways**: WAN hattı *online* mı? *Down* görünüyorsa güvenlik duvarı kendi ağ geçidine ulaşamıyordur - kabloyu, modemi ve servis sağlayıcıyı kontrol edin. PPPoE kullanıyorsanız **System Logs > PPPoE** sekmesine bakın (*authentication failed* = kullanıcı adı veya parola hatası).
2. Güvenlik duvarından `1.1.1.1` adresine **Ping** atın. Çalışıyorsa güvenlik duvarının kendi internet çıkışı sağlamdır.
3. `freebsd.org` için **DNS lookup** çalıştırın. Ping çalışırken DNS başarısız oluyorsa aşağıdaki DNS bölümüne geçin.
4. İstemci cihazı kontrol edin: doğru alt ağdan IP adresi almış mı, ağ geçidi ve DNS olarak güvenlik duvarını mı görüyor (`ipconfig /all`, `ip a`)? IP alamıyorsa [**Services > DHCP Server**](/tr/manual/services-dhcp-server/) altında ilgili ağ açık mı ve adres havuzu dolu mu kontrol edin.
5. Cihazın bulunduğu ağın [**Firewall > Rules**](/tr/manual/firewall-rules/) sayfasında internete izin veren *pass* kuralı var mı? **Firewall Log** ekranını cihazın IP adresiyle süzün: paketleri hangi kural engelliyor?
6. [**Firewall > NAT > Outbound**](/tr/manual/firewall-nat/): otomatik giden NAT arayüz ağlarını ve statik rotalı ağları kapsar; hibrit veya manuel modda elle eklenen ağlar için ayrıca kural gerekir.

## DNS çalışmıyor

| Belirti | Kontrol edilecek nokta |
| :--- | :--- |
| Güvenlik duvarının kendisi dahil hiçbir ad çözülmüyor | [**Services > Service Status**](/tr/manual/services-service-status/): DNS çözümleyici çalışıyor mu? [**Services > DNS Resolver**](/tr/manual/services-dns-resolver/): üst DNS sunucuları erişilebilir mi (ping atın)? *Resolver* modunda WAN hattının internetteki kök DNS sunucularına çıkışına izin verilmelidir. |
| Güvenlik duvarı çözüyor ama cihazlar çözemiyor | Cihazlar DNS sunucusu olarak güvenlik duvarını almalıdır ([**Services > DHCP Server**](/tr/manual/services-dhcp-server/): DNS servers alanı boş). Arayüz kuralları güvenlik duvarına DNS (port 53) izni vermelidir. Alt yönlendirici arkasındaki ağlara ([**Interfaces > Routes**](/tr/manual/interfaces-routes/)) Unbound henüz doğrudan yanıt vermez - bu ağlara şimdilik ayrı DNS sunucusu tanımlayın. |
| Yalnızca tek bir site çözülmüyor | [**Firewall > Blocklists**](/tr/manual/firewall-blocklists/) ve [**Firewall > Web Filter**](/tr/manual/firewall-web-filter/) sayfalarındaki sorgulama araçlarıyla alan adının hangi liste veya kategori tarafından engellendiğini kontrol edin. |
| Yerel iç adlar çözülmüyor | [**Services > DNS Resolver**](/tr/manual/services-dns-resolver/): *host overrides* ve yerel alan adı (`Local domain`); Active Directory için etki alanı denetleyicisine *domain override* ekleyin. |
| Değişiklikten sonra eski yanıt geliyor | Cihazlar yanıtları birkaç dakika önbellekte tutar - Windows'ta `ipconfig /flushdns` çalıştırın. |

## Yaptığım değişiklikler kendi kendine geri alındı

Süresi içinde **Keep changes** düğmesine tıklanmamıştır - ya da yaptığınız değişiklik bağlantınızı kestiği için tarayıcınız onay gönderememiştir. Güvenli uygulama koruması tam olarak görevini yapmış ve önceki çalışan ayarları geri yüklemiştir.

- [**Diagnostics > System Logs > Configuration**](/tr/manual/diagnostics-system-logs/) sekmesi yapılan Apply işlemini ve *automatic rollback* kaydını gösterir.
- Değişikliği bağlantınızı kesmeyecek şekilde yeniden uygulayın - örneğin LAN IP adresini değiştirirken Apply deyin, bilgisayarınızı yeni ağa alın, paneli yeni IP adresinden açıp süre dolmadan **Keep changes** ile onaylayın.
- Değişiklik geri alınmak yerine doğrudan **Apply başarısız** mı oldu? Aynı günlük nedenini açıkça yazar; sistemde hiçbir şey değiştirilmemiştir.

## Port yönlendirme çalışmıyor

1. LAN içinden değil **dışarıdan** (örneğin telefonun mobil verisinden) test edin - içeriden erişim için yerel IP adresini veya *host override* / NAT yansımasını kullanın.
2. [**Firewall > NAT > Port Forward**](/tr/manual/firewall-nat/): kural etkin mi, **Source** alanı test yaptığınız dış adrese izin veriyor mu, hedef iç IP ve port doğru mu?
3. **Firewall Log** ekranını port numarasıyla süzün: WAN üzerinde engelleniyor mu? Öyleyse yönlendirmenin kaynak kısıtlaması uyuşmuyordur. Günlükte hiçbir paket görünmüyor mu? Trafik güvenlik duvarına hiç ulaşmıyordur: servis sağlayıcı modemi yönlendirici modundaysa portu orada da yönlendirin (veya modemi *bridge* moduna alın). Bazı servis sağlayıcılar `25`, `80`, `443` gibi portları şebeke düzeyinde kapatabilir.
4. İç sunucu yanıt veriyor mu? Sunucunun varsayılan ağ geçidi bu güvenlik duvarı olmalı ve sunucunun kendi yerel güvenlik duvarı ilgili porta izin vermelidir.

## VPN bağlanmıyor

[**VPN > OpenVPN**](/tr/manual/vpn-openvpn/), [**VPN > WireGuard S2S**](/tr/manual/vpn-wireguard-s2s/) ve [**VPN > IPsec**](/tr/manual/vpn-ipsec/) bölümlerindeki sorun giderme tablolarına bakınız. Özetle:

- dış adres ve port dışarıdan erişilebilir olmalı (modem arkasındaysa yönlendirilmiş olmalı);
- **System Logs > Authentication** (OpenVPN girişleri), **IPsec log** ve WireGuard son el sıkışma zamanını (*Last handshake*) kontrol edin;
- VPN istemcilerinin erişebileceği yerel ağlar işaretlenmiş olmalı ve VPN tünel ağı kullanıcının ev ağıyla çakışmamalıdır.

## Bir web sitesi veya uygulama engelleniyor

1. Sitenin alan adıyla **Firewall > Web Filter > Check a site** ve **Firewall > Blocklists > Check a domain** araçlarını çalıştırın: hangi kategori veya liste engelliyor?
2. İlgili sayfadan istisna (*Exception* / *Allow list*) ekleyin ya da o kategoriyi/listeyi kapatın.
3. **Firewall Log**: bağlantı bir IP engelleme listesi veya güvenlik duvarı kuralı tarafından mı durduruluyor?
4. [**Services > Intrusion Detection > Alerts**](/tr/manual/services-intrusion-detection/): engelleme modundaki bir arayüzde Suricata mı düşürüyor? Yanlış pozitifse ilgili kuralda **Disable rule** kullanın.

## Güvenlik duvarı veya internet yavaş

| Kontrol | Bakılacak yer |
| :--- | :--- |
| CPU, bellek ve işlemci sıcaklığı | [**Dashboard**](/tr/manual/dashboard/) |
| Hat kapasitesinin dolması | [**Diagnostics > Traffic Graphs**](/tr/manual/diagnostics-traffic-graphs/) - tepe (*peak*) hızların hat sınırına dayanması |
| Kapasiteyi hangi cihazın tükettiği | [**Diagnostics > States**](/tr/manual/diagnostics-tools-states-arp/), [**Web Activity**](/tr/manual/diagnostics-web-activity/) |
| Çok fazla eşzamanlı bağlantı | [**System > Settings > Firewall & NAT**](/tr/manual/system-settings-firewall-networking-tunables/): kullanımdaki durum sayısının üst sınıra oranı |
| Block modunda Saldırı Tespit (IPS) | Suricata yüksek CPU kullanabilir: kategori sayısını azaltın veya çok yoğun arayüzlerde *Watch* kipini seçin |
| Hat hızının altında geçiş performansı | [**System > Settings > Networking**](/tr/manual/system-settings-firewall-networking-tunables/): önce LRO, gerekirse TSO donanım hızlandırmasını kapatın |

## Yardım isterken paylaşılacak bilgiler

Bir sorunu bildirirken (GitHub üzerinde veya iş arkadaşınıza) şunları ekleyin:

- sürüm numarası (kontrol panelinin üst çubuğunda) ve donanım modeli;
- ne yaptığınız, ne beklediğiniz ve gerçekte ne olduğu;
- **System Logs** veya **Firewall Log** içindeki ilgili satırlar - konu Apply ile ilgiliyse `stratum-config diff` çıktısı.

**Paylaşmadan önce genel IP adreslerini, alan adlarını ve kullanıcı adlarını maskeleyin.** Parolaları, özel anahtarları, jetonları, OPNsense `config.xml` dosyasını veya yedek dosyalarını kesinlikle herkese açık paylaşmayın.
