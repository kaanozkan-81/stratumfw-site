---
title: "Ürün Genel Bakış"
lead: "Ağınız üzerinde mutlak kontrol sağlayan, gereksiz yüklerden arındırılmış, API odaklı modern bir güvenlik duvarı işletim sistemi."
seo_title: "Ürün Özeti — Modern FreeBSD 15.1 ve pf Aboneliksiz Güvenlik Duvarı"
description: "Stratum Firewall'ın FreeBSD 15.1 pf paket filtreleme, Suricata 8 IPS, Unbound DNSBL, WireGuard ve ZFS Önyükleme Ortamlarını Python 3.12 FastAPI kontrol katmanıyla nasıl birleştirdiğini inceleyin."
---

## Stratum Firewall Nedir?

Stratum Firewall, **FreeBSD** üzerine inşa edilmiş kurumsal düzeyde bir **Ağ Güvenliği İşletim Sistemidir**. Şeffaflık, ham performans ve modern yönetilebilirlik talep eden mühendisler için tasarlanmıştır.

Tescilli komut satırı arayüzleri (CLI) veya ağır, bulut bağımlı paneller arkasına saklanan geleneksel güvenlik duvarı cihazlarının aksine Stratum, sisteme doğrudan bir arayüz görevi gören **hafif bir FastAPI web paneli** sunar.

Arayüzde yaptığınız her değişiklik doğrulanır, bildirimsel olarak tek bir `config.json` dosyasına yazılır ve anında yerel çekirdek yapılandırmalarına (örneğin `pf.conf` ve `wg0.conf`) dönüştürülür.

## Stratum Felsefesi

<div class="card-grid grid-3" style="margin-top:32px;">
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 3L3 6v6c0 4.4 3.3 6.5 7 8c3.7-1.5 7-3.6 7-8V6l-7-3z"/></svg></div>
    <h3>Sıfır Satıcı Bağımlılığı</h3>
    <p>Tamamen açık standartlar üzerine inşa edilmiştir. Abonelik yok, gizli telemetri yok, tescilli bulut bağımlılığı yok. Donanım da yazılım da size ait.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M8 4H4v4M12 16h4v-4M4 16l5-5M16 4l-5 5"/></svg></div>
    <h3>Önce API Mimarisi</h3>
    <p>Tüm sistem bir REST API tarafından yönetilir. Web paneli sadece bir istemcidir. Güvenlik duvarınızı Ansible, Terraform veya özel Python betikleriyle otomatikleştirin.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3l-8 8 8 8 8-8-8-8z"/><path d="M8 12l4-4"/></svg></div>
    <h3>Çekirdek Düzeyinde Güvenlik</h3>
    <p>Kullanıcı alanında (userspace) filtreleme yapmıyoruz. Stratum, hat hızında (wire speed) durum bilgili denetim için FreeBSD'nin yerel <strong>pf paket filtresini</strong> kullanır.</p>
  </div>
</div>

## Temel Yetenekler

Stratum, gelişigüzel entegre edilmiş 30 betik koleksiyonu değildir. Derin entegrasyonlara sahip, uyumlu ve amaca yönelik inşa edilmiş bir platformdur.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>pf ve Yönlendirme Motoru</h3>
    <p>Doğrudan FreeBSD'nin pf'i ile desteklenen tam durum bilgili paket denetimi (SPI), Ağ Adresi Çevirisi (NAT), port yönlendirme ve VLAN desteği.</p>
    <a href="/tr/product/features/" class="card-link">Daha fazla bilgi →</a>
  </div>
  <div class="card">
    <h3>WireGuard VPN</h3>
    <p>Yerel, yüksek performanslı uzaktan erişim. Yerleşik QR kod oluşturma ve sıfır harici bağımlılık (kriptografi dahili olarak yönetilir).</p>
    <a href="/tr/product/features/" class="card-link">Daha fazla bilgi →</a>
  </div>
  <div class="card">
    <h3>Tehdit İstihbaratı ve Karalisteler</h3>
    <p>Tüm katmanlarda koruma. Kötü amaçlı IP'leri güvenlik duvarı düzeyinde (IPv4/IPv6) engelleyin ve otomatik akış güncellemeleriyle kötü amaçlı alan adlarını DNS düzeyinde (DNSBL) filtreleyin.</p>
    <a href="/tr/product/features/" class="card-link">Daha fazla bilgi →</a>
  </div>
  <div class="card">
    <h3>Güvenli Uygulama İş Akışı</h3>
    <p>Kendinizi bir daha asla kilitlemeyin. Her yapılandırma değişikliği uygulanmadan önce <code>pfctl -nf</code> ile test edilir. WAN'ı yanlış yapılandırırsanız sistem otomatik olarak geri alır.</p>
    <a href="/tr/product/architecture/" class="card-link">Mimari hakkında bilgi edinin →</a>
  </div>
</div>

## Neden FreeBSD?

Linux'u değil, FreeBSD'yi seçtik. Çünkü FreeBSD, tek ve tutarlı, güvenli bir ağ cihazı olacak şekilde tasarlanmış bir işletim sistemidir.

- **pf paket filtresi** güvenlik duvarı motorları için altın standarttır.
- **ZFS**, yapılandırmalar ve günlükler için kurumsal düzeyde depolama bütünlüğü sunar.
- **Öngörülebilir sürüm döngüleri**, güvenlik duvarınızın gece yarısı üst kaynak paketleme değişiklikleri nedeniyle bozulmayacağı anlamına gelir.

## Modern Kurumsal İçin İnşa Edildi

İster bir homelab, ister küçük bir işletme yönetin, ister dağıtık kurumsal altyapı, Stratum eski nesil kurumsal güvenlik duvarlarının yükü olmadan ihtiyacınız olan araçları sunar.

<div class="stats" style="margin-top:32px; border:1px solid var(--line); border-radius:14px;">
  <div class="wrap">
    <div class="stat"><b>FreeBSD</b><span>İşletim Sistemi Temeli</span></div>
    <div class="stat"><b>pf</b><span>Güvenlik Duvarı Motoru</span></div>
    <div class="stat"><b>FastAPI</b><span>Yönetim Düzlemi</span></div>
    <div class="stat"><b>JSON</b><span>Bildirimsel Yapılandırma</span></div>
  </div>
</div>
