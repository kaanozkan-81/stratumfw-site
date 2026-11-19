---
title: "Stratum Firewall v0.36.0: Yetki Ayrıştırılmış Misafir Captive Portal ve Kesintisiz pf Oturumları"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-36-0-captive-portal"
description: "Stratum Firewall v0.36.0 ile gelen Services > Captive Portal; çoklu arayüz bölgeleri, parolasız koşul onayı veya yerel bcrypt hesapları, MAC kontrolü ve pf yeniden yüklemelerinde kopmayan yetki ayrıştırılmış misafir ağ mimarisi sunuyor."
lead: "Misafir Wi-Fi ağlarınız için gizli anahtarları web sürecine açmayan ve Apply işlemlerinde aktif misafirleri düşürmeyen Türkçe/İngilizce Captive Portal."
images: ["/images/og-stratum-firewall.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "captive portal", "misafir wifi", "pf", "v0.36.0"]
---

Oteller, kampüsler, klinikler ve şube ofislerindeki misafir ağları, henüz kimliği doğrulanmamış telefon ve dizüstü bilgisayarların doğrudan eriştiği bir giriş sayfası sunar. [Stratum Firewall](https://stratumfw.com/tr/) **`v0.36.0`** sürümüyle devreye aldığımız **`Services > Captive Portal`** modülü; pfSense alışkanlığını yetki ayrıştırılmış (privilege-separated) güvenli bir mimari ve kesintisiz oturum sürekliliğiyle birleştiriyor. Tüm detayları [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) sayfasında inceleyebilirsiniz.

![Stratum Firewall v0.36.0 Yetki Ayrıştırılmış Captive Portal Mimarisi](/images/captive-portal-architecture.svg)

### 1. Çoklu Arayüz Bölgeleri ve Otomatik Giriş Ekranı

Her Captive Portal **bölgesi (`Zone`)**, bir veya birden fazla iç ağ arayüzünü (örneğin Misafir VLAN veya Ziyaretçi Wi-Fi) kapsar:

- **Giriş Öncesi Tam İzolasyon:** Henüz giriş yapmamış bir cihaz yalnızca güvenlik duvarındaki DNS servisine, giriş sayfasına ve bölgede izin verilen hedeflere ulaşabilir.
- **Otomatik İşletim Sistemi Algılaması:** Giriş yapmamış istemcilerin `http://` istekleri portala yönlendirilir; böylece iOS, Android, macOS, Windows ve Linux cihazlar kendi bağlantı kontrolleri üzerinden giriş ekranını kendiliğinden açar.
- **Giriş Sonrası Standart Kural ve Bant Sınırları:** Cihaz giriş yaptıktan sonra arayüzün kendi `pf` kuralları, [Security Guard](https://stratumfw.com/tr/product/security-guard/) Web/DNS filtreleri ve `dummynet` cihaz başı hız sınırları standart biçimde uygulanır.

### 2. Koşul Onaylı Hızlı Giriş veya Cihaz Sınırlı Yerel Hesaplar

Her bölge için ağ yapınıza uygun doğrulama yöntemini seçebilirsiniz:

- **Parolasız Giriş (Koşulları Kabul Et ve Bağlan):** Ziyaretçiler kullanım koşullarını onaylayarak tek dokunuşla bağlanır.
- **Yerel Kullanıcı Hesapları:** İsteğe bağlı **son kullanma tarihi** ve **kullanıcı başına eşzamanlı cihaz sınırı** tanımlayabilirsiniz; ardışık hatalı parola denemeleri ilgili IP adresini birkaç dakikalığına kilitler.
- **Süre Sınırı, Boşta Kalma ve MAC Kontrolü:** Toplam oturum süresi ve boşta kalma zaman aşımı tanımlanabilir; IP adresini başka bir cihaz devraldığında **MAC kontrolü** oturumu otomatik kapatır. Bölgenin giriş yöntemi değiştirildiğinde bağlı cihazlardan yeni yöntemle giriş yapmaları istenir.
- **Cihaz ve Hedef İstisnaları:** Giriş ekranı görmemesi gereken yazıcı ve TV kutuları gibi cihazları veya giriş öncesi açık hedefleri istisna listesine ekleyebilirsiniz.

### 3. Yetkisiz Web Süreci ve `Apply` Sırasında Kopmayan Oturumlar

Misafir ağındaki her cihaz giriş sayfasına doğrudan erişebildiği için, [Güvenlik ve Doğrulama](https://stratumfw.com/tr/product/security/) ilkelerimiz doğrultusunda misafir tarafı olabildiğince küçük tutuldu:

| Mimari Katman | `v0.36.0` Captive Portal Çalışma Biçimi |
| :--- | :--- |
| **Misafir Giriş Web Süreci** | **Yetkisiz (unprivileged)** kullanıcı altında çalışır; hiçbir gizli bilgi veya parola tutmaz |
| **Kimlik Doğrulama Servisi** | Yerel soket üzerinden ayrı bir **`root` servisi** tarafından doğrulanır |
| **Parola Saklama Biçimi** | Yalnızca `root` erişimli dosyada **`bcrypt` özeti** olarak tutulur; tarayıcıya asla gönderilmez |
| **Kesintisiz Oturum Sürekliliği** | Aktif misafirler `pf` yenilemelerinde, `Apply` işleminde ve servis yeniden başlatmada **bağlı kalır** |
| **Operatör ve Yönetici Ayrımı** | **Operatörler** hesapları yönetip cihazları düşürebilir; bölgeleri **Yöneticiler** yönetir; her giriş/çıkış loglanır |

### 4. Mobil ve Koyu Tema Uyumlu Türkçe / İngilizce Arayüz

Giriş sayfasının **başlığını, mesajını, kullanım koşullarını, düğme metnini, rengini, logosunu ve dilini (Türkçe veya İngilizce)** dilediğiniz gibi özelleştirebilirsiniz; sayfa telefon ekranlarına ve koyu temaya otomatik uyum sağlar.

### `v0.36.0` Sürümüne Yükseltin veya Deneyin

- **Mevcut Kurulumlar:** **`System > Update`** ekranından (`stratum-update run`) OpenZFS Önyükleme Ortamı (`bectl`) güvencesiyle tek tıkla imzalı `v0.36.0` sürümüne yükseltebilirsiniz.
- **Yeni Kurulum ve İnceleme:** Saf FreeBSD 15.1 gücünü sunan kurulum dosyalarını [Stratum Firewall İndir](https://stratumfw.com/tr/download/) sayfasından edinebilir, arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde inceleyebilir, kampüs ve misafir ağı senaryolarını [Eğitim ve Kampüs Çözümleri](https://stratumfw.com/tr/solutions/education/) sayfasında görebilir ve sürüm geçmişini [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel) üzerinden takip edebilirsiniz.
