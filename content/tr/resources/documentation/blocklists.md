---
title: "IP ve DNS Karalisteleri"
lead: "Ağınızı her iki katmanda koruyun — kötü amaçlı IP'leri paket düzeyinde, kötü amaçlı alan adlarını DNS düzeyinde engelleyin."
---

Stratum Firewall, modern siber tehditlere karşı iki katmanlı bir savunma mekanizması sunar. Kötü amaçlı IP adreslerini güvenlik duvarı düzeyinde engelleyebilir ve aynı anda kötü amaçlı alan adlarını (reklamlar, izleyiciler, kimlik avı) DNS çözümleyici düzeyinde engelleyebilirsiniz.

## Katman 1: IP Karalisteleri (IPv4/IPv6)

IP Karalisteleri, genel tehdit akışlarını (veya özel listelerinizi) `pf` tablolarını kullanarak doğrudan engellemenize olanak tanır. Bu, çekirdek düzeyinde gerçekleşir ve milyonlarca IP ile bile sıfır performans etkisi sağlar.

### Nasıl Etkinleştirilir

1. **Güvenlik → Karalisteler** bölümüne gidin.
2. **IP Karalisteleri** sekmesini seçin.
3. **IP Karalistelerini Etkinleştir** seçeneğini açın.

### Bir Akış (Feed) Ekleme

Stratum standart CIDR listelerini destekler (örn. Spamhaus DROP, DShield, Abuse.ch).

1. **Akış Ekle**'ye tıklayın.
2. Ham metin dosyasının URL'sini yapıştırın (örn. `https://www.spamhaus.org/drop/drop.txt`).
3. Güncelleme aralığını ayarlayın (örn. her 12 saatte bir).
4. **Kaydet**'e tıklayın.

Stratum listeyi hemen indirecek, geçerli IP'leri ayrıştıracak ve bunları `<blocklist_feed_name>` adlı bir `pf` tablosuna ekleyecektir. Bu IP'lerle eşleşen gelen ve giden trafiği engellemek için otomatik olarak varsayılan bir kural uygulanır.

### Özel IP Engelleme

Bir akışa gerek kalmadan manuel olarak bir IP veya alt ağı da engelleyebilirsiniz:

1. IP Karalisteleri sekmesinde **Özel IP'ler** bölümüne gidin.
2. IP'yi (örn. `1.2.3.4`) veya CIDR'ı (örn. `192.168.50.0/24`) ekleyin.
3. **Kaydet**'e tıklayın.

## Katman 2: DNSBL (DNS Engelleme)

DNSBL, ağınızdaki cihazların kötü amaçlı veya istenmeyen alan adlarını çözümlemesini önler. Engellenen alan adları için `NXDOMAIN` döndürmek üzere Unbound'un RPZ (Yanıt Politikası Bölgeleri) özelliğini kullanıyoruz.

### Nasıl Etkinleştirilir

1. **Güvenlik → Karalisteler** bölümüne gidin.
2. **DNSBL** sekmesini seçin.
3. **DNSBL'yi Etkinleştir** seçeneğini açın.

### Kategorileri Seçme

Alan adlarını manuel olarak yönetmek yerine Stratum, özenle seçilmiş akışlar sunar. Engellemek istediğiniz kategorileri seçin (örn. Reklamlar, Kötü Amaçlı Yazılım, Kimlik Avı, Yetişkin İçerik). Listeler otomatik olarak indirilecek ve yerel bir bölge dosyasında derlenecektir.

### Alan Adlarını İzin Listesine Ekleme

Engellenen bir akış, ihtiyaç duyduğunuz bir alan adını yanlışlıkla engelliyorsa (yanlış pozitif), bunu geçersiz kılabilirsiniz:
1. **İzin Listesi** bölümüne gidin.
2. Alan adını (örn. `ornek.com`) ekleyin.
3. **Kaydet**'e tıklayın.

## İzleme ve Günlükler

DNSBL tarafından bir alan adı engellendiğinde Unbound bunu günlüğe kaydeder. Bu günlükleri **Teşhisler → Sistem Günlükleri** altında (dnsbl filtresi ile) görüntüleyebilirsiniz. IP Karalisteleri için, engellenen paketleri **Teşhisler → Güvenlik Duvarı Günlüğü** bölümünde görebilirsiniz; buradaki engellenen paketler `ipblock-in-*` veya `ipblock-out-*` etiketlerini taşır.

## En İyi Uygulamalar

- **DNSBL ile başlayın:** Meşru IP trafiğini bozma riski olmadan tehditleri engellemeye başlamanın en güvenli yoludur.
- **IP Karalistelerinde dikkatli olun:** Büyük IP aralıklarını engellemek bazen CDN'lere (İçerik Dağıtım Ağları) erişimi bozabilir. Giden engellemeyi etkinleştirmeden önce akışları her zaman doğrulayın.
- **Güncelleme Sıklığı:** Taze tehdit verisi ihtiyacını güvenlik duvarınızın bant genişliğiyle dengeleyin. Günlük güncellemeler genellikle küçük işletmeler için yeterlidir.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>API'yi Keşfedin</h3>
    <p>Karaliste yönetimini REST API kullanarak otomatikleştirin.</p>
    <a href="/tr/resources/documentation/rest-api/" class="card-link">API Referansı →</a>
  </div>
  <div class="card">
    <h3>Sistem Günlükleri</h3>
    <p>Engellenen trafiği izleyin ve ağ sorunlarını teşhis edin.</p>
    <a href="/tr/resources/documentation/command-line/" class="card-link">Günlükler Kılavuzu →</a>
  </div>
</div>
