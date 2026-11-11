---
title: "Sıkça Sorulan Sorular"
description: "Stratum Firewall v0.30.3 mimarisi, OPNsense ve OpenVPN göçü, Suricata IPS, GeoIP filtreleme, ZFS güncellemeleri ve kurulum imajları hakkında sıkça sorulan sorular."
lead: "Platform, OPNsense göçü, güvenlik modülleri, güncellemeler ve dağıtım hakkında en sık sorulan sorulara yanıtlar."
full_width: true
---
<div class="faq-sections">

<section class="faq-section">
  <h2>Genel ve Platform</h2>
  <div class="faq">
    <details><summary>Stratum Firewall nedir?</summary><div class="faq-body">Stratum Firewall (Stratum FW), <strong>FreeBSD 15.1-RELEASE</strong> çekirdeği ve yerel <code>pf</code> paket filtresi üzerine inşa edilmiş, modern ve asenkron bir <strong>Python 3.12 / FastAPI</strong> web arayüzüyle yönetilen yeni nesil güvenlik duvarı işletim sistemidir.</div></details>
    <details><summary>Kurulum yapmadan önce arayüzü canlı deneyebilir miyim?</summary><div class="faq-body">Evet! Hiçbir dosya indirmeden <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">Canlı Demo (demo.stratumfw.com)</a> adresinden gerçek bir FreeBSD 15.1 sanal makinesi üzerinde çalışan Stratum FW v0.30.3 arayüzünü inceleyebilirsiniz (<code>demo</code> / <code>Stratum2026</code> bilgileri giriş ekranında otomatik doldurulur).</div></details>
    <details><summary>Neden Linux yerine FreeBSD 15.1 tercih edildi?</summary><div class="faq-body">FreeBSD, dağınık paketlerin birleşimi değil; çekirdeği, ağ yığını, <code>pf</code> güvenlik duvarı, <code>netmap</code> altyapısı ve <strong>ZFS</strong> dosya sistemi bir bütün olarak geliştirilen tutarlı bir işletim sistemidir. Bu yapı kurumsal ağ geçitleri için yüksek öngörülebilirlik ve kararlılık sağlar.</div></details>
    <details><summary>Kullanıcı sınırı, abonelik veya telemetri var mı?</summary><div class="faq-body">Hayır. Kullanıcı başına lisans, cihaz sınırı veya abonelik ücreti yoktur. Sistem dışarıya hiçbir telemetri ya da analiz verisi göndermez; tamamen kapalı devre (air-gapped) ağlarda çevrimdışı <code>.sfu</code> paketleriyle çalıştırılabilir.</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>OPNsense Göçü, Mimari ve Güncellemeler</h2>
  <div class="faq">
    <details><summary>Mevcut OPNsense kurallarımı ve OpenVPN kullanıcılarımı aktarabilir miyim?</summary><div class="faq-body">Evet (<code>v0.25.0</code> – <code>v0.30.3</code>). <strong>Sistem &gt; Yedekleme &gt; OPNsense'den İçe Aktar</strong> aracıyla bir OPNsense <code>config.xml</code> dosyasını yükleyerek arayüzleri, VLAN'ları, takma adları, güvenlik duvarı ve NAT kurallarını, DHCP rezervasyonlarını ve DNS ayarlarını önizlemeli olarak taşıyabilirsiniz. <em>"OpenVPN'i de içe aktar"</em> seçeneğiyle CA, sertifikalar, <code>tls-crypt</code> anahtarı ve kullanıcı parolaları (PHP <code>$2y$</code> bcrypt ve <code>argon2id</code> dahil) devralınır; son kullanıcıların bilgisayarlarındaki mevcut <code>.ovpn</code> profilleri hiçbir değişiklik yapılmadan bağlanmaya devam eder.</div></details>
    <details><summary>"Bildirimsel Yapılandırma" ve "Güvenli Uygulama" (Safe-Apply) nedir?</summary><div class="faq-body">Tüm ayarlar tek bir <code>config.json</code> kaynağında tutulur ve <code>pf.conf</code>, Unbound, dnsmasq, Suricata, OpenVPN ve WireGuard dosyaları buradan üretilir. Bir değişiklik uygulandığında önce sözdizimi doğrulaması (<code>pfctl -n</code> vb.) yapılır ve <strong>60 saniyelik geri sayım</strong> başlar; bağlantınızı onaylamazsanız sistem sizi kilitlememek için otomatik olarak önceki çalışan yapılandırmaya geri döner.</div></details>
    <details><summary>Güncellemeler nasıl çalışır? Hatalı güncellemede geri dönebilir miyim?</summary><div class="faq-body">Stratum FW, <strong>Sistem &gt; Güncelleme</strong> menüsünden imzalı güncelleme sunucusu üzerinden veya çevrimdışı <code>.sfu</code> dosyasıyla güncellenir. Her güncelleme öncesinde otomatik olarak yeni bir <strong>ZFS Önyükleme Ortamı (Boot Environment)</strong> oluşturulur; istediğiniz an tek tıkla veya açılış menüsünden önceki sürüme dönebilirsiniz.</div></details>
    <details><summary>Rol Tabanlı Erişim Kontrolü (RBAC) ve REST API desteği var mı?</summary><div class="faq-body">Evet. <strong>Yönetici (Admin)</strong>, <strong>Operatör</strong>, <strong>Salt Okunur (Read-Only)</strong> ve yalnızca VPN bağlantısı kurabilen <strong>VPN-only</strong> hesap rolleri desteklenir. Tüm yönetim işlemleri REST API üzerinden yürütüldüğü için Ansible, Terraform veya Python betikleriyle otomatikleştirilebilir.</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>Güvenlik, VPN ve Ağ Yetenekleri</h2>
  <div class="faq">
    <details><summary>Saldırı Tespit ve Engelleme Sistemi (Suricata IPS/IDS) nasıl çalışır?</summary><div class="faq-body">Her ağ arayüzü için bağımsız olarak <strong>İzle (Watch / IDS)</strong> veya <strong>Engelle (Block / Inline IPS)</strong> modu seçilebilir. İki mod aynı anda farklı arayüzlerde kullanıldığında Stratum, engelleme portları için <code>netmap</code>, izleme portları için <code>pcap</code> tabanlı iki ayrı Suricata sürecini denetler ve kural değişikliklerini bağlantıları koparmadan canlı uygular.</div></details>
    <details><summary>Ülke bazlı engelleme (GeoIP), DNSBL ve IP karalisteleri için lisans gerekir mi?</summary><div class="faq-body">Hayır, tamamı yerleşik ve ücretsizdir. Beş bölgesel internet kayıt kuruluşundan (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC) haftalık çekilen <strong>Ülke (GeoIP) takma adları</strong>, CDN uyumlu dinamik <strong>Hostname takma adları</strong>, saatlik güncellenen 63 adet IP tehdit beslemesi ve Unbound RPZ tabanlı <strong>DNSBL Web Filtreleme</strong> hiçbir harici API anahtarı gerektirmeden çalışır.</div></details>
    <details><summary>WireGuard, Çift WAN OpenVPN, Sanal IP (VIP) ve Statik Rota desteği var mı?</summary><div class="faq-body">Evet. Çekirdek tabanlı <strong>WireGuard (<code>if_wg</code>)</strong> ile QR kodlu anında eş oluşturma, <strong>OpenVPN</strong> üzerinde çift WAN doğru arayüzden yanıt verme (<code>multihome</code>) ve Windows <code>block-outside-dns</code> desteği mevcuttur. Ayrıca <code>/29</code> gibi çoklu genel IP blokları için <strong>Sanal IP'ler (VIP)</strong> ve alt yönlendiriciler için <strong>Statik Rotalar</strong> (<code>stratum-netextra</code> ile temiz çekirdek senkronizasyonu) desteklenir.</div></details>
    <details><summary>Sorun giderme için hangi yerleşik tanılama araçları bulunuyor?</summary><div class="faq-body">Wireshark analizi için filtrelenebilir <strong>Paket Yakalama (<code>.pcap</code>)</strong>, rastgele (mobil) MAC tespiti ve tek tıkla <strong>Wake-on-LAN</strong> destekli <strong>ARP Tablosu</strong>, canlı kural sayacı/log izleme ve mevcut fiziksel port atamalarını koruyarak cihaz erişimini kaybetmeyen <strong>Uzaktan Güvenli Fabrika Sıfırlaması</strong> yerleşik olarak gelir.</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>Kurulum ve Destek</h2>
  <div class="faq">
    <details><summary>Hangi kurulum imajları mevcut?</summary><div class="faq-body">Stratum FW v0.30.3 (FreeBSD 15.1 amd64) için standart monitörlü sunucular ve sanal makineler (Proxmox, VMware, KVM) için <strong>VGA ISO (<code>.iso</code>)</strong> ve monitörsüz donanımlar (COM1 / IPMI seri konsol) için USB belleğe yazdırılabilir <strong>Serial IMG (<code>.img</code>)</strong> kurulum dosyaları <a href="/tr/download/">İndir</a> sayfasında sunulmaktadır.</div></details>
    <details><summary>Nasıl destek alabilir veya hata bildirebilirim?</summary><div class="faq-body">Doğrudan <a href="mailto:info@stratumfw.com">info@stratumfw.com</a> e-posta adresinden bize ulaşabilir veya <a href="https://github.com/kaanozkan-81/stratum-panel/issues" target="_blank" rel="noopener">GitHub Issues</a> üzerinden bildirim oluşturabilirsiniz.</div></details>
    <!-- STRATUM-V0300-FAQ-START -->
    <details><summary>Yönetim paneli girişinde İki Aşamalı Doğrulama (2FA / TOTP) var mı?</summary><div class="faq-body">Evet. <strong>v0.30.0</strong> sürümüyle birlikte tüm panel kullanıcıları <strong>Hesabım (<code>My Account</code>)</strong> ekranından <em>Google Authenticator</em>, <em>Microsoft Authenticator</em>, <em>1Password</em> veya <em>Bitwarden</em> uyumlu <strong>TOTP iki aşamalı girişi</strong> etkinleştirebilir. Kurulum QR kodu gömülü kütüphaneyle doğrudan tarayıcıda çizilir; gizli anahtar güvenlik duvarından dışarı çıkmaz. Telefon kaybına karşı <strong>10 adet tek kullanımlık kurtarma kodu</strong> üretilir ve gerektiğinde başka bir yönetici veya fiziksel konsol (<code>stratum-menu</code>) üzerinden 2FA kapatılabilir.</div></details>
    <details><summary>Kullanıcıların elle 8.8.8.8 vererek DNSBL ve Web Filtresini aşmasını engelleyebilir miyim?</summary><div class="faq-body">Evet. <strong>Servisler &rarr; DNS Çözümleyici &rarr; Force DNS</strong> (<code>v0.29.2</code>) özelliğini seçtiğiniz iç LAN, Misafir veya VLAN ağlarında açtığınızda cihazların hangi DNS sunucusunu sorduğuna bakılmaksızın tüm sorgular güvenlik duvarının çözümleyicisine yönlendirilir ve <strong>DNS over TLS (<code>TCP 853</code>)</strong> engellenir. <strong>Active Directory</strong> bulunan ortamlarda etki alanı denetleyicilerini <strong>İstisna takma adına (<code>Exception alias</code>)</strong> ekleyebilir, üstelik hiçbir gezinme geçmişi kaydetmeden filtrelemeyi zorunlu tutabilirsiniz.</div></details>
    <details><summary>Çoklu WAN (Multi-WAN) özelliğinde 4G/5G LTE, Kablo veya Starlink gibi dinamik DHCP yedek hatlar çalışır mı?</summary><div class="faq-body">Evet (<code>v0.29.1</code>). İkincil ve üçüncül WAN arayüzleri (<code>WAN2</code>, <code>WAN3</code>), Statik IP ve PPPoE&rsquo;nin yanı sıra <strong>DHCP</strong> ile otomatik adres alabilir. Ek bir DHCP WAN varsayılan rotayı tek başına devralmaz; hat geçişi saniyelik ağ geçidi izleyicisinin kontrolünde kalır ve operatör yeni bir ağ geçidi verdiğinde kurallar Uygula (Apply) gerektirmeden saniyeler içinde güncellenir.</div></details>
    <!-- STRATUM-V0300-FAQ-END -->
  </div>
</section>

</div>

<div style="text-align:center; margin-top:64px;">
  <h3 style="font-family:var(--font-display); font-size:22px; margin-bottom:12px;">Başka sorularınız mı var?</h3>
  <p style="color:var(--ash); margin-bottom:24px;">Tahmin yürütmenize gerek yok — doğrudan bize yazın, memnuniyetle yanıtlayalım.</p>
  <a href="/tr/company/contact/" class="btn btn-primary">İletişime Geçin</a>
</div>
