---
title: "Güvenlik Duvarı ve UTM Karşılaştırması"
seo_title: "Stratum FW vs OPNsense, pfSense, FortiGate, Sophos ve WatchGuard — Ücretsiz Firewall"
description: "Stratum Firewall v0.30.0 ile OPNsense, pfSense, IPFire ve yıllık abonelikli ticari UTM cihazlarının (Fortinet FortiGate, Sophos UTM/XGS, WatchGuard Firebox) teknik ve maliyet karşılaştırması — FreeBSD 15.1 pf, FastAPI, ZFS geri alma ve 55 kategorili Web Filtresi."
lead: "Stratum Firewall'ı hem açık kaynak dağıtımlarla (OPNsense, pfSense, IPFire) hem de her yıl döviz bazlı lisans yenilemesi gerektiren ticari UTM cihazlarıyla (Fortinet FortiGate, Sophos UTM/XGS, WatchGuard Firebox) mühendislik ve toplam sahip olma maliyeti açısından karşılaştırın."
translationKey: "firewall-comparison"
aliases:
  - /tr/karsilastirma/
  - /tr/opnsense-alternatifi/
  - /tr/pfsense-alternatifi/
  - /tr/fortigate-alternatifi/
  - /tr/sophos-alternatifi/
  - /tr/watchguard-alternatifi/
full_width: true
seoTitle: "Stratum FW vs OPNsense vs pfSense CE vs IPFire Karşılaştırma"
seoDescription: "Stratum Firewall ile OPNsense, pfSense CE ve IPFire dağıtımlarını FreeBSD 15.1 pf, FastAPI vs PHP, ZFS geri alma, DPI ve VPN başlıklarında yan yana kıyaslayın."
---
<script type="application/ld+json">{"@context": "https://schema.org", "@type": "FAQPage", "mainEntity": [{"@type": "Question", "name": "Stratum Firewall ile OPNsense, pfSense ve IPFire arasındaki temel mimari fark nedir?", "acceptedAnswer": {"@type": "Answer", "text": "Stratum Firewall; FreeBSD 15.1-RELEASE ve pf çekirdeği üzerinde sıfırdan yazılmış Python 3.12 / FastAPI asenkron yönetim katmanı, SQLite WAL yapılandırma deposu, ZFS Önyükleme Ortamları ve 60 saniyelik Safe-Apply otomatik geri alma mekanizmasıyla çalışır. OPNsense ve pfSense PHP/XML tabanlı yönetim katmanı kullanırken, IPFire Linux çekirdeği ve Perl CGI arayüzü üzerinde çalışır."}}, {"@type": "Question", "name": "Stratum Firewall; lisansı biten Fortinet FortiGate, WatchGuard Firebox veya kullanım ömrü dolan Sophos UTM 9 cihazlarının yerine kullanılabilir mi?", "acceptedAnswer": {"@type": "Answer", "text": "Evet. 30 Haziran 2026'da kullanım ömrü (EOL) sona eren Sophos UTM 9 cihazlarından çıkan veya her yıl döviz bazlı FortiGuard UTP ve WatchGuard Basic/Total Security Suite yenileme maliyeti ödemek istemeyen işletmeler; standart x86-64 mini sunucular veya Proxmox üzerinde Suricata 8 IPS, 55 kategorili Web Filtresi, Unbound RPZ DNSBL, 63 IP tehdit listesi, Çoklu WAN yedekleme ve sınırsız WireGuard/OpenVPN özelliklerini yıllık 0 $ maliyetle kullanabilir."}}, {"@type": "Question", "name": "Ticari UTM abonelikleri bittiğinde yaşanan kısıtlamalar Stratum Firewall'da var mı?", "acceptedAnswer": {"@type": "Answer", "text": "FortiGate, Sophos XGS ve WatchGuard cihazlarında yıllık abonelik yenilenmediğinde kategori bazlı Web/DNS filtreleme durur, IPS imzaları donar ve işletim sistemi güncellemeleri kilitlenir. Stratum Firewall'da seri numarası kilidi, özellik anahtarı (Feature Key) veya süresi dolan herhangi bir güvenlik aboneliği yoktur."}}, {"@type": "Question", "name": "OPNsense'den Stratum Firewall'a geçerken OpenVPN istemcilerini yeniden kurmak gerekir mi?", "acceptedAnswer": {"@type": "Answer", "text": "Hayır. Stratum Firewall içindeki yerleşik OPNsense config.xml geçiş sihirbazı; arayüzleri, VLAN'ları, Sanal IP'leri, kuralları ve OpenVPN sunucu sertifika/anahtar yapısını olduğu gibi aktarır; uzaktaki kullanıcılar yeni .ovpn profili indirmeden bağlanmaya devam eder."}}, {"@type": "Question", "name": "ZFS Önyükleme Ortamları ve 55 kategorili Web Filtresi ücretsiz sürümde dahil mi?", "acceptedAnswer": {"@type": "Answer", "text": "Evet. Ücretli Plus sürümleri veya üçüncü parti eklenti abonelikleri gerektirmeden ZFS Boot Environments, 60 saniyelik Safe-Apply geri alma, 63 IP tehdit listesi, Unbound RPZ DNSBL, 55 kategorili Web Filtresi ve çift motorlu Suricata 8 IPS/IDS tamamen ücretsiz sunulur."}}, {"@type": "Question", "name": "ISO kurulum imajını indirmeden önce Stratum Firewall'ı tarayıcıda deneyebilir miyim?", "acceptedAnswer": {"@type": "Answer", "text": "Evet. Kayıt olmadan https://demo.stratumfw.com adresindeki gerçek FreeBSD 15.1 sanal makinesi üzerinde paneli canlı inceleyebilir, ardından https://stratumfw.com/tr/download/ sayfasından VGA .iso veya Seri konsol .img imajını indirebilirsiniz."}}]}</script>

