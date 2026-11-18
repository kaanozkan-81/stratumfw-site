---
title: "Security Guard"
seoTitle: "Stratum Security Guard: Ücretsiz 5 Katmanlı Tehdit Savunması"
description: "Stratum FW içine yerleşik katmanlı tehdit savunması: 63 IP listesi, 5-RIR GeoIP, 93 DNSBL, 56 kategorili Web Filtresi ve Suricata IPS. Aboneliksiz ve ücretsiz."
seoDescription: "Stratum Security Guard: 63 çekirdek pf IP listesi, 5-RIR GeoIP, 93 Unbound DNSBL, 56 UT1 Web Filtresi kategorisi ve fail-open Suricata IPS ile yıllık 0 $ koruma."
date: 2026-11-18
translationKey: "product-security-guard"
aliases: ["/tr/security-guard/"]
---

<style>
.sg-wrap { display:flex; flex-direction:column; gap:2.25rem; margin:1.5rem 0 2.5rem; }
.sg-dark { background:#08101f !important; border:1px solid #1e293b !important; border-radius:18px !important; padding:2rem !important; box-shadow:0 22px 48px -12px rgba(8,16,31,0.45) !important; }
.sg-eyebrow { display:inline-block !important; font-size:0.74rem !important; font-weight:700 !important; letter-spacing:0.09em !important; text-transform:uppercase !important; color:#fbbf24 !important; background:rgba(245,158,11,0.12) !important; border:1px solid rgba(245,158,11,0.32) !important; padding:0.28rem 0.7rem !important; border-radius:999px !important; margin-bottom:0.75rem !important; }
.sg-dark h2 { color:#f8fafc !important; font-size:clamp(1.45rem,2.2vw,1.85rem) !important; line-height:1.25 !important; margin:0 0 0.65rem 0 !important; font-weight:800 !important; }
.sg-lead { color:#cbd5e1 !important; font-size:0.96rem !important; line-height:1.65 !important; margin:0 0 1.4rem 0 !important; max-width:920px !important; }
.sg-kpis { display:grid !important; grid-template-columns:repeat(4,minmax(0,1fr)) !important; gap:0.9rem !important; margin-bottom:1.5rem !important; }
.sg-kpi { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:12px !important; padding:0.95rem 1rem !important; }
.sg-kpi strong { display:block !important; font-size:1.45rem !important; font-weight:800 !important; color:#f8fafc !important; line-height:1.15 !important; }
.sg-kpi span { display:block !important; font-size:0.78rem !important; color:#94a3b8 !important; margin-top:0.25rem !important; }
.sg-fig { margin:0 !important; border-radius:14px !important; overflow:hidden !important; border:1px solid #1e293b !important; background:#050b16 !important; }
.sg-fig img { display:block !important; width:100% !important; height:auto !important; }
.sg-fig figcaption { padding:0.65rem 1rem !important; font-size:0.8rem !important; color:#94a3b8 !important; text-align:center !important; border-top:1px solid #1e293b !important; }
.sg-layers { display:grid !important; grid-template-columns:repeat(2,minmax(0,1fr)) !important; gap:1.15rem !important; }
.sg-card { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.35rem 1.4rem !important; }
.sg-card-wide { grid-column:1 / -1 !important; }
.sg-badge { display:inline-block !important; font-size:0.7rem !important; font-weight:700 !important; letter-spacing:0.06em !important; text-transform:uppercase !important; color:#38bdf8 !important; margin-bottom:0.4rem !important; }
.sg-card h3 { color:#f8fafc !important; font-size:1.12rem !important; font-weight:700 !important; margin:0 0 0.65rem 0 !important; }
.sg-card ul { margin:0 !important; padding-left:1.15rem !important; display:flex !important; flex-direction:column !important; gap:0.45rem !important; }
.sg-card li { color:#cbd5e1 !important; font-size:0.87rem !important; line-height:1.55 !important; margin:0 !important; }
.sg-card li strong { color:#f8fafc !important; font-weight:700 !important; }
.sg-card code { background:#1e293b !important; color:#7dd3fc !important; padding:0.1rem 0.35rem !important; border-radius:4px !important; font-size:0.8rem !important; }
.sg-split { display:grid !important; grid-template-columns:minmax(0,1.05fr) minmax(0,0.95fr) !important; gap:1.5rem !important; align-items:center !important; }
.sg-why-list { display:flex !important; flex-direction:column !important; gap:0.75rem !important; }
.sg-why-item { background:#0f172a !important; border:1px solid #1e293b !important; border-left:3px solid #f59e0b !important; border-radius:10px !important; padding:0.9rem 1.05rem !important; }
.sg-why-item strong { display:block !important; color:#f8fafc !important; font-size:0.92rem !important; margin-bottom:0.2rem !important; }
.sg-why-item p { color:#cbd5e1 !important; font-size:0.84rem !important; line-height:1.5 !important; margin:0 !important; }
.sg-grid3 { display:grid !important; grid-template-columns:repeat(3,minmax(0,1fr)) !important; gap:1.1rem !important; margin-top:1.4rem !important; }
.sg-cta-row { display:flex !important; flex-wrap:wrap !important; gap:0.75rem !important; margin-top:1.25rem !important; }
.sg-btn-pri { display:inline-flex !important; align-items:center !important; padding:0.65rem 1.25rem !important; border-radius:8px !important; background:#f59e0b !important; color:#0f172a !important; font-weight:700 !important; font-size:0.86rem !important; text-decoration:none !important; }
.sg-btn-sec { display:inline-flex !important; align-items:center !important; padding:0.65rem 1.2rem !important; border-radius:8px !important; background:#1e293b !important; color:#f8fafc !important; border:1px solid #334155 !important; font-weight:600 !important; font-size:0.86rem !important; text-decoration:none !important; }
@media (max-width:960px) {
.sg-kpis, .sg-layers, .sg-split, .sg-grid3 { grid-template-columns:1fr !important; }
.sg-dark { padding:1.35rem !important; }
}
</style>
<div class="sg-wrap">
<section class="sg-dark">
<span class="sg-eyebrow">ÇOK KATMANLI TEHDİT SAVUNMASI &bull; YILLIK 0 $ ABONELİK</span>
<h2>Süresi Asla Dolmayan Katmanlı Ağ Savunması</h2>
<p class="sg-lead">Stratum Firewall ağınızı birbirini tamamlayan beş savunma katmanıyla korur. Her katman farklı bir tehdit türünü daha iç ağa ulaşmadan durdurur, tamamı tek bir yönetim panelinden kontrol edilir ve hiçbir yıllık abonelik, özellik anahtarı veya ücretli güvenlik paketi gerektirmeden yerleşik olarak gelir.</p>
<div class="sg-kpis">
<div class="sg-kpi"><strong>63 Liste</strong><span>Çekirdek pf Çift Yönlü IP Engelleme</span></div>
<div class="sg-kpi"><strong>5 RIR</strong><span>Lisanssız Haftalık Ülke Bazlı GeoIP</span></div>
<div class="sg-kpi"><strong>93 + 56</strong><span>Unbound DNSBL ve UT1 Web Kategorisi</span></div>
<div class="sg-kpi"><strong>52 Kural Seti</strong><span>Suricata ET Open IPS &bull; Fail-Open netmap</span></div>
</div>
<figure class="sg-fig">
<img src="/images/security-guard-layers.jpg" alt="Stratum Security Guard 5 Katmanli Derinlemesine Savunma Mimarisi" loading="eager" width="1280" height="720">
<figcaption>Her katman kendi uzmanlık alanındaki tehdidi eler: çekirdek pf tablolarının düşürdüğü paketler DNS veya derin paket analizi (DPI) işlemcisini hiç yormaz.</figcaption>
</figure>
</section>
<section class="sg-dark">
<span class="sg-eyebrow">SAVUNMA MİMARİSİ &bull; BEŞ AKTİF KALKAN</span>
<h2>Security Guard Katmanları Nasıl Çalışır?</h2>
<p class="sg-lead">Çekirdek seviyesinde hat hızında IP engellemeden istemcisiz IoT DNS korumasına ve satır içi Suricata analizine kadar tüm motorlar açık tehdit istihbaratı kaynaklarından otomatik güncellenir; izin listeleriniz ise her zaman mutlak önceliğe sahiptir.</p>
<div class="sg-layers">
<div class="sg-card">
<span class="sg-badge">KATMAN 01 &bull; GÜVENLİK DUVARI &rarr; ENGELLEME LİSTELERİ</span>
<h3>IP Engelleme Listeleri (63 Hazır Kaynak)</h3>
<ul>
<li><strong>Bilinen saldırganları sınırda düşürür:</strong> Botnet komuta-kontrol (C2) sunucularını, port tarayıcılarını, kaba kuvvet saldırganlarını ve ele geçirilmiş IP adreslerini daha servislerinize ulaşmadan engeller.</li>
<li><strong>Zengin katalog ve özel URL desteği:</strong> Tek katalogda 63 hazır liste (<code>abuse.ch Feodo Tracker</code>, <code>Spamhaus DROP</code>, <code>CINS Army</code>, <code>Emerging Threats</code>, <code>blocklist.de</code>, <code>SANS ISC</code>, <code>GreenSnow</code> vb.) ve kendi URL listeleriniz.</li>
<li><strong>Çift yönlü koruma:</strong> Dışarıdan gelen saldırılar WAN kapısında düşürülür; içerideki bir cihaz zararlı bir C2 adresine çıkmaya çalıştığında anında durdurularak enfekte cihazların erken tespitini sağlar.</li>
<li><strong>Sıkı ağlar için ek listeler:</strong> Tor çıkış düğümleri, açık vekil sunucular (proxy), VPN servisleri, DNS-over-HTTPS (DoH) sunucuları ve torrent izleyicileri için isteğe bağlı listeler.</li>
<li><strong>Kesintisiz ve hata korumalı:</strong> Yeniden başlatma gerektirmeden doğrudan çekirdek <code>pf</code> tablolarına yüklenir; izin listesi her zaman kazanır, yerel özel IP aralıkları asla engellenmez ve indirme hatası olursa son sağlam liste korunur.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">KATMAN 02 &bull; DOĞRUDAN 5 RIR VERİSİ</span>
<h3>GeoIP Ülke Kuralları</h3>
<ul>
<li><strong>Kurallarda doğrudan ülke seçimi:</strong> Dışarıya açık bir servisinize yalnızca Türkiye'den erişim izni verin veya kurumunuzla hiçbir iş ilişkisi olmayan bölgeleri tek kuralda kapatın.</li>
<li><strong>Hesap veya lisans anahtarı gerektirmez:</strong> Ülke IP blokları doğrudan beş bölgesel internet kayıt kuruluşundan (<code>ARIN</code>, <code>RIPE NCC</code>, <code>APNIC</code>, <code>LACNIC</code>, <code>AFRINIC</code>) çekilir ve her hafta otomatik güncellenir.</li>
</ul>
<span class="sg-badge" style="margin-top:1rem;">KATMAN 03 &bull; YERLEŞİK UNBOUND RPZ</span>
<h3>DNS Engelleme Listeleri (DNSBL &bull; 93 Liste)</h3>
<ul>
<li><strong>Ağdaki her cihaz için istemcisiz koruma:</strong> Telefonlar, akıllı TV'ler ve IoT cihazları dahil tüm istemcilerde reklam, izleyici, zararlı yazılım, oltalama (phishing) ve kripto madenciliği alan adlarını hiçbir uygulama kurmadan durdurur.</li>
<li><strong>93 liste ve alt alan adı kapsaması:</strong> <code>EasyList</code>, <code>AdGuard</code>, <code>StevenBlack</code>, <code>HaGeZi</code>, <code>OpenPhish</code>, <code>PhishTank</code> ve <code>NoCoin</code> kataloglarını içerir; engellenen alan adının tüm alt alan adları da otomatik engellenir.</li>
<li><strong>Ek sunucu maliyeti yok:</strong> Doğrudan güvenlik duvarının kendi <code>Unbound</code> çözümleyicisi içinde gecikmesiz çalışır.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">KATMAN 04 &bull; AĞ BAZLI İÇERİK POLİTİKASI</span>
<h3>Web Filtresi (56 UT1 Kategorisi ve Zorunlu DNS)</h3>
<ul>
<li><strong>Her VLAN için ayrı politika:</strong> Misafir Wi-Fi ağında sıkı filtreleme, personel ağında kurumsal kurallar, sunucu VLAN'ında ise engelsiz erişim tanımlayın.</li>
<li><strong>Günlük güncellenen 56 kategori:</strong> Universit&eacute; Toulouse Capitole (<code>UT1</code>) listeleriyle yetişkin içerik, kumar, zararlı yazılım, oltalama, sosyal medya, yayın, oyun ve VPN/proxy aşma servislerini kapsar.</li>
<li><strong>Güvenli Arama ve Zorunlu DNS:</strong> Google, Bing, DuckDuckGo ve YouTube için kısıtlı modu zorlar; kendi DNS sunucusunu (<code>8.8.8.8</code> vb.) kullanan cihazları yerel çözümleyiciye yönlendirir ve DNS-over-TLS (<code>853</code>) kaçışlarını engellerken tanımlı istisna siteleri korur.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">KATMAN 05 &bull; DERİN PAKET ANALİZİ</span>
<h3>Saldırı Tespit ve Önleme (Suricata IPS/IDS)</h3>
<ul>
<li><strong>52 Emerging Threats Open kural kategorisi:</strong> İstismar girişimlerini (exploit), zararlı yazılım bağlantılarını ve protokol anomalilerini zamanlanmış otomatik kural güncellemeleriyle denetler.</li>
<li><strong>Arayüz bazlı İzle veya Engelle:</strong> Önce <strong>İzle (Watch)</strong> kipinde ağınızın normal davranışını öğrenin, ardından yalnızca koruma kattığı arayüzlerde satır içi <code>netmap</code> <strong>Engelle (Block)</strong> kipini açın.</li>
<li><strong>Fail-open sürekliliği ve tek tıkla kural susturma:</strong> Suricata durursa trafik kesilmek yerine akmaya devam eder (fail-open); yanlış alarm üreten tekil bir kuralı tek tıkla kapatabilirsiniz ve bu tercih güncellemelerde korunur.</li>
</ul>
</div>
</div>
</section>
<section class="sg-dark">
<div class="sg-split">
<div>
<span class="sg-eyebrow">SIFIR LİSANS VERGİSİ &bull; NEDEN ÜCRETSİZ?</span>
<h2>Security Guard Neden Ücretsizdir ve Neden Tercih Edilir?</h2>
<p class="sg-lead" style="margin-bottom:1rem;">Ticari UTM üreticileri Web Filtreleme, DNS koruması ve IPS imzalarını her yıl döviz bazında yenilenen abonelik paketlerine bağlar; sözleşme bittiği gün koruma durur. Stratum FW ise küresel güvenlik topluluğunun açık tehdit istihbaratı kaynaklarını doğrudan kendi donanımınızda derleyerek aracı maliyetini ortadan kaldırır.</p>
<div class="sg-why-list">
<div class="sg-why-item"><strong>Yerleşik ve Süresiz Ücretsiz (0 $/Yıl)</strong><p>Tüm savunma katmanları işletim sisteminin parçasıdır; abonelik, özellik lisansı veya süresi dolan tehdit beslemesi yoktur.</p></div>
<div class="sg-why-item"><strong>Yanlış Alarmlara Karşı Tasarlandı</strong><p>Engellemeden önce izleme, tek tıkla kural istisnası, her zaman öncelikli izin listesi ve indirme hatasında son sağlam listeyi koruma mimarisi ağı bozmadan korur.</p></div>
<div class="sg-why-item"><strong>Tek Panel, Tek Yedek, Tek Yetki Modeli</strong><p>Her katman için ayrı eklenti veya ek sanal makine kurmaya gerek kalmaz; aynı yedekleme, RBAC ve bildirim altyapısını paylaşır.</p></div>
<div class="sg-why-item"><strong>Üretim Ortamında Kanıtlanmış Mimari</strong><p>Segmente edilmiş VLAN'lar, IPTV yayını, yoğun misafir hotspot ağı ve personel ağlarını barındıran gerçek otel üretim altyapısında kesintisiz çalışmaktadır.</p></div>
</div>
</div>
<figure class="sg-fig">
<img src="/images/security-guard-sources.jpg" alt="Stratum Security Guard Acik Tehdit Istihbarati Kaynaklari ve Ucretsiz Model" loading="lazy" width="1200" height="900">
<figcaption>Açık ve akademik tehdit istihbaratı kaynakları hata korumalı olarak doğrudan cihaz üzerinde derlenir.</figcaption>
</figure>
</div>
</section>
<section class="sg-dark">
<span class="sg-eyebrow">GÖRÜNÜRLÜK, UYARILAR VE ÖZ KORUMA</span>
<h2>Nelerin Durdurulduğunu Görün — Kendini de Koruyan Güvenlik Duvarı</h2>
<p class="sg-lead">Tehditleri sessizce engellemek işin yalnızca yarısıdır. Security Guard; hangi listenin neyi durdurduğunu, 30 günlük VLAN geçiş matrisini ve anlık Telegram/e-posta uyarılarını sunarken güvenlik duvarının kendisini de çok katmanlı olarak korur.</p>
<figure class="sg-fig">
<img src="/images/security-guard-visibility.jpg" alt="Stratum Security Guard Birlesik Engelleme Raporu, 30 Gunluk VLAN Matrisi ve Telegram Uyarilari" loading="lazy" width="1200" height="900">
<figcaption>Birleşik Engelleme Raporu, 30 günlük Ağlar Arası VLAN matrisi ve anlık Telegram / e-posta bildirimleri.</figcaption>
</figure>
<div class="sg-grid3">
<div class="sg-card">
<span class="sg-badge">KANIT VE DENETİM</span>
<h3>Durdurulan Trafiği İzleyin</h3>
<ul>
<li><strong>Engellenenler Raporu:</strong> IP listelerinin, DNSBL'in ve Web Filtresinin neyi durdurduğunu ve hangi listenin eşleştiğini tek ekranda gösterir.</li>
<li><strong>Ağlar Arası (30 Günlük Matris):</strong> VLAN'ları güvenle ayırmak ve ayrım sonrasında nelerin kesildiğini doğrulamak için 30 günlük iç ağ erişim matrisi.</li>
<li><strong>Denetimli Web Aktivitesi:</strong> DHCP geçmişiyle eşleştirilmiş cihaz bazlı sorgu kayıtları; yalnızca yöneticilere açıktır ve her inceleme kayıt altına alınır.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">ANLIK BİLDİRİMLER</span>
<h3>Telegram ve E-posta Uyarıları</h3>
<ul>
<li><strong>Güvenlik olayları:</strong> Yüksek önem dereceli Suricata saldırı alarmları ve başarısız giriş denemeleri sonrası engellenen IP adresleri.</li>
<li><strong>Operasyonel süreklilik:</strong> WAN hattının düşmesi, başarısız tehdit listesi indirmeleri, geri alınan yapılandırmalar ve yedekleme hataları.</li>
<li><strong>Sistem sağlığı:</strong> Yeni yazılım güncellemeleri, disk doluluk eşiği, CPU sıcaklığı ve TLS sertifika süresi uyarıları.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">ÖZ KORUMA MİMARİSİ</span>
<h3>Kendini Koruyan Kontrol Katmanı</h3>
<ul>
<li><strong>Yerel TOTP 2FA ve Giriş Koruması:</strong> Cihaz üzerinde üretilen QR kodlu 2FA ve 5 dakikada 5 hatalı denemede 5 dakikalık otomatik IP engeli.</li>
<li><strong>Kapalı WAN Erişimi ve İzole Anahtarlar:</strong> Panel ve SSH varsayılan olarak WAN'a kapalıdır; parolalar ve VPN anahtarları <code>0600</code> izinli dosyalarda tutulur ve tarayıcıya gönderilmez.</li>
<li><strong>Safe-Apply ve İmzalı Güncellemeler:</strong> Onaylanmazsa otomatik geri alınan değişiklikler ve ZFS önyükleme ortamı korumalı imzalı paketler.</li>
</ul>
</div>
</div>
<div class="sg-cta-row">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="sg-btn-pri">Canlı Demoda Security Guard'ı İncele &rarr;</a>
<a href="/tr/download/" class="sg-btn-sec">Ücretsiz v0.34.2 ISO / IMG İndir</a>
<a href="/tr/product/security/" class="sg-btn-sec">Kriptografi ve Güncelleme Doğrulaması</a>
</div>
</section>
</div>
