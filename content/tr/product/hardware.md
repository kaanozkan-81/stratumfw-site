---
title: "Donanım ve Cihaz Profilleri"
seo_title: "Stratum FW Donanım Profilleri — 4 Port, 8 Port SFP+ ve 12 Port 1U Rack Güvenlik Duvarı"
description: "Stratum FW 4 portlu fansız masaüstü, 8 portlu (2x SFP+ + 6x RJ45) şube ve 12 portlu (4x SFP+ + 8x RJ45) 1U rack tipi güvenlik duvarı profillerini inceleyin."
lead: "Sessiz 4 portlu masaüstü kutulardan 8 portlu SFP+ şube geçitlerine ve 12 portlu 1U kabinet cihazlarına kadar sıfır donanım bağımlılığı."
images: ["/images/hardware-stratum-family.jpg"]
---

<style>
.container *:has(.sg-wrap) { max-width: 100% !important; width: 100% !important; }
.hw-wrap { width: 100%; max-width: 1240px; margin: 0 auto; color: #f8fafc !important; }
.hw-hero { background: linear-gradient(145deg, #08101f 0%, #0f172a 100%) !important; border: 1px solid rgba(245, 158, 11, 0.32) !important; border-radius: 20px; padding: 34px; margin-bottom: 32px; box-shadow: 0 20px 45px rgba(2, 6, 23, 0.22); }
.hw-eyebrow { display: inline-block; background: rgba(245, 158, 11, 0.14) !important; color: #fbbf24 !important; border: 1px solid rgba(245, 158, 11, 0.38) !important; border-radius: 999px; padding: 5px 14px; font-size: 0.78rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 14px; }
.hw-hero h2 { color: #f8fafc !important; font-size: clamp(1.75rem, 3vw, 2.4rem); line-height: 1.2; margin: 0 0 12px 0 !important; font-weight: 800; }
.hw-hero p { color: #cbd5e1 !important; font-size: 1.04rem; line-height: 1.68; max-width: 920px; margin: 0 0 24px 0 !important; }
.hw-family-img { width: 100%; border-radius: 14px; border: 1px solid rgba(148, 163, 184, 0.22); display: block; margin-bottom: 20px; }
.hw-pill-row { display: flex; flex-wrap: wrap; gap: 10px; }
.hw-pill { background: #08101f !important; border: 1px solid #1e293b !important; color: #e2e8f0 !important; border-radius: 999px; padding: 7px 15px; font-size: 0.86rem; font-weight: 600; }
.hw-pill strong { color: #fbbf24 !important; }
.hw-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 24px; margin-bottom: 36px; }
@media (max-width: 980px) { .hw-grid { grid-template-columns: 1fr; } }
.hw-card { background: #0f172a !important; border: 1px solid #1e293b !important; border-radius: 18px; overflow: hidden; display: flex; flex-direction: column; box-shadow: 0 14px 32px rgba(2, 6, 23, 0.16); }
.hw-card-featured { border-color: rgba(245, 158, 11, 0.5) !important; }
.hw-card img { width: 100%; aspect-ratio: 16 / 9; object-fit: cover; display: block; border-bottom: 1px solid #1e293b; }
.hw-card-body { padding: 24px; display: flex; flex-direction: column; flex: 1; }
.hw-tier { color: #fbbf24 !important; font-size: 0.76rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 6px; }
.hw-card h3 { color: #f8fafc !important; font-size: 1.32rem; margin: 0 0 10px 0 !important; font-weight: 700; }
.hw-port-badges { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 14px; }
.hw-badge-amber { background: rgba(245, 158, 11, 0.15) !important; color: #fbbf24 !important; border: 1px solid rgba(245, 158, 11, 0.35) !important; border-radius: 8px; padding: 4px 10px; font-size: 0.8rem; font-weight: 700; }
.hw-badge-cyan { background: rgba(56, 189, 248, 0.14) !important; color: #7dd3fc !important; border: 1px solid rgba(56, 189, 248, 0.32) !important; border-radius: 8px; padding: 4px 10px; font-size: 0.8rem; font-weight: 700; }
.hw-card p { color: #cbd5e1 !important; font-size: 0.94rem; line-height: 1.62; margin: 0 0 14px 0 !important; }
.hw-spec-list { list-style: none !important; padding: 0 !important; margin: auto 0 0 0 !important; border-top: 1px solid #1e293b; padding-top: 14px !important; }
.hw-spec-list li { color: #cbd5e1 !important; font-size: 0.88rem; line-height: 1.55; margin-bottom: 7px; padding-left: 16px; position: relative; }
.hw-spec-list li::before { content: "•"; color: #fbbf24 !important; position: absolute; left: 0; font-weight: 700; }
.hw-table-box { background: #0f172a !important; border: 1px solid #1e293b !important; border-radius: 18px; padding: 28px; margin-bottom: 32px; overflow-x: auto; }
.hw-table-box h3 { color: #f8fafc !important; margin: 0 0 16px 0 !important; font-size: 1.35rem; }
.hw-table { width: 100%; border-collapse: collapse; min-width: 680px; }
.hw-table th { text-align: left; padding: 12px 14px; background: #08101f !important; color: #fbbf24 !important; font-size: 0.83rem; text-transform: uppercase; letter-spacing: 0.05em; border-bottom: 1px solid #334155; }
.hw-table td { padding: 12px 14px; color: #e2e8f0 !important; font-size: 0.92rem; border-bottom: 1px solid #1e293b; }
.hw-table tr:last-child td { border-bottom: none; }
.hw-table code { background: #08101f !important; color: #7dd3fc !important; padding: 2px 6px; border-radius: 5px; font-size: 0.84rem; }
.hw-cta { background: linear-gradient(135deg, #08101f 0%, #0f172a 100%) !important; border: 1px solid rgba(56, 189, 248, 0.3) !important; border-radius: 18px; padding: 28px 32px; display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 20px; }
.hw-cta h3 { color: #f8fafc !important; margin: 0 0 6px 0 !important; font-size: 1.28rem; }
.hw-cta p { color: #cbd5e1 !important; margin: 0 !important; font-size: 0.95rem; max-width: 680px; }
.hw-cta-btns { display: flex; flex-wrap: wrap; gap: 12px; }
.hw-btn-amber { background: #f59e0b !important; color: #08101f !important; font-weight: 700; padding: 11px 20px; border-radius: 10px; text-decoration: none !important; font-size: 0.93rem; display: inline-block; }
.hw-btn-outline { background: rgba(255,255,255,0.06) !important; color: #f8fafc !important; border: 1px solid rgba(148,163,184,0.35) !important; font-weight: 600; padding: 11px 20px; border-radius: 10px; text-decoration: none !important; font-size: 0.93rem; display: inline-block; }
</style>
<div class="sg-wrap hw-wrap">
<div class="hw-hero">
<span class="hw-eyebrow">Stratum FW Donanım Ailesi • Sıfır Donanım Bağımlılığı</span>
<h2>4 Portlu Masaüstü, 8 Portlu Fiber Şube ve 12 Portlu 1U Rack Profilleri</h2>
<p>Saf FreeBSD 15.1-RELEASE gücü, durum bilgili <code style="color:#7dd3fc !important;">pf</code>, OpenZFS Önyükleme Ortamları ve <code style="color:#7dd3fc !important;">115200 8N1</code> seri konsol kurtarma mimarisi için tasarlandı. Stratum FW'yi ister fansız kompakt alüminyum kasalarda, ister SFP+ fiber şube ve 1U rack cihazlarında, ister kendi mevcut <code style="color:#7dd3fc !important;">amd64</code> sunucu ve sanallaştırma altyapınızda <strong style="color:#fbbf24 !important;">$0/yıl</strong> maliyetle çalıştırın.</p>
<img src="/images/hardware-stratum-family.jpg" alt="Stratum FW 4, 8 ve 12 Portlu Donanım Ailesi" class="hw-family-img">
<div class="hw-pill-row">
<span class="hw-pill"><strong>SFW-400:</strong> 4x RJ45 Fansız Masaüstü</span>
<span class="hw-pill"><strong>SFW-800:</strong> 2x SFP+ ve 6x RJ45 Şube (8 Port)</span>
<span class="hw-pill"><strong>SFW-1200:</strong> 4x SFP+ ve 8x RJ45 1U Rack (12 Port)</span>
<span class="hw-pill"><strong>Kendi Donanımınız:</strong> Tüm x86-64, Proxmox, KVM ve VMware</span>
</div>
</div>
<div class="hw-grid">
<div class="hw-card">
<img src="/images/hardware-stratum-4port.jpg" alt="Stratum FW 4 Portlu Fansız Masaüstü Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">Kompakt Fansız Masaüstü • 4 Port</div>
<h3>Stratum FW 4 Portlu Profil</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">4x RJ45 Ethernet</span>
<span class="hw-badge-cyan">USB-C Konsol + USB 3.0</span>
</div>
<p>Ev laboratuvarları, küçük ofisler, klinikler ve mağaza noktaları için sessiz pasif soğutmalı CNC alüminyum gövde; çift internet yedekliliği ve güvenli uzak erişim sunar.</p>
<ul class="hw-spec-list">
<li><strong>Arayüzler:</strong> <code style="color:#7dd3fc !important;">0 – 3</code> RJ45 portlarıyla WAN1, WAN2 (4G/5G DHCP), LAN ve izole Misafir Wi-Fi</li>
<li><strong>Dahil Özellikler:</strong> 5 katmanlı Security Guard, Misafir Captive Portal, WireGuard ve OpenVPN</li>
<li><strong>Kurulum Medyası:</strong> OpenZFS üzerinde Seri Konsol <code style="color:#7dd3fc !important;">.img</code> veya VGA <code style="color:#7dd3fc !important;">.iso</code></li>
</ul>
</div>
</div>
<div class="hw-card hw-card-featured">
<img src="/images/hardware-stratum-8port.jpg" alt="Stratum FW 8 Portlu (2x SFP+ ve 6x RJ45) Şube Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">Fiber Şube ve KOBİ Ağ Geçidi • 8 Port</div>
<h3>Stratum FW 8 Portlu Profil</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">2x SFP+ Fiber</span>
<span class="hw-badge-cyan">6x RJ45 Ethernet</span>
</div>
<p>İki adet SFP+ fiber yuvasını (<code style="color:#7dd3fc !important;">1 – 2</code>) altı adet bakır RJ45 portuyla (<code style="color:#7dd3fc !important;">3 – 8</code>) birleştiren geniş fansız soğutucu gövde; çoklu VLAN ve şubeler arası VPN merkezleri için idealdir.</p>
<ul class="hw-spec-list">
<li><strong>Fiber Omurga:</strong> Doğrudan fiber sonlandırma veya omurga anahtarlara 10G <code style="color:#7dd3fc !important;">802.1Q</code> VLAN bağlantısı</li>
<li><strong>Şubeler Arası VPN:</strong> Politika tabanlı IPsec (<code style="color:#7dd3fc !important;">IKEv2/IKEv1</code>) ve çekirdek WireGuard S2S (<code style="color:#7dd3fc !important;">wg1+</code>)</li>
<li><strong>Satır İçi Koruma:</strong> Suricata 8 <code style="color:#7dd3fc !important;">netmap</code> IPS ve <code style="color:#7dd3fc !important;">dummynet</code> cihaz başı adil bant sınırı</li>
</ul>
</div>
</div>
<div class="hw-card">
<img src="/images/hardware-stratum-12port-1u.jpg" alt="Stratum FW 12 Portlu (4x SFP+ ve 8x RJ45) 1U Rack Güvenlik Duvarı">
<div class="hw-card-body">
<div class="hw-tier">19 İnç 1U Rack Tipi • 12 Port</div>
<h3>Stratum FW 12 Portlu 1U Profil</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">4x SFP+ Fiber</span>
<span class="hw-badge-cyan">8x RJ45 Ethernet</span>
</div>
<p>Dört adet SFP+ fiber yuvası (<code style="color:#7dd3fc !important;">1 – 4</code>), sekiz adet bakır RJ45 portu (<code style="color:#7dd3fc !important;">5 – 12</code>) ve ön panel RJ45 seri konsoluyla kampüsler, oteller ve merkez ofisler için yüksek port yoğunluğu.</p>
<ul class="hw-spec-list">
<li><strong>Yüksek Yoğunluklu Yönlendirme:</strong> Çoklu fiber WAN, <code style="color:#7dd3fc !important;">/29</code> Sanal IP, <code style="color:#7dd3fc !important;">1:1 NAT</code> ve 30 günlük VLAN matrisi</li>
<li><strong>Kampüs ve Konaklama:</strong> Çok bölgeli Misafir Captive Portal ve 56 kategorili UT1 Web Filtresi</li>
<li><strong>Yapay Zeka Tanılama:</strong> Yerel LAN <code style="color:#7dd3fc !important;">Ollama</code> ve <code style="color:#7dd3fc !important;">PUB-1</code> maskelemeli 15 araçlı salt okunur AI Asistanı</li>
</ul>
</div>
</div>
</div>
<div class="hw-table-box">
<h3>Donanım Port ve Yetenek Karşılaştırma Tablosu</h3>
<table class="hw-table">
<thead>
<tr>
<th>Özellik</th>
<th>4 Portlu Masaüstü</th>
<th>8 Portlu Şube</th>
<th>12 Portlu 1U Rack</th>
<th>Kendi x86-64 / VM Sunucunuz</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Kasa Tipi</strong></td>
<td>Kompakt Fansız Masaüstü</td>
<td>Geniş Fansız Masaüstü</td>
<td>19 inç 1U Kabinet (Rack)</td>
<td>Mini-PC, Sunucu, Proxmox, KVM</td>
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
<td>—</td>
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
<td><strong style="color:#fbbf24 !important;">$0 / Yıl</strong> (Tüm modüller açık)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Yıl</strong> (Tüm modüller açık)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Yıl</strong> (Tüm modüller açık)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Yıl</strong> (Tüm modüller açık)</td>
</tr>
</tbody>
</table>
</div>
<div class="hw-cta">
<div>
<h3>Kendi Donanımınıza Hemen Kurun veya Donanım Boyutlandırma İçin Ulaşın</h3>
<p>Security Guard, Çok Üreticili VPN, Misafir Captive Portal ve Yerleşik Yapay Zeka Asistanı dahil tüm özellikler seri numarası kilidi olmadan ücretsiz <code style="color:#7dd3fc !important;">amd64</code> imajında hazır gelir.</p>
</div>
<div class="hw-cta-btns">
<a href="/tr/download/" class="hw-btn-amber">ISO ve Serial IMG İndir →</a>
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="hw-btn-outline">Canlı Demoyu Dene</a>
<a href="/tr/company/contact/" class="hw-btn-outline">Donanım ve Kurumsal İletişim</a>
</div>
</div>
</div>
