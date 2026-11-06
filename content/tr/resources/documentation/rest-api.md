---
title: "REST API"
lead: "Her şeyi otomatikleştirin. Stratum paneli sadece bir istemcidir — gerçek güç API'dedir."
---

Stratum Firewall, **API-Öncelikli** bir mimariyle tasarlanmıştır. Web arayüzünde gerçekleştirdiğiniz her işlem bir REST API aracılığıyla yürütülür. Bu, özel panolar oluşturabileceğiniz, dağıtımları otomatikleştirebileceğiniz ve Stratum'u mevcut Altyapı-Kod-Olarak (IaC) iş akışlarınıza (Ansible, Terraform vb.) entegre edebileceğiniz anlamına gelir.

## Temel URL

Tüm API uç noktaları güvenlik duvarınızın panel URL'sine görelidir:

~~
https://<guvenlik-duvari-ip>:8000/api/
~~

## Kimlik Doğrulama

Stratum, web arayüzü için oturum çerezlerini kullanır, ancak API otomasyonu için oturum almak üzere giriş uç noktasını kullanmalısınız.

### 1. Giriş

Oturum çerezi almak için bir POST isteği gönderin.

~~bash
curl -X POST https://192.168.1.1:8000/login \
  -d "username=admin&password=guvenli_sifreniz" \
  -c cookies.txt
~~

`-c cookies.txt` parametresi, sonraki istekler için oturum çerezini bir dosyaya kaydeder.

### 2. Kimliği Doğrulanmış İstek

CSRF saldırılarını önlemek için `X-Requested-With: stratum` başlığını eklemelisiniz.

~~bash
curl -X GET https://192.168.1.1:8000/api/wireguard \
  -b cookies.txt \
  -H "X-Requested-With: stratum"
~~

## Rol Tabanlı Erişim Kontrolü (RBAC)

API izinleriniz kullanıcı rolünüze bağlıdır:

| Rol | İzinler |
|---|---|
| **Yönetici (Admin)** | Tüm uç noktalarda tam CRUD. |
| **Operatör** | Okuma erişimi. `/api/diag/*`'a POST atabilir. |
| **Salt Okunur** | Okuma erişimi. Yazma istekleri `403 Forbidden` döndürür. |

## Yaygın Uç Noktalar

### Sistem Durumu

~~bash
curl -X GET https://192.168.1.1:8000/api/status \
  -b cookies.txt -H "X-Requested-With: stratum"
~~

### WireGuard Eşleri

~~bash
curl -X GET https://192.168.1.1:8000/api/wireguard \
  -b cookies.txt -H "X-Requested-With: stratum"
~~

Yeni bir eş oluşturun:

~~bash
curl -X POST https://192.168.1.1:8000/api/wireguard/peers \
  -b cookies.txt -H "X-Requested-With: stratum" \
  -H "Content-Type: application/json" \
  -d '{"name": "Ansible-Laptop", "psk": true}'
~~

## API Üzerinden Güvenli Uygulama İş Akışı

API aracılığıyla bir yapılandırma değişikliği gönderdiğinizde, arayüzdekiyle aynı **Güvenli Uygulama** iş akışına girer:

1. API JSON'u doğrular.
2. `apply_needed: true` ile bir başarı yanıtı döndürür.
3. Değişiklikleri korumak için 60 saniye içinde `/api/apply/confirm` çağırmalısınız.

## Hata İşleme

- `400 Bad Request`: Doğrulama hatası (örn. geçersiz IP).
- `401 Unauthorized`: Oturum süresi dolmuş veya geçersiz.
- `403 Forbidden`: Rolünüz bu eyleme izin vermiyor.
- `404 Not Found`: Kaynak mevcut değil.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Komut Satırı</h3>
    <p>Temel FreeBSD CLI araçlarını öğrenin.</p>
    <a href="/tr/resources/documentation/command-line/" class="card-link">CLI Referansı →</a>
  </div>
  <div class="card">
    <h3>Dosya ve Yol Referansı</h3>
    <p>Yapılandırmanın nerede yaşadığı.</p>
    <a href="/tr/resources/documentation/file-paths/" class="card-link">Dosya Referansı →</a>
  </div>
</div>