<div class="compare-layout">

<div class="cmp-block">
  <div class="cmp-head">
    <span class="eyebrow">Bölüm 01 · Açık Kaynak Mimari Matrisi</span>
    <h2>Stratum Firewall vs. OPNsense vs. pfSense CE vs. IPFire</h2>
    <p>Açık kaynak güvenlik duvarı arayan ağ yöneticileri genellikle klasik PHP/XML mimarili FreeBSD dağıtımları (<a href="https://opnsense.org/" target="_blank" rel="noopener">OPNsense</a> ve <a href="https://www.pfsense.org/" target="_blank" rel="noopener">pfSense</a>) ya da bölge (zone) tabanlı Linux dağıtımları (<a href="https://www.ipfire.org/" target="_blank" rel="noopener">IPFire</a>) arasında seçim yapar. Stratum Firewall, sahada kanıtlanmış <strong>FreeBSD 15.1-RELEASE + pf</strong> veri düzlemini korurken eski PHP/XML yönetim katmanını sıfırdan yazılmış <strong>Python 3.12 &amp; FastAPI</strong> mimarisi, 60 saniyelik Safe-Apply kilitlenme koruması ve yerleşik 55 kategorili Web Filtresi ile birleştirir.</p>
  </div>

  <div class="cmp-table-wrap">
    <table class="cmp-table">
      <thead>
        <tr>
          <th>Yetenek / Katman</th>
          <th class="cmp-highlight">Stratum FW (v0.30.3)</th>
          <th>OPNsense (26.7)</th>
          <th>pfSense CE (2.9)</th>
          <th>IPFire (2.29)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Temel İşletim Sistemi</strong></td>
          <td class="cmp-highlight">FreeBSD 15.1-RELEASE (<code>amd64</code>)</td>
          <td>FreeBSD 15.1 (<code>amd64</code>)</td>
          <td>FreeBSD (<code>amd64</code>)</td>
          <td>Sıkılaştırılmış Linux 6.18 LTS</td>
        </tr>
        <tr>
          <td><strong>Çekirdek Paket Filtresi</strong></td>
          <td class="cmp-highlight">FreeBSD <code>pf</code> durum bilgili + radix tabloları</td>
          <td>FreeBSD <code>pf</code> + <code>ipfw</code></td>
          <td>FreeBSD <code>pf</code> + <code>dnctl</code></td>
          <td>Linux Netfilter (<code>iptables</code>)</td>
        </tr>
        <tr>
          <td><strong>Yönetim Katmanı ve Web Arayüzü</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Modern Asenkron</span><br>Python 3.12 + FastAPI REST API (sıfır PHP)</td>
          <td>PHP 8.5 / Phalcon MVC + Python <code>configd</code></td>
          <td>Monolitik PHP <code>webConfigurator</code></td>
          <td>Perl 5.42 CGI betikleri</td>
        </tr>
        <tr>
          <td><strong>Yapılandırma Veritabanı</strong></td>
          <td class="cmp-highlight">SQLite WAL + deterministik atomik üretim</td>
          <td>Tekil <code>config.xml</code> dosyası</td>
          <td>Tekil <code>config.xml</code> dosyası</td>
          <td>Düz metin dosyaları ve SQLite</td>
        </tr>
        <tr>
          <td><strong>ZFS Önyükleme Ortamları (Ücretsiz Sürüm)</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Ücretsiz Dahil</span><br>Web arayüzü ve CLI üzerinden tam geri alma</td>
          <td><span class="cmp-badge-yes">Ücretsiz Dahil</span><br><code>bectl</code> arayüz ve CLI desteği</td>
          <td><span class="cmp-badge-no">Sadece Plus</span><br>Ücretli <a href="https://docs.netgate.com/pfsense/en/latest/backup/zfsbe/index.html" target="_blank" rel="noopener">pfSense Plus</a> gerektirir</td>
          <td><span class="cmp-badge-no">Desteklenmiyor</span><br>Linux ext4/xfs disk yapısı</td>
        </tr>
        <tr>
          <td><strong>60 sn Safe-Apply Otomatik Geri Alma</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Yerleşik</span><br>Bağlantı koparsa 60 sn sonunda otomatik geri alır</td>
          <td><span class="cmp-badge-partial">Manuel</span><br>Manuel geri alma / konsol müdahalesi</td>
          <td><span class="cmp-badge-partial">Manuel</span><br>Manuel geri alma / konsol müdahalesi</td>
          <td><span class="cmp-badge-partial">Manuel</span><br>Manuel geri alma</td>
        </tr>
        <!-- STRATUM-CMP-2FA-START -->
        <tr>
          <td><strong>Yönetici İki Aşamalı Giriş (2FA) &amp; RBAC</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Yerleşik TOTP 2FA</span><br>Cihaz içi yerel QR TOTP (<code>v0.30.0</code>), 10 kurtarma kodu, 4 RBAC rolü ve konsol sıfırlama</td>
          <td><span class="cmp-badge-yes">Yerleşik</span><br>TOTP + yetki grupları</td>
          <td><span class="cmp-badge-partial">Ek Paket</span><br>FreeRADIUS veya harici eklenti gerektirir</td>
          <td><span class="cmp-badge-no">Temel Parola</span><br>Standart HTTP yönetici girişi</td>
        </tr>
        <!-- STRATUM-CMP-2FA-END -->
        <tr>
          <td><strong>IPS / IDS Saldırı Önleme</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Çift Motorlu</span><br>Arayüz bazlı Suricata 8 (<code>netmap</code> IPS + <code>pcap</code> IDS)</td>
          <td><span class="cmp-badge-yes">Yerleşik</span><br>Suricata IPS/IDS</td>
          <td><span class="cmp-badge-partial">Ek Paket</span><br>Suricata veya Snort paketi</td>
          <td><span class="cmp-badge-yes">Yerleşik</span><br>Suricata IPS</td>
        </tr>
        <tr>
          <td><strong>DNSBL ve Web Kategori Filtresi</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Yerleşik &amp; Ücretsiz</span><br>Unbound 1.26 RPZ + 55 kategorili Web Filtresi + <strong>Zorunlu DNS</strong> (DoT engeli &amp; AD istisnası)</td>
          <td><span class="cmp-badge-partial">Eklenti Bazlı</span><br>Unbound DNSBL; kategori filtresi için 3. parti Zenarmor</td>
          <td><span class="cmp-badge-partial">Ek Paket</span><br><code>pfBlockerNG</code> + SquidGuard</td>
          <td><span class="cmp-badge-yes">Yerleşik</span><br>IPFire DBL + Location ASN DB</td>
        </tr>
        <tr>
          <td><strong>VPN Protokolleri</strong></td>
          <td class="cmp-highlight">Çekirdek WireGuard (<code>if_wg</code> + QR) ve Çoklu WAN OpenVPN</td>
          <td>Çekirdek WireGuard, OpenVPN ve IPsec</td>
          <td>WireGuard paketi, OpenVPN ve IPsec</td>
          <td>WireGuard, OpenVPN ve IPsec</td>
        </tr>
        <tr>
          <td><strong>Çoklu WAN ve Politika Yönlendirmesi</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">1 sn İzleme + DHCP WAN2</span><br>Statik/PPPoE/DHCP Çoklu WAN, Politika Yönlendirme, <strong>1:1 NAT</strong> ve Kural Zamanlama</td>
          <td>Ağ geçidi grupları ve politika kuralları</td>
          <td>Ağ geçidi grupları ve politika kuralları</td>
          <td>Kırmızı/Yeşil/Mavi/Turuncu bölge modeli</td>
        </tr>
        <tr>
          <td><strong>Kesintisiz OPNsense Geçiş Aracı</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Tek Tıkla İçe Aktar</span><br><code>config.xml</code>, DHCP WAN2, Zorunlu DNS, URL tablo tehdit listeleri ve OpenVPN PKI devralma</td>
          <td>Kendi yerel formatı</td>
          <td>Manuel dönüştürme gerekir</td>
          <td>Sıfırdan kurulum gerekir</td>
        </tr>
        <tr>
          <td><strong>Yapay Zeka Tanılama (MCP Sunucusu)</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Yerleşik</span><br>Salt okunur Model Context Protocol (MCP) telemetrisi</td>
          <td><span class="cmp-badge-no">Yok</span></td>
          <td><span class="cmp-badge-no">Yok</span></td>
          <td><span class="cmp-badge-no">Yok</span></td>
        </tr>
        <tr>
          <td><strong>Tarayıcıda Kurulumsuz Canlı Demo</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Canlı QEMU VM</span><br><a href="https://demo.stratumfw.com" target="_blank" rel="noopener">demo.stratumfw.com</a> üzerinde anında erişim</td>
          <td>Resmi canlı sanal makine demosu yok</td>
          <td>Resmi canlı sanal makine demosu yok</td>
          <td>Resmi canlı sanal makine demosu yok</td>
        </tr>
        <tr>
          <td><strong>Kurulum Medyası ve Çevrimdışı Güncelleme</strong></td>
          <td class="cmp-highlight">1.3 GB VGA <code>.iso</code>, 1.5 GB Seri <code>.img</code> ve imzalı <code>.sfu</code> paketi</td>
          <td>DVD ISO, VGA/Seri IMG</td>
          <td>Netgate Çevrimiçi Kurucu / ISO</td>
          <td>ISO ve Flash imajı</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="cmp-block" id="ticari-utm-gecis">
  <div class="cmp-head">
    <span class="eyebrow">Bölüm 02 · Yıllık UTM Lisans Maliyetlerinden Çıkış</span>
    <h2>Fortinet FortiGate, Sophos UTM/XGS ve WatchGuard Aboneliklerine Ücretsiz Alternatif</h2>
    <p>Her yıl binlerce KOBİ, çok şubeli perakende zinciri ve eğitim kurumu; donanımın kendisinden daha pahalı hale gelen yıllık güvenlik paketi yenilemeleri veya kullanım ömrü dolan (EOL) cihaz dayatmalarıyla karşılaşır. Stratum Firewall, standart x86-64 mini sunucular veya Proxmox sanal makineleri üzerinde <strong>yıllık 0 $ lisans maliyetiyle</strong> kurumsal güvenlik katmanlarını kesintisiz çalıştırmanızı sağlar.</p>
  </div>

  <div class="card-grid grid-3" style="margin-bottom:32px;">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Fortinet FortiGate (FortiGuard UTP) Geçişi</h3>
      <p><a href="https://docs.fortinet.com/document/fortigate/8.0.2/administration-guide/289376/license-expiration" target="_blank" rel="noopener">FortiGuard UTP aboneliği sona erdiği anda</a> kategori bazlı Web ve DNS filtreleme durur (hatta ayar değiştirilmezse tüm web trafiğini kesebilir), IPS imzaları donar, FortiOS güncellemeleri kilitlenir ve geç yenilemeler geriye dönük faturalandırılır. Stratum FW'de 55 kategorili Web Filtresi, DNSBL, 63 IP tehdit listesi ve Suricata 8 IPS süresiz aktiftir.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 3h5v5M4 20L21 3M21 16v5h-5M15 15l6 6M4 4l5 5"/></svg></div>
      <h3>Sophos SG UTM 9 (EOL) ve XGS Yenilemeleri</h3>
      <p><a href="https://support.sophos.com/support/s/article/KBA-000003353" target="_blank" rel="noopener">Sophos UTM 9, 30 Haziran 2026 itibarıyla kullanım ömrünü (EOL) tamamladı</a> (tüm uzatılmış destek 31 Aralık 2026'da tamamen bitiyor) ve kurumları yeni XGS donanımı ile yıllık Xstream Protection aboneliklerine zorluyor. Stratum FW, eski Sophos yöneticilerine alışkın oldukları sade tek ekran yönetimini aboneliksiz sunar.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div>
      <h3>WatchGuard Firebox Güvenlik Paketleri</h3>
      <p>WatchGuard Firebox cihazlarında IPS, WebBlocker URL filtreleme ve itibar korumasının açık kalması için her yıl cihaz başına <a href="https://www.watchguard.com/help/docs/help-center/en-us/Content/en-us/my_products/subscription_expiration.html" target="_blank" rel="noopener">Basic veya Total Security Suite</a> özellik anahtarı (Feature Key) yenilenmesi gerekir. Stratum FW'de özellik anahtarı, tünel sınırı veya donanım kilidi yoktur.</p>
    </div>
  </div>

  <div class="cmp-table-wrap">
    <table class="cmp-table">
      <thead>
        <tr>
          <th>Operasyonel Kriter</th>
          <th class="cmp-highlight">Stratum FW (0 $ / Yıl)</th>
          <th>Fortinet FortiGate (UTP / ATP)</th>
          <th>Sophos SG UTM 9 / XGS</th>
          <th>WatchGuard Firebox</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Yıllık Güvenlik Yenilemesi</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Süresiz 0 $</span><br>Lisans anahtarı, seri numarası kaydı veya bulut hesabı yok</td>
          <td><span class="cmp-badge-no">Seri No Bazlı Ücretli</span><br>Her yıl FortiCare + FortiGuard UTP/ATP sözleşmesi gerekir</td>
          <td><span class="cmp-badge-no">Cihaz Bazlı Ücretli</span><br>UTM 9 EOL (Haz 2026); XGS için yıllık Xstream paketi gerekir</td>
          <td><span class="cmp-badge-no">Özellik Anahtarı</span><br>Her yıl cihaz başına Basic / Total Security Suite yenilemesi</td>
        </tr>
        <tr>
          <td><strong>Abonelik Bittiğinde Ne Olur?</strong></td>
          <td class="cmp-highlight"><span class="cmp-badge-yes">Asla Kapanmaz</span><br>Tüm tehdit listeleri, imzalar ve sistem güncellemeleri sürer</td>
          <td>Kategori Web/DNS filtresi durur; IPS donar; yazılım güncellemesi kapanır</td>
          <td>Süresi biten koruma modülleri durur ve ilgili yönetim sekmeleri kilitlenir</td>
          <td>WebBlocker, IPS ve tehdit istihbaratı devre dışı kalır</td>
        </tr>
        <tr>
          <td><strong>Web ve DNS İçerik Filtreleme</strong></td>
          <td class="cmp-highlight">Unbound 1.26 RPZ DNSBL + 55 kategorili Web Filtresi + Zorunlu Güvenli Arama</td>
          <td>FortiGuard Bulut URL/DNS sorgusu (aktif UTP şart)</td>
          <td>Xstream Web &amp; DNS Protection aboneliği</td>
          <td>WebBlocker ve DNSWatch aboneliği</td>
        </tr>
        <tr>
          <td><strong>IPS ve Fidye Yazılımı C2 Listeleri</strong></td>
          <td class="cmp-highlight">Çift motorlu Suricata 8 IPS/IDS + 63 otomatik IP beslemesi (<em>Spamhaus, Feodo, Abuse.ch</em>)</td>
          <td>FortiGuard IPS ve Botnet IP/Alan Adı aboneliği</td>
          <td>Xstream Network Protection IPS aboneliği</td>
          <td>WatchGuard IPS ve RED aboneliği</td>
        </tr>
        <tr>
          <td><strong>Uzaktan Çalışma ve Şube VPN</strong></td>
          <td class="cmp-highlight"><strong>Sınırsız</strong> çekirdek WireGuard (QR kod) ve Çoklu WAN OpenVPN</td>
          <td>Modele göre tünel kapasitesi; gelişmiş FortiClient EMS ücretli</td>
          <td>Modele göre kapasite; RED / ZTNA lisanslaması</td>
          <td>Kutu modeline göre şube ve mobil VPN limitleri</td>
        </tr>
        <tr>
          <td><strong>Uzaktan Değişiklik ve Güncelleme Güvenliği</strong></td>
          <td class="cmp-highlight"><strong>60 sn Safe-Apply</strong> otomatik geri alma + ZFS Önyükleme Ortamları</td>
          <td>Çift flash bölüm (yama indirmek için aktif FortiCare gerekir)</td>
          <td>Çift yazılım alanı (güncelleme için destek paketi gerekir)</td>
          <td>Fireware yedek imajı (güncelleme için aktif destek gerekir)</td>
        </tr>
        <tr>
          <td><strong>Donanım ve Sanallaştırma Özgürlüğü</strong></td>
          <td class="cmp-highlight">Standart tüm x86-64 mini-PC, kabin sunucusu, Proxmox, VMware ve KVM</td>
          <td>Kapalı FortiGate donanımı veya çekirdek lisanslı FortiGate-VM</td>
          <td>Sophos XGS donanımı veya vCPU/RAM sınırlı SFOS sanal lisans</td>
          <td>Kapalı Firebox donanımı veya kademeli FireboxV lisansı</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="cmp-block">
  <div class="cmp-head">
    <span class="eyebrow">Temel Mimari Farklar</span>
    <h2>Mühendisler neden modern bir alternatif olarak Stratum Firewall'ı inceliyor?</h2>
    <p>Stratum Firewall hem eski PHP/XML karmaşasını hem de her yıl yenilenen UTM abonelik yükünü ortadan kaldırarak ağ güvenliğinin en kritik %95'lik çekirdeğini tek ve hızlı bir yapıda birleştirir.</p>
  </div>

  <div class="card-grid grid-2">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="16" rx="2"/><rect x="9" y="9" width="6" height="6"/><path d="M9 1v3M15 1v3M9 20v3M15 20v3M20 9h3M20 14h3M1 9h3M1 14h3"/></svg></div>
      <h3>Sıfır PHP, Sıfır Monolitik XML Darboğazı</h3>
      <p>Yüzlerce güvenlik kuralı, 63 tehdit listesi ve çoklu arayüz grafikleri; Python 3.12, FastAPI ve SQLite WAL mimarisi sayesinde devasa <code>config.xml</code> dosyalarının yeniden yazılmasını beklemeden milisaniyeler içinde yanıt verir.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
      <h3>60 Saniyelik Safe-Apply ve Ücretsiz ZFS Geri Alma</h3>
      <p>Uzaktaki bir şube cihazında yanlış kural yüzünden erişimi kaybetme riski ortadan kalkar. Her kritik değişiklik 60 saniyelik onay penceresiyle uygulanır; bağlantı kesilirse sistem otomatik olarak önceki çalışan duruma döner.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 3h5v5M4 20L21 3M21 16v5h-5M15 15l6 6M4 4l5 5"/></svg></div>
      <h3>OPNsense'den İstemciye Dokunmadan Geçiş</h3>
      <p>Mevcut OPNsense <code>config.xml</code> yedeğinizi yükleyerek arayüzleri, VLAN'ları, <code>/29</code> Sanal IP bloklarını, takma adları, NAT kurallarını ve OpenVPN sertifika altyapısını tek adımda taşıyabilirsiniz.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"/></svg></div>
      <h3>Birleşik IP, DNSBL ve 55 Kategorili Web Filtresi</h3>
      <p>Yıllık UTM paketleri yenilemeye veya ayrı eklentiler kurmaya gerek kalmadan 63 çekirdek IP listesi, Unbound 1.26 RPZ DNSBL, 55 kategorili Web Filtresi ve birleşik engelleme raporları yerleşik gelir.</p>
    </div>
  </div>
</div>

<div class="cmp-block">
  <div class="cmp-head">
    <span class="eyebrow">Şeffaf Mühendislik Rehberi</span>
    <h2>Hangi güvenlik duvarı platformu sizin senaryonuza daha uygun?</h2>
    <p>Her açık kaynak ve ticari güvenlik duvarı farklı öncelikler için optimize edilmiştir. Altyapı ihtiyacınıza göre dürüst önerimiz:</p>
  </div>

  <div class="card-grid grid-2">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Ne zaman Stratum Firewall tercih edilmeli?</h3>
      <p><a href="/tr/solutions/smb/">Yıllık UTM aboneliklerinden çıkmak isteyen KOBİ'ler</a>, şube ağları, ev laboratuvarları ve eğitim kurumlarında; hızlı FastAPI arayüzü, 60 sn Safe-Apply koruması, ücretsiz ZFS geri alma, 55 kategorili Web Filtresi, çift motorlu Suricata 8 ve OPNsense geçişi arıyorsanız idealdir (Not: v0.30.0 şu an aktif Beta aşamasındadır).</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></svg></div>
      <h3>Ne zaman OPNsense, pfSense veya Ticari NGFW seçilmeli?</h3>
      <p>Topolojiniz şu anda aktif/pasif CARP yüksek erişilebilirlik (HA) kümesi, BGP/OSPF dinamik yönlendirme (FRR) veya IPsec IKEv2 tünelleri gerektiriyorsa <strong>OPNsense / pfSense</strong>; regülasyon gereği tam TLS 1.3 şifre çözme (<code>ssl-bump</code>), bulut sandbox ve 7/24 yerinde donanım değişim SLA'i zorunluysa ticari bir NGFW tercih edilmelidir.</p>
    </div>
  </div>
</div>

<div class="cmp-block">
  <div class="cmp-head">
    <span class="eyebrow">Sıkça Sorulan Sorular</span>
    <h2>Stratum Firewall değerlendirme ve UTM geçiş soruları</h2>
  </div>
  <div class="cmp-faq">
    <div class="cmp-faq-item">
      <h3>Lisansı biten FortiGate, WatchGuard veya EOL Sophos UTM 9 yerine geçebilir mi?</h3>
      <p>Evet. Standart çok portlu bir x86-64 mini sunucu veya Proxmox sanal makinesi üzerine Stratum kurarak; her yıl döviz bazlı FortiGuard UTP, WatchGuard Security Suite veya ömrü dolan Sophos SG/UTM kutularının yerine yıllık 0 $ maliyetle Suricata 8 IPS, 55 kategorili Web Filtresi, Çoklu WAN ve sınırsız VPN konumlandırabilirsiniz.</p>
    </div>
    <div class="cmp-faq-item">
      <h3>Stratum Firewall kayıt olmadan tamamen ücretsiz mi indiriliyor?</h3>
      <p>Evet. Hem 1.3 GB VGA <code>.iso</code> hem de 1.5 GB Seri <code>.img</code> kurulum imajları hesap açmadan, cihaz aktivasyon kodu istemeden ve kullanıcı sınırı olmadan SHA-256 doğrulamasıyla doğrudan indirilir.</p>
    </div>
    <div class="cmp-faq-item">
      <h3>OPNsense'den geçiş yaptığımda sahadaki OpenVPN bilgisayarları kopar mı?</h3>
      <p>Hayır. Geçiş sihirbazı OPNsense CA sertifikasını, sunucu anahtarlarını, şifreleme parametrelerini ve tünel alt ağlarını birebir içe aktardığı için sahadaki bilgisayar ve telefonlar profil değiştirmeden bağlanmaya devam eder.</p>
    </div>
    <div class="cmp-faq-item">
      <h3>Donanım hazırlamadan önce gerçek arayüzü test edebilir miyim?</h3>
      <p>Evet — <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">demo.stratumfw.com</a> adresinden izole edilmiş gerçek bir FreeBSD 15.1 Stratum Firewall sanal makinesine kayıt olmadan anında bağlanabilirsiniz.</p>
    </div>
  </div>
</div>

<div class="cmp-block">
  <div class="cmp-cta">
    <div class="cmp-cta-text">
      <h3>Yıllık güvenlik duvarı lisans vergisine son verin — 10 saniyede canlı deneyin</h3>
      <p>Tüm güvenlik duvarı kurallarını, engelleme raporlarını ve Çoklu WAN ayarlarını canlı demoda inceleyin ya da fiziksel sunucu ve Proxmox kurulumları için FreeBSD 15.1 ISO/IMG dosyasını indirin.</p>
    </div>
    <div class="cmp-cta-actions">
      <a href="/tr/download/" class="btn btn-primary">Ücretsiz ISO / IMG İndir</a>
      <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-secondary">Canlı Demoyu Aç</a>
      <a href="/tr/solutions/smb/" class="btn btn-secondary">KOBİ UTM Geçiş Rehberi</a>
    </div>
  </div>
</div>

</div>
