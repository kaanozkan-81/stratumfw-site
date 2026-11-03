---
title: "Özellikler"
lead: "Modern bir ağı güvene almak, yönetmek ve izlemek için ihtiyacınız olan her yetenek — gereksiz yükler olmadan."
full_width: true
---

<div class="features-layout">
<aside class="features-sidebar">
  <nav class="features-nav">
    <h4>Kategoriler</h4>
    <ul>
      <li><a href="#networking">Ağ ve Yönlendirme</a></li>
      <li><a href="#security">Güvenlik ve Tehdit İstihbaratı</a></li>
      <li><a href="#vpn">VPN ve Erişim Kontrolü</a></li>
      <li><a href="#management">Yönetim ve API</a></li>
    </ul>
  </nav>
</aside>

<div class="features-content">
<section id="networking" class="feature-category">
  <span class="eyebrow">Kategori 01</span>
  <h2>Ağ ve Yönlendirme</h2>
  <p class="category-lead">Dünyanın en çok savaş görmüş ağ yığını olan FreeBSD ve pf tarafından desteklenir.</p>

  <article class="feature-item">
    <h3>pf Paket Filtresi</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Kullanıcı alanındaki (userspace) güvenlik duvarları yüksek verimi kaldıramaz ve paketlere gereksiz gecikme ekler.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> FreeBSD'nin yerel pf'ini kullanıyoruz. Tüm durum bilgili denetim, NAT ve yönlendirme doğrudan çekirdek içinde gerçekleşir ve ağır yük altında bile hat hızında performans sağlar.</p>
    <ul class="feature-tech">
      <li>Durum Bilgili Paket Denetimi (SPI)</li>
      <li>Hedef NAT (Port Yönlendirme) ve 1:1 NAT</li>
      <li>Trafik şekillendirme ve önceliklendirme (ALTQ)</li>
      <li>Gerçek zamanlı durum ve kural günlüğü</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Çoklu WAN ve Yedeklilik</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Tek bir internet bağlantısı tek bir hata noktasıdır. Kesinti maliyetlidir.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Birden fazla WAN arayüzü yapılandırın, gerisini Stratum halletsin. Sağlık kontrollü yedeklilik (failover), birincil bağlantınız koptuğunda trafiği otomatik olarak yönlendirir.</p>
    <ul class="feature-tech">
      <li>Birden fazla WAN arayüzü (DHCP, Statik, PPPoE)</li>
      <li>Yapılandırılabilir ağ geçidi grupları ve failover politikaları</li>
      <li>Birincil bağlantı geri geldiğinde otomatik kurtarma</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>VLAN'lar ve Köprüler</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Daha fazla donanım satın almadan trafiği (Misafir, IoT, Personel) segmentlere ayırmanız gerekir.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> 802.1Q VLAN etiketleme ve yazılım köprüleme için yerel destek. Ağınızı mantıksal olarak segmentlere ayırın ve VLAN başına güvenlik duvarı kuralları atayın.</p>
    <ul class="feature-tech">
      <li>Herhangi bir fiziksel arayüzde 802.1Q VLAN desteği</li>
      <li>Bağlantı toplama (LAGG) desteği</li>
      <li>Yazılım köprüleme (if_bridge)</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>DNS ve DHCP Servisleri</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Küçük bir ağ için ayrı DHCP ve DNS sunucularını yönetmek aşırı ve hataya açıktır.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Entegre DHCP (dnsmasq) ve DNS çözümleyici (Unbound). Statik eşlemeleri, DNS geçersiz kılmalarını ve DHCP havuzlarını doğrudan panelden yönetin.</p>
    <ul class="feature-tech">
      <li>DNSSEC doğrulamalı Unbound</li>
      <li>DHCP ve yerel DNS için dnsmasq</li>
      <li>Özel DNS geçersiz kılmaları ve ana bilgisayar kayıtları</li>
    </ul>
  </article>
</section>

