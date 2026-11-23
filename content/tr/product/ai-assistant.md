---
title: "Yapay Zeka Asistanı ve MCP"
seo_title: "Stratum Firewall Yapay Zeka Asistanı: Salt Okunur MCP ve Ollama"
description: "Tanılama > Yapay Zeka Asistanı ile güvenlik duvarınıza doğal dilde sorular sorun. 16 salt okunur MCP aracı, genel IP maskeleme ve yerel Ollama desteği."
seoDescription: "16 salt okunur MCP aracı, otomatik PUB-1 IP maskeleme, sıfır kural bozma riski ve %100 yerel Ollama veya Claude/OpenAI/DeepSeek/Gemini desteği."
date: 2026-11-18
translationKey: "product-ai-assistant"
aliases: ["/tr/ai-assistant/"]
---

<style>
.container *:has(.ai-wrap), main article:has(.ai-wrap), .prose:has(.ai-wrap), .content:has(.ai-wrap), .markdown:has(.ai-wrap), .single-content:has(.ai-wrap), .article-content:has(.ai-wrap), .page-content:has(.ai-wrap), .post-content:has(.ai-wrap) { max-width:100% !important; width:100% !important; margin-left:0 !important; margin-right:0 !important; }
.ai-wrap { width:100% !important; max-width:1240px !important; margin:1.5rem auto 3rem auto !important; display:flex !important; flex-direction:column !important; gap:2.5rem !important; }
.ai-dark { width:100% !important; box-sizing:border-box !important; background:#08101f !important; border:1px solid #1e293b !important; border-radius:20px !important; padding:2.5rem !important; box-shadow:0 24px 52px -12px rgba(8,16,31,0.48) !important; }
.ai-eyebrow { display:inline-block !important; font-size:0.75rem !important; font-weight:700 !important; letter-spacing:0.09em !important; text-transform:uppercase !important; color:#34d399 !important; background:rgba(16,185,129,0.12) !important; border:1px solid rgba(52,211,153,0.35) !important; padding:0.3rem 0.78rem !important; border-radius:999px !important; margin-bottom:0.8rem !important; }
.ai-dark h2 { color:#f8fafc !important; font-size:clamp(1.6rem,2.4vw,2.05rem) !important; line-height:1.22 !important; margin:0 0 0.7rem 0 !important; font-weight:800 !important; max-width:none !important; }
.ai-lead { color:#cbd5e1 !important; font-size:1rem !important; line-height:1.65 !important; margin:0 0 1.65rem 0 !important; max-width:1040px !important; }
.ai-kpis { display:grid !important; grid-template-columns:repeat(4,minmax(0,1fr)) !important; gap:1.15rem !important; margin-bottom:1.75rem !important; }
.ai-kpi { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.15rem 1.3rem !important; }
.ai-kpi strong { display:block !important; font-size:1.55rem !important; font-weight:800 !important; color:#f8fafc !important; line-height:1.15 !important; white-space:nowrap !important; }
.ai-kpi span { display:block !important; font-size:0.84rem !important; color:#94a3b8 !important; margin-top:0.35rem !important; line-height:1.4 !important; }
.ai-fig { width:100% !important; max-width:100% !important; margin:0 !important; border-radius:16px !important; overflow:hidden !important; border:1px solid #1e293b !important; background:#050b16 !important; }
.ai-fig img { display:block !important; width:100% !important; max-width:100% !important; height:auto !important; margin:0 !important; }
.ai-fig figcaption { padding:0.8rem 1.2rem !important; font-size:0.86rem !important; color:#94a3b8 !important; text-align:center !important; border-top:1px solid #1e293b !important; max-width:none !important; }
.ai-grid2 { display:grid !important; grid-template-columns:repeat(2,minmax(0,1fr)) !important; gap:1.35rem !important; margin-top:1.5rem !important; }
.ai-grid3 { display:grid !important; grid-template-columns:repeat(3,minmax(0,1fr)) !important; gap:1.25rem !important; margin-top:1.5rem !important; }
.ai-card { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.5rem 1.6rem !important; }
.ai-badge { display:inline-block !important; font-size:0.72rem !important; font-weight:700 !important; letter-spacing:0.07em !important; text-transform:uppercase !important; color:#fbbf24 !important; margin-bottom:0.45rem !important; }
.ai-card h3 { color:#f8fafc !important; font-size:1.16rem !important; font-weight:700 !important; margin:0 0 0.7rem 0 !important; }
.ai-card p { color:#cbd5e1 !important; font-size:0.9rem !important; line-height:1.58 !important; margin:0 !important; max-width:none !important; }
.ai-card ul { margin:0 !important; padding-left:1.2rem !important; display:flex !important; flex-direction:column !important; gap:0.5rem !important; max-width:none !important; }
.ai-card li { color:#cbd5e1 !important; font-size:0.9rem !important; line-height:1.56 !important; margin:0 !important; max-width:none !important; }
.ai-card li strong { color:#f8fafc !important; font-weight:700 !important; }
.ai-card code { background:#1e293b !important; color:#7dd3fc !important; padding:0.12rem 0.38rem !important; border-radius:4px !important; font-size:0.82rem !important; }
.ai-cta-row { display:flex !important; flex-wrap:wrap !important; gap:0.85rem !important; margin-top:1.6rem !important; }
.ai-btn-pri { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.35rem !important; border-radius:9px !important; background:#f59e0b !important; color:#0f172a !important; font-weight:700 !important; font-size:0.9rem !important; text-decoration:none !important; }
.ai-btn-sec { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.3rem !important; border-radius:9px !important; background:#1e293b !important; color:#f8fafc !important; border:1px solid #334155 !important; font-weight:600 !important; font-size:0.9rem !important; text-decoration:none !important; }
@media (max-width:1024px) {
.ai-kpis { grid-template-columns:repeat(2,minmax(0,1fr)) !important; }
.ai-grid2, .ai-grid3 { grid-template-columns:1fr !important; }
}
@media (max-width:640px) {
.ai-kpis { grid-template-columns:1fr !important; }
.ai-dark { padding:1.4rem !important; }
}
</style>
<div class="ai-wrap">
<section class="ai-dark">
<span class="ai-eyebrow">TANILAMA &rarr; YAPAY ZEKA ASİSTANI &bull; v0.35.2</span>
<h2>Güvenlik Duvarınızla Kendi Dilinizde Konuşun — Sıfır Değişiklik Riskiyle</h2>
<p class="ai-lead">Tanılama &rarr; Yapay Zeka Asistanı ekranından sistem yöneticileri güvenlik duvarının canlı durumunu doğal dilde sorgulayabilir ("Son bir saatte nereden gelen neler engellendi?", "WAN2 hattı sağlıklı mı?", "İç ağda şüpheli davranan bir cihaz var mı?"). Asistan sorulan dilde yanıt verir, verileri 16 salt okunur MCP aracından çeker, kullandığı araçları her yanıtın altında listeler ve bir kural değişikliği gerektiğinde doğrudan ilgili panel sayfasını tarif eder.</p>
<div class="ai-kpis">
<div class="ai-kpi"><strong>15 Araç</strong><span>Salt Okunur Canlı MCP Telemetrisi</span></div>
<div class="ai-kpi"><strong>0 Yazma Riski</strong><span>pf, NAT veya Sistem Ayarlarını Değiştiremez</span></div>
<div class="ai-kpi"><strong>PUB-1 Maske</strong><span>Genel IP'ler Bulut Öncesi Maskelenir</span></div>
<div class="ai-kpi"><strong>5 Sağlayıcı</strong><span>Claude, OpenAI, DeepSeek, Gemini ve Ollama</span></div>
</div>
<figure class="ai-fig">
<img src="/images/ai-assistant-ui.jpg" alt="Stratum Firewall Yerlesik Yapay Zeka Asistani Canli Arayuzu" loading="eager" width="1376" height="768">
<figcaption>Deterministik kural sayımı, şeffaf MCP araç rozetleri ve güvenli panel yönlendirmeleriyle doğal dilde canlı tanılama.</figcaption>
</figure>
</section>
<section class="ai-dark">
<span class="ai-eyebrow">GİZLİLİK ODAKLI MİMARİ &bull; SIFIR ANAHTAR SIZINTISI</span>
<h2>Yapay Zeka Sorgularında Ağ Gizliliği Nasıl Korunur?</h2>
<p class="ai-lead">Bir sınır güvenlik duvarını yapay zeka modellerine bağlamak katı güvenlik sınırları gerektirir. Stratum FW, genel IP adreslerini cihaz üzerinde otomatik maskeler, gizli anahtar erişimini araç katmanında kapatır ve yerel Ollama ile %100 ağ içi çalışma imkanı sunar.</p>
<figure class="ai-fig">
<img src="/images/ai-assistant-privacy.jpg" alt="Stratum AI Assistant IP Maskeleme ve Yerel Ollama Mimarisi" loading="lazy" width="1200" height="896">
<figcaption>Gerçek genel IP adresleri dış sağlayıcıya çıkmadan önce PUB-1, PUB-2 takma adlarıyla değiştirilir ve yanıtta yerel olarak geri yazılır; yerel Ollama ile hiçbir veri ağ dışına çıkmaz.</figcaption>
</figure>
<div class="ai-grid2">
<div class="ai-card">
<span class="ai-badge">OTOMATİK ANONİMLEŞTİRME</span>
<h3>Genel IP Maskeleme ve Geçici RAM Oturumu</h3>
<ul>
<li><strong>Varsayılan IP maskeleme:</strong> Genel IPv4 adresleri dış sağlayıcıya gönderilmeden önce takma adlarla (<code>PUB-1</code>, <code>PUB-2</code>...) değiştirilir, tarayıcıdaki yanıtta ise gerçek adresleriyle gösterilir.</li>
<li><strong>Gizli verilere sıfır erişim:</strong> Hiçbir MCP aracı parolaları, <code>0600</code> gizli anahtarları veya VPN profillerini döndürmez; sağlayıcı API anahtarları <code>0600</code> izinli dosyalarda tutulur (şifreli <code>.stbk</code> yedeklerine dahildir) ve tek tıkla <strong>Test</strong> edilebilir.</li>
<li><strong>1 saatlik bellek içi sohbet:</strong> Yalnızca Yöneticilere açıktır; kimin sorgu yaptığı ve hangi araçların çalıştığı günlüğe yazılır, ancak sorular ve yanıtlar diske kaydedilmez (1 saat bellekte kalır).</li>
</ul>
</div>
<div class="ai-card">
<span class="ai-badge">KESİN DOĞRULUK &bull; v0.35.2</span>
<h3>Deterministik Kural Sayımı ve Harici MCP Desteği</h3>
<ul>
<li><strong>Hazır kural ve NAT toplamları:</strong> Uzun port yönlendirme veya güvenlik kuralı listelerinde modelin yanlış saymasını önlemek için MCP araçları toplam, etkin ve devre dışı kural sayılarını kesin olarak hesaplayıp modele verir.</li>
<li><strong>Net paket sayacı açıklaması:</strong> Araçlar <code>0</code> paket sayacının <em>"hiç kullanılmadı"</em> değil, <em>"son Apply/yenilemeden beri eşleşmedi"</em> anlamına geldiğini açıkça belirterek yanıltıcı önerileri önler.</li>
<li><strong>Panel içi sohbet + harici MCP istemcileri:</strong> Dahili ekranın yanı sıra <code>Sistem &rarr; Ayarlar &rarr; MCP</code> altından IP kısıtlamalı, iptal edilebilir token ve araç bazlı açma/kapama ile harici masaüstü MCP istemcileri bağlanabilir.</li>
</ul>
</div>
</div>
</section>
<section class="ai-dark">
<span class="ai-eyebrow">ESNEK ÇALIŞMA &bull; BULUT VEYA %100 YEREL AĞ</span>
<h2>Bulut Modelleri veya Ağ İçi Yerel Ollama Seçeneği</h2>
<div class="ai-grid3">
<div class="ai-card">
<span class="ai-badge">%100 YEREL YAPAY ZEKA</span>
<h3>Ağ İçi Ollama Sunucusu</h3>
<p>Stratum FW'yi kendi yerel ağınızdaki bir <strong>Ollama</strong> sunucusuna bağlayın. Hiçbir soru, günlük özeti veya ağ verisi kurum dışına çıkmadan, sıfır API maliyetiyle tamamen kapalı devre çalışır.</p>
</div>
<div class="ai-card">
<span class="ai-badge">ÜRETİMDE DOĞRULANMIŞ BULUT</span>
<h3>Claude, OpenAI, DeepSeek ve Gemini</h3>
<p>Otomatik <code>PUB-1</code> genel IP maskelemesiyle Anthropic Claude, OpenAI, DeepSeek veya Google Gemini modellerini kullanın. Üretim ortamında DeepSeek ile Türkçe/İngilizce doğrulaması tamamlanmıştır.</p>
</div>
<div class="ai-card">
<span class="ai-badge">14 CANLI TELEMETRİ ARACI</span>
<h3>Tam Görünürlük, Sıfır Kilitlenme Riski</h3>
<p>Sistem durumu, arayüzler, WAN gecikme/yedeklilik, servisler, güvenlik duvarı günlüğü, engelleme özeti, kurallar, NAT, DHCP kiraları, VPN oturumları, Suricata alarmları, değişiklik geçmişi ve trafik grafiklerini kapsar.</p>
</div>
</div>
<div class="ai-cta-row">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="ai-btn-pri">Canlı Demoda Tanılama Ekranlarını İncele &rarr;</a>
<a href="/tr/download/" class="ai-btn-sec">Stratum FW İndir</a>
<a href="/tr/resources/changelog/" class="ai-btn-sec">v0.35.2 Sürüm Notları</a>
</div>
</section>
</div>
