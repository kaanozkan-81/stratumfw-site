---
title: "Özellikler"
description: "Stratum Firewall v0.30.0 tüm özellikleri: durum bilgili pf kuralları, Çoklu WAN politika yönlendirmesi, 63 IP tehdit listesi, Unbound DNSBL, 55 kategorili Web Filtresi, Suricata 8 IPS/IDS, WireGuard, OpenVPN ve ZFS geri alma."
lead: "Stratum Firewall, kendini kanıtlamış pf paket filtresini şimşek hızındaki bir FastAPI yönetim paneliyle birleştirir. Tescilli bağımlılıklar yok, mutlak kontrol var. Modern altyapılar için güvenlik odaklı bir mimariyle inşa edildi."
full_width: true
seo_title: "Özellikler — pf, Suricata 8 IPS, 55 Kategorili Web Filtresi ve Çoklu WAN VPN"
---

<!-- STRATUM-TELEMETRY-SHOWCASE-START -->
<div class="stratum-telemetry-banner">
<div class="stratum-tel-visual" onclick="if(typeof openLightbox==='function'){openLightbox('/images/features/dpi-telemetry.jpg')}else{window.open('/images/features/dpi-telemetry.jpg','_blank')}"><img src="/images/features/dpi-telemetry.jpg" alt="FreeBSD 15.1 pf ve Suricata 8 DPI Veri Hattı — 37.287 Kural, 2,11+ Milyar Paket, %0,00 Kayıp" loading="lazy"></div>
<div class="stratum-tel-body">
<div class="stratum-tel-eyebrow"><span class="stratum-tel-dot"></span>Canlı Üretim Telemetrisi · Çoklu WAN & Çoklu VLAN</div>
<h3 class="stratum-tel-title">Sahada Kanıtlanmış Çekirdek ve DPI Performansı</h3>
<div class="stratum-tel-grid">
<div class="stratum-tel-stat"><span class="stratum-tel-val amber">37.287</span><span class="stratum-tel-lbl">Aktif ET Open Suricata Kuralı</span></div>
<div class="stratum-tel-stat"><span class="stratum-tel-val">2,11+ Milyar</span><span class="stratum-tel-lbl">İncelenen Canlı Üretim Paketi</span></div>
<div class="stratum-tel-stat"><span class="stratum-tel-val emerald">%0,00</span><span class="stratum-tel-lbl">Çekirdek Tamponu Paket Kaybı</span></div>
</div>
<p class="stratum-tel-desc">Standart güvenlik duvarı dağıtımlarında on binlerce derin paket inceleme imzası etkinleştirildiğinde çekirdek tamponlarında darboğaz ve paket düşmeleri yaşanırken; Stratum Firewall, donanım belleğine göre otomatik ölçeklenen Suricata 8 denetimi ve FreeBSD 15.1 çekirdek veri düzlemi sayesinde çoklu WAN ve çoklu VLAN taşıyan canlı kurumsal üretim trafiğinde 37.287 aktif ET Open kuralı altında 2,11 milyarı aşkın paketi %0,00 paket kaybıyla işler.</p>
</div>
</div>
<!-- STRATUM-TELEMETRY-SHOWCASE-END -->

