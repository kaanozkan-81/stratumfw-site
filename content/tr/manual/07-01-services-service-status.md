---
title: "Servisler > Servis Durumu"
seo_title: "07.1 Servisler > Servis Durumu (Services > Service Status) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarındaki her servisin durumu (çalışıyor, durdu veya kapalı), çalışma süresi, bellek kullanımı ve servisleri başlatma, durdurma ve yeniden başlatma."
weight: 701
chapter_num: "07.1"
url: "/tr/manual/services-service-status/"
aliases:
  - "/tr/manual/07-01-services-service-status/"
translationKey: "manual-07-01-services-service-status"
---

**Services > Service Status** sayfası güvenlik duvarını oluşturan tüm arka plan programlarını, her birinin çalışıp çalışmadığını listeler ve yöneticilerin servisleri başlatmasına, durdurmasına veya yeniden başlatmasına olanak tanır.

![Services > Service Status](/images/manual/svc-status.png)

## Servis listesi

| Sütun | Anlamı |
| :--- | :--- |
| **Service** | Servisin adı ve kısa açıklaması; bağlantı tıklandığında ilgili servisin ayar sayfası açılır. |
| **Status** | **Running** (çalışıyor); **Stopped** (durdu) - ayarları çalışması gerektiğini söylüyorsa yanında *should run* ibaresi yer alır ([kontrol panelinin](/tr/manual/dashboard/) **Services** kartında da sayılır); veya **Switched off** (kapalı) - mevcut yapılandırmada kullanılmıyor. Ayarlarda kapatıldığı halde hala çalışan bir servis *switched off in settings* olarak işaretlenir. |
| **Running for** | Servisin ne kadar süredir kesintisiz çalıştığı. |
| **Memory** | Servisin kullandığı bellek miktarı. |
| **PID** | Komut satırı incelemeleri için işlem numarası (Process ID). |
| **Actions** | Servisin izin verdiği ölçüde **Start**, **Stop**, **Restart** düğmeleri. |

Ayarlarında kapatılmış olan servisler varsayılan olarak gizlenir; **Show switched-off services** kutusu işaretlendiğinde kapalı servisler de tabloda gösterilir. **Refresh** düğmesi durumları yeniden okur.

## Servisler

