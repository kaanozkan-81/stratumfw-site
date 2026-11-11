---
title: "Çözümler"
description: "KOBİ, çok şubeli kurumsal ağlar, eğitim kampüsleri ve kapalı devre altyapılar için FreeBSD 15.1 tabanlı Stratum Firewall dağıtım mimarileri."
lead: "Tek ofisli işletmelerden çok şubeli kurumsal yapılara ve binlerce kullanıcılı eğitim kampüslerine kadar Stratum Firewall'ın ağ mimarinize nasıl uyum sağladığını inceleyin."
full_width: true
hide_pages: true
---
<div class="solutions-layout">

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Dağıtım Profilleri</span>
    <h2>Kurum ölçeğinize göre özelleşen güvenlik mimarisi</h2>
    <p>Stratum Firewall; kullanıcı sayısı sınırı, yıllık lisans dayatması veya bulut bağımlılığı olmadan tüm güvenlik modüllerini tek bir FreeBSD 15.1 ZFS işletim sisteminde sunar.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="2" width="16" height="20" rx="2"/><path d="M9 22v-4h6v4M8 6h.01M16 6h.01M12 6h.01M12 10h.01M12 14h.01M16 10h.01M16 14h.01M8 10h.01M8 14h.01"/></svg></div>
      <h3>Küçük ve Orta Ölçekli İşletmeler (KOBİ)</h3>
      <p>Tek bir kompakt cihazda Çoklu WAN (Fiber + 4G/5G yedeklilik), misafir Wi-Fi izolasyonu, QR kodlu WireGuard uzaktan çalışma ve 63 tehdit listesiyle fidye yazılımı koruması.</p>
      <a href="/tr/solutions/smb/" class="card-link">KOBİ Mimarisini İncele →</a>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg></div>
      <h3>Kurumsal ve Çok Şubeli Ağlar</h3>
      <p>Kesintisiz OPNsense &amp; OpenVPN devralma, <code>/29</code> Sanal IP ve statik yönlendirme, çift motorlu Suricata 8 IPS, REST API otomasyonu, RBAC, SNMP ve şifreli SFTP yedekleme.</p>
      <a href="/tr/solutions/enterprise/" class="card-link">Kurumsal Mimariyi İncele →</a>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M22 10v6M2 10l10-5 10 5-10 5z"/><path d="M6 12v5c0 2 3 3 6 3s6-1 6-3v-5"/></svg></div>
      <h3>Eğitim, Kampüs ve Kamu</h3>
      <p>VLAN bazlı 55 kategorili Web Filtresi (UT1 katalogları), zorunlu Güvenli Arama, öğrenci başına adil hız sınırı (<code>dummynet</code>) ve kullanıcı başına sıfır lisans maliyeti.</p>
      <a href="/tr/solutions/education/" class="card-link">Kampüs Mimarisini İncele →</a>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Kritik Kullanım Senaryoları</span>
    <h2>Sahadaki gerçek ağ problemlerine doğrudan yanıtlar</h2>
    <p>Geleneksel güvenlik duvarlarında ek eklenti, harici API anahtarı veya karmaşık betikler gerektiren yetenekler Stratum FW içerisinde yerleşik olarak gelir.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 3h5v5M4 20L21 3M21 16v5h-5M15 15l6 6M4 4l5 5"/></svg></div>
      <h3>Sıfır Kesintili OPNsense &amp; OpenVPN Göçü</h3>
      <p>Mevcut OPNsense <code>config.xml</code> dosyanızı yükleyerek kuralları, VLAN/VIP yapılarını ve tüm OpenVPN sunucunuzu (CA, sertifikalar ve kullanıcı parolaları dahil) istemci cihazlara dokunmadan devralın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="6" cy="19" r="3"/><path d="M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15"/><circle cx="18" cy="5" r="3"/></svg></div>
      <h3>Çoklu İSS Sürekliliği &amp; Politika Yönlendirme</h3>
      <p>1 saniyelik ağ geçidi izlemeyle hat koptuğunda anında yedek İSS'ye geçin; misafir veya yedekleme trafiğini kural bazlı Politika Yönlendirme (Policy Routing) ile ikincil hattan çıkarın.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Çok Katmanlı Tehdit Engelleme &amp; Raporlama</h3>
      <p>Çekirdek <code>pf</code> IP tabloları, Unbound RPZ DNSBL, 55 kategorili Web Filtresi ve satır içi <code>netmap</code> Suricata 8 IPS katmanlarını pfBlockerNG tarzı birleşik raporlarla denetleyin.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><path d="M6 6h.01M6 18h.01"/></svg></div>
      <h3>Kapalı Devre (Air-Gapped) &amp; Savunma Ağları</h3>
      <p>Sıfır dış telemetri ve çevrimdışı imzalı güncelleme paketleri (<code>.sfu</code>) sayesinde internet çıkışı olmayan üretim, enerji, sağlık ve kamu ağlarında tam bağımsız çalışır.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div>
      <h3>Sıfır Güven Hibrit ve Uzaktan Çalışma</h3>
      <p>Çekirdek içi <code>if_wg</code> WireGuard ve çift WAN <code>multihome</code> destekli OpenVPN (yalnızca VPN yetkili <code>VPN-only</code> hesaplarla) üzerinden çalışanlara VLAN bazlı erişim verin.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="16" rx="2"/><rect x="9" y="9" width="6" height="6"/><path d="M9 1v3M15 1v3M9 20v3M15 20v3M20 9h3M20 14h3M1 9h3M1 14h3"/></svg></div>
      <h3>API Otomasyonu &amp; Yapay Zeka (MCP) Tanılama</h3>
      <p>FastAPI REST uç noktalarıyla şube kurulumlarını standartlaştırın; salt-okunur Model Context Protocol (MCP) sunucusuyla yapay zeka asistanlarından canlı güvenlik analizi alın.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Mimari Karşılaştırma Matrisi</span>
    <h2>Hangi ortamda hangi Stratum modülleri öne çıkıyor?</h2>
  </div>

  <div class="sol-table-wrap">
    <table class="sol-table">
      <thead>
        <tr>
          <th>Yetenek Katmanı</th>
          <th>KOBİ &amp; Şube Ofisler</th>
          <th>Kurumsal &amp; Veri Merkezi</th>
          <th>Eğitim &amp; Kampüs Ağları</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Ağ &amp; Yönlendirme</strong></td>
          <td>Fiber + LTE Otomatik Failover, Misafir VLAN</td>
          <td><code>/29</code> Sanal IP (VIP), Statik Rotalar, Politika Yönlendirme</td>
          <td>İdare / Akademik / Öğrenci / Yurt 802.1Q VLAN'ları</td>
        </tr>
        <tr>
          <td><strong>Tehdit &amp; İçerik Koruması</strong></td>
          <td>63 IP Tehdit Listesi + Reklam/Phishing DNSBL</td>
          <td>Çift Motorlu Suricata 8 IPS + 5-RIR Lisanssız GeoIP</td>
          <td>55 Kategorili Web Filtresi + Google/YouTube Güvenli Arama</td>
        </tr>
        <tr>
          <td><strong>Bant Genişliği &amp; VPN</strong></td>
          <td>QR Kodlu WireGuard VPN, Misafir Ağ Hız Sınırı</td>
          <td>OPNsense OpenVPN Devralma, Çift WAN <code>multihome</code> VPN</td>
          <td>Öğrenci Cihazı Başına Adil Hız Sınırı (<code>dummynet</code>)</td>
        </tr>
        <tr>
          <td><strong>Yönetim &amp; Denetim</strong></td>
          <td>60 sn Safe-Apply, Telegram/E-posta Uyarıları</td>
          <td>REST API, 4 Rol RBAC, SNMP, Uzak Syslog, Şifreli SFTP</td>
          <td>Birleşik Engelleme Raporları, Web Etkinliği &amp; DHCP Geçmişi</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="sol-cta-box">
  <div>
    <h3>Kurulum yapmadan canlı sistem üzerinde test edin</h3>
    <p>Gerçek bir FreeBSD 15.1 sanal makinesi üzerinde çalışan Stratum FW v0.28.3 yönetim panelini tarayıcınızda hemen inceleyin veya donanımınıza kurun.</p>
  </div>
  <div class="sol-cta-actions">
    <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-primary">Canlı Demo'yu Aç</a>
    <a href="/tr/download/" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">ISO / IMG İndir</a>
  </div>
</div>

</div>
