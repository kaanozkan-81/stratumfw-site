---
title: "Sistem Mimarisi"
seo_title: "Stratum Firewall v0.30.0 Mimarisi — FreeBSD 15.1, pf, FastAPI, TOTP 2FA ve ZFS"
description: "Stratum Firewall v0.30.0 teknik mimarisi — FreeBSD 15.1-RELEASE çekirdeği, pf ve 1:1 binat veri düzlemi, çift motorlu Suricata 8 IPS, AD istisnalı Zorunlu DNS, TOTP 2FA destekli FastAPI kontrol düzlemi ve ZFS Önyükleme Ortamları."
lead: "Sıfır PHP yükü, tekil bildirimsel yapılandırma kaynağı, 60 saniyelik Safe-Apply geri alma güvencesi ve saf FreeBSD 15.1 çekirdek veri düzlemi üzerine inşa edilen v0.30.0 sistem mimarisi."
full_width: true
---

<div class="solutions-layout">
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">01 · Dört Katmanlı Yığın (v0.30.0)</span>
<h2>Kontrol düzlemi ile çekirdek veri düzlemi arasında kesin ayrım</h2>
<p>Geleneksel güvenlik duvarı dağıtımlarında yönetim arayüzü, eklentiler ve sistem betikleri aynı monolitik PHP/XML katmanında iç içe geçer. Stratum Firewall ise yüksek hızlı paket işlemeyi tamamen <strong>FreeBSD 15.1-RELEASE</strong> çekirdeğine bırakırken, yönetim ve doğrulama katmanını asenkron <strong>Python 3.12 &amp; FastAPI</strong> mimarisiyle izole eder.</p>
</div>
<div class="arch-stack">
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Katman 04 · Yönetim &amp; Kimlik</span>
<h3>Asenkron Kontrol ve Gözlemlenebilirlik</h3>
</div>
<div class="arch-layer-body">
<p><strong>Nginx TLS</strong> ters vekili arkasında çalışan <strong>Python 3.12 / FastAPI</strong> uygulaması; web arayüzünü, REST API uç noktalarını, 12 sütunlu özelleştirilebilir kontrol panelini ve <strong>1s/24s Güvenlik Duvarı Günlük Özeti</strong> analizlerini sunar. Hesaplar tarayıcıda yerel çizilen QR kodlu <strong>TOTP İki Aşamalı Giriş</strong> (<code>v0.30.0</code>), 10 kurtarma kodu, giriş koruması ve 4 kademeli RBAC ile korunur.</p>
<div class="arch-pills">
<span class="arch-pill"><b>UI/API:</b> Python 3.12 + FastAPI</span>
<span class="arch-pill"><b>Kimlik:</b> Yerel QR TOTP 2FA + RBAC</span>
<span class="arch-pill"><b>AI:</b> Salt-Okunur MCP (15+ Araç)</span>
<span class="arch-pill"><b>İzleme:</b> bsnmpd + Uzak Syslog + Telegram/SMTP</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Katman 03 · Durum &amp; Orkestrasyon</span>
<h3>Bildirimsel Motor ve Canlı Yenileme</h3>
</div>
<div class="arch-layer-body">
<p>Tüm ağ niyeti tek bir denetlenebilir <code>config.json</code> dosyasında tutulur; parolalar, <strong>TOTP 2FA gizli anahtarları</strong> ve VPN/TLS özel anahtarları ise <code>0600</code> izinli <code>root</code> dosyalarında ayrı saklanır. <code>stratum-config</code> her değişikliği uygulamadan önce sözdizimi testlerinden geçirip <strong>60 sn Safe-Apply</strong> sayacını başlatır; <code>stratum-pfrefresh</code> ise kural zamanlamalarını ve dinamik DHCP WAN ağ geçidi değişimlerini Apply gerektirmeden canlı işler.</p>
<div class="arch-pills">
<span class="arch-pill"><b>Durum:</b> config.json + SQLite WAL</span>
<span class="arch-pill"><b>Derleyici:</b> stratum-config (Ön Doğrulamalı)</span>
<span class="arch-pill"><b>Canlı pf:</b> stratum-pfrefresh + stratum-netextra</span>
<span class="arch-pill"><b>Geçiş:</b> OPNsense XML + OpenVPN PKI Aktarıcı</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Katman 02 · Tehdit &amp; Tünel Servisleri</span>
<h3>Derin Paket Denetimi, Zorunlu DNS ve PKI</h3>
</div>
<div class="arch-layer-body">
<p><strong>Suricata 8</strong> arayüz bazında satır içi <code>netmap</code> IPS ve pasif <code>pcap</code> IDS süreçlerini paralel çalıştırır ve <code>SIGUSR2</code> ile kesintisiz kural yeniler. <strong>Unbound 1.26 RPZ</strong> DNSBL ve 55 kategorili Web Filtresi; <strong>Zorunlu DNS</strong> (<code>v0.29.2</code>) ve DoT (<code>TCP 853</code>) engeliyle aşılması imkansız hale getirilir (Active Directory istisna takma adı destekli, sıfır gezinme kaydı). <strong>OpenVPN</strong> yerleşik EC CA ve çift WAN <code>multihome</code> desteğiyle çalışır.</p>
<div class="arch-pills">
<span class="arch-pill"><b>IPS/IDS:</b> Suricata 8 (netmap + pcap)</span>
<span class="arch-pill"><b>DNS:</b> Unbound 1.26 RPZ + Force DNS</span>
<span class="arch-pill"><b>Rapor:</b> Birleşik Engelleme Raporu (7 Gün)</span>
<span class="arch-pill"><b>SSL VPN:</b> OpenVPN EC CA + multihome</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Katman 01 · Çekirdek &amp; ZFS</span>
<h3>FreeBSD 15.1 Veri Düzlemi ve Atomik Disk</h3>
</div>
<div class="arch-layer-body">
<p><strong>FreeBSD 15.1-RELEASE</strong> (<code>amd64</code>) çekirdeğindeki <strong>pf</strong>; durum bilgili filtreleme, çift yönlü <strong>1:1 NAT</strong> (<code>binat</code>), Port Yönlendirme, Politika Yönlendirme, 63 IP tehdit beslemesi ve 5-RIR lisanssız GeoIP radix tablolarını hat hızında yürütür. Çekirdek içi <strong>WireGuard</strong> (<code>if_wg</code>), <strong>dummynet</strong> hız sınırlayıcı, 1 saniyelik Statik/PPPoE/<strong>DHCP Çoklu WAN</strong> izleyicisi ve <strong>ZFS Önyükleme Ortamları</strong> kesintisiz çalışma sağlar.</p>
<div class="arch-pills">
<span class="arch-pill"><b>Çekirdek:</b> FreeBSD 15.1 pf + binat + dummynet</span>
<span class="arch-pill"><b>VPN:</b> Kernel WireGuard (if_wg)</span>
<span class="arch-pill"><b>WAN:</b> Statik / PPPoE (mpd5) / Dinamik DHCP WAN2</span>
<span class="arch-pill"><b>Dosya Sistemi:</b> ZFS Boot Environments (bectl)</span>
</div>
</div>
</div>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">02 · Paket ve Yapılandırma Akışı</span>
<h2>Bir paketin ve bir kural değişikliğinin yaşam döngüsü</h2>
<p>Stratum Firewall hem ağ paketlerini hem de yönetici değişikliklerini deterministik, öngörülebilir ve kendi kendini onaran bir boru hattından geçirir.</p>
</div>
<div class="sol-steps">
<div class="sol-step">
<span class="sol-step-num">AKIŞ 01 · VERİ DÜZLEMİ</span>
<h3>NIC &rarr; Suricata netmap &rarr; pf &rarr; dummynet</h3>
<p>Arayüze giren paket önce etkinse satır içi <code>netmap</code> Suricata IPS denetiminden, ardından <code>pf</code> bellek içi IP engelleme tablolarından, <strong>Zorunlu DNS</strong> / Port Yönlendirme / <strong>1:1 NAT</strong> dönüşümünden, arayüz grubu ve zamanlamalı kurallardan geçerek <code>dummynet</code> hız borularına ve seçili WAN ağ geçidine iletilir.</p>
</div>
<div class="sol-step">
<span class="sol-step-num">AKIŞ 02 · 60 SN SAFE-APPLY</span>
<h3>Ön Doğrulama &rarr; Atomik Yükleme &rarr; Otomatik Geri Alma</h3>
<p>Değişiklik uygulandığında <code>stratum-config</code> tüm servis dosyalarını üretip <code>pfctl -n</code>, <code>nginx -t</code>, <code>sshd -t</code> ve <code>unbound-checkconf</code> ile doğrular. Yönetici <strong>60 saniye</strong> içinde bağlantısını onaylamazsa sistem bir önceki çalışan <code>config.json</code> durumuna otomatik döner.</p>
</div>
<div class="sol-step">
<span class="sol-step-num">AKIŞ 03 · CANLI SENKRONİZASYON</span>
<h3>stratum-pfrefresh &amp; Dinamik Tablolar</h3>
<p>Saat aralıklı kurallar (<code>Schedules</code>) dakikada bir kontrol edilir ve izin süresi bittiğinde o kuralın açtığı mevcut bağlantılar kapatılır. İkincil <strong>DHCP WAN</strong> kira ağ geçidi değişimleri, WAN failover geçişleri, 5 dakikalık Hostname çözümleri ve IPBL güncellemeleri Apply gerektirmeden canlı uygulanır.</p>
</div>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">03 · Alt Sistem Matrisi (v0.30.0)</span>
<h2>Teknik bileşen ve izolasyon referansı</h2>
<p>Her modül, gereksiz üçüncü parti bağımlılıklar olmadan doğrudan FreeBSD 15.1 yerli araçları ve denetlenebilir servis yapılandırmaları üzerinde çalışır.</p>
</div>
<div class="sol-table-wrap">
<table class="sol-table">
<thead>
<tr>
<th>Alt Sistem</th>
<th>Çekirdek Servis / Sürücü</th>
<th>Veri &amp; Güvenlik Sınırı</th>
<th>v0.30.0 Mimari Yetenekleri</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>İşletim Sistemi &amp; Disk</strong></td>
<td>FreeBSD 15.1-RELEASE · <code>ZFS</code> (<code>bectl</code>)</td>
<td><code>zroot/ROOT/*</code> (Son 5 ortam korunur)</td>
<td>Çevrimdışı VGA <code>.iso</code> ve Seri <code>.img</code> kurulumu; imzalı çevrimiçi ve kapalı devre <code>.sfu</code> güncellemeleri öncesi atomik ZFS anlık görüntüsü.</td>
</tr>
<tr>
<td><strong>Paket Filtresi, NAT &amp; Zamanlama</strong></td>
<td>Kernel <code>pf</code> · <code>dummynet</code> · <code>stratum-pfrefresh</code></td>
<td><code>/etc/pf.conf</code> + kalıcı radix tabloları</td>
<td>Arayüz grupları, çift yönlü <strong>1:1 NAT</strong> (WAN ana IP kilitleme korumalı), Port Yönlendirme + NAT Yansıması, dakikalık <strong>Kural Zamanlamaları</strong> (süre sonunda state kapatma) ve WAN varsayılan engel gürültü anahtarı.</td>
</tr>
<tr>
<td><strong>Çoklu WAN &amp; Yönlendirme</strong></td>
<td>Gateway Monitor · <code>mpd5</code> · <code>dhclient</code> · <code>stratum-netextra</code></td>
<td><code>rc.conf</code> kirletmeyen çekirdek VIP/rota senkronu</td>
<td>1 saniyelik gecikme/kayıp yoklaması, Statik/PPPoE ve varsayılan rotayı bozmayan <strong>Dinamik DHCP ek WAN</strong> (<code>WAN2</code>/<code>WAN3</code>), simetrik <code>reply-to</code>, kural bazlı Politika Yönlendirme ve <code>/29</code> Sanal IP.</td>
</tr>
<tr>
<td><strong>DNS, Zorunlu DNS &amp; Web Filtresi</strong></td>
<td><code>Unbound 1.26</code> (DNSSEC + RPZ) · <code>dnsmasq</code></td>
<td>Web Etkinliğinden bağımsız sıfır gezinme kaydı</td>
<td>pfBlockerNG + HaGeZi DNSBL, ağ bazlı <strong>55 kategorili Web Filtresi</strong> + Zorunlu Güvenli Arama, <strong>AD istisna takma adlı Zorunlu DNS</strong> (<code>Force DNS</code>) ve DoT (<code>TCP 853</code>) engelleme.</td>
</tr>
<tr>
<td><strong>Tehdit Listeleri &amp; IPS/IDS</strong></td>
<td><code>Suricata 8</code> (<code>netmap</code> + <code>pcap</code>) · <code>stratum-ipbl</code></td>
<td>7 günlük SQLite birleşik engelleme veritabanı</td>
<td>63 otomatik IPv4 tehdit beslemesi, 5-RIR lisanssız Ülke (GeoIP) tabloları, arayüz bazlı çift motorlu Suricata IPS/IDS (<code>SIGUSR2</code> canlı kural yenileme) ve pfBlockerNG tarzı <strong>Birleşik Engelleme Raporları</strong>.</td>
</tr>
<tr>
<td><strong>Uzaktan Erişim VPN</strong></td>
<td>Kernel <code>if_wg</code> (WireGuard) · <code>OpenVPN</code></td>
<td>Cihaz içi RFC 7748 X25519 ve yerleşik EC PKI</td>
<td>Çekirdek hızında QR kodlu WireGuard (kesintisiz eş ekleme) ve çoklu WAN <code>multihome</code>, <code>tls-crypt</code>, <code>VPN-only</code> kullanıcı destekli OpenVPN sunucusu.</td>
</tr>
<tr>
<td><strong>Kimlik, 2FA &amp; Yedekleme</strong></td>
<td>FastAPI Auth · Yerel TOTP · OpenSSH SFTP</td>
<td><code>0600 root</code> gizli dosyalar + <code>AES-256-GCM</code> (<code>scrypt</code>)</td>
<td>Tarayıcıda yerel çizilen QR kodlu <strong>TOTP 2FA</strong> (<code>v0.30.0</code>), tek kullanımlık kod koruması, 10 kurtarma kodu, konsol 2FA sıfırlama (<code>stratum-menu</code>) ve zamanlanmış şifreli <code>.stbk</code> SFTP yedekleme.</td>
</tr>
<tr>
<td><strong>OPNsense Geçiş Motoru</strong></td>
<td>Bildirimsel XML Dönüştürücü</td>
<td>Uygulama öncesi etkileşimli uyumluluk raporu</td>
<td>OPNsense <code>config.xml</code> içinden VLAN, VIP, kurallar, Politika Yönlendirme, <strong>DHCP WAN2</strong>, <strong>Zorunlu DNS</strong>, katalog <strong>URL-tablo IP listeleri</strong> ve OpenVPN PKI + <code>$2y$</code>/<code>argon2id</code> parolalarını devralma.</td>
</tr>
<tr>
<td><strong>Tanılama, Günlük Özeti &amp; MCP</strong></td>
<td><code>filterlog</code> Analizcisi · <code>tcpdump</code> · MCP · <code>bsnmpd</code></td>
<td>Salt-okunur AI araç sınırı + özetlenmiş API jetonları</td>
<td><strong>1s/24s Güvenlik Duvarı Günlük Özeti</strong> (kaynak IP, ülke, hedef servis portu ve 5dk/saatlik grafikler), Wireshark <code>.pcap</code> yakalama, ARP/WoL ve Claude/ChatGPT/Gemini uyumlu salt-okunur <strong>MCP sunucusu</strong>.</td>
</tr>
</tbody>
</table>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">04 · Güvenlik Mühendisliği İlkeleri</span>
<h2>Varsayılan olarak sertleştirilmiş tasarım</h2>
<p>Stratum Firewall, hem dış ağdan gelebilecek saldırılara hem de yönetici hatalarına karşı savunma derinliği (defense-in-depth) ilkeleriyle donatılmıştır.</p>
</div>
<div class="card-grid grid-3">
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="7.5" cy="15.5" r="5.5"/><path d="m21 2-9.6 9.6"/><path d="m15.5 7.5 3 3L22 7l-3-3"/></svg></div>
<h3>İzole Gizli Anahtar Kasası (<code>0600</code>)</h3>
<p>Kullanıcı parola özetleri, <strong>TOTP 2FA sırları</strong>, WireGuard/OpenVPN özel anahtarları, PPPoE şifreleri ve bildirim jetonları asla <code>config.json</code> içine yazılmaz; yalnızca <code>root</code> erişimli ayrı dosyalarda tutulur.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div>
<h3>Sıfır Bulut Sızıntılı TOTP 2FA</h3>
<p>İki aşamalı giriş kurulumundaki QR kodu harici bir internet servisine gönderilmeden doğrudan tarayıcıda gömülü kütüphaneyle çizilir; anahtar güvenlik duvarından asla dışarı çıkmaz ve kurulum sonrası tekrar gösterilmez.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
<h3>Kendi Kendini Kilitleme Koruması</h3>
<p>O anda bağlı olduğunuz IP adresini yönetimden düşürecek SSH/Web değişiklikleri ve WAN arayüzünün kendi yönetim IP adresini <strong>1:1 NAT</strong> hedefine bağlama girişimleri doğrulama katmanında otomatik reddedilir.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"/></svg></div>
<h3>Gizlilik Odaklı Zorunlu DNS</h3>
<p><strong>Force DNS</strong> özelliği iç ağlardaki kaçak DNS ve DoT sorgularını güvenlik duvarına yönlendirirken <em>Web Etkinliği</em> modülünden tamamen bağımsız çalışır; tek başına açıldığında hiçbir gezinme geçmişi kaydetmez.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polyline points="4 17 10 11 4 5"/><line x1="12" y1="19" x2="20" y2="19"/></svg></div>
<h3>%100 Salt-Okunur Yapay Zeka (MCP)</h3>
<p>Model Context Protocol sunucusu kural, parola veya özel anahtar değiştirebilecek hiçbir yazma ucu barındırmaz; IP kısıtlı, özetlenmiş (hashed) jetonlarla yalnızca canlı tanılama verisi sunar.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
<h3>Temiz FreeBSD Sistem Üretimi</h3>
<p>Üretilen <code>/etc/rc.conf</code>, <code>/etc/pf.conf</code> ve servis dosyaları deterministiktir; FreeBSD varsayılanları gereksiz fark üretmez ve <code>/etc/sysctl.conf.local</code> değişikliklerinde orijinal dosya korunur.</p>
</div>
</div>
</div>
<div class="sol-cta-box">
<div>
<h3>v0.30.0 mimarisini canlı ortamda test edin</h3>
<p>Kayıt olmadan tarayıcınızda çalışan gerçek FreeBSD 15.1 QEMU sanal makinesine bağlanın veya doğrudan ZFS kurulum imajlarını indirin.</p>
</div>
<div class="sol-cta-actions">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-primary">Canlı Demo'yu Aç</a>
<a href="/tr/download/" class="btn btn-secondary" style="background:rgba(255,255,255,0.12);color:#FFFFFF !important;border-color:rgba(255,255,255,0.28);">ISO / IMG İndir</a>
<a href="/tr/resources/documentation/" class="btn btn-secondary" style="background:rgba(255,255,255,0.12);color:#FFFFFF !important;border-color:rgba(255,255,255,0.28);">Dokümantasyon</a>
</div>
</div>
</div>
