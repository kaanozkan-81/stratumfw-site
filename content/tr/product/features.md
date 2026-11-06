---
title: "Özellikler"
description: "Stratum Firewall'un temel yeteneklerini keşfedin: Çoklu VLAN ağ topolojisi, FreeBSD pf çekirdek paket filtreleme, DNSBL web filtreleme, IP karalisteleri ve FastAPI kontrolü."
lead: "Saf FreeBSD 14/15 gücü üzerine inşa edilen, asenkron Python 3.12 ve FastAPI kontrol katmanıyla yönetilen yeni nesil güvenlik duvarı yetenekleri."
---

<div class="feature-showcase-page" style="margin-top: 16px;">

  <div class="feature-row" style="display:flex;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall çoklu arayüz, WAN, Ofis LAN, Misafir Wi-Fi, IoT VLAN bölütleme ve WireGuard VPN ağ topolojisi diyagramı" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">AĞ TOPOLOJİSİ &amp; VLAN YÖNETİMİ</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Ağ Topolojisi ve Çoklu VLAN Bölütleme ile Tam İzolasyon</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">Fiziksel ağ kartlarınızı ve sanal yerel ağlarınızı (VLAN) tek bir merkezden yöneterek ofis, misafir, personel ve IoT cihaz trafiğini birbirinden güvenli biçimde ayırın.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall, karmaşık kurumsal ve kişisel ağ yapılarını sadeleştiren esnek bir çoklu arayüz mimarisi sunar. Fiziksel ağ portlarınızı (<code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, <code>igb3</code> OPT2) ihtiyacınıza göre yapılandırabilir; aynı fiziksel hat üzerinde Ofis LAN, Misafir Wi-Fi, Personel ve Güvenlik Kameraları (IoT) gibi mantıksal olarak tamamen ayrıştırılmış VLAN bölgeleri oluşturabilirsiniz. Entegre DHCP Sunucusu, DNS Çözümleyici (Resolver) ve NTP zaman senkronizasyonu servisleri sayesinde her alt ağ kendi adresleme ve erişim politikalarıyla bağımsız olarak çalışır.</p>
      <ul>
        <li><strong>Çoklu WAN ve Arayüz Ataması:</strong> <code>pppoe0</code>, <code>igb1</code>, <code>igb2</code> ve <code>igb3</code> dahil çoklu fiziksel ve mantıksal ağ arayüzlerini esnek biçimde tanımlama.</li>
        <li><strong>Mantıksal VLAN Segmentasyonu:</strong> Misafir ağlarını ve güvenlik kameralarını kritik yerel sunuculardan izole ederek yatay tehdit yayılımını engelleme.</li>
        <li><strong>Yerleşik Temel Ağ Servisleri:</strong> Her VLAN ve arayüz için bağımsız DHCP havuzları, yerel DNS çözümleme ve NTP senkronizasyonunu tek panelden yönetme.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;flex-direction:row-reverse;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD çekirdek düzeyinde pf paket filtreleme ve çok katmanlı siber güvenlik kalkanı görseli" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">FREEBSD PF ÇEKİRDEK KORUMASI</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Ağ Kalkanı ve Kernel Düzeyinde pf Paket Filtreleme</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">FreeBSD 14/15 çekirdeğinde çalışan yerel pf (Packet Filter) motoru, durum bilgili paket denetimi (Stateful Inspection) ve Safe-Apply kural doğrulamasıyla ağınızı kesintisiz korur.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall, gücünü doğrudan FreeBSD 14/15 işletim sistemi çekirdeğindeki efsanevi <code>pf</code> (Packet Filter) altyapısından alır. Gelen ve giden her veri paketi çekirdek düzeyinde durum bilgili denetimden (Stateful Inspection) geçirilir; şüpheli bağlantılar anında düşürülürken güvenli trafik minimum gecikmeyle iletilir. Gelişmiş Ağ Adresi Dönüştürme (NAT) ve ALTQ tabanlı bant genişliği/trafik şekillendirme yetenekleri ağ performansını optimize eder. Ayrıca <strong>Safe-Apply</strong> mekanizması, yazdığınız her kuralı sisteme uygulamadan önce <code>pf</code> sözdizimine göre otomatik test ederek hatalı yapılandırmaların ağ bağlantısını kesmesini önler.</p>
      <ul>
        <li><strong>Kernel Düzeyinde Stateful Inspection:</strong> FreeBSD <code>pf</code> altyapısıyla yüksek verimli, düşük gecikmeli paket filtreleme ve bağlantı durumu takibi.</li>
        <li><strong>Safe-Apply Otomatik Kural Doğrulama:</strong> Güvenlik duvarı ve arayüz değişikliklerini canlı sisteme işlemeden önce sözdizimi doğrulamasından geçirerek kilitlenmeleri engelleme.</li>
        <li><strong>NAT, ALTQ ve Canlı Kural İstatistikleri:</strong> Takma adlar (Aliases), <code>Pass / Block / Reject</code> eylemleri, anlık kural eşleşme sayıları (Hits) ve ALTQ trafik şekillendirme ile tam kontrol.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL alan adı filtreleme ve tehdit istihbaratı IP karaliste koruma katmanları" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">TEHDİT İSTİHBARATI &amp; DNSBL</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">DNS Tabanlı Web Filtreleme (DNSBL) ve Otomatik IP Karalisteleri</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">Küresel tehdit istihbarat akışlarını (threat feeds) ve DNS karalistelerini kullanarak zararlı IP adreslerini ve kötü amaçlı alan adlarını ağınıza ulaşmadan durdurun.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall, geleneksel port bazlı güvenlik duvarı kurallarını proaktif tehdit engelleme katmanlarıyla güçlendirir. <strong>IP Karaliste (IP Blocklists)</strong> modülü, güncel siber tehdit istihbaratı akışlarını (threat feeds) otomatik olarak içeri aktarır ve bilinen saldırgan IP adreslerini doğrudan paket seviyesinde engeller. <strong>DNSBL (DNS Tabanlı Web Filtreleme)</strong> motoru ise oltalama (phishing), zararlı yazılım (malware), izleyici ve reklam alan adlarını DNS sorgusu aşamasında yakalayarak <code>NXDOMAIN</code> yanıtıyla düşürür; üstelik dahili alan adı sorgulama aracıyla herhangi bir domain'in filtre durumunu arayüzden anında test etmenize olanak tanır.</p>
      <ul>
        <li><strong>DNS Seviyesinde Engelleme (DNSBL):</strong> Zararlı ve istenmeyen alan adlarına yapılan istekleri <code>NXDOMAIN</code> yanıtıyla ağ genelinde istemci yazılımı gerektirmeden kesme.</li>
        <li><strong>Dinamik IP Tehdit Akışları:</strong> Açık kaynaklı ve kurumsal tehdit istihbaratı listelerini içe aktararak bilinen botnet ve saldırgan IP'lerini paket düzeyinde bloklama.</li>
        <li><strong>Anlık Alan Adı Test Aracı:</strong> Herhangi bir alan adının karalistede olup olmadığını yönetim arayüzü üzerinden tek tıkla sorgulama ve doğrulama.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;flex-direction:row-reverse;align-items:center;gap:48px;margin-bottom:40px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API kontrol paneli, WireGuard X25519 şifreleme ve canlı telemetri mimarisi" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">MODERN MİMARİ &amp; API KONTROLÜ</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Sıfır Bağımlılıklı Siber Güvenlik Mimarisi ve Modern API Kontrolü</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">"Saf FreeBSD gücü. Modern API kontrolü." Asenkron Python 3.12 / FastAPI arka ucu, tekil config.json bildirimsel yapılandırması ve yerel WireGuard kriptografisiyle yeni nesil güvenlik duvarı deneyimi.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall, hantal ve eski nesil güvenlik duvarı arayüzlerinin yerini almak üzere modern yazılım mühendisliği ilkeleriyle geliştirilmiştir. Asenkron Python 3.12 ve FastAPI üzerine inşa edilen arka uç, çok düşük bellek tüketimiyle hem hızlı bir web arayüzü hem de tam kapsamlı bir REST API sunar. Tüm sistem durumu tek bir bildirimsel <code>config.json</code> dosyasında tutulur ve Python motoru tarafından <code>pf.conf</code>, DHCP ve DNS yapılandırma dosyalarına dinamik olarak dönüştürülür. Harici kütüphane gerektirmeyen sıfırdan yazılmış X25519 (RFC 7748) kriptografili WireGuard VPN ve OpenVPN desteği, Rol Tabanlı Erişim Kontrolü (RBAC) ve canlı sistem telemetrisi ile tam kapsamlı bir siber güvenlik merkezi oluşturur.</p>
      <ul>
        <li><strong>FastAPI &amp; Bildirimsel config.json Mimarisi:</strong> Tek bir JSON kaynağından dinamik sistem dosyası üretimi ve otomasyona hazır tam kapsamlı REST API.</li>
        <li><strong>Sıfır Bağımlılıklı WireGuard &amp; OpenVPN:</strong> Harici bağımlılık olmadan yerel X25519 (RFC 7748) anahtar üretimiyle yüksek hızlı WireGuard ve OpenVPN tünelleme.</li>
        <li><strong>RBAC ve Canlı Telemetri İzleme:</strong> Yönetici (Administrator), Operatör (Operator) ve Salt Okunur (Read-Only) yetkilendirme rolleriyle birlikte gerçek zamanlı donanım, trafik ve log izleme.</li>
      </ul>
    </div>
  </div>

</div>
