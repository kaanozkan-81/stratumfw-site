---
title: "Küçük İşletme"
lead: "Kurumsal düzeyde karmaşıklık veya maliyet olmadan kurumsal düzeyde güvenlik."
---

## Küçük bir işletme ağının gerçeği

Küçük bir işletme yönetiyorsanız, ağınız muhtemelen şöyle görünür:

- Genellikle güvenilmez olan bir internet bağlantısı
- Elinden gelenin en iyisini yapan, koli bandıyla bir arada tutulan bir tüketici yönlendiricisi
- Özel bir BT güvenlik personeli yok
- Her ay artan bulut abonelikleri

Tehditler gerçektir, ancak bütçe değildir. **Stratum FW tam olarak bu boşluk için tasarlandı.**

## Stratum neyi değiştirir?

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Abonelik Yok</h3>
    <p>Kendi donanımınızda çalıştırın. Aylık lisans ücreti yok, kullanıcı başına maliyet yok. Donanım için bir kez ödeyin, yazılım sonsuza kadar sizin olsun.</p>
  </div>
  <div class="card">
    <h3>Güvenilir İnternet (Çoklu WAN)</h3>
    <p>İki internet bağlantısı (örn. Fiber + LTE) bağlayın. Birincil bağlantı koptuğunda Stratum otomatik olarak yedek bağlantıya geçer. Personeliniz hiçbir şey fark etmez.</p>
  </div>
  <div class="card">
    <h3>Güvenli Uzaktan Erişim</h3>
    <p>Yerleşik WireGuard VPN. Personelin evden dosyalara güvenli bir şekilde erişmesini sağlayın. Bir QR kod oluşturun, telefonlarıyla tarasınlar ve bağlansınlar.</p>
  </div>
  <div class="card">
    <h3>Reklamları ve Kötü Amaçlı Yazılımları Engelleyin</h3>
    <p>DNSBL ve IP Karalistelerini tek tıkla etkinleştirin. Stratum, kötü amaçlı alan adlarını ve IP'leri cihazlarınıza ulaşmadan önce engeller.</p>
  </div>
</div>

## Tipik bir dağıtım

<div class="smb-scenario">
  <div class="smb-scenario-step">
    <span class="smb-scenario-num">01</span>
    <div>
      <h4>Donanım</h4>
      <p>İki ağ bağlantı noktasına sahip küçük form faktörlü bir bilgisayar. Toplam maliyet: tüketici bulut aboneliklerinin bir yılından daha az.</p>
    </div>
  </div>
  <div class="smb-scenario-step">
    <span class="smb-scenario-num">02</span>
    <div>
      <h4>Yazılım</h4>
      <p>FreeBSD'yi kurun ve Stratum panelini konuşlandırın. Lisans yok, yinelenen ücret yok.</p>
    </div>
  </div>
  <div class="smb-scenario-step">
    <span class="smb-scenario-num">03</span>
    <div>
      <h4>Güvenlik</h4>
      <p>Uzaktan çalışma için WireGuard'ı etkinleştirin, reklam engelleme için DNSBL'yi açın ve güvenilirlik için Çoklu WAN'ı yapılandırın.</p>
    </div>
  </div>
</div>

<div style="margin-top:32px;">
  <a href="/tr/download/" class="btn btn-primary">Başlayın</a>
  <a href="mailto:info@stratumfw.com" class="btn btn-secondary">Bize ulaşın</a>
</div>