<!-- STRATUM-OPNSENSE-SHOWCASE-START -->
<div class="opn-feature-showcase">
  <div class="opn-showcase-head">
    <span class="opn-showcase-eyebrow">Kesintisiz Geçiş Motoru &middot; Sıfırdan Kurulum Gerekmez</span>
    <h2>OPNsense <code>config.xml</code> ve OpenVPN Doğrudan Geçiş Desteği</h2>
    <p>Güvenlik duvarı işletim sistemini değiştirmek, yüzlerce kuralı hafta sonu boyunca elle yeniden yazmak veya uzaktaki çalışanların bilgisayarlarına tek tek yeni VPN profili dağıtmak anlamına gelmemeli. Mevcut OPNsense <code>config.xml</code> yedeğinizi <strong>Sistem &rarr; Yedekleme &rarr; OPNsense'den İçe Aktar</strong> ekranına yükleyip dakikalar içinde önizlemeli geçiş yapabilirsiniz.</p>
  </div>

  <div class="opn-showcase-visual">
    <img src="/images/features/opnsense-migration.jpg" alt="OPNsense config.xml yedek dosyasından Stratum Firewall FreeBSD 15.1 sistemine doğrudan geçiş mimarisi" width="1280" height="714" loading="eager">
    <div class="opn-showcase-caption">
      <span><b>KAYNAK:</b> OPNsense config.xml (Kurallar, VLAN, Sanal IP, DHCP, DNS, OpenVPN PKI)</span>
      <span><b>MOTOR:</b> Etkileşimli Port Eşleştirici ve Ön Kontrol Raporu &rarr; Stratum FW v0.30.0</span>
    </div>
  </div>

  <div class="opn-showcase-grid">
    <div class="opn-mig-card">
      <h3>1. Etkileşimli Port, VLAN ve Sanal IP Eşleştirme</h3>
      <p>Kaydetmeden önce OPNsense ağ kartlarını yeni donanımınızın portlarıyla eşleştirin; <code>802.1Q</code> VLAN'ları, arayüz gruplarını, statik rotaları, <code>/29</code> Sanal IP'leri ve statik/PPPoE/DHCP Çoklu WAN hatlarını tek adımda taşıyın.</p>
    </div>
    <div class="opn-mig-card">
      <h3>2. pf Kuralları, NAT ve Politika Yönlendirmesi</h3>
      <p>Hem klasik hem yeni MVC formatındaki güvenlik kurallarını, giden NAT ayarlarını ve kaynak kısıtlamaları korunmuş Port Yönlendirmelerini aktarır; kural bazlı WAN seçimleri Politika Yönlendirmesine çevrilir, zamanlamalı kurallar güvenlik için pasif alınır.</p>
    </div>
    <div class="opn-mig-card">
      <h3>3. İstemciye Dokunmadan OpenVPN Devralma</h3>
      <p>OpenVPN CA sertifikasını, sunucu anahtarlarını, <code>tls-crypt</code> / <code>tls-auth</code> anahtarlarını, istemci sertifikalarını ve kullanıcı parolalarını (PHP <code>$2y$</code> bcrypt ve <code>argon2id</code> dahil) birebir aktarır; sahadaki cihazlar <code>.ovpn</code> değiştirmeden bağlanır.</p>
    </div>
    <div class="opn-mig-card">
      <h3>4. Takma Adlar, GeoIP ve Tehdit Listeleri</h3>
      <p>İç içe takma adları çözümler, alan adı kayıtlarını 5 dakikada bir güncellenen Hostname takma adına, GeoIP kurallarını ücretsiz 5-RIR ülke tablolarına dönüştürür ve URL tablolarındaki katalog listelerini (<code>CINS Army</code>, <code>blocklist.de</code> vb.) otomatik açar.</p>
    </div>
    <div class="opn-mig-card">
      <h3>5. Zorunlu DNS, DHCP ve Hız Sınırı Aktarımı</h3>
      <p><code>dnsmasq</code> ve <code>ISC dhcpd</code> havuzlarını, sabit MAC eşleşmelerini, Unbound kayıtlarını ve <code>dummynet</code> hız limitlerini taşır; DNS yönlendirme port kurallarını istisna takma adlarıyla birlikte doğrudan <strong>Zorunlu DNS</strong> ayarına çevirir.</p>
    </div>
    <div class="opn-mig-card">
      <h3>6. Gizlilik Korumalı Rapor ve 60 sn Safe-Apply</h3>
      <p>Yüklenen XML dosyası yalnızca bellekte okunur, diske kaydedilmez ve raporda hiçbir gizli anahtar gösterilmez. Uygulama öncesi tüm çakışmalar raporlanır ve geçiş 60 saniyelik otomatik geri alma sayacıyla korunur.</p>
    </div>
  </div>

  <div class="opn-showcase-actions">
    <span class="opn-showcase-note">Gerçek OPNsense profilleri, çoklu VLAN topolojileri ve canlı OpenVPN istemcileriyle doğrulanmıştır.</span>
    <div class="opn-showcase-btns">
      <a href="/tr/resources/documentation/opnsense-migration/" class="btn btn-primary">OPNsense Geçiş Rehberini Oku &rarr;</a>
      <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Canlı Demoda İncele</a>
    </div>
  </div>
</div>
<!-- STRATUM-OPNSENSE-SHOWCASE-END -->

