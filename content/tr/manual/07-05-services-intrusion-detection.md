---
title: "Servisler > Saldırı Tespit ve Önleme (IDS/IPS)"
seo_title: "07.5 Servisler > Saldırı Tespit (Services > Intrusion Detection) — Stratum FW Kullanım Kılavuzu"
description: "ET Open kurallarıyla Suricata: arayüz başına izleme (Watch) veya engelleme (Block), kural kategorileri, güncellemeler, uyarılar, gürültülü kuralları kapatma, bildirimler, bellek ve fail-open davranışı."
weight: 705
chapter_num: "07.5"
url: "/tr/manual/services-intrusion-detection/"
aliases:
  - "/tr/manual/07-05-services-intrusion-detection/"
translationKey: "manual-07-05-services-intrusion-detection"
---

Saldırı tespit sistemi (IDS), ağ trafiğini saldırıları tanımlayan binlerce kuralla karşılaştırır - komuta-kontrol sunucusuna bağlanan zararlı yazılımlar, istismar girişimleri (exploit), port taramaları, oltalama (phishing) ve daha fazlası - ve bir kural eşleştiğinde uyarı üretir. **Block** modunda ise seçilen kategorilerdeki kurallarla eşleşen paketleri anında düşürür (saldırı *önleme* / IPS).

Stratum FW, Proofpoint tarafından yayınlanan ücretsiz ve günlük güncellenen **ET Open** (Emerging Threats) kural kümesiyle birlikte **Suricata** motorunu kullanır.

Sayfada iki sekme yer alır: **Alerts** ve **Settings**.

## Ayarlar (Settings)

![Services > Intrusion Detection > Settings](/images/manual/ids-settings.png)

### Etkinleştirme ve arayüz seçimi

1. **Watch the traffic of the selected interfaces and raise alerts** anahtarını açın.
2. **Interfaces** altından her arayüzün çalışma modunu belirleyin:

| Mod | Ne yapar |
| :--- | :--- |
| **Off** | Arayüz denetlenmez. |
| **Watch** | Yalnızca uyarı üretir. Suricata trafiğin bir kopyasını okur; hiçbir paket düşürülmez. |
| **Block** | Suricata paketlerin geçiş yoluna yerleşir: **Block** olarak işaretlenmiş kategorilerdeki kurallar bu arayüzde eşleşen paketleri düşürür. Diğer kategoriler yalnızca uyarı üretmeye devam eder. |

Arayüzler tek tek ayarlanır - örneğin WAN arayüzü yalnızca izleme (**Watch**), yerel ağlar ise engelleme (**Block**) modunda çalıştırılabilir.

3. Aşağıdaki **Rule categories** bölümünden kural kategorilerini seçin ve **Automatic rule updates** sıklığını belirleyin.
4. **Save** ve **Apply changes** düğmelerine tıklayın. Küçük işlemcilerde kuralların belleğe yüklenmesi yaklaşık bir dakika sürer.

> **Hem WAN hem yerel ağları izlemek** çoğu uyarının iki kez görünmesine yol açar. Yerel ağ tarafında izlemenin avantajı *hangi iç cihazın* olaya karıştığını doğrudan göstermesidir; WAN tarafı ise güvenlik duvarı kurallarının zaten düşürdüğü dış saldırıları da görür.

### Kural kategorileri (Rule categories)

Üç grupta toplam 52 kategori bulunur:

| Grup | İçeriği |
| :--- | :--- |
| **Threats** (Tehditler - 26) | Zararlı yazılımlar, mobil zararlılar, istismar araçları (exploit kit), güncel tehditler, oltalama, saldırı yanıtları, kripto madenciler, taramalar, hizmet engelleme (DoS), shellcode, solucanlar, web sunucusu / web uygulaması / istemci saldırıları, SQL enjeksiyonu, zararlı DNS, şüpheli istemci başlıkları, botnet komuta-kontrol, ele geçirilmiş ve bilinen saldırgan adres listeleri ... |
| **Protocols and services** (Protokoller ve Servisler - 15) | Belirli protokol ve servislere özgü kurallar: FTP, SMTP, IMAP, POP3, SNMP, Telnet, TFTP, NetBIOS, RPC, VoIP, SCADA, ICMP, dinamik DNS, ActiveX ve diğerleri. |
| **Policy and information** (Politika ve Bilgi - 11, gürültülü) | Doğrudan saldırıdan ziyade *neyin kullanıldığını* tanımlayan kurallar - bilgi, politika, tehdit avcılığı, sohbet, oyunlar, P2P, dosya paylaşımı, uygunsuz içerik, uzaktan erişim, kötüye kullanılan servisler, Tor. Denetim için yararlıdır ancak günlük kullanımda çok sayıda uyarı üretir. |

**Recommended selection** varsayılan olarak açık gelen 24 tehdit kategorisini seçer. Her kategorinin yanında engelleme modundaki arayüzlerde trafiği düşürüp düşürmeyeceğini belirleyen bir **Block** işareti vardır.

**Kategori değişiklikleri kaydedildiğinde Apply gerekmeden anında devreye girer**: önce kurallar sınanır, ardından Suricata kuralları kesintisiz biçimde yeniden yükler.

> **İpucu - mükerrer işi önleyin:** *ciarmy*, *dshield*, *drop* ve *compromised* kategorileri kötü amaçlı IP listeleridir. IP engelleme listeleri ([**Firewall > Blocklists**](/tr/manual/firewall-blocklists/)) benzer listelerle açıksa bu adresler daha Suricata görmeden paket filtresinde düşürülür; bu kategorileri açık tutmak yalnızca ek bellek harcar. Kapatmayı değerlendirebilirsiniz.

