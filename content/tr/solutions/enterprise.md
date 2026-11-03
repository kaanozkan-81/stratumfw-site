---
title: "Kurumsal"
lead: "API odaklı, yüksek kullanılabilirlik ve denetlenebilirlik. Modern altyapı ekipleri için inşa edildi."
---

## Kurumsal ölçekte ne değişir?

Kurumsal bir ağ, sadece bir güvenlik duvarından daha fazlasını gerektirir. **Otomasyon, görünürlük ve katı erişim kontrolü** gerektirir.

- **Birden fazla site** — şubeler, veri merkezleri ve bulut ortamları
- **Uyumluluk** — ISO 27001, SOC 2 ve katı denetim izleri
- **Altyapı-Kod Olarak** — Ansible, Terraform ve otomatik dağıtımlar
- **Sıfır Güven** — Yöneticiler için Katı Rol Tabanlı Erişim Kontrolü (RBAC)

**Stratum FW bu talepleri karşılamak için sıfırdan inşa edildi.**

## Stratum nasıl uyum sağlar?

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Önce API Mimarisi</h3>
    <p>Her yapılandırma yüzeyi bir REST API aracılığıyla sunulur. Güvenlik duvarı dağıtımlarınızı otomatikleştirin, CI/CD süreçlerinizle entegre edin ve manuel tıklamaları ortadan kaldırın.</p>
  </div>
  <div class="card">
    <h3>Katı RBAC</h3>
    <p>Yöneticiler, Operatörler ve Salt Okunur kullanıcılar arasında ayrım yapın. Ekibiniz genelinde en az yetki ilkesini uygulayın.</p>
  </div>
  <div class="card">
    <h3>Bildirimsel Yapılandırma</h3>
    <p>Tek bir `config.json` doğruluk kaynağıdır. Güvenlik duvarı durumunuzu kolayca sürüm kontrolüne alın, yedekleyin veya saniyeler içinde yeni bir düğüme dağıtın.</p>
  </div>
  <div class="card">
    <h3>Denetlenebilir ve Şeffaf</h3>
    <p>Kara kutular yok. Oluşturulan `pf.conf` ve `wg0.conf` dosyaları insan tarafından okunabilir. Çekirdeğin her zaman tam olarak ne yürüttüğünü bilin.</p>
  </div>
</div>

## Operasyonel avantajlar

<table class="table">
  <thead>
    <tr>
      <th>Endişe</th>
      <th>Geleneksel yığın</th>
      <th>Stratum FW ile</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Otomasyon</td><td>Kırılgan ekran kazıma betikleri</td><td>Temiz, belgelenmiş REST API</td></tr>
    <tr><td>Değişiklik yönetimi</td><td>Manuel, hataya açık, denetlemesi zor</td><td>Güvenli Uygulama, atomik geri alma, katı denetim günlükleri</td></tr>
    <tr><td>Performans</td><td>Kullanıcı alanı filtreleme (yavaş)</td><td>Çekirdek düzeyinde pf (hat hızı)</td></tr>
    <tr><td>Satıcı bağımlılığı</td><td>Tescilli donanım ve işletim sistemi</td><td>Standart FreeBSD, herhangi bir x86-64 donanımda çalışır</td></tr>
  </tbody>
</table>

<div style="margin-top:32px;">
  <a href="/tr/product/architecture/" class="btn btn-primary">Mimariyi Okuyun</a>
  <a href="mailto:info@stratumfw.com" class="btn btn-secondary">Satışla İletişime Geçin</a>
</div>