<!-- STRATUM-V0300-FEATURES-START -->
<section class="section" style="padding-top: 12px; padding-bottom: 28px;">
  <div class="section-head" style="max-width: 820px; margin: 0 auto 28px auto; text-align: center;">
    <span class="eyebrow">v0.28.4 – v0.30.0 Yenilikleri</span>
    <h2>İki Aşamalı Kimlik Doğrulama, Zorunlu DNS ve Dinamik Çoklu WAN</h2>
    <p>Aşağıdaki tüm kurumsal yetenekler FreeBSD 15.1 tabanlı Stratum Firewall v0.30.0 sürümünde hiçbir eklenti, bulut bağımlılığı veya yıllık lisans ücreti olmadan yerleşik gelir.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Yerleşik İki Aşamalı Giriş — TOTP 2FA (<code>v0.30.0</code>)</h3>
      <p>Yönetici ve operatör hesaplarını 6 haneli doğrulayıcı uygulama kodlarıyla (<em>Google/Microsoft Authenticator, 1Password, Bitwarden</em>) koruyun. QR kodu tarayıcıda yerel çizilir; gizli anahtar cihazdan dışarı çıkmaz. Tek kullanımlık kod koruması, 10 kurtarma kodu ve konsoldan sıfırlama dahildir.</p>
    </div>
    <div class="card">
      <h3>AD İstisnalı Zorunlu DNS (<code>v0.29.2</code>)</h3>
      <p>Seçilen iç ağlarda elle DNS (<code>8.8.8.8</code>) girilmiş cihazların sorgularını güvenlik duvarına yönlendirin ve DNS over TLS (port 853) kaçışlarını kapatın; Active Directory etki alanı denetleyicilerini İstisna takma adıyla hariç tutun ve hiçbir gezinme kaydı tutmadan DNSBL/Web Filtresini zorunlu kılın.</p>
    </div>
    <div class="card">
      <h3>Dinamik DHCP Çoklu WAN (<code>v0.29.1</code>)</h3>
      <p>Kablo modem, 4G/5G LTE veya Starlink hatlarını varsayılan rotayı bozmadan ikincil/üçüncül DHCP WAN olarak bağlayın. Operatör ağ geçidi değiştiğinde yedekleme, <code>reply-to</code> ve Politika Yönlendirme kuralları Uygula gerektirmeden saniyeler içinde güncellenir.</p>
    </div>
    <div class="card">
      <h3>Zamanlamalı pf Kuralları (<code>v0.29.0</code>)</h3>
      <p>İsimlendirilmiş saat aralıklarını (<code>Mon-Fri 08:00-18:00</code>) kurallara bağlayın. <code>stratum-pfrefresh</code> ile dakikada bir Uygula gerektirmeden devreye girer; süre bittiğinde zamanlamalı izin kurallarının açtığı mevcut bağlantıları otomatik kapatır.</p>
    </div>
    <div class="card">
      <h3>Çift Yönlü 1:1 NAT (<code>v0.28.9</code>)</h3>
      <p>İç sunuculara veya alt ağlara kendilerine ait genel Sanal IP (VIP) ya da yönlendirilmiş genel ağ bloklarını çift yönlü atayın; WAN'ın kendi IP adresini reddeden kilitlenme koruması ve canlı WAN izin kuralı sayacıyla güvenle yönetin.</p>
    </div>
    <div class="card">
      <h3>1s / 24s Güvenlik Duvarı Günlük Özeti (<code>v0.28.8</code>)</h3>
      <p>Son 1 veya 24 saatteki en yoğun kaynak IP'leri (ülkesiyle), hedef portları (<code>SSH</code>, <code>RDP</code>), ülkeleri (GeoIP), anlaşılır sistem kural isimlerini ve 5 dakikalık/saatlik arayüz grafiklerini tek tıkla canlı günlüğe filtreleyerek inceleyin.</p>
    </div>
  </div>
</section>
<!-- STRATUM-V0300-FEATURES-END -->

