---
title: "Kurumsal ve Çok Şubeli Ağlar"
description: "Kurumsal altyapı ekipleri için API-öncelikli FreeBSD 15.1 güvenlik duvarı — kesintisiz OPNsense geçişi, /29 Sanal IP, çift motorlu Suricata IPS, RBAC, SNMP, Uzak Syslog ve ZFS geri alma."
lead: "Çok şubeli ağlar, veri merkezleri ve regülasyonlara tabi kurumlar için API-öncelikli otomasyon, denetlenebilir tekil yapılandırma ve çekirdek hızında güvenlik."
full_width: true
---
<div class="solutions-layout">

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Kurumsal Altyapı Mimarisi</span>
    <h2>Otomasyon, denetlenebilirlik ve kesintisiz operasyon için sıfırdan tasarlandı</h2>
    <p>Kurumsal ölçekte güvenlik duvarı yönetimi; yalnızca paket filtrelemekten ibaret değildir. Değişikliklerin ön kontrolden geçmesi, kimlik ve yetki ayrımı (ISO 27001 / KVKK / SOC 2 denetim izleri), merkezi izleme ve eski cihazlardan kesintisiz geçiş yeteneği gerektirir.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="16" rx="2"/><rect x="9" y="9" width="6" height="6"/><path d="M9 1v3M15 1v3M9 20v3M15 20v3M20 9h3M20 14h3M1 9h3M1 14h3"/></svg></div>
      <h3>API-Öncelikli &amp; Bildirimsel Mimari</h3>
      <p>Tüm sistem durumu tek bir şeffaf <code>config.json</code> kaynağında tutulur ve asenkron FastAPI REST uç noktalarıyla yönetilir. Ansible, Terraform veya Python iş akışlarınızla yüzlerce kuralı ekran kazımaya gerek kalmadan standartlaştırın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 3h5v5M4 20L21 3M21 16v5h-5M15 15l6 6M4 4l5 5"/></svg></div>
      <h3>Kesintisiz OPNsense &amp; OpenVPN Devralma</h3>
      <p>Mevcut OPNsense cihazlarınızın <code>config.xml</code> yedeklerini port eşleştirme ve önizleme raporuyla içe aktarın. CA, sertifikalar, <code>tls-crypt</code> ve <code>$2y$</code>/<code>argon2id</code> parola özetleri korunduğu için sahadaki yüzlerce OpenVPN istemcisi kesintisiz bağlanmaya devam eder.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="6" cy="19" r="3"/><path d="M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15"/><circle cx="18" cy="5" r="3"/></svg></div>
      <h3>Sanal IP Blokları (<code>/29</code>), Statik Rota &amp; Politika Yönlendirme</h3>
      <p>Çoklu genel IP bloklarınızı (VIP) farklı iç sunuculara NAT yansımasıyla yönlendirin, omurga anahtarların arkasındaki alt ağlara statik rotalar tanımlayın (<code>stratum-netextra</code>) ve trafiği kural bazında istediğiniz İSS hattından çıkarın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Çift Motorlu Suricata 8 IPS &amp; 5-RIR GeoIP</h3>
      <p>Her arayüzde bağımsız <em>İzle (<code>pcap</code>)</em> veya satır içi <em>Engelle (<code>netmap</code>)</em> modu çalıştırın; <code>SIGUSR2</code> ile kural kategorilerini bağlantı koparmadan yenileyin ve 5 bölgesel kayıt kuruluşundan çekilen lisanssız Ülke (GeoIP) tablolarıyla saldırı yüzeyini daraltın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/></svg></div>
      <h3>4 Kademeli RBAC, SNMP &amp; Uzak Syslog</h3>
      <p><strong>Yönetici</strong>, <strong>Operatör</strong>, <strong>Salt Okunur</strong> ve <strong>VPN-only</strong> rolleriyle en az yetki ilkesini uygulayın. Salt-okunur <code>bsnmpd</code> (IF-MIB, HOST-RESOURCES, pf) ile Zabbix/LibreNMS/PRTG'den izleyin ve günlükleri SIEM / Uzak Syslog sunucularına aktarın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
      <h3>Atomik ZFS Geri Alma, Kapalı Devre <code>.sfu</code> &amp; MCP</h3>
      <p>60 saniyelik Safe-Apply sayacı ve her güncelleme öncesi alınan ZFS Önyükleme Ortamlarıyla bakım penceresi riskini sıfırlayın; kapalı devre ağlarda imzalı <code>.sfu</code> paketleriyle güncelleyin ve salt-okunur MCP sunucusuyla yapay zeka destekli kök neden analizi yapın.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Mimari Karşılaştırma</span>
    <h2>Geleneksel güvenlik duvarı yığınları karşısında Stratum FW</h2>
  </div>

  <div class="sol-table-wrap">
    <table class="sol-table">
      <thead>
        <tr>
          <th>Operasyonel Alan</th>
          <th>Geleneksel Güvenlik Duvarı Yığınları</th>
          <th>Stratum Firewall (FreeBSD 15.1)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Yapılandırma Doğruluğu</strong></td>
          <td>Dağınık XML dosyaları, eklenti çakışmaları ve yavaş PHP yeniden yüklemeleri</td>
          <td>Tekil <code>config.json</code> kaynağından deterministik üretilen yerel FreeBSD dosyaları</td>
        </tr>
        <tr>
          <td><strong>Değişiklik ve Bakım Güvenliği</strong></td>
          <td>Uzak şubede hatalı kural sonrası erişim kaybı ve yerinde müdahale maliyeti</td>
          <td><code>pfctl -n</code> ön kontrolü + 60 sn otomatik Safe-Apply geri alma + ZFS Boot Environments</td>
        </tr>
        <tr>
          <td><strong>Veri Gizliliği &amp; Hava Boşluğu</strong></td>
          <td>Bulut lisans doğrulaması ve zorunlu üretici telemetrisi</td>
          <td>%100 yerel çalışma, sıfır telemetri, çevrimdışı imzalı <code>.sfu</code> güncellemeleri</td>
        </tr>
        <tr>
          <td><strong>Felaket Kurtarma (DR)</strong></td>
          <td>Sertifika ve VPN anahtarlarını yeni cihaza taşırken yaşanan kopukluklar</td>
          <td>Tüm özel anahtarları ve sertifikaları kapsayan zamanlanmış <code>AES-256-GCM</code> SFTP (<code>.stbk</code>) yedekleri</td>
        </tr>
        <tr>
          <td><strong>Olay İnceleme &amp; Tanılama</strong></td>
          <td>Ham günlüklerde manuel arama veya pahalı ek raporlama lisansları</td>
          <td>Birleşik Engelleme Raporları, yerleşik Wireshark <code>.pcap</code> yakalama ve salt-okunur AI MCP araçları</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="sol-cta-box">
  <div>
    <h3>Kurumsal mimariyi ve canlı kontrol düzlemini inceleyin</h3>
    <p>Teknik mimari detaylarına göz atın, Canlı Demo'da arayüzü test edin veya mühendislik ekibimizle iletişime geçin.</p>
  </div>
  <div class="sol-cta-actions">
    <a href="/tr/product/architecture/" class="btn btn-primary">Mimariyi İncele</a>
    <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Canlı Demo</a>
    <a href="/tr/company/contact/" class="btn btn-secondary" style="background:transparent;color:#F8FAFC;border-color:rgba(248,250,252,0.4);">İletişime Geçin</a>
  </div>
</div>

</div>
