---
title: "Stratum Firewall: Yetki Ayrıştırılmış Captive Portal, RADIUS Muhasebeleştirme ve MikroTik Hız Sınırı Desteği"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-36-1-captive-portal-radius"
aliases:
  - /tr/blog/stratum-firewall-v0-36-0-yetki-ayristirilmis-captive-portal/
description: "Stratum Firewall; yetkisiz süreç izolasyonlu Captive Portal, FreeRADIUS kimlik doğrulama ve muhasebeleştirme (Accounting), WISPr ve Mikrotik-Rate-Limit uyumlu cihaz başı dummynet hız kuyrukları sunuyor."
lead: "Oteller, kampüsler, yurtlar ve şube ağları için mevcut MikroTik / FreeRADIUS altyapısıyla değişiklik gerektirmeden çalışan, yetki ayrıştırılmış Türkçe/İngilizce Captive Portal."
images: ["/images/hardware-stratum-family.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "captive portal", "radius", "freeradius", "mikrotik"]
---

Oteller, üniversite kampüsleri, öğrenci yurtları, klinikler ve çok şubeli işletmelerdeki misafir ağlarında iki kritik ihtiyaç bir arada bulunur: **henüz doğrulanmamış cihazlara açılan giriş sayfasının en küçük saldırı yüzeyine sahip olması** ve **mevcut RADIUS oturum/log altyapısıyla sıfır uyumsuzlukla çalışması**.

[Stratum Firewall](https://stratumfw.com/tr/) ve sürümleriyle gelen **`Services > Captive Portal`**; yetki ayrıştırılmış güvenli mimariyi, eksiksiz `FreeRADIUS` kimlik doğrulama ve muhasebeleştirmeyi (`Accounting`), cihaz başı `dummynet` bant genişliği kuyruklarını ve doğrudan **`Mikrotik-Rate-Limit`** uyumluluğunu bir araya getiriyor. Tüm sürüm detaylarını [Sürüm Notları](https://stratumfw.com/tr/download/#changelog) sayfasında bulabilirsiniz.

![Stratum Firewall Captive Portal, RADIUS NAS ve Cihaz Başı dummynet Mimarisi](/images/captive-portal-architecture.svg)

### 1. Çoklu Arayüz Bölgeleri, Otomatik Giriş Ekranı ve Üç Doğrulama Yöntemi

Her Captive Portal **bölgesi (`Zone`)**, bir veya daha fazla iç ağ arayüzünü (örneğin Misafir Wi-Fi veya Yurt VLAN'ı) kapsar:

- **Giriş Öncesi Tam İzolasyon:** Giriş yapmamış bir cihaz yalnızca güvenlik duvarındaki DNS servisine, giriş sayfasına ve izin verilen hedeflere erişebilir; `http://` istekleri portala yönlendirildiği için telefon ve bilgisayarlar bağlantı penceresini kendiliğinden açar.
- **Bölge Başına Üç Esnek Giriş Seçeneği:**
  1. **Parolasız Giriş:** Kullanım koşullarını onaylayarak tek dokunuşla bağlantı.
  2. **Yerel Kullanıcı Hesapları (`bcrypt`):** İsteğe bağlı son kullanma tarihi, kullanıcı başına eşzamanlı cihaz sınırı ve hatalı parola denemelerinde geçici IP kilidi.
  3. **RADIUS Sunucusu (`FreeRADIUS` ve Standart Sunucular):** Her istekte **`Message-Authenticator`** içeren `PAP` doğrulaması yapar ve sunucudan dönen her yanıtı paylaşılan gizli anahtara (`0600` izinli root dosyasında saklanır, bir daha gösterilmez) karşı doğrular. Birincil sunucu yanıt vermezse otomatik olarak **ikincil RADIUS sunucusuna** geçer.

### 2. Tam RADIUS Accounting ve Değişiklik Gerektirmeyen `Mikrotik-Rate-Limit` Uyumluluğu

Bir otel, yurt veya kampüs ağında güvenlik duvarı geçişi yaparken mevcut RADIUS ve log sunucunuzu yeniden yapılandırmanız gerekmez:

- **Start, Ara Güncelleme ve Stop Muhasebe Kayıtları:** Oturum süresi, indirilen/yüklenen trafik miktarı ve çıkış nedeni (kullanıcı çıkışı, boşta kalma, süre dolumu, yönetici müdahalesi vb.) ile birlikte `Start`, periyodik güncellemeler ve `Stop` paketleri eksiksiz iletilir; sunucunun `Class` bilgisi geri gönderilir. Böylece RADIUS oturum kayıtları ve bunlara dayalı log sistemleri Stratum FW arkasındaki cihazları diğer tüm `NAS` cihazlarıyla aynı biçimde kaydeder.
- **`WISPr` ve `Mikrotik-Rate-Limit` Desteği:** RADIUS sunucusundan gelen `Session-Timeout`, `Idle-Timeout` ve `Acct-Interim-Interval` değerlerinin yanı sıra **`WISPr-Bandwidth-Max-Up/Down`** veya **`Mikrotik-Rate-Limit`** hız tanımları doğrudan cihaza uygulanır — mevcut bir MikroTik hotspot/RADIUS kurulumu hiçbir ayar değişikliği gerektirmeden çalışmaya devam eder.
- **Bölge ve Cihaz Başı `dummynet` Kuyrukları:** Giriş yöntemi ne olursa olsun her bölge için cihaz başı indirme/yükleme (`Mbit/s`) sınırı belirlenebilir. Her cihaza kendi bağımsız `dummynet` kuyruğu açılır; RADIUS üzerinden gelen hız değeri bölge varsayılanının önüne geçer. Aktif cihaz listesinde her istemcinin trafiği ve hız sınırı canlı olarak görünür.

### 3. Yetki Ayrıştırılmış Mimari, Kontrol Paneli Kartı ve Kopmayan Oturumlar

[Güvenlik ve Doğrulama](https://stratumfw.com/tr/product/security/) ilkelerimiz doğrultusunda misafir tarafı en dar yetkiyle çalışır:

| Mimari Katman | – Çalışma Biçimi |
| :--- | :--- |
| **Misafir Giriş Web Süreci** | **Yetkisiz (unprivileged)** kullanıcı altında çalışır; parola veya RADIUS sırrı barındırmaz |
| **Root Doğrulama Soketi** | Yerel `bcrypt` özetlerini ve imzalı RADIUS yanıtlarını yerel soket üzerinden ayrı bir `root` servisi doğrular |
| **Kesintisiz Oturum Sürekliliği** | Aktif cihazlar `pf` yenilemelerinde, `120 sn` Safe-Apply işleminde ve servis yeniden başlatmada **bağlı kalır** |
| **MAC Kontrolü ve İstisnalar** | IP adresi el değiştirdiğinde oturumu kapatır; yazıcı ve TV kutuları için istisna tanımlar |
| **Dashboard Kartı ve RBAC** | Yeni **Captive portal** kontrol paneli kartı RADIUS muhasebe teyidi gelmezse uyarır; **Operatörler** hesap ve oturumları yönetebilir |

### Stratum Firewall'ı Güncelleyin veya İnceleyin

- **Tek Tıkla İmzalı Güncelleme:** Mevcut kurulumlarınızı **`System > Update`** (`stratum-update run`) üzerinden OpenZFS Önyükleme Ortamı (`bectl`) güvencesiyle doğrudan en güncel imzalı sürüme yükseltebilirsiniz.
- **Donanım Profilleri ve Kurulum:** Yeni [4 Port, 8 Port SFP+ ve 12 Port 1U Donanım Profillerini](https://stratumfw.com/tr/product/hardware/) inceleyebilir, ücretsiz kurulum imajlarını [Stratum Firewall İndir](https://stratumfw.com/tr/download/) sayfasından çekebilir, arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde deneyebilir ve tüm notları [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel) üzerinde takip edebilirsiniz.