<style>
.feature-showcase-page {
  width: 100%;
  max-width: 100%;
}
.feature-module {
  margin-bottom: 88px;
  padding-bottom: 72px;
  border-bottom: 1px solid #e2e8f0;
}
.feature-module:last-of-type {
  margin-bottom: 24px;
  padding-bottom: 0;
  border-bottom: none;
}
.feature-module-head {
  width: 100%;
  max-width: 100%;
  margin-bottom: 28px;
}
.feature-badge-pill {
  display: inline-block;
  padding: 6px 14px;
  background: #fffbeb;
  color: #d97706;
  border: 1px solid rgba(245, 158, 11, 0.35);
  font-size: 12.5px;
  font-weight: 700;
  border-radius: 30px;
  margin-bottom: 14px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
.feature-module-head h2 {
  font-family: var(--font-display);
  margin: 0 0 14px 0;
  font-size: 34px;
  font-weight: 800;
  line-height: 1.22;
  letter-spacing: -0.015em;
  color: #0f1e3d;
}
.feature-module-lead {
  font-size: 19px;
  font-weight: 600;
  color: #1e293b;
  margin: 0 0 14px 0;
  line-height: 1.6;
}
.feature-module-desc {
  font-size: 16.5px;
  color: #475569;
  line-height: 1.8;
  margin: 0;
}
.feature-visual-box {
  width: 100%;
  margin: 32px 0 36px 0;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 24px 48px rgba(15, 30, 61, 0.16);
  border: 1px solid rgba(15, 30, 61, 0.08);
  background: #0b1320;
}
.feature-visual-box img {
  display: block;
  width: 100%;
  height: auto;
  object-fit: cover;
}
.feature-module .card-grid.grid-3 {
  margin-top: 28px;
}
.feature-module .card h3 {
  font-family: var(--font-display);
  margin: 0 0 10px 0;
  font-size: 18.5px;
  font-weight: 700;
  color: #0f1e3d;
}
.feature-module .card p {
  margin: 0;
  font-size: 15px;
  line-height: 1.65;
  color: #475569;
}
</style>
<div class="feature-showcase-page">
<div class="feature-module" id="ag-topolojisi">
<div class="feature-module-head">
<span class="feature-badge-pill">01 · AĞ TOPOLOJİSİ &amp; VLAN YÖNETİMİ</span>
<h2>Ağ Topolojisi ve Çoklu VLAN Bölütleme ile Tam İzolasyon</h2>
<p class="feature-module-lead">Fiziksel ağ kartlarınızı ve sanal yerel ağlarınızı (VLAN) tek bir merkezden yöneterek ofis, misafir, personel ve IoT cihaz trafiğini birbirinden güvenli biçimde ayırın.</p>
<p class="feature-module-desc">Stratum Firewall, karmaşık kurumsal ve kişisel ağ topolojilerini sadeleştiren esnek bir çoklu arayüz mimarisi sunar. Fiziksel ağ portlarınızı (<code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, <code>igb3</code> OPT2) ihtiyacınıza göre yapılandırabilir; aynı fiziksel altyapı üzerinde Ofis LAN, Misafir Wi-Fi, Personel ve Güvenlik Kameraları (IoT) gibi mantıksal olarak tamamen ayrıştırılmış 802.1Q VLAN bölgeleri oluşturabilirsiniz. Entegre DHCP Sunucusu, DNS Çözümleyici (Resolver) ve NTP zaman senkronizasyonu servisleri sayesinde her alt ağ kendi IP adresleme havuzu, alt ağ sınırları ve VLAN'lar arası güvenlik duvarı politikalarıyla bağımsız olarak çalışır.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall çoklu arayüz, WAN, Ofis LAN, Misafir Wi-Fi, IoT VLAN bölütleme ve WireGuard VPN ağ topolojisi diyagramı" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Çoklu WAN ve Arayüz Ataması</h3>
<p><code>pppoe0</code>, <code>igb1</code>, <code>igb2</code> ve <code>igb3</code> dahil tüm fiziksel ağ kartlarını yerel PPPoE, statik IP ve dinamik DHCP desteğiyle esnek biçimde tanımlayın.</p>
</div>
<div class="card">
<h3>Mantıksal VLAN Segmentasyonu</h3>
<p>Misafir kablosuz ağlarını, IoT cihazlarını ve güvenlik kameralarını kritik yerel sunuculardan izole ederek ağ içi yatay tehdit yayılımını tamamen engelleyin.</p>
</div>
<div class="card">
<h3>Yerleşik Temel Ağ Servisleri</h3>
<p>Her VLAN ve fiziksel arayüz için bağımsız DHCP adres havuzlarını, statik IP eşlemelerini, yerel DNS çözümlemeyi ve NTP senkronizasyonunu tek panelden yönetin.</p>
</div>
</div>
</div>
<div class="feature-module" id="pf-ag-kalkani">
<div class="feature-module-head">
<span class="feature-badge-pill">02 · FREEBSD PF ÇEKİRDEK KORUMASI</span>
<h2>Ağ Kalkanı ve Kernel Düzeyinde pf Paket Filtreleme</h2>
<p class="feature-module-lead">FreeBSD 15.1/15 çekirdeğinde çalışan yerel pf (Packet Filter) motoru, durum bilgili paket denetimi (Stateful Inspection) ve Safe-Apply kural doğrulamasıyla ağınızı kesintisiz korur.</p>
<p class="feature-module-desc">Stratum Firewall, gücünü doğrudan FreeBSD 15.1/15 işletim sistemi çekirdeğindeki kendini kanıtlamış <code>pf</code> (Packet Filter) altyapısından alır. Gelen ve giden her veri paketi çekirdek düzeyinde durum bilgili denetimden (Stateful Inspection) geçirilir; yetkisiz erişim girişimleri anında düşürülürken güvenli trafik minimum işlemci yükü ve gecikmeyle iletilir. Gelişmiş Ağ Adresi Dönüştürme (NAT) ve ALTQ tabanlı bant genişliği/trafik şekillendirme yetenekleri yoğun yük altında bile kritik uygulamaların performansını korur. Ayrıca <strong>Safe-Apply</strong> mekanizması, yazdığınız her kuralı canlı sisteme uygulamadan önce <code>pf</code> sözdizimine göre otomatik test ederek hatalı yapılandırmaların sizi sistem dışına kilitlemesini önler.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD çekirdek düzeyinde pf paket filtreleme ve çok katmanlı siber güvenlik kalkanı görseli" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Kernel Düzeyinde Stateful Inspection</h3>
<p>Kullanıcı alanı (user-space) darboğazlarına takılmadan, doğrudan FreeBSD 15.1/15 çekirdeğinde çalışan yüksek verimli paket filtreleme ve durum takibi.</p>
</div>
<div class="card">
<h3>Safe-Apply Otomatik Doğrulama</h3>
<p>Güvenlik duvarı kurallarını ve arayüz değişikliklerini canlı çekirdeğe işlemeden önce sözdizimi testinden geçirerek bağlantı kopmalarını ve kilitlenmeleri önleme.</p>
</div>
<div class="card">
<h3>NAT, ALTQ ve Canlı Kural İstatistikleri</h3>
<p>Yeniden kullanılabilir Takma Adlar (Aliases), <code>Pass / Block / Reject</code> politikaları, anlık kural eşleşme sayıları (Hits) ve ALTQ trafik önceliklendirme kontrolü.</p>
</div>
</div>
</div>
<div class="feature-module" id="dnsbl-web-filtreleme">
<div class="feature-module-head">
<span class="feature-badge-pill">03 · TEHDİT İSTİHBARATI &amp; DNSBL</span>
<h2>DNS Tabanlı Web Filtreleme (DNSBL) ve Otomatik IP Karalisteleri</h2>
<p class="feature-module-lead">Küresel tehdit istihbarat akışlarını (threat feeds) ve DNS karalistelerini kullanarak zararlı IP adreslerini ve kötü amaçlı alan adlarını ağınıza ulaşmadan durdurun.</p>
<p class="feature-module-desc">Stratum Firewall, geleneksel port ve protokol kurallarını iki katmanlı proaktif tehdit istihbaratıyla güçlendirir. <strong>IP Karaliste (IP Blocklists)</strong> modülü, açık kaynaklı ve kurumsal tehdit istihbaratı akışlarını (threat feeds) otomatik olarak içeri aktarır; bilinen botnet komuta-kontrol sunucularını, tarayıcıları ve saldırgan IP aralıklarını çekirdek düzeyindeki yüksek hızlı <code>pf</code> tablolarına yükleyerek paket seviyesinde engeller. <strong>DNSBL (DNS Tabanlı Web Filtreleme)</strong> motoru ise oltalama (phishing), zararlı yazılım (malware), izleyici ve reklam alan adlarını DNS sorgusu aşamasında yakalayarak <code>NXDOMAIN</code> yanıtıyla düşürür; üstelik dahili alan adı sorgulama aracıyla herhangi bir domain'in filtre durumunu arayüzden anında test etmenize olanak tanır.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL alan adı filtreleme ve tehdit istihbaratı IP karaliste koruma katmanları" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Ağ Genelinde DNSBL Engelleme</h3>
<p>İstemci cihazlara ek yazılım kurmaya gerek kalmadan, zararlı ve istenmeyen alan adlarına yapılan sorguları DNS katmanında <code>NXDOMAIN</code> yanıtıyla kesme.</p>
</div>
<div class="card">
<h3>Dinamik IP Tehdit Akışları</h3>
<p>Güncel siber tehdit istihbaratı listelerini doğrudan çekirdek <code>pf</code> tablolarına aktararak saldırgan IP ağlarını sıfır gecikme kaybıyla sınırda bloklama.</p>
</div>
<div class="card">
<h3>Anlık Alan Adı Test Aracı</h3>
<p>Herhangi bir alan adının aktif DNSBL politikalarınız tarafından engellenip engellenmediğini yönetim paneli üzerinden gerçek zamanlı sorgulama ve doğrulama.</p>
</div>
</div>
</div>
<div class="feature-module" id="modern-api-mimarisi">
<div class="feature-module-head">
<span class="feature-badge-pill">04 · MODERN MİMARİ &amp; API KONTROLÜ</span>
<h2>Sıfır Bağımlılıklı Siber Güvenlik Mimarisi ve Modern API Kontrolü</h2>
<p class="feature-module-lead">"Saf FreeBSD gücü. Modern API kontrolü." Asenkron Python 3.12 / FastAPI arka ucu, tekil config.json bildirimsel yapılandırması ve yerel WireGuard kriptografisiyle yeni nesil güvenlik duvarı deneyimi.</p>
<p class="feature-module-desc">Hantal ve eski nesil PHP/XML tabanlı güvenlik duvarı arayüzlerinin yerini almak üzere sıfırdan tasarlanan Stratum Firewall, FreeBSD çekirdek performansını geliştirici odaklı modern bir kontrol katmanıyla buluşturur. Asenkron <strong>Python 3.12</strong> ve <strong>FastAPI</strong> üzerine inşa edilen arka uç, çok düşük bellek tüketimiyle hem hızlı bir web arayüzü hem de tam kapsamlı bir <strong>REST API</strong> sunar. Tüm sistem durumu tek bir okunabilir, bildirimsel <code>config.json</code> dosyasında tutulur ve çekirdek motor tarafından dinamik olarak yerel <code>pf.conf</code>, DHCP ve DNS yapılandırma dosyalarına derlenir. Harici kütüphane gerektirmeyen sıfırdan yazılmış X25519 (RFC 7748) kriptografili <strong>WireGuard VPN</strong> ve <strong>OpenVPN</strong> desteği, Rol Tabanlı Erişim Kontrolü (RBAC) ve canlı sistem telemetrisiyle ağ güvenliğine modern "Infrastructure-as-Code" sadeliği kazandırır.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API kontrol paneli, WireGuard X25519 şifreleme ve canlı telemetri mimarisi" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>FastAPI &amp; Bildirimsel config.json</h3>
<p>Tüm güvenlik duvarı durumunu denetlenebilir tek bir JSON kaynağından yönetin ve tüm ağ operasyonlarını asenkron REST API üzerinden otomatikleştirin.</p>
</div>
<div class="card">
<h3>Sıfır Bağımlılıklı WireGuard &amp; OpenVPN</h3>
<p>Harici paket bağımlılığı olmadan sıfırdan yazılmış yerel RFC 7748 X25519 anahtar üretimiyle yüksek hızlı WireGuard ve esnek OpenVPN tünelleme.</p>
</div>
<div class="card">
<h3>RBAC ve Canlı Sistem Telemetrisi</h3>
<p>Yönetici, Operatör ve Salt Okunur erişim rolleriyle yetkilendirmeyi ayırırken canlı CPU, bellek, disk, sıcaklık, trafik ve güvenlik duvarı loglarını izleyin.</p>
</div>
</div>
</div>
</div>
