---
title: "Donanım ve Cihaz Profilleri"
seo_title: "Stratum FW Donanım Profilleri — 4 Port, 8 Port SFP+ ve 12 Port 1U Rack Güvenlik Duvarı"
description: "Stratum FW 4 portlu fansız masaüstü, 8 portlu (2x SFP+ + 6x RJ45) şube ve 12 portlu (4x SFP+ + 8x RJ45) 1U rack tipi güvenlik duvarı profillerini inceleyin."
lead: "Sessiz 4 portlu masaüstü kutulardan 8 portlu SFP+ şube geçitlerine ve 12 portlu 1U kabinet cihazlarına kadar sıfır donanım bağımlılığı."
translationKey: "product-hardware"
full_width: true
images: ["/images/hardware-stratum-family.jpg"]
---

<style>
main:has(.hw-layout) .container,
.container:has(.hw-layout) {
max-width: 1240px !important;
width: 100% !important;
margin-left: auto !important;
margin-right: auto !important;
}
.container *:has(.hw-layout),
.prose:has(.hw-layout),
article:has(.hw-layout),
.single-content:has(.hw-layout),
.page-content:has(.hw-layout) {
max-width: 100% !important;
width: 100% !important;
}
.hw-layout {
width: 100%;
font-family: var(--font-sans, 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif);
color: #1E293B;
}
.hw-layout code {
background: #F1F5F9 !important;
color: #0F172A !important;
border: 1px solid #CBD5E1 !important;
padding: 2px 7px !important;
border-radius: 5px !important;
font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace !important;
font-size: 0.84em !important;
font-weight: 600 !important;
}
.hw-showcase-frame {
background: #FFFFFF;
border: 1px solid #E2E8F0;
border-radius: 18px;
overflow: hidden;
box-shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
margin-bottom: 56px;
}
.hw-showcase-img {
width: 100%;
display: block;
border-bottom: 1px solid #E2E8F0;
}
.hw-kpi-strip {
display: grid;
grid-template-columns: repeat(4, minmax(0, 1fr));
background: #0F172A;
}
@media (max-width: 960px) {
.hw-kpi-strip { grid-template-columns: repeat(2, minmax(0, 1fr)); }
}
@media (max-width: 560px) {
.hw-kpi-strip { grid-template-columns: 1fr; }
}
.hw-kpi-cell {
padding: 18px 22px;
border-right: 1px solid #1E293B;
}
.hw-kpi-cell:last-child {
border-right: none;
}
.hw-kpi-label {
display: block;
font-size: 11.5px;
font-weight: 700;
text-transform: uppercase;
letter-spacing: 0.07em;
color: #FBBF24 !important;
margin-bottom: 4px;
}
.hw-kpi-val {
display: block;
font-size: 14.5px;
font-weight: 600;
color: #F8FAFC !important;
line-height: 1.4;
}
.hw-sec-head {
max-width: 820px;
margin-bottom: 28px;
}
.hw-sec-eyebrow {
display: block;
color: #D97706 !important;
font-weight: 700;
font-size: 12.5px;
text-transform: uppercase;
letter-spacing: 0.08em;
margin-bottom: 6px;
}
.hw-sec-head h2 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
font-size: 28px !important;
font-weight: 700 !important;
color: #090D16 !important;
letter-spacing: -0.015em;
line-height: 1.25 !important;
margin: 0 0 8px 0 !important;
}
.hw-sec-head p {
font-size: 16px !important;
color: #475569 !important;
line-height: 1.62 !important;
margin: 0 !important;
}
.hw-grid {
display: grid;
grid-template-columns: repeat(3, minmax(0, 1fr));
gap: 24px;
margin-bottom: 60px;
}
@media (max-width: 1024px) {
.hw-grid { grid-template-columns: 1fr; }
}
.hw-card {
background: #FFFFFF !important;
border: 1px solid #E2E8F0 !important;
border-radius: 16px;
overflow: hidden;
display: flex;
flex-direction: column;
box-shadow: 0 6px 22px rgba(15, 23, 42, 0.05);
transition: transform 0.18s ease, box-shadow 0.18s ease;
}
.hw-card-featured {
border: 2px solid #F59E0B !important;
box-shadow: 0 10px 28px rgba(245, 158, 11, 0.10);
}
.hw-card img {
width: 100%;
aspect-ratio: 16 / 9;
object-fit: cover;
display: block;
border-bottom: 1px solid #E2E8F0;
}
.hw-card-body {
padding: 24px;
display: flex;
flex-direction: column;
flex: 1;
}
.hw-tier {
font-size: 12px;
font-weight: 700;
letter-spacing: 0.06em;
text-transform: uppercase;
color: #B45309 !important;
margin-bottom: 6px;
}
.hw-card h3 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
font-size: 20px !important;
font-weight: 700 !important;
color: #090D16 !important;
letter-spacing: -0.01em;
margin: 0 0 12px 0 !important;
}
.hw-badges {
display: flex;
flex-wrap: wrap;
gap: 8px;
margin-bottom: 14px;
}
.hw-badge-amber {
background: #FEF3C7 !important;
color: #92400E !important;
border: 1px solid #FDE68A !important;
border-radius: 999px;
padding: 4px 11px;
font-size: 12.5px;
font-weight: 700;
}
.hw-badge-slate {
background: #E0F2FE !important;
color: #075985 !important;
border: 1px solid #BAE6FD !important;
border-radius: 999px;
padding: 4px 11px;
font-size: 12.5px;
font-weight: 700;
}
.hw-card p {
font-size: 14.8px !important;
line-height: 1.6 !important;
color: #334155 !important;
margin: 0 0 16px 0 !important;
}
.hw-list {
list-style: none !important;
padding: 14px 0 0 0 !important;
margin: auto 0 0 0 !important;
border-top: 1px solid #F1F5F9;
}
.hw-list li {
font-size: 14px !important;
line-height: 1.55 !important;
color: #334155 !important;
margin-bottom: 8px !important;
padding-left: 16px !important;
position: relative;
}
.hw-list li:last-child {
margin-bottom: 0 !important;
}
.hw-list li strong {
color: #0F172A !important;
}
.hw-list li::before {
content: "•";
color: #D97706 !important;
font-weight: 800;
position: absolute;
left: 0;
}
.hw-table-wrap {
overflow-x: auto;
border: 1px solid #E2E8F0;
border-radius: 16px;
box-shadow: 0 8px 24px rgba(15, 23, 42, 0.05);
background: #FFFFFF;
margin-bottom: 56px;
}
.hw-table {
width: 100%;
border-collapse: collapse;
font-size: 14.5px;
min-width: 780px;
}
.hw-table thead th {
background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%) !important;
color: #F8FAFC !important;
font-weight: 700 !important;
text-align: left;
padding: 16px 18px;
border-bottom: 3px solid #F59E0B;
white-space: nowrap;
}
.hw-table tbody td {
padding: 14px 18px;
border-bottom: 1px solid #E2E8F0;
color: #1E293B !important;
vertical-align: middle;
line-height: 1.5;
}
.hw-table tbody tr:nth-child(even) td {
background: #F8FAFC;
}
.hw-table tbody tr:last-child td {
border-bottom: none;
}
.hw-table tbody td strong {
color: #090D16 !important;
}
.hw-price-pill {
display: inline-block;
background: #ECFDF5 !important;
color: #047857 !important;
border: 1px solid #A7F3D0 !important;
font-weight: 700;
font-size: 12.5px;
padding: 3px 10px;
border-radius: 999px;
}
.hw-cta-dark {
background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%) !important;
border: 1px solid rgba(245, 158, 11, 0.4) !important;
border-radius: 18px;
padding: 32px 36px;
display: flex;
flex-wrap: wrap;
align-items: center;
justify-content: space-between;
gap: 24px;
}
.hw-cta-dark h3 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
color: #F8FAFC !important;
font-size: 22px !important;
font-weight: 700 !important;
margin: 0 0 6px 0 !important;
}
.hw-cta-dark p {
color:#1E293B !important;
font-size: 15px !important;
line-height: 1.6 !important;
margin: 0 !important;
max-width: 680px;
}
.hw-cta-dark code {
background: rgba(255, 255, 255, 0.12) !important;
color: #FDE68A !important;
border: 1px solid rgba(251, 191, 36, 0.35) !important;
}
.hw-cta-actions {
display: flex;
flex-wrap: wrap;
gap: 12px;
}
.hw-btn-primary {
background: #F59E0B !important;
color: #090D16 !important;
font-weight: 700 !important;
font-size: 14.5px;
padding: 11px 22px;
border-radius: 10px;
text-decoration: none !important;
display: inline-block;
}
.hw-btn-secondary {
background: #FFFFFF !important;
color: #0F172A !important;
font-weight: 600 !important;
font-size: 14.5px;
padding: 11px 20px;
border-radius: 10px;
text-decoration: none !important;
display: inline-block;
}
</style>
<div class="hw-layout">
<div class="hw-showcase-frame">
<img src="/images/hardware-stratum-family.jpg" alt="Stratum FW 4, 8 ve 12 Portlu Donanım Ailesi" class="hw-showcase-img">
<div class="hw-kpi-strip">
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-400 • Kompakt Uç</span>
<span class="hw-kpi-val">4x RJ45 Fansız Masaüstü</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-800 • Fiber Şube</span>
<span class="hw-kpi-val">2x SFP+ ve 6x RJ45 (8 Port)</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-1200 • 1U Kurumsal</span>
<span class="hw-kpi-val">4x SFP+ ve 8x RJ45 (12 Port)</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">Sıfır Donanım Bağımlılığı</span>
<span class="hw-kpi-val">Tüm amd64 Sunucu ve VM • $0/Yıl</span>
</div>
</div>
</div>
<div class="hw-sec-head">
<span class="hw-sec-eyebrow">Referans Cihaz Profilleri</span>
<h2>Saf FreeBSD 15.1 Gücü İçin Ölçeklenebilir Üç Donanım Profili</h2>
<p>Her Stratum FW kurulumu; ister kompakt fansız kutularda, ister SFP+ fiber şube ve 1U kabinet cihazlarında, ister kendi mevcut <code>amd64</code> sunucunuzda çalışsın aynı hızlı FastAPI kontrol katmanını, durum bilgili <code>pf</code> motorunu, OpenZFS Önyükleme Ortamlarını ve <code>115200 8N1</code> seri konsol kurtarma mimarisini sunar.</p>
</div>
<div class="hw-grid">
<div class="hw-card">
<img src="/images/hardware-stratum-4port.jpg" alt="Stratum FW 4 Portlu Fansız Masaüstü Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">Kompakt Fansız Masaüstü • 4 Port</div>
<h3>Stratum FW 4 Portlu Profil</h3>
<div class="hw-badges">
<span class="hw-badge-amber">4x RJ45 Ethernet</span>
<span class="hw-badge-slate">USB-C Konsol + USB 3.0</span>
</div>
<p>Ev laboratuvarları, küçük ofisler, klinikler ve mağaza noktaları için sessiz pasif soğutmalı CNC alüminyum kasa; çift internet yedekliliği ve güvenli VPN erişimi sağlar.</p>
<ul class="hw-list">
<li><strong>Port Dizilimi:</strong> <code>0 – 3</code> RJ45 portlarıyla WAN1, 4G/5G DHCP WAN2, LAN ve Misafir Wi-Fi</li>
<li><strong>Güvenlik ve Erişim:</strong> 5 katmanlı Security Guard, Misafir Captive Portal, WireGuard ve OpenVPN</li>
<li><strong>Kurulum Medyası:</strong> OpenZFS üzerinde Seri Konsol <code>.img</code> veya VGA <code>.iso</code></li>
</ul>
</div>
</div>
<div class="hw-card hw-card-featured">
<img src="/images/hardware-stratum-8port.jpg" alt="Stratum FW 8 Portlu Fiber Şube Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">Fiber Şube ve KOBİ Geçidi • 8 Port</div>
<h3>Stratum FW 8 Portlu Profil</h3>
<div class="hw-badges">
<span class="hw-badge-amber">2x SFP+ Fiber</span>
<span class="hw-badge-slate">6x RJ45 Ethernet</span>
</div>
<p>İki adet SFP+ fiber yuvasını (<code>1 – 2</code>) altı adet bakır RJ45 portuyla (<code>3 – 8</code>) birleştiren geniş fansız soğutucu gövde; çoklu VLAN ve şubeler arası VPN merkezleri için idealdir.</p>
<ul class="hw-list">
<li><strong>Fiber Omurga:</strong> Doğrudan fiber sonlandırma veya anahtarlara 10G <code>802.1Q</code> VLAN bağlantısı</li>
<li><strong>Şubeler Arası VPN:</strong> Politika tabanlı IPsec (<code>IKEv2/IKEv1</code>) ve çekirdek WireGuard S2S (<code>wg1+</code>)</li>
<li><strong>Satır İçi Koruma:</strong> Suricata 8 <code>netmap</code> IPS ve <code>dummynet</code> cihaz başı adil hız sınırı</li>
</ul>
</div>
</div>
<div class="hw-card">
<img src="/images/hardware-stratum-12port-1u.jpg" alt="Stratum FW 12 Portlu 1U Rack Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">19 İnç 1U Kabinet Tipi • 12 Port</div>
<h3>Stratum FW 12 Portlu 1U Profil</h3>
<div class="hw-badges">
<span class="hw-badge-amber">4x SFP+ Fiber</span>
<span class="hw-badge-slate">8x RJ45 Ethernet</span>
</div>
<p>Dört adet SFP+ fiber yuvası (<code>1 – 4</code>), sekiz adet bakır RJ45 portu (<code>5 – 12</code>) ve ön panel RJ45 seri konsoluyla kampüsler, oteller ve merkez ofisler için yüksek port yoğunluğu.</p>
<ul class="hw-list">
<li><strong>Merkez Yönlendirme:</strong> Çoklu fiber WAN, <code>/29</code> Sanal IP, <code>1:1 NAT</code> ve 30 günlük VLAN matrisi</li>
<li><strong>Kampüs ve Konaklama:</strong> Çok bölgeli Captive Portal + RADIUS (<code>Mikrotik-Rate-Limit</code>) ve 56 kategorili Web Filtresi</li>
<li><strong>Yapay Zeka Tanılama:</strong> Yerel LAN <code>Ollama</code> ve <code>PUB-1</code> maskelemeli 15 araçlı salt okunur AI Asistanı</li>
</ul>
</div>
</div>
</div>
<div class="hw-sec-head">
<span class="hw-sec-eyebrow">Teknik Karşılaştırma</span>
<h2>Donanım Port ve Kurulum Matrisi</h2>
<p>4, 8 ve 12 portlu referans profilleri kendi standart x86-64 sunucu ve sanal makine kurulumlarınızla yan yana inceleyin.</p>
</div>
<div class="hw-table-wrap">
<table class="hw-table">
<thead>
<tr>
<th>Özellik</th>
<th>4 Portlu Masaüstü</th>
<th>8 Portlu Şube</th>
<th>12 Portlu 1U Rack</th>
<th>Kendi amd64 / VM Sunucunuz</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Kasa Formu</strong></td>
<td>Kompakt Fansız Masaüstü</td>
<td>Geniş Fansız Masaüstü</td>
<td>19 inç 1U Kabinet (Rack)</td>
<td>Mini-PC, Rack Sunucu, Proxmox, KVM</td>
</tr>
<tr>
<td><strong>Toplam Ağ Portu</strong></td>
<td><strong>4 Port</strong></td>
<td><strong>8 Port</strong></td>
<td><strong>12 Port</strong></td>
<td>Donanım / NIC kapasitesi kadar</td>
</tr>
<tr>
<td><strong>SFP+ Fiber Yuvası</strong></td>
<td>Yok</td>
<td><code>2x SFP+</code> (Port 1–2)</td>
<td><code>4x SFP+</code> (Port 1–4)</td>
<td>Desteklenir (<code>ix</code>, <code>ixl</code> vb.)</td>
</tr>
<tr>
<td><strong>RJ45 Bakır Port</strong></td>
<td><code>4x RJ45</code> (Port 0–3)</td>
<td><code>6x RJ45</code> (Port 3–8)</td>
<td><code>8x RJ45</code> (Port 5–12)</td>
<td>Desteklenir (<code>igb</code>, <code>em</code>, <code>vtnet</code>)</td>
</tr>
<tr>
<td><strong>Konsol ve Kurulum</strong></td>
<td>USB-C Seri + USB 3.0 (<code>.img</code>)</td>
<td>USB-C Seri + USB 3.0 (<code>.img</code>)</td>
<td>RJ45 Seri + USB 3.0 (<code>.img</code> / <code>.iso</code>)</td>
<td>VGA <code>.iso</code> veya Seri <code>.img</code></td>
</tr>
<tr>
<td><strong>Geri Alma Güvencesi</strong></td>
<td>120 sn Safe-Apply + OpenZFS BE</td>
<td>120 sn Safe-Apply + OpenZFS BE</td>
<td>120 sn Safe-Apply + OpenZFS BE</td>
<td>120 sn Safe-Apply + OpenZFS BE</td>
</tr>
<tr>
<td><strong>Yazılım ve UTM Lisansı</strong></td>
<td><span class="hw-price-pill">$0 / Yıl — Tüm Modüller Açık</span></td>
<td><span class="hw-price-pill">$0 / Yıl — Tüm Modüller Açık</span></td>
<td><span class="hw-price-pill">$0 / Yıl — Tüm Modüller Açık</span></td>
<td><span class="hw-price-pill">$0 / Yıl — Tüm Modüller Açık</span></td>
</tr>
</tbody>
</table>
</div>
<div class="hw-cta-dark">
<div>
<h3>Kendi Donanımınıza Hemen Kurun veya Boyutlandırma İçin Ulaşın</h3>
<p>Security Guard, Çok Üreticili S2S VPN, Misafir Captive Portal ve Yerleşik Yapay Zeka Asistanı dahil tüm yetenekler ücretsiz <code>amd64</code> kurulum imajında seri numarası kilidi olmadan hazır gelir.</p>
</div>
<div class="hw-cta-actions">
<a href="/tr/download/" class="hw-btn-primary">ISO ve Serial IMG İndir →</a>
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="hw-btn-secondary">Canlı Demoyu Dene</a>
<a href="/tr/company/contact/" class="hw-btn-secondary">Donanım ve Kurumsal İletişim</a>
</div>
</div>
</div>
