---
title: "Özellikler"
description: "Stratum Firewall v0.30.0 tüm özellikleri: durum bilgili pf kuralları, Çoklu WAN politika yönlendirmesi, 63 IP tehdit listesi, Unbound DNSBL, 55 kategorili Web Filtresi, Suricata 8 IPS/IDS, WireGuard, OpenVPN ve ZFS geri alma."
lead: "Stratum Firewall, kendini kanıtlamış pf paket filtresini şimşek hızındaki bir FastAPI yönetim paneliyle birleştirir. Tescilli bağımlılıklar yok, mutlak kontrol var. Modern altyapılar için güvenlik odaklı bir mimariyle inşa edildi."
full_width: true
seo_title: "Özellikler — pf, Suricata 8 IPS, 55 Kategorili Web Filtresi ve Çoklu WAN VPN"
seoTitle: "Özellikler: FreeBSD pf, Suricata 8 DPI ve 2FA | Stratum FW"
seoDescription: "FreeBSD 15.1 üzerinde durum korumalı pf kuralları, renkli ayırıcılar, Suricata 8 DPI, 55 kategorili Web Filtresi, AD istisnalı Zorunlu DNS, TOTP 2FA ve Çoklu WAN."
---

<!-- STRATUM-DPI-SHOWCASE-START -->
<style>
.stratum-dpi-showcase{position:relative;margin:2.5rem 0;border-radius:20px;overflow:hidden;background:radial-gradient(120% 90% at 50% 0%,#101f3c 0%,#08101f 52%,#060b16 100%) !important;border:1px solid rgba(245,158,11,0.24);box-shadow:0 24px 60px rgba(2,6,23,0.55),inset 0 1px 0 rgba(255,255,255,0.06);}
.stratum-dpi-top{position:relative;z-index:3;padding:1.85rem 2.25rem 0.5rem;text-align:center;}
.stratum-dpi-eyebrow{display:inline-flex;align-items:center;gap:0.5rem;padding:0.32rem 0.85rem;border-radius:999px;background:rgba(245,158,11,0.12);border:1px solid rgba(245,158,11,0.32);color:#fbbf24 !important;font-size:0.74rem;font-weight:700;letter-spacing:0.09em;text-transform:uppercase;margin-bottom:0.8rem;}
.stratum-dpi-dot{width:7px;height:7px;border-radius:50%;background:#34d399;box-shadow:0 0 10px #34d399;}
.stratum-dpi-title{margin:0 auto !important;padding:0 !important;border:0 !important;max-width:960px;color:#f8fafc !important;font-size:clamp(1.35rem,2.2vw,1.88rem) !important;font-weight:800 !important;line-height:1.24 !important;letter-spacing:-0.02em;}
.stratum-dpi-metrics{position:relative;z-index:3;max-width:980px;margin:1.15rem auto 0;display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:1rem;}
.stratum-dpi-metric{padding:0.9rem 1.05rem;border-radius:14px;background:rgba(10,18,35,0.86);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:1px solid rgba(148,163,184,0.18);box-shadow:0 10px 24px rgba(2,6,23,0.4);text-align:center;}
.stratum-dpi-metric--amber{border-top:2px solid rgba(251,191,36,0.8);}
.stratum-dpi-metric--cyan{border-top:2px solid rgba(56,189,248,0.8);}
.stratum-dpi-metric--emerald{border-top:2px solid rgba(52,211,153,0.8);}
.stratum-dpi-val{display:block;font-size:clamp(1.45rem,2.05vw,1.92rem);font-weight:800;line-height:1.15;letter-spacing:-0.02em;font-variant-numeric:tabular-nums;white-space:nowrap;margin-bottom:0.28rem;}
.stratum-dpi-val--amber{color:#fbbf24 !important;}
.stratum-dpi-val--cyan{color:#38bdf8 !important;}
.stratum-dpi-val--emerald{color:#34d399 !important;}
.stratum-dpi-lbl{display:block;color:#cbd5e1 !important;font-size:0.82rem;font-weight:600;line-height:1.35;}
.stratum-dpi-visual{position:relative;z-index:1;margin:0.5rem auto 0;max-width:1100px;}
.stratum-dpi-visual img{display:block;width:100% !important;max-width:100% !important;height:auto !important;margin:0 !important;padding:0 !important;border:0 !important;border-radius:0 !important;box-shadow:none !important;-webkit-mask-image:linear-gradient(180deg,transparent 0%,#000 6%,#000 95%,transparent 100%);mask-image:linear-gradient(180deg,transparent 0%,#000 6%,#000 95%,transparent 100%);}
.stratum-dpi-bottom{position:relative;z-index:3;padding:1.4rem 2.25rem 2.05rem;background:linear-gradient(180deg,rgba(7,13,25,0.88) 0%,rgba(5,9,18,0.98) 100%);border-top:1px solid rgba(148,163,184,0.14);text-align:center;}
.stratum-dpi-desc{max-width:880px;margin:0 auto 1.1rem !important;color:#cbd5e1 !important;font-size:1rem !important;line-height:1.72 !important;}
.stratum-dpi-desc strong{color:#f8fafc !important;font-weight:700;}
.stratum-dpi-pills{display:flex;flex-wrap:wrap;justify-content:center;gap:0.55rem;}
.stratum-dpi-pill{display:inline-flex;align-items:center;padding:0.34rem 0.8rem;border-radius:999px;background:rgba(30,41,59,0.65);border:1px solid rgba(148,163,184,0.22);color:#e2e8f0 !important;font-size:0.78rem;font-weight:600;}
@media (max-width:900px){.stratum-dpi-top{padding:1.5rem 1.25rem 0.4rem;}.stratum-dpi-val{white-space:normal;}.stratum-dpi-bottom{padding:1.25rem 1.25rem 1.65rem;}}
@media (max-width:680px){.stratum-dpi-metrics{grid-template-columns:1fr;gap:0.65rem;}.stratum-dpi-visual{margin-top:0.35rem;}}
</style>
<section class="stratum-dpi-showcase">
  <div class="stratum-dpi-top">
    <div class="stratum-dpi-eyebrow"><span class="stratum-dpi-dot"></span>Canlı Üretim Telemetrisi</div>
    <h2 class="stratum-dpi-title">Sahada Kanıtlanmış Çekirdek ve DPI Performansı</h2>
    <div class="stratum-dpi-metrics">
      <div class="stratum-dpi-metric stratum-dpi-metric--amber">
        <span class="stratum-dpi-val stratum-dpi-val--amber">37.287</span>
        <span class="stratum-dpi-lbl">Aktif ET Open Suricata Kuralı</span>
      </div>
      <div class="stratum-dpi-metric stratum-dpi-metric--cyan">
        <span class="stratum-dpi-val stratum-dpi-val--cyan">2,11+ Milyar</span>
        <span class="stratum-dpi-lbl">İncelenen Canlı Üretim Paketi</span>
      </div>
      <div class="stratum-dpi-metric stratum-dpi-metric--emerald">
        <span class="stratum-dpi-val stratum-dpi-val--emerald">%0,00</span>
        <span class="stratum-dpi-lbl">Çekirdek Tamponu Paket Kaybı</span>
      </div>
    </div>
  </div>
  <div class="stratum-dpi-visual">
    <img src="/images/features/dpi-telemetry.jpg" alt="Stratum Firewall FreeBSD 15.1 pf ve Suricata 8 DPI Canlı Üretim Telemetrisi" loading="lazy" decoding="async">
  </div>
  <div class="stratum-dpi-bottom">
    <p class="stratum-dpi-desc">Standart güvenlik duvarı dağıtımlarında on binlerce derin paket inceleme imzası etkinleştirildiğinde çekirdek tamponlarında darboğaz ve paket düşmeleri yaşanırken; <strong>Stratum Firewall</strong>, donanım belleğine göre otomatik ölçeklenen <strong>Suricata 8</strong> denetimi ve saf <strong>FreeBSD 15.1</strong> çekirdek veri düzlemi sayesinde çoklu WAN ve çoklu VLAN taşıyan canlı kurumsal üretim trafiğinde <strong>37.287 aktif ET Open kuralı</strong> altında <strong>2,11 milyarı aşkın paketi %0,00 paket kaybıyla</strong> işler.</p>
    <div class="stratum-dpi-pills">
      <span class="stratum-dpi-pill">Saf FreeBSD 15.1 pf</span>
      <span class="stratum-dpi-pill">Suricata 8 Çift Motor (netmap + pcap)</span>
      <span class="stratum-dpi-pill">Bellek Duyarlı Otomatik Ölçekleme</span>
      <span class="stratum-dpi-pill">Çoklu WAN ve Çoklu VLAN</span>
    </div>
  </div>
</section>
<!-- STRATUM-DPI-SHOWCASE-END -->

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
<section class="section stratum-dark-module">
  <div class="section-head" style="max-width: 820px; margin: 0 auto 28px auto; text-align: center;">
    <span class="eyebrow stratum-module-eyebrow">v0.28.8 – v0.31.1 Yenilikleri</span>
    <h2>İki Aşamalı Kimlik Doğrulama, Zorunlu DNS ve Dinamik Çoklu WAN</h2>
    <p>Aşağıdaki tüm kurumsal yetenekler FreeBSD 15.1 tabanlı Stratum Firewall v0.31.1 sürümünde hiçbir eklenti, bulut bağımlılığı veya yıllık lisans ücreti olmadan yerleşik gelir.</p>
  </div>
  <div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard"><h3>Kural ve NAT Ayırıcıları + Görünümler (v0.31.1)</h3><p>Güvenlik Duvarı, Port Yönlendirme ve manuel Giden NAT kurallarını pfSense tarzı renkli başlıklarla gruplayın; yukarı/aşağı oklarla sıralayın ve Tümü, Varsayılan, Özel görünümleri arasında geçiş yapın.</p></div>
<div class="card stratum-subcard"><h3>HTTPS Oturum Koruması ve Çoklu Adresli VPN (v0.30.5)</h3><p>Yönetim paneli oturum çerezini yalnızca HTTPS üzerinden iletilecek şekilde korur, çoklu dış adresli OpenVPN profillerinde her adresi ayrı remote satırına yazar ve dummynet hız sınırlayıcıyı yerleşik sunar.</p></div>
<div class="card stratum-subcard"><h3>OPNsense DNSBL ve Tam Havuzlu DHCP Geçişi (v0.30.2)</h3><p>OPNsense üzerindeki Unbound DNSBL listelerini (Steven Black, AdGuard, özel HTTPS kaynakları, izin/engel/joker kayıtları) ve ağ geçidi IP adresini kapsayan tam aralıklı DHCP havuzlarını bölmeden aktarır.</p></div>
    <div class="card stratum-subcard">
      <h3>Yerleşik İki Aşamalı Giriş — TOTP 2FA (<code>v0.30.0</code>)</h3>
      <p>Yönetici ve operatör hesaplarını 6 haneli doğrulayıcı uygulama kodlarıyla (<em>Google/Microsoft Authenticator, 1Password, Bitwarden</em>) koruyun. QR kodu tarayıcıda yerel çizilir; gizli anahtar cihazdan dışarı çıkmaz. Tek kullanımlık kod koruması, 10 kurtarma kodu ve konsoldan sıfırlama dahildir.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>AD İstisnalı Zorunlu DNS (<code>v0.29.2</code>)</h3>
      <p>Seçilen iç ağlarda elle DNS (<code>8.8.8.8</code>) girilmiş cihazların sorgularını güvenlik duvarına yönlendirin ve DNS over TLS (port 853) kaçışlarını kapatın; Active Directory etki alanı denetleyicilerini İstisna takma adıyla hariç tutun ve hiçbir gezinme kaydı tutmadan DNSBL/Web Filtresini zorunlu kılın.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Dinamik DHCP Çoklu WAN (<code>v0.29.1</code>)</h3>
      <p>Kablo modem, 4G/5G LTE veya Starlink hatlarını varsayılan rotayı bozmadan ikincil/üçüncül DHCP WAN olarak bağlayın. Operatör ağ geçidi değiştiğinde yedekleme, <code>reply-to</code> ve Politika Yönlendirme kuralları Uygula gerektirmeden saniyeler içinde güncellenir.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Zamanlamalı pf Kuralları (<code>v0.29.0</code>)</h3>
      <p>İsimlendirilmiş saat aralıklarını (<code>Mon-Fri 08:00-18:00</code>) kurallara bağlayın. <code>stratum-pfrefresh</code> ile dakikada bir Uygula gerektirmeden devreye girer; süre bittiğinde zamanlamalı izin kurallarının açtığı mevcut bağlantıları otomatik kapatır.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Çift Yönlü 1:1 NAT (<code>v0.28.9</code>)</h3>
      <p>İç sunuculara veya alt ağlara kendilerine ait genel Sanal IP (VIP) ya da yönlendirilmiş genel ağ bloklarını çift yönlü atayın; WAN'ın kendi IP adresini reddeden kilitlenme koruması ve canlı WAN izin kuralı sayacıyla güvenle yönetin.</p>
    </div>
    <div class="card stratum-subcard">
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
<div class="feature-module stratum-dark-pillar" id="ag-topolojisi">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">AĞ TOPOLOJİSİ &amp; VLAN YÖNETİMİ</span>
<h2>Ağ Topolojisi ve Çoklu VLAN Bölütleme ile Tam İzolasyon</h2>
<p class="feature-module-lead">Fiziksel ağ kartlarınızı ve sanal yerel ağlarınızı (VLAN) tek bir merkezden yöneterek ofis, misafir, personel ve IoT cihaz trafiğini birbirinden güvenli biçimde ayırın.</p>
<p class="feature-module-desc">Stratum Firewall, karmaşık kurumsal ve kişisel ağ topolojilerini sadeleştiren esnek bir çoklu arayüz mimarisi sunar. Fiziksel ağ portlarınızı (<code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, <code>igb3</code> OPT2) ihtiyacınıza göre yapılandırabilir; aynı fiziksel altyapı üzerinde Ofis LAN, Misafir Wi-Fi, Personel ve Güvenlik Kameraları (IoT) gibi mantıksal olarak tamamen ayrıştırılmış 802.1Q VLAN bölgeleri oluşturabilirsiniz. Entegre DHCP Sunucusu, DNS Çözümleyici (Resolver) ve NTP zaman senkronizasyonu servisleri sayesinde her alt ağ kendi IP adresleme havuzu, alt ağ sınırları ve VLAN'lar arası güvenlik duvarı politikalarıyla bağımsız olarak çalışır.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall çoklu arayüz, WAN, Ofis LAN, Misafir Wi-Fi, IoT VLAN bölütleme ve WireGuard VPN ağ topolojisi diyagramı" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Çoklu WAN ve Arayüz Ataması</h3>
<p><code>pppoe0</code>, <code>igb1</code>, <code>igb2</code> ve <code>igb3</code> dahil tüm fiziksel ağ kartlarını yerel PPPoE, statik IP ve dinamik DHCP desteğiyle esnek biçimde tanımlayın.</p>
</div>
<div class="card stratum-subcard">
<h3>Mantıksal VLAN Segmentasyonu</h3>
<p>Misafir kablosuz ağlarını, IoT cihazlarını ve güvenlik kameralarını kritik yerel sunuculardan izole ederek ağ içi yatay tehdit yayılımını tamamen engelleyin.</p>
</div>
<div class="card stratum-subcard">
<h3>Yerleşik Temel Ağ Servisleri</h3>
<p>Her VLAN ve fiziksel arayüz için bağımsız DHCP adres havuzlarını, statik IP eşlemelerini, yerel DNS çözümlemeyi ve NTP senkronizasyonunu tek panelden yönetin.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="pf-ag-kalkani">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">FREEBSD PF ÇEKİRDEK KORUMASI</span>
<h2>Ağ Kalkanı ve Çekirdek Düzeyinde pf Paket Filtreleme</h2>
<p class="feature-module-lead">FreeBSD 15.1/15 çekirdeğinde çalışan yerel pf (Packet Filter) motoru, durum bilgili paket denetimi (Stateful Inspection) ve Safe-Apply kural doğrulamasıyla ağınızı kesintisiz korur.</p>
<p class="feature-module-desc">Stratum Firewall, gücünü doğrudan FreeBSD 15.1/15 işletim sistemi çekirdeğindeki kendini kanıtlamış <code>pf</code> (Packet Filter) altyapısından alır. Gelen ve giden her veri paketi çekirdek düzeyinde durum bilgili denetimden (Stateful Inspection) geçirilir; yetkisiz erişim girişimleri anında düşürülürken güvenli trafik minimum işlemci yükü ve gecikmeyle iletilir. Gelişmiş Ağ Adresi Dönüştürme (NAT) ve ALTQ tabanlı bant genişliği/trafik şekillendirme yetenekleri yoğun yük altında bile kritik uygulamaların performansını korur. Ayrıca <strong>Safe-Apply</strong> mekanizması, yazdığınız her kuralı canlı sisteme uygulamadan önce <code>pf</code> sözdizimine göre otomatik test ederek hatalı yapılandırmaların sizi sistem dışına kilitlemesini önler.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD çekirdek düzeyinde pf paket filtreleme ve çok katmanlı siber güvenlik kalkanı görseli" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Çekirdek Düzeyinde Stateful Inspection</h3>
<p>Kullanıcı alanı (user-space) darboğazlarına takılmadan, doğrudan FreeBSD 15.1/15 çekirdeğinde çalışan yüksek verimli paket filtreleme ve durum takibi.</p>
</div>
<div class="card stratum-subcard">
<h3>Safe-Apply Otomatik Doğrulama</h3>
<p>Güvenlik duvarı kurallarını ve arayüz değişikliklerini canlı çekirdeğe işlemeden önce sözdizimi testinden geçirerek bağlantı kopmalarını ve kilitlenmeleri önleme.</p>
</div>
<div class="card stratum-subcard">
<h3>NAT, ALTQ ve Canlı Kural İstatistikleri</h3>
<p>Yeniden kullanılabilir Takma Adlar (Aliases), <code>Pass / Block / Reject</code> politikaları, anlık kural eşleşme sayıları (Hits) ve ALTQ trafik önceliklendirme kontrolü.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="dnsbl-web-filtreleme">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">TEHDİT İSTİHBARATI &amp; DNSBL</span>
<h2>DNS Tabanlı Web Filtreleme (DNSBL) ve Otomatik IP Karalisteleri</h2>
<p class="feature-module-lead">Küresel tehdit istihbarat akışlarını (threat feeds) ve DNS karalistelerini kullanarak zararlı IP adreslerini ve kötü amaçlı alan adlarını ağınıza ulaşmadan durdurun.</p>
<p class="feature-module-desc">Stratum Firewall, geleneksel port ve protokol kurallarını iki katmanlı proaktif tehdit istihbaratıyla güçlendirir. <strong>IP Karaliste (IP Blocklists)</strong> modülü, açık kaynaklı ve kurumsal tehdit istihbaratı akışlarını (threat feeds) otomatik olarak içeri aktarır; bilinen botnet komuta-kontrol sunucularını, tarayıcıları ve saldırgan IP aralıklarını çekirdek düzeyindeki yüksek hızlı <code>pf</code> tablolarına yükleyerek paket seviyesinde engeller. <strong>DNSBL (DNS Tabanlı Web Filtreleme)</strong> motoru ise oltalama (phishing), zararlı yazılım (malware), izleyici ve reklam alan adlarını DNS sorgusu aşamasında yakalayarak <code>NXDOMAIN</code> yanıtıyla düşürür; üstelik dahili alan adı sorgulama aracıyla herhangi bir domain'in filtre durumunu arayüzden anında test etmenize olanak tanır.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL alan adı filtreleme ve tehdit istihbaratı IP karaliste koruma katmanları" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Ağ Genelinde DNSBL Engelleme</h3>
<p>İstemci cihazlara ek yazılım kurmaya gerek kalmadan, zararlı ve istenmeyen alan adlarına yapılan sorguları DNS katmanında <code>NXDOMAIN</code> yanıtıyla kesme.</p>
</div>
<div class="card stratum-subcard">
<h3>Dinamik IP Tehdit Akışları</h3>
<p>Güncel siber tehdit istihbaratı listelerini doğrudan çekirdek <code>pf</code> tablolarına aktararak saldırgan IP ağlarını sıfır gecikme kaybıyla sınırda bloklama.</p>
</div>
<div class="card stratum-subcard">
<h3>Anlık Alan Adı Test Aracı</h3>
<p>Herhangi bir alan adının aktif DNSBL politikalarınız tarafından engellenip engellenmediğini yönetim paneli üzerinden gerçek zamanlı sorgulama ve doğrulama.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="modern-api-mimarisi">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">MODERN MİMARİ &amp; API KONTROLÜ</span>
<h2>Sıfır Bağımlılıklı Siber Güvenlik Mimarisi ve Modern API Kontrolü</h2>
<p class="feature-module-lead">"Saf FreeBSD gücü. Modern API kontrolü." Asenkron Python 3.12 / FastAPI arka ucu, tekil config.json bildirimsel yapılandırması ve yerel WireGuard kriptografisiyle yeni nesil güvenlik duvarı deneyimi.</p>
<p class="feature-module-desc">Hantal ve eski nesil PHP/XML tabanlı güvenlik duvarı arayüzlerinin yerini almak üzere sıfırdan tasarlanan Stratum Firewall, FreeBSD çekirdek performansını geliştirici odaklı modern bir kontrol katmanıyla buluşturur. Asenkron <strong>Python 3.12</strong> ve <strong>FastAPI</strong> üzerine inşa edilen arka uç, çok düşük bellek tüketimiyle hem hızlı bir web arayüzü hem de tam kapsamlı bir <strong>REST API</strong> sunar. Tüm sistem durumu tek bir okunabilir, bildirimsel <code>config.json</code> dosyasında tutulur ve çekirdek motor tarafından dinamik olarak yerel <code>pf.conf</code>, DHCP ve DNS yapılandırma dosyalarına derlenir. Harici kütüphane gerektirmeyen sıfırdan yazılmış X25519 (RFC 7748) kriptografili <strong>WireGuard VPN</strong> ve <strong>OpenVPN</strong> desteği, Rol Tabanlı Erişim Kontrolü (RBAC) ve canlı sistem telemetrisiyle ağ güvenliğine modern "Infrastructure-as-Code" sadeliği kazandırır.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API kontrol paneli, WireGuard X25519 şifreleme ve canlı telemetri mimarisi" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>FastAPI &amp; Bildirimsel config.json</h3>
<p>Tüm güvenlik duvarı durumunu denetlenebilir tek bir JSON kaynağından yönetin ve tüm ağ operasyonlarını asenkron REST API üzerinden otomatikleştirin.</p>
</div>
<div class="card stratum-subcard">
<h3>Sıfır Bağımlılıklı WireGuard &amp; OpenVPN</h3>
<p>Harici paket bağımlılığı olmadan sıfırdan yazılmış yerel RFC 7748 X25519 anahtar üretimiyle yüksek hızlı WireGuard ve esnek OpenVPN tünelleme.</p>
</div>
<div class="card stratum-subcard">
<h3>RBAC ve Canlı Sistem Telemetrisi</h3>
<p>Yönetici, Operatör ve Salt Okunur erişim rolleriyle yetkilendirmeyi ayırırken canlı CPU, bellek, disk, sıcaklık, trafik ve güvenlik duvarı loglarını izleyin.</p>
</div>
</div>
</div>
</div>

<!-- STRATUM-UNIFIED-DARK-STYLE-START -->
<style>
.opn-feature-showcase,.stratum-dark-module,.stratum-dark-pillar{position:relative;width:100% !important;max-width:100% !important;box-sizing:border-box;margin:2.5rem 0 !important;border-radius:20px !important;overflow:hidden;background:radial-gradient(120% 90% at 50% 0%,#101f3c 0%,#08101f 52%,#060b16 100%) !important;border:1px solid rgba(56,189,248,0.22) !important;box-shadow:0 24px 60px rgba(2,6,23,0.55),inset 0 1px 0 rgba(255,255,255,0.06) !important;}
.stratum-dark-module,.stratum-dark-pillar{padding:2.3rem 2.25rem 2.2rem !important;text-align:center;}
.stratum-dark-module > .wrap,.stratum-dark-module > .container,.stratum-dark-pillar > .wrap,.stratum-dark-pillar > .container{max-width:100% !important;width:100% !important;padding:0 !important;margin:0 !important;}
.feature-showcase-page{max-width:100% !important;width:100% !important;margin:0 !important;padding:0 !important;background:transparent !important;border:0 !important;box-shadow:none !important;}
.feature-showcase-page hr,.stratum-dark-pillar hr{display:none !important;}
.stratum-dark-module .stratum-module-eyebrow,.stratum-dark-pillar .stratum-module-eyebrow{display:inline-flex !important;align-items:center;justify-content:center;gap:0.45rem;padding:0.32rem 0.9rem !important;margin:0 auto 0.85rem !important;border-radius:999px !important;background:rgba(56,189,248,0.12) !important;border:1px solid rgba(56,189,248,0.32) !important;color:#38bdf8 !important;font-size:0.74rem !important;font-weight:700 !important;letter-spacing:0.09em !important;text-transform:uppercase !important;width:auto !important;}
.stratum-dark-module .stratum-module-eyebrow{background:rgba(245,158,11,0.12) !important;border-color:rgba(245,158,11,0.34) !important;color:#fbbf24 !important;}
.stratum-dark-module h1,.stratum-dark-module h2,.stratum-dark-pillar h2{margin:0 auto 0.8rem !important;padding:0 !important;border:0 !important;max-width:920px !important;color:#f8fafc !important;font-size:clamp(1.38rem,2.25vw,1.92rem) !important;font-weight:800 !important;line-height:1.24 !important;letter-spacing:-0.02em !important;text-align:center !important;}
.stratum-dark-module p,.stratum-dark-pillar p{color:#cbd5e1 !important;font-size:1rem !important;line-height:1.7 !important;margin:0 auto 1.5rem !important;max-width:860px;text-align:center;}
.stratum-dark-pillar .stratum-pillar-media{background:transparent !important;border:0 !important;box-shadow:none !important;padding:0 !important;margin:0.4rem auto 1.65rem !important;max-width:1040px !important;border-radius:16px !important;overflow:hidden;}
.stratum-dark-pillar img{display:block;width:100% !important;max-width:100% !important;height:auto !important;margin:0 auto !important;padding:0 !important;border:0 !important;border-radius:14px !important;box-shadow:none !important;-webkit-mask-image:linear-gradient(180deg,transparent 0%,#000 5%,#000 95%,transparent 100%);mask-image:linear-gradient(180deg,transparent 0%,#000 5%,#000 95%,transparent 100%);}
.stratum-dark-module .stratum-subgrid,.stratum-dark-pillar .stratum-subgrid{display:grid !important;grid-template-columns:repeat(auto-fit,minmax(275px,1fr)) !important;gap:1rem !important;margin-top:1.25rem !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard,.stratum-dark-pillar .stratum-subcard{padding:1.2rem 1.25rem !important;border-radius:14px !important;background:rgba(10,18,35,0.84) !important;backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:1px solid rgba(148,163,184,0.18) !important;border-top:2px solid rgba(56,189,248,0.75) !important;box-shadow:0 10px 24px rgba(2,6,23,0.4) !important;text-align:left !important;transition:transform 0.18s ease,border-color 0.18s ease;}
.stratum-dark-module .stratum-subcard:nth-child(3n+1),.stratum-dark-pillar .stratum-subcard:nth-child(3n+1){border-top-color:rgba(251,191,36,0.8) !important;}
.stratum-dark-module .stratum-subcard:nth-child(3n+2),.stratum-dark-pillar .stratum-subcard:nth-child(3n+2){border-top-color:rgba(56,189,248,0.8) !important;}
.stratum-dark-module .stratum-subcard:nth-child(3n+3),.stratum-dark-pillar .stratum-subcard:nth-child(3n+3){border-top-color:rgba(52,211,153,0.8) !important;}
.stratum-dark-module .stratum-subcard:hover,.stratum-dark-pillar .stratum-subcard:hover{transform:translateY(-2px);border-color:rgba(148,163,184,0.34) !important;}
.stratum-dark-module .stratum-subcard h3,.stratum-dark-module .stratum-subcard h4,.stratum-dark-pillar .stratum-subcard h3,.stratum-dark-pillar .stratum-subcard h4{color:#f8fafc !important;font-size:1.02rem !important;font-weight:700 !important;line-height:1.35 !important;margin:0 0 0.5rem !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard p,.stratum-dark-module .stratum-subcard li,.stratum-dark-pillar .stratum-subcard p,.stratum-dark-pillar .stratum-subcard li{color:#cbd5e1 !important;font-size:0.92rem !important;line-height:1.62 !important;margin:0 !important;max-width:none !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard strong,.stratum-dark-module .stratum-subcard em,.stratum-dark-pillar .stratum-subcard strong,.stratum-dark-pillar .stratum-subcard em{color:#f8fafc !important;}
.stratum-dark-module .stratum-subcard code,.stratum-dark-pillar .stratum-subcard code{background:rgba(56,189,248,0.12) !important;color:#7dd3fc !important;border:1px solid rgba(56,189,248,0.24) !important;padding:0.1rem 0.36rem !important;border-radius:5px !important;font-size:0.84em !important;}
@media (max-width:900px){.stratum-dark-module,.stratum-dark-pillar{padding:1.65rem 1.3rem 1.65rem !important;}}
@media (max-width:680px){.stratum-dark-module .stratum-subgrid,.stratum-dark-pillar .stratum-subgrid{grid-template-columns:1fr !important;gap:0.75rem !important;}}
</style>
<!-- STRATUM-UNIFIED-DARK-STYLE-END -->
