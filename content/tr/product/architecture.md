---
title: "Mimari"
lead: "FreeBSD'nin ham gücü üzerine inşa edilmiş modern bir yönetim düzlemi. Bildirimsel, denetlenebilir ve tasarım gereği güvenli."
---

## Tasarım Prensipleri

Stratum Firewall bir kabuk betiği koleksiyonu değildir. Platformdaki her kod satırını şekillendiren üç temel ilke üzerine inşa edilmiş uyumlu bir sistemdir:

<div class="card-grid grid-3" style="margin-top:32px;">
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 2v16M4 6l6-4 6 4M4 14l6 4 6-4"/></svg></div>
    <h3>Bildirimsel Durum</h3>
    <p>Tek bir doğruluk kaynağı vardır: <code>config.json</code>. <code>pf.conf</code> dosyasını asla doğrudan düzenlemezsiniz. Sistem niyetinizi okur ve yerel yapılandırma dosyalarını oluşturur.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l5 5 9-11"/></svg></div>
    <h3>Varsayılan Olarak Güvenli</h3>
    <p>Her değişiklik aşamalandırılır. Bir kural çekirdeğe uygulanmadan önce sistem sözdizimini doğrulamak için <code>pfctl -nf</code> kullanır ve bağlantıyı kaybederseniz geri almak için bir onay zaman aşımı kullanır.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="10" cy="10" r="7"/><path d="M10 3v7l5 3"/></svg></div>
    <h3>Önce API</h3>
    <p>Web paneli sadece bir istemcidir. Arayüzde gerçekleştirilen her işlem REST API aracılığıyla yürütülür. Bu, tam Altyapı-Kod Olarak (IaC) otomasyonuna olanak tanır.</p>
  </div>
</div>

## Sistem Katmanları

Stratum kesin olarak ayrılmış üç katmanda çalışır. Bir katmandaki hata veya değişiklik kontrolsüz bir şekilde diğerine yayılamaz.

<div class="arch-layers">
  <div class="arch-layer">
    <div class="arch-layer-num">03</div>
    <div class="arch-layer-body">
      <h3>Yönetim Düzlemi</h3>
      <p class="arch-layer-stack">Python 3.12 · FastAPI · Uvicorn · Jinja2 · Vanilla JS</p>
      <p>Web arayüzü ve REST API. Kimlik doğrulamayı, Rol Tabanlı Erişim Kontrolünü (RBAC), girdi doğrulamayı yönetir ve yapılandırma değişikliklerini aşamalandırır. Durumsuzdur (stateless) ve sistemin mevcut durumuna tepki verir.</p>
    </div>
  </div>
  <div class="arch-layer">
    <div class="arch-layer-num">02</div>
    <div class="arch-layer-body">
      <h3>Yapılandırma Motoru</h3>
      <p class="arch-layer-stack">Python · generators.py · safe_apply.py</p>
      <p>Operasyonun beyni. <code>config.json</code> dosyasını okur ve yerel FreeBSD yapılandırmalarını dinamik olarak oluşturur: <code>pf.conf</code>, <code>wg0.conf</code>, <code>unbound.conf</code> ve <code>rc.conf</code>. Bir değişiklik başarısız olursa atomik geri almaları da yönetir.</p>
    </div>
  </div>
  <div class="arch-layer">
    <div class="arch-layer-num">01</div>
    <div class="arch-layer-body">
      <h3>Veri Düzlemi (Çekirdek)</h3>
      <p class="arch-layer-stack">FreeBSD 14 · pf · ALTQ · WireGuard (Çekirdek modülü)</p>
      <p>Tüm paket işleme burada gerçekleşir. pf, hat hızında durum bilgili denetim, NAT ve yönlendirmeyi yönetir. WireGuard, maksimum VPN verimi için tamamen çekirdekte çalışır.</p>
    </div>
  </div>
</div>

## Teknoloji Yığını

Kendini kanıtlamış, standart bileşenler kullanıyoruz. Kara kutular yok.

<table class="table">
  <thead>
    <tr>
      <th>Katman</th>
      <th>Bileşen</th>
      <th>Rol</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>İşletim Sistemi</td><td>FreeBSD 14+</td><td>Temel. Sağlam ağ yığını ve güvenliğiyle bilinir.</td></tr>
    <tr><td>Paket Filtresi</td><td>pf (Packet Filter)</td><td>Durum bilgili güvenlik duvarı, NAT ve trafik şekillendirme (ALTQ).</td></tr>
    <tr><td>VPN</td><td>WireGuard</td><td>Çekirdek içi, yüksek hızlı kriptografik tüneller.</td></tr>
    <tr><td>Panel Arka Ucu</td><td>Python 3.12 · FastAPI</td><td>Eşzamansız, yüksek performanslı REST API ve web sunucusu.</td></tr>
    <tr><td>Panel Ön Ucu</td><td>Jinja2 · Vanilla JS</td><td>Ağır ön uç çerçeveleri olmadan sunucu taraflı işleme.</td></tr>
    <tr><td>Yapılandırma Motoru</td><td>Python 3 (Dahili)</td><td><code>config.json</code> dosyasını yerel sistem dosyalarına çevirir.</td></tr>
  </tbody>
</table>

## Güvenlik Modeli

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Rol Tabanlı Erişim Kontrolü (RBAC)</h3>
    <p>Panel üç farklı kullanıcı rolünü destekler. <b>Yönetici (Admin)</b> her şeyi değiştirebilir. <b>Operatör</b> teşhisleri çalıştırabilir ve durumları yönetebilir. <b>Salt Okunur</b> panoyu ve günlükleri görüntüleyebilir ancak değişiklik yapamaz.</p>
  </div>
  <div class="card">
    <h3>Katı Sır Yönetimi</h3>
    <p>Özel anahtarlar (örneğin WireGuard anahtarları) asla ana <code>config.json</code> dosyasında saklanmaz. Diske <code>chmod 600</code> izinleriyle yazılır ve yalnızca yapılandırma oluşturulurken okunur.</p>
  </div>
  <div class="card">
    <h3>Sıfır Harici Bağımlılık</h3>
    <p>Kritik arayüzümüz için harici paket depolarına güvenmiyoruz. Panel standart Python kütüphanelerinde çalışır ve Uvicorn aracılığıyla sunulur, bu da saldırı yüzeyini en aza indirir.</p>
  </div>
  <div class="card">
    <h3>Kurcalamaya Dayanıklı Günlükler</h3>
    <p>Her işlem syslog aracılığıyla <code>/var/log/auth.log</code> dosyasına kaydedilir. Değişiklikler, bunları yapan belirli kullanıcı hesabına bağlanır.</p>
  </div>
</div>

---

<div class="card-grid grid-2" style="margin-top:48px;">
  <div class="card">
    <h3>Özellikleri Keşfedin</h3>
    <p>Bu mimarinin üzerine inşa edilen yeteneklerin tam listesini görün.</p>
    <a href="/tr/product/features/" class="card-link">Özelliklere göz atın →</a>
  </div>
  <div class="card">
    <h3>Dökümantasyonu Okuyun</h3>
    <p>Stratum Firewall kurulumu, yapılandırması ve API referansı.</p>
    <a href="/tr/resources/documentation/" class="card-link">Dökümantasyonu açın →</a>
  </div>
</div>
