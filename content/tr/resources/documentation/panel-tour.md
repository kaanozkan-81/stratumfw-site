---
title: "Panel Turu"
lead: "Stratum arayüzünde gezinin — düzeni, modülleri ve Güvenli Uygulama (Safe-Apply) iş akışını anlayın."
---

Stratum yönetim paneli, eski nesil güvenlik duvarı arayüzlerinde bulunan karmaşadan arındırılmış, hızlı ve duyarlı olacak şekilde tasarlanmıştır. REST API üzerine inşa edilmiş ince bir istemcidir; yani gerçekleştirdiğiniz her işlem, otomasyon için kullanabileceğiniz API tarafından desteklenir.

## Düzen

Arayüz üç ana bölüme ayrılmıştır:

1. **Kenar Çubuğu Navigasyonu:** İşleve göre gruplandırılmıştır (Genel Bakış, Arayüzler, Güvenlik Duvarı, Servisler, VPN, Sistem, Teşhisler).
2. **Üst Çubuk:** Mevcut sayfa konumunu (breadcrumb), aktif kullanıcı rolünüzü ve sistem durumu göstergelerini gösterir.
3. **Ana İçerik Alanı:** Aktif modülün yapılandırmasının veya panosunun bulunduğu yerdir.

## Pano (Dashboard)

![Stratum Dashboard](/images/docs/dashboard.png)

Oturum açtığınızda varsayılan görünümdür. Pano son derece özelleştirilebilir ve ağınızın sağlığına dair bir bakışta genel bir görünüm sunar.

- **Sistem Hayati Verileri:** FreeBSD ana bilgisayarının CPU, RAM ve Disk kullanımı.
- **Arayüz Trafiği:** WAN ve LAN için gerçek zamanlı RX/TX (Alınan/Gönderilen) grafikleri.
- **Aktif Durumlar:** `pf` tarafından izlenen aktif bağlantıların sayısı.
- **Güvenlik Uyarıları:** IP Karalisteleri veya DNSBL'den gelen son engellemeler.

## Rol Tabanlı Görünümler

Neyi göreceğiniz rolünüze bağlıdır:
- **Yöneticiler (Admins)** her menü öğesini görür.
- **Operatörler** teşhis araçlarını, durumları ve günlükleri görür, ancak yapılandırma sayfaları (güvenlik duvarı kuralları gibi) salt okunurdur veya gizlidir.
- **Salt Okunur** kullanıcılar yalnızca panoyu, metrikleri ve günlükleri görür.

## Güvenli Uygulama (Safe-Apply) Göstergesi

Ekranın üst kısmında bazen yeşil veya sarı bir çubuk belirdiğini göreceksiniz. Bu **Uygulama Çubuğu**'dur (Apply Bar).

Stratum bildirimsel bir yapılandırma motoru kullandığından, siz "Uygula" (Apply) düğmesine tıklayana kadar değişiklikler bellekte aşamalandırılır. Bu, birden fazla ayarlama yapmanıza ve hepsini tek seferde onaylamanıza olanak tanır. Uygula'ya tıkladığınızda, Güvenli Uygulama iş akışı kuralları doğrular, bunları `pf`'e uygular ve bağlantıyı onaylamanız için size bir zaman aşımı penceresi verir.

## Temel Modüller

- **Arayüzler (Interfaces):** Fiziksel Ağ Kartlarını (NIC) WAN, LAN veya OPT rollerine atayın.
- **Güvenlik Duvarı / Kurallar (Firewall / Rules):** `pf` kural setini yönetin. Sürükle ve bırak ile sıralama.
- **VPN / WireGuard:** Anahtarlar oluşturun, eşleri yönetin ve QR kodlarını tarayın.
- **Güvenlik / Karalisteler (Security / Blocklists):** Tehdit akışlarını ve DNSBL kategorilerini yönetin.
- **Teşhisler (Diagnostics):** Canlı `pf` durumları, paket yakalama ve sistem günlükleri.

## Sonraki Adımlar

Düzeni anladığınıza göre, güvenlik duvarınızı yapılandırma zamanı geldi.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>İlk Yapılandırma</h3>
    <p>WAN/LAN arayüzlerini ve yönetici erişimini ayarlayın.</p>
    <a href="/tr/resources/documentation/initial-configuration/" class="card-link">Kılavuzu Okuyun →</a>
  </div>
  <div class="card">
    <h3>pf ve Güvenlik Duvarı Kuralları</h3>
    <p>Güvenlik duvarı kurallarını güvenli bir şekilde yazmayı, sıralamayı ve uygulamayı öğrenin.</p>
    <a href="/tr/resources/documentation/pf-rules/" class="card-link">Kılavuzu Okuyun →</a>
  </div>
</div>
