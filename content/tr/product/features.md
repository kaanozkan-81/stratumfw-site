---
title: "Özellikler"
description: "Stratum Firewall v0.30.0 tüm özellikleri: durum bilgili pf kuralları, Çoklu WAN politika yönlendirmesi, 63 IP tehdit listesi, Unbound DNSBL, 55 kategorili Web Filtresi, Suricata 8 IPS/IDS, WireGuard, OpenVPN ve ZFS geri alma."
lead: "Ağ altyapınızı bölütlemek, korumak, filtrelemek ve otomatikleştirmek için gereken her şey — saf FreeBSD 15.1/15 gücü üzerine inşa edildi, modern FastAPI mimarisiyle yönetiliyor."
full_width: true
seo_title: "Özellikler — pf, Suricata 8 IPS, 55 Kategorili Web Filtresi ve Çoklu WAN VPN"
---
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