| Servis | Ne iş yapar | İşlemler |
| :--- | :--- | :--- |
| **Packet filter** | Güvenlik duvarı kuralları ve NAT (`pf`). Yeniden başlatıldığında kuralları yeniden yükler. | Restart |
| **Firewall logging** | `pflogd` - kuralların günlüklediği paketleri diske yazar. | Start, stop, restart |
| **DHCP server** | `dnsmasq` - DHCP sunucusu ve Unbound kullanılmadığında DNS çözümleyici. | Start, stop, restart |
| **DNS resolver** | `Unbound` - DNS çözümleme, DNS engelleme listeleri, Web Filtresi, Anahtar Kelimeler ve Uygulama Kontrolü engelleme sayfası yönlendirmeleri. | Start, stop, restart |
| **Time server** | `ntpd` zaman eşitleme servisi. | Start, stop, restart |
| **Intrusion detection** | Suricata saldırı tespit/önleme motoru (bir veya iki motor). | Start, stop, restart |
| **Application Control** | Capsicum kum havuzundaki `stratum-appd` (`stratum-dpi`) toplayıcısı ve yetkisiz engelleme sayfası sunucusu (`_stratumbp`) ([**Firewall > Application Control**](/tr/manual/firewall-application-control/)). | Start, stop, restart |
| **OpenVPN** | OpenVPN sunucusu. | Start, stop, restart |
| **WireGuard** | Çekirdek WireGuard uzaktan erişim arayüzü (`wg0`). | Start, stop, restart |
| **IPsec** | `strongSwan` (`charon`) - noktadan noktaya IPsec tünelleri. | Start, stop, restart |
| **Download Control** | Squid web vekil sunucusu (`:3128`) ve dosya denetleyicisi ([**Firewall > Download Control**](/tr/manual/firewall-download-control/)). Denetleyici durursa indirmeler kesilmeden devam eder. | Start, stop, restart |
| **Captive portal** | Karşılama/oturum açma sayfası ve aktif oturumlar. Yeniden başlatıldığında oturum açmış cihazların bağlantısı kopmaz. | Start, stop, restart |
| **PPPoE** | `mpd5`. Durdurulduğunda PPPoE WAN bağlantısı kesilir. | Start, stop, restart |
| **Gateway monitor** | Ağ geçidi durumu, yedekli hat devretme ve politika tabanlı yönlendirme ([**Interfaces > Gateways**](/tr/manual/interfaces-gateways/)). | Start, stop, restart |
| **Firewall log collector** | [**Diagnostics > Firewall Log**](/tr/manual/diagnostics-firewall-log/) ekranını besler. | Start, stop, restart |
| **Traffic collector** | [**Diagnostics > Traffic Graphs**](/tr/manual/diagnostics-traffic-graphs/) ekranını besler. | Start, stop, restart |
| **Web Activity collector** | [**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/) ekranını besler; Web Activity kapalıyken yalnızca [**Web Filter > Keywords**](/tr/manual/firewall-web-filter/) eşleşmelerini hiçbir gezinme kaydı tutmadan izler. | Start, stop, restart |
| **SNMP** | SNMP ajanı (`bsnmpd`). | Start, stop, restart |
| **SSH** | Yeniden başlatıldığında açık SSH oturumları kopmadan bağlı kalır. | Restart |
| **Web server** | `nginx`. Yeniden başlatma sonrasında tarayıcı sayfası kendiliğinden yenilenir. | Restart |
| **Web panel** | Bu yönetim paneli. Birkaç saniye içinde tekrar yanıt verir. | Restart |
| **System log** | `syslogd` sistem günlük servisi. | Restart |
| **Scheduler** | `cron` - güncellemeler, yedeklemeler, takma ad ve engelleme listesi yenilemeleri. | Restart |

Mevcut yapılandırmanın kullanmadığı servisler - tünel tanımlanmamışsa IPsec, PPPoE WAN yoksa PPPoE - **Switched off** durumundadır ve **Show switched-off services** işaretlenmedikçe gizlenir.

## Başlatma, durdurma ve yeniden başlatma

- **Restart** beklenmedik şekilde yanıt vermeyi kesen bir servis (örneğin sorgulara cevap vermeyen DNS çözümleyici) için standart çözümdür. Çoğu servis için yeniden başlatma kesintisizdir; istisnalar yukarıdaki tabloda belirtilmiştir (PPPoE bağlantıyı keser, web paneli birkaç saniyeliğine ayrılır).
- **Stop** bir servisi siz tekrar başlatana, o servisi değiştiren bir sonraki **Apply** işlemine ya da sistem yeniden başlatılana kadar durdurur. Bir servisin açılışta çalışıp çalışmayacağı buradan değil kendi ayar sayfasından belirlenir - bir servisi kalıcı olarak kapatmak için kendi ayar sayfasından devre dışı bırakın.
- Bazı temel servisler olmadan güvenlik duvarı çalışamayacağı için bu servisler durdurulamaz, yalnızca yeniden başlatılabilir (packet filter, SSH, web server, web panel, system log, scheduler).

Yalnızca yöneticiler servisleri başlatabilir, durdurabilir ve yeniden başlatabilir; yapılan her işlem sistem günlüğüne kaydedilir.

> **İpucu:** **Stopped** olarak görünen ve tekrar başlamayan bir servis neden başlamadığını genellikle [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) altında açıklar. Yapılandırma hataları ise çoğunlukla bu aşamaya gelmeden yakalanır: **Apply** işlemi önce servislerin yapılandırmasını denetler ve servisin kabul etmeyeceği bir değişikliği reddeder.
