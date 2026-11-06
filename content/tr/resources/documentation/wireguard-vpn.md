---
title: "WireGuard VPN"
lead: "WireGuard kullanarak yüksek performanslı, sıfır bağımlılıklı uzaktan erişim yapılandırın."
---

Stratum Firewall, modern ve yüksek performanslı VPN protokolü **WireGuard** için yerel entegrasyon içerir. Ağır harici paketlere güvenen diğer güvenlik duvarlarının aksine Stratum, X25519 kriptografisini dahili olarak uygular; bu da maksimum güvenlik ve minimum saldırı yüzeyi sağlar.

## Neden WireGuard?

- **Şimşek Hızında:** Çekirdek alanında (kernel space) çalışır, OpenVPN'den önemli ölçüde daha yüksek verim sağlar.
- **Durumsuz (Stateless):** Ağ değişikliklerini (örneğin bir telefonda Wi-Fi'den LTE'ye geçiş) sorunsuz şekilde yönetir.
- **Modern Kriptografi:** En son kriptografi teknolojilerini (ChaCha20, Poly1305, Curve25519, BLAKE2) kullanır.

## Sunucuyu Ayarlama

![WireGuard Setup](/images/docs/wireguard.png)

Eş oluşturmadan önce WireGuard hizmetini etkinleştirmelisiniz.

1. **VPN → WireGuard** bölümüne gidin.
2. **WireGuard'ı Etkinleştir** seçeneğini açın.
3. **Dinleme Portu:** Belirli bir gereksiniminiz yoksa varsayılan olarak bırakın (UDP 51820).
4. **Tünel Ağı:** VPN'iniz için özel IP aralığını tanımlayın (örn. `10.202.0.0/24`). Güvenlik duvarı ilk adresi (`10.202.0.1`) alır, eşler geri kalanı alır.
5. **Erişim Ağları:** VPN kullanıcılarının erişmesine izin verilen dahili ağları (örn. LAN) seçin.
6. **Kaydet** ve **Uygula**'ya tıklayın.

## Bir Eş (Cihaz) Oluşturma

"Eş" (Peer), VPN'e bağlanan tek bir cihazdır (bir telefon, bir dizüstü bilgisayar).

1. **Eşler** bölümünün altında **Eş Ekle**'ye tıklayın.
2. **Cihaz Adı:** Tanınabilir bir isim verin (örn. "Kaan iPhone").
3. **Anahtar Oluşturma:** "Anahtar çiftini güvenlik duvarında oluştur" seçeneğini seçili bırakın. Stratum, cihaz için özel ve genel anahtarları güvenli bir şekilde oluşturacaktır.
4. **Ön Paylaşılan Anahtar (PSK):** Bunu etkinleştirmenizi şiddetle tavsiye ederiz. Ekstra bir simetrik şifreleme katmanı ekler (kuantum sonrası direnç).
5. **Kaydet**'e tıklayın.

## Mobil Cihaz Bağlama (QR Kod)

Bir telefonu bağlamanın en kolay yolu QR kodudur.

1. WireGuard sayfasında, yeni oluşturduğunuz eşi bulun.
2. **QR Kod** düğmesine tıklayın.
3. Telefonunuzda resmi WireGuard uygulamasını açın (iOS veya Android).
4. **"+" → "QR kodundan oluştur"** seçeneğine dokunun ve ekranınızdaki kodu tarayın.

Telefonunuz artık bağlı. Stratum panelindeki "Son El Sıkışma" sütununu kontrol ederek bağlantıyı doğrulayabilirsiniz.

## Dizüstü Bilgisayar Bağlama (Yapılandırma Dosyası)

Dizüstü bilgisayarlar için yapılandırma dosyasını doğrudan indirebilirsiniz.

1. Eşin yanındaki **İndir** simgesine tıklayın.
2. Bir `.conf` dosyası alacaksınız.
3. Dizüstü bilgisayarınızda WireGuard uygulamasını açın, **"Dosyadan tünel(ler) içe aktar"** seçeneğine tıklayın ve indirdiğiniz `.conf` dosyasını seçin.

## Güvenlik En İyi Uygulamaları

- **Cihaz Başına Bir Eş:** Bir eş yapılandırmasını asla birden fazla kişi arasında paylaşmayın. Bir cihaz kaybolursa, diğerlerini etkilemeden o belirli eşi silebilirsiniz.
- **Anahtar Yönetimi:** Eşlerin özel anahtarları güvenlik duvarında sıkı izinlerle (`chmod 600`) saklanır. Ana `config.json` dosyasında asla açığa çıkarılmazlar.
- **Kullanılmayan Eşleri Silin:** Bir çalışan ayrılırsa veya bir cihaz kullanımdan kaldırılırsa, erişimi iptal etmek için eşi hemen silin.

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>DNS'nizi Koruyun</h3>
    <p>DNSBL ile kötü amaçlı alan adlarını nasıl engelleyeceğinizi öğrenin.</p>
    <a href="/tr/resources/documentation/blocklists/" class="card-link">Karalisteler Kılavuzu →</a>
  </div>
  <div class="card">
    <h3>API ile Otomatikleştirin</h3>
    <p>REST API'mizi kullanarak eşleri programatik olarak yönetin.</p>
    <a href="/tr/resources/documentation/rest-api/" class="card-link">API Referansı →</a>
  </div>
</div>
