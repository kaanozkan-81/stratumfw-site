---
title: "Sıkça Sorulan Sorular"
lead: "Platform, lisanslama, dağıtım ve destek hakkında en sık sorulan sorulara yanıtlar."
full_width: true
---
<div class="faq-sections">
<section class="faq-section">
  <h2>Genel</h2>
  <div class="faq">
    <details><summary>Stratum Firewall nedir?</summary><div class="faq-body">Stratum Firewall, açık kaynaklı, kendi sunucunuzda barındırılan bir güvenlik duvarı ve ağ güvenliği platformudur. FreeBSD çekirdeği ve pf paket filtresinin ham gücünü, modern bir FastAPI yönetim paneliyle birleştirir.</div></details>
    <details><summary>Neden Linux yerine FreeBSD?</summary><div class="faq-body">FreeBSD, dağınık bir paket koleksiyonu değil, uyumlu bir işletim sistemi olarak tasarlanmıştır. Dünyanın en çok savaş görmüş ağ yığınına, pf için yerel desteğe ve ZFS'ye sahiptir.</div></details>
    <details><summary>Açık kaynak mı?</summary><div class="faq-body">Evet. Kullanıcı başına lisans, cihaz başına ücret veya abonelik gereksinimi yoktur.</div></details>
    <details><summary>Telemetri veya bulut bağımlılığı var mı?</summary><div class="faq-body">Kesinlikle hayır. Telemetri, analiz veya bulut bağımlılığı yoktur.</div></details>
  </div>
</section>
<section class="faq-section">
  <h2>Mimari ve Yönetim</h2>
  <div class="faq">
    <details><summary>"Bildirimsel Yapılandırma" ne anlama geliyor?</summary><div class="faq-body">Karmaşık güvenlik duvarı kurallarını asla elle düzenlemezsiniz. Sistemi web arayüzü veya API üzerinden yapılandırırsınız, bu da niyetinizi tek bir config.json dosyasına kaydeder.</div></details>
    <details><summary>"Güvenli Uygulama" (Safe-Apply) iş akışı nedir?</summary><div class="faq-body">Safe-Apply, kendinizi kilitlemenizi önler. Kritik bir değişiklik yapıldığında doğrulanır ve onaylamazsanız otomatik olarak geri alınır.</div></details>
    <details><summary>Stratum'u betiklerle otomatikleştirebilir miyim?</summary><div class="faq-body">Evet. Tüm platform API-önceliklidir. Ansible, Terraform veya özel betiklerle otomatikleştirebilirsiniz.</div></details>
  </div>
</section>
<section class="faq-section">
  <h2>Destek ve Sürümler</h2>
  <div class="faq">
    <details><summary>Hangi kurulum imajları mevcut?</summary><div class="faq-body">Stratum FW v0.27.1 (FreeBSD 15.1 amd64) için hem standart ekranlı sistemlere yönelik <strong>VGA ISO (.iso)</strong> hem de başsız cihazlara yönelik <strong>Seri Konsol USB (.img)</strong> kurulum imajları <a href="/tr/download/">İndir</a> sayfasında sunulmaktadır. Ayrıca kurulum yapmadan <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">Canlı Demo</a> üzerinden arayüzü deneyebilirsiniz.</div></details>
    <details><summary>Nasıl yardım alabilirim?</summary><div class="faq-body">info@stratumfw.com adresinden bize ulaşabilir veya GitHub'da bir sorun açabilirsiniz.</div></details>
  </div>
</section>
</div>
