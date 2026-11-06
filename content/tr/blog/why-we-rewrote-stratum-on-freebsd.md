---
title: "Stratum Firewall'ı Neden FreeBSD Üzerine Yeniden Yazdık (Ve Neden Geriye Bakmadık)"
date: 2026-11-03
author: "Stratum Security Team"
description: "Parçalanmış bir Linux prototipinden modern, FreeBSD tabanlı, API odaklı bir NGFW'ye (Yeni Nesil Güvenlik Duvarı) uzanan mühendislik yolculuğumuza derinlemesine bir bakış."
tags: ["freebsd", "architecture", "ngfw", "declarative-config", "mimari"]
---

Stratum Firewall projesini başından beri takip ediyorsanız, devasa bir mimari değişimi fark etmişsinizdir. Onlarca farklı harici bağımlılığa (Suricata, CrowdSec, Stalwart) sahip, Debian tabanlı ağır bir Linux cihazından, yepyeni ve derli toplu bir **FreeBSD tabanlı Ağ Güvenliği İşletim Sistemine** geçtik.

Bunu Linux kötü olduğu için yapmadık. Bunu, ağ mühendisleri olarak bizim bile kullanmak isteyeceğimiz bir güvenlik duvarı inşa etmek istediğimiz için yaptık.

İşte sıfırlama düğmesine neden bastığımızın ve FreeBSD'ye geçişimizin bugüne kadarki en iyi kararımız olmasının hikayesi.

## "Araçlar Koleksiyonu" Yaklaşımının Sorunu

İlk prototipimiz çalışıyordu, ancak kırılgandı. `iptables` (ve daha sonra `nftables`), Python betikleri, harici bir IDS ve ağır veritabanı bağımlılıkları üzerine inşa edilmişti.

Sorun **durumdu (state)**. Bir güncelleme bir paketi bozarsa, güvenlik duvarı da bozulurdu. Bir yapılandırma betiği yarıda başarısız olursa, bağlantıyı kaybedebilirdiniz. Özellikle güvenlik duvarı olmak üzere tasarlanmamış bir işletim sisteminin üzerine kumdan bir kale inşa ediyorduk.

## Dönüm Noktası: Neden FreeBSD?

Paketleri yönlendirmek ve trafiği filtrelemek için sıfırdan tasarlanmış bir işletim sistemine ihtiyacımız vardı. Üç ödün vermez nedenle FreeBSD'yi seçtik:

### 1. `pf` Paket Filtresi
OpenBSD'den gelen `pf`, durum bilgili paket denetimi için dünya çapında altın standart olarak kabul edilir. İnanılmaz derecede hızlı, okunabilir ve kendini kanıtlamıştır. FreeBSD'yi kullanarak, kurallarımızı kullanıcı alanındaki bir proxy üzerinden çevirmek yerine doğrudan çekirdeğin yerel filtreleme motoruyla etkileşime girebiliriz.

### 2. Paket Yığını Değil, Uyumlu İşletim Sistemi
FreeBSD eksiksiz bir işletim sistemidir. Çekirdek, ağ araçları (`ifconfig`, `pfctl`) ve kullanıcı alanı birlikte geliştirilir. Artık bir Linux dağıtım güncellemesinin ağ yığınımızı bir gecede bozması konusunda endişelenmiyoruz. Öngörülebilir, kararlı ve güvenlidir.

### 3. Çekirdek İçi (In-Kernel) Performans
WireGuard'ın çekirdekte yerel olarak çalışmasından `ALTQ` trafik şekillendirmeye kadar FreeBSD, mütevazı donanımlarda bile ağ iş yüklerini hat hızında (wire speed) yönetir.

## Bildirimsel Yapılandırmanın Gücü

Belki de en önemli değişiklik işletim sistemi değil, yapılandırmayı ele alma şeklimizdir.

Eski sistemde, `iptables` veya `pf.conf` dosyalarını manuel olarak düzenlemeniz gerekiyordu. Stratum FW'de **Bildirimsel Yapılandırma Motorunu** tanıttık.

Niyetinizi web arayüzünde yapılandırırsınız. Biz bu niyeti tek bir, insan tarafından okunabilir `config.json` dosyasına kaydederiz. Ardından Python motorumuz bu JSON'u yerel FreeBSD yapılandırma dosyalarına (`pf.conf`, `wg0.conf` vb.) çevirir.

**Bu neden oyunun kurallarını değiştirir?**

- **Sıfır Kilitlenme (Safe-Apply):** Herhangi bir yapılandırma çekirdeğe uygulanmadan önce, motorumuz `pfctl -nf` kullanarak test eder. Bağlantınızı kesecek bir yazım hatası yaparsanız, sistem bunu uygulamanızı engeller. Yanlışlıkla WAN IP'nizi engellerseniz, otomatik olarak geri alınmadan önce bağlantıyı onaylamanız için 60 saniyelik bir geri sayım başlar.
- **Altyapı-Kod-Olarak (IaC):** Her şey tek bir JSON dosyası ve bir REST API tarafından yönetildiğinden, güvenlik duvarınızı sürüm kontrolüne alabilirsiniz. Ansible veya Terraform kullanarak saniyeler içinde yeni bir şube ofisi kurabilirsiniz.
- **Mutlak Şeffaflık:** Yapılandırmalarımızı bir SQLite veritabanında veya tescilli bir ikili dosyada saklamıyoruz. Sunucuya SSH ile bağlanabilir, `/etc/pf.conf` dosyasını açabilir ve çekirdeğin tam olarak ne çalıştırdığını görebilirsiniz.

## Sıfır Bağımlılıklı WireGuard

Minimal bir saldırı yüzeyine olan bağlılığımızı kanıtlamak için, sadece harici bir VPN kütüphanesini sarmalamadık. X25519 kriptografisini kendimiz uyguladık. Bu, Stratum FW'deki WireGuard'ın harici bağımlılıklarının olmadığı ve maksimum verimlilikle çalıştığı anlamına gelir.

## İleriye Bakış

Şu anda **v0.12.2 (Geliştirici Önizlemesi)** aşamasındayız. Çekirdek motor çalışıyor, API kararlı ve FreeBSD temeli kaya gibi sağlam.

Önümüzdeki aylarda, önceden oluşturulmuş ISO kurulum medyamızı yayınlayacağız, L3 ve L7 tehdit koruması için IP/DNS Karalistelerini tamamlayacağız ve Çoklu WAN yedeklilik mantığını tanıtacağız.

Her zaman istediğimiz güvenlik duvarını inşa ediyoruz. Eğer siz de hantal eski cihazlardan veya verilerinizi rehin tutan bulut tabanlı sistemlerden bıktıysanız, bize katılmanızı bekliyoruz.

---

*Mimariyi kendiniz görmek ister misiniz? [Mimari Genel Bakış](/tr/product/architecture/) sayfamızı okuyun veya kaynak koduna [GitHub](https://github.com/kaanozkan-81/stratum-panel) üzerinden göz atın.*