<section id="security" class="feature-category">
  <span class="eyebrow">Kategori 02</span>
  <h2>Güvenlik ve Tehdit İstihbaratı</h2>
  <p class="category-lead">Paket düzeyinden DNS katmanına kadar katmanlı savunma.</p>

  <article class="feature-item">
    <h3>IP Karalisteleri (IPv4/IPv6)</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Kötü amaçlı IP'leri manuel olarak engellemek imkansızdır. Tehdit akışları her dakika güncellenir.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Genel tehdit akışlarını doğrudan pf tablolarına alın. Stratum karalisteleri otomatik olarak indirir, ayrıştırır ve uygular, böylece kötü amaçlı trafiği çevre biriminde düşürürsünüz.</p>
    <ul class="feature-tech">
      <li>Standart CIDR ve IP listelerini içe aktarın</li>
      <li>Dinamik pf tablosu güncellemeleri (tam yeniden yükleme yok)</li>
      <li>Ayrı gelen ve giden engelleme kuralları</li>
      <li>Akış durumu ve isabet sayaçları</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>DNSBL (DNS Engelleme)</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Reklamlar, izleyiciler ve kötü amaçlı yazılım alan adları ağınızı yavaşlatır ve güvenlik riski oluşturur.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Alan adlarını çözümleyici düzeyinde engellemek için DNSBL'yi etkinleştirin. Bir cihaz kötü amaçlı bir alan adını çözmeye çalışırsa anında NXDOMAIN yanıtı alır.</p>
    <ul class="feature-tech">
      <li>Unbound aracılığıyla RPZ (Yanıt Politikası Bölgeleri)</li>
      <li>Özel izin ve engelleme listeleri</li>
      <li>Planlanmış akış güncellemeleri</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Güvenli Uygulama İş Akışı</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Tek bir hatalı güvenlik duvarı kuralı kendi sunucunuzdan sizi kilitleyebilir.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Her değişiklik doğrulanır. Bir WAN IP'sini veya güvenlik duvarı kuralını değiştirirseniz, Stratum size bağlantıyı onaylamanız için bir zaman aşımı verir. Onaylamazsanız otomatik olarak geri alır.</p>
    <ul class="feature-tech">
      <li>Uçuş öncesi sözdizimi kontrolü (<code>pfctl -nf</code>)</li>
      <li>Atomik yapılandırma değişimi</li>
      <li>Bağlantı kaybında otomatik geri alma</li>
    </ul>
  </article>
</section>

<section id="vpn" class="feature-category">
  <span class="eyebrow">Kategori 03</span>
  <h2>VPN ve Erişim Kontrolü</h2>
  <p class="category-lead">Güvenli uzaktan erişim ve katı yetkilendirme.</p>

  <article class="feature-item">
    <h3>WireGuard VPN</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Geleneksel VPN'ler ağır, yapılandırması zor ve yavaştır.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Tam entegre WireGuard. Stratum, X25519 kriptografisini dahili olarak uygular, yani sıfır harici bağımlılık anlamına gelir. Saniyeler içinde eş yapılandırmaları ve QR kodları oluşturun.</p>
    <ul class="feature-tech">
      <li>Sıfır bağımlılıklı X25519 kriptografisi</li>
      <li>Mobil cihazlar için QR kod oluşturma</li>
      <li>Ekstra güvenlik için Ön Paylaşılan Anahtarlar (PSK)</li>
      <li>Eş başına izin verilen IP'ler ve yönlendirme</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Rol Tabanlı Erişim Kontrolü (RBAC)</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Yönetici şifresini herkese dağıtmak büyük bir güvenlik riskidir.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Kimin ne yapabileceğini tam olarak tanımlayın. Her API uç noktası ve arayüz sayfası katı rol izinlerini uygular.</p>
    <ul class="feature-tech">
      <li><b>Yönetici (Admin):</b> Tüm yapılandırma ve kullanıcılara tam erişim</li>
      <li><b>Operatör:</b> Durumları yönetebilir, teşhis çalıştırabilir, günlükleri görebilir</li>
      <li><b>Salt Okunur:</b> Yalnızca panoları ve metrikleri görüntüleyebilir</li>
    </ul>
  </article>
</section>

<section id="management" class="feature-category">
  <span class="eyebrow">Kategori 04</span>
  <h2>Yönetim ve API</h2>
  <p class="category-lead">Her şeyi otomatikleştirin. Arayüz sadece bir istemcidir.</p>

  <article class="feature-item">
    <h3>Bildirimsel Yapılandırma Motoru</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Güvenlik duvarı yapılandırmaları genellikle dağınık ve denetlemesi zordur.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> Tüm sistem durumu tek bir <code>config.json</code> dosyasında yaşar. Python motoru bu JSON'u yerel FreeBSD dosyalarına çevirir.</p>
    <ul class="feature-tech">
      <li>Tek doğruluk kaynağı</li>
      <li>İnsan tarafından okunabilir oluşturulmuş dosyalar</li>
      <li>Yedeklemesi ve geri yüklemesi kolay</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>REST API</h3>
    <p class="feature-problem"><strong>Sorun:</strong> Manuel tıklamalar ölçeklenmez.</p>
    <p class="feature-solution"><strong>Stratum nasıl çözer:</strong> FastAPI arka ucu, her yapılandırma yüzeyini bir REST API aracılığıyla sunar. Özel panolar oluşturun, dağıtımları otomatikleştirin veya mevcut CI/CD'nizle entegre edin.</p>
    <ul class="feature-tech">
      <li>JSON tabanlı uç noktalar</li>
      <li>Oturum ve Token tabanlı kimlik doğrulama</li>
      <li>Ansible / Terraform için mükemmel</li>
    </ul>
  </article>
</section>
</div>
</div>