### Engellemeye geçiş önerisi

İlk bir-iki hafta tüm arayüzlerde **Watch** moduyla başlayın, oluşan uyarıları inceleyin ve normal trafiğinizde yanlış uyarı veren kuralları kapatın. Ardından yerel ağları **Block** moduna alın ve başlangıçta uyarılarına güvendiğiniz birkaç temel tehdit kategorisinde - örneğin **malware**, **botnet command-and-control** ve **exploit kits** - engellemeyi açıp zamanla kapsamı genişletin.

### Engelleme mimarisi nasıl çalışır?

- Engelleme (**Block**) modundaki arayüzler tıpkı OPNsense'te olduğu gibi **netmap** üzerinden geçer. Engelleme modundaki bir VLAN veya PPPoE arayüzü kendi fiziksel portu üzerinde denetlenir; dolayısıyla aynı fiziksel port üzerindeki VLAN'lar aynı modu paylaşır ve o portta donanım hızlandırma (hardware offloading) kapatılır.
- **Watch** ve **Block** modları aynı anda kullanıldığında tıpkı pfSense'te olduğu gibi **iki ayrı Suricata motoru** çalışır (biri hat üzerinde engelleyen, diğeri kopya okuyan). Kurallar iki kez yüklendiği için bellek kullanımı da yaklaşık iki katına çıkar.
- **Fail open (kesintisiz geçiş):** Suricata durursa trafik tamamen kesilmek yerine filtresiz akmaya devam eder. Servisin yeniden başlatılması engelleme portlarındaki trafiği yalnızca birkaç saniye duraksatır.
- Önerilen kategorilerle bellek kullanımı yaklaşık 1-2 GB düzeyindedir. 8 GB'tan az belleğe sahip sistemlerde sayfa her motora daha küçük bellek sınırları verildiğini uyarır - bu tür sistemlerde seçili kategori sayısını az tutun.

Servisi açıp kapatmak ve arayüz modlarını değiştirmek **Apply changes** gerektirir.

### Kural güncellemeleri

| Ayar | Anlamı |
| :--- | :--- |
| **Automatic rule updates** | Saatlik, 6 saatte bir, 12 saatte bir, günlük, haftalık veya kapalı. ET Open günlük yayınlandığı için **Daily** (günlük) ideal tercihtir. |
| **On**, **At** | Güncelleme günü ve saati. |

**Update rules now** kuralları hemen günceller; **Update log** sonucu gösterir. Başarısız bir güncelleme mevcut çalışan kuralları korur.

## Uyarılar (Alerts)

![Services > Intrusion Detection > Alerts](/images/manual/ids-alerts.png)

**Alerts** sekmesi en son uyarıları listeler: **Time**, **Severity**, **Rule** (kuralın açıklaması), **Source**, **Destination** ve **Interface**. Önem derecesine göre (**All severities**, **High only**, **High and medium**) süzebilir veya arama yapabilirsiniz; **Refresh** listeyi yeniler.

Uyarı satırlarındaki rozetler:

| Rozet | Anlamı |
| :--- | :--- |
| **Blocked** | Paket engelleme modundaki bir arayüzde düşürüldü. |
| **Port closed** | Paket güvenlik duvarında açık olmayan bir porta geldi - güvenlik duvarı kuralları paketi zaten düşürdü. WAN üzerindeki internet geneli taramalar için tipiktir; zararsızdır ve bildirim olarak gönderilmez. |

### Gürültülü bir kuralı devre dışı bırakma

Bazı kurallar ağınızdaki normal trafik için uyarı üretebilir - veri sızıntısına benzeyen bir yedekleme yazılımı veya alışılmadık HTTP başlığı kullanan bir cihaz gibi. Uyarı satırındaki **Disable rule** düğmesi yalnızca o tekil kuralı kapatır. Değişiklik anında geçerli olur ve devre dışı bırakılan kurallar listesi kural güncellemeleri sonrasında da korunur.

Kapatılan kurallar **Settings** sekmesindeki **Disabled rules** altında listelenir ve her biri **Enable again** ile yeniden açılabilir.

### Bir uyarıyı doğru okuma

- **Kaynak dışarıda, hedef içeride, `Port closed`:** güvenlik duvarının zaten düşürdüğü bir dış tarama. Yapılacak bir işlem yoktur.
- **Kaynak içeride, hedef dışarıda:** ağınızdaki bir cihaz bir saldırı kuralının tanımladığı davranışı sergiledi - örneğin bilinen bir komuta-kontrol sunucusuyla iletişim kurmaya çalıştı. İlk olarak bu iç cihazı inceleyin.
- **Normal trafik üzerinde sürekli tekrarlayan aynı kural:** yanlış pozitif (false positive) - **Disable rule** ile kapatın.

Yapay Zeka Asistanı ([**Diagnostics > AI Assistant**](/tr/manual/diagnostics-ai-assistant/)) belirli bir dönemdeki uyarıları özetleyebilir ve açıklayabilir.

## Bildirimler

Yüksek önem dereceli uyarılar Telegram veya e-posta ile gönderilebilir (**System > Settings > Notifications**, *Intrusion detection* olayı). Güvenlik duvarında kapalı olan portlara yönelik uyarılar varsayılan olarak bildirim dışı tutulur; istenirse ayrı bir olay seçeneğiyle açılabilir.
