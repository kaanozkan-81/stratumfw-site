---
title: "Dokümantasyon"
description: "Stratum Firewall v0.28.3 resmi dokümantasyonu — VGA ISO ve Serial IMG kurulumu, OPNsense göçü, pf kuralları, Çoklu WAN politika yönlendirme, Suricata IPS, DNSBL, WireGuard, OpenVPN ve REST API."
lead: "FreeBSD 15.1 tabanlı Stratum Firewall'ı kurmak, taşımak, yapılandırmak ve işletmek için ihtiyacınız olan tüm teknik rehberler."
full_width: true
hide_pages: true
---

<div class="docs-layout">

<div class="docs-intro">
  <p>Stratum Firewall; ZFS Önyükleme Ortamları (Boot Environments), bildirimsel yapılandırma motoru ve 60 saniyelik Güvenli Uygulama (Safe-Apply) geri alma mimarisiyle anahtar teslim bir FreeBSD 15.1 güvenlik duvarı işletim sistemi olarak sunulur. Kuruluma başlamak için <a href="/tr/download/">VGA ISO veya Serial IMG imajını indirin</a> ya da önce <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">Canlı Tarayıcı Demosu</a> üzerinden arayüzü inceleyin.</p>
</div>

<section id="getting-started" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">01</span>
    <h2>Başlarken ve Göç</h2>
    <p>ISO/IMG medyasından kurulum yapın, ilk önyükleme ayarlarını tamamlayın veya mevcut OPNsense cihazınızı taşıyın.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Kurulum Rehberi (.iso &amp; .img)</h3>
      <p>SHA-256 doğrulaması yapın, VGA ISO veya Seri Konsol USB imajını yazdırın ve ZFS üzerine kurun.</p>
      <a href="/tr/resources/documentation/installation/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>İlk Yapılandırma</h3>
      <p>WAN/LAN arayüzlerini atayın, DHCP/PPPoE bağlantılarını, yönetici güvenliğini ve güncellemeleri ayarlayın.</p>
      <a href="/tr/resources/documentation/initial-configuration/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>OPNsense'den Geçiş</h3>
      <p>OPNsense <code>config.xml</code> kurallarını, VLAN/VIP yapılarını ve OpenVPN sunucusunu kesintisiz devralın.</p>
      <a href="/tr/resources/documentation/opnsense-migration/" class="card-link">Rehberi Oku →</a>
    </div>
  </div>
</section>

<section id="networking" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">02</span>
    <h2>Panel, Güvenlik Duvarı ve Yönlendirme</h2>
    <p>Özelleştirilebilir web panelini yönetin, durum bilgili pf kuralları yazın ve Çoklu WAN trafiğini yönlendirin.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Panel Turu ve Operasyonlar</h3>
      <p>12 sütunlu kontrol panelini özelleştirin, servis durumlarını yönetin, RBAC ve şifreli SFTP yedeklerini kurun.</p>
      <a href="/tr/resources/documentation/panel-tour/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>pf Kuralları, NAT ve Takma Adlar</h3>
      <p>Arayüz grupları, port yönlendirme, NAT yansıması, Ülke (GeoIP)/Hostname takma adları ve hız sınırı tanımlayın.</p>
      <a href="/tr/resources/documentation/pf-rules/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>Çoklu WAN, Politika Yönlendirme ve VIP</h3>
      <p>1 saniyelik ağ geçidi izleme, otomatik hat yedekleme, kural bazlı WAN seçimi, Sanal IP ve statik rota kurun.</p>
      <a href="/tr/resources/documentation/multi-wan/" class="card-link">Rehberi Oku →</a>
    </div>
  </div>
</section>

<section id="security-vpn" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">03</span>
    <h2>Tehdit Koruması ve VPN</h2>
    <p>IP, DNS ve paket yükü katmanlarında tehditleri durdurun ve uzaktaki kullanıcıları güvenle bağlayın.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Engelleme Listeleri, Web Filtresi ve IPS</h3>
      <p>63 IP tehdit beslemesini, Unbound DNSBL, 55 kategorili Web Filtresi, Suricata IPS ve birleşik raporları açın.</p>
      <a href="/tr/resources/documentation/blocklists/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>VPN Genel Bakış (WireGuard &amp; OpenVPN)</h3>
      <p>WireGuard ve OpenVPN'i karşılaştırın, çift WAN OpenVPN erişimini ve VPN-only hesapları yapılandırın.</p>
      <a href="/tr/resources/documentation/vpn/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>WireGuard VPN Kurulumu</h3>
      <p>Çekirdek tabanlı <code>if_wg</code> tünellerini cihaz içi anahtar üretimi, QR kod ve VLAN izinleriyle devreye alın.</p>
      <a href="/tr/resources/documentation/wireguard-vpn/" class="card-link">Rehberi Oku →</a>
    </div>
  </div>
</section>

<section id="reference" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">04</span>
    <h2>Otomasyon, Tanılama ve Referans</h2>
    <p>REST API ile otomatikleştirin, MCP üzerinden yapay zeka bağlayın, paket yakalayın ve dosya yapısını inceleyin.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>REST API, MCP ve SNMP</h3>
      <p>FastAPI uç noktalarıyla otomasyon kurun, MCP ile yapay zeka tanılama yapın ve SNMP ile izleyin.</p>
      <a href="/tr/resources/documentation/rest-api/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>Tanılama ve Komut Satırı</h3>
      <p>Wireshark <code>.pcap</code> paket yakalama, ARP/Wake-on-LAN, Web Etkinliği ve Stratum CLI araçlarını kullanın.</p>
      <a href="/tr/resources/documentation/command-line/" class="card-link">Rehberi Oku →</a>
    </div>
    <div class="card">
      <h3>Dosya ve Yol Referansı</h3>
      <p><code>config.json</code> kaynağını, gizli anahtar dizinlerini, üretilen dosyaları ve ZFS önyükleme ortamlarını tanıyın.</p>
      <a href="/tr/resources/documentation/file-paths/" class="card-link">Rehberi Oku →</a>
    </div>
  </div>
</section>

<section class="docs-contact">
  <h3>Kurulum veya yapılandırma konusunda sorunuz mu var?</h3>
  <p><a href="/tr/resources/faq/">Sıkça Sorulan Sorular</a> ve <a href="/tr/resources/changelog/">Sürüm Notları</a> sayfalarına göz atabilir veya doğrudan ekibimize ulaşabilirsiniz.</p>
  <a href="/tr/company/contact/" class="btn btn-primary">Bize Ulaşın</a>
</section>

</div>
