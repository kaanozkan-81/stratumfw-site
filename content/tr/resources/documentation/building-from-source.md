---
title: "Kaynak Koddan Derleme"
lead: "Stratum FW yönetim panelini ve yapılandırma motorunu mevcut bir FreeBSD 15.1 sistemi üzerinde nasıl kuracağınız."
---

Stratum Firewall şu anda **Geliştirici Önizlemesi** aşamasındadır. Önceden oluşturulmuş FreeBSD ISO kurulum medyası henüz mevcut değildir. Ancak geliştiriciler, paneli ve yapılandırma motorunu doğrudan mevcut bir FreeBSD 15.1 sistemi üzerine kurabilirler.

## Ön Koşullar

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

- **FreeBSD 15.1** veya daha yeni bir sürüm çalıştıran bir makine
- Root (veya `sudo`) erişimi
- En az 2 GB RAM ve 10 GB depolama alanı
- `pf` güvenlik duvarı hakkında temel bilgi

## Adım 1: Sistem Bağımlılıklarını Kurun

Stratum, Python 3.12+ ve birkaç sistem aracı gerektirir. Root olarak oturum açın ve `pkg` kullanarak bunları kurun:

~~bash
pkg update
pkg install -y python312 py312-pip py312-sqlite3 git
~~

`pf`'nin `/etc/rc.conf` içinde etkinleştirildiğinden emin olun:

~~bash
sysrc pf_enable="YES"
service pf start
~~

## Adım 2: Depoyu Klonlayın

Kaynak kodu genel GitHub depomuzdan klonlayın:

~~bash
git clone https://github.com/kaanozkan-81/stratum-panel.git
cd stratum-panel
~~

*(Not: Çekirdek panel kaynak kodu şu anda özel bir depoda barındırılmaktadır ve v1.0.0 sürümünde genel depoya birleştirilecektir. Bu kılavuz, yaklaşan mimariyi ana hatlarıyla açıklar).*

## Adım 3: Python Ortamını Ayarlayın

Bağımlılıkları izole etmek için standart bir Python sanal ortamı kullanıyoruz:

~~bash
python3.12 -m venv /opt/stratum/venv
source /opt/stratum/venv/bin/activate
pip install --upgrade pip
pip install fastapi uvicorn jinja2 itsdangerous psutil passlib bcrypt
~~

## Adım 4: İlk Yapılandırmayı Oluşturun

Stratum, bildirimsel bir JSON dosyasına dayanır. Varsayılan yapılandırmayı başlatın:

~~bash
mkdir -p /usr/local/etc/stratum
# Bu, gelecekte bir kurulum betiği tarafından yönetilecektir
echo '{"system": {"hostname": "stratum"}}' > /usr/local/etc/stratum/config.json
~~

## Adım 5: Paneli Çalıştırın

FastAPI uygulamasını Uvicorn kullanarak başlatın:

~~bash
cd /opt/stratum/app
uvicorn main:app --host 0.0.0.0 --port 8000
~~

Artık geliştirici paneline `http://<ip-adresiniz>:8000` adresinden erişebilirsiniz.

## Sonraki Adımlar

Panel çalışmaya başladığında, API belgelerini keşfedebilir veya güvenlik duvarını yapılandırmaya başlayabilirsiniz. **Güvenli Uygulama (Safe-Apply)** mekanizması, yapılandırma sırasında kendinizi sistemden kilitlemenizi önler.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>API'yi Keşfedin</h3>
    <p>REST API kullanarak Stratum'u nasıl otomatikleştireceğinizi öğrenin.</p>
    <a href="/tr/product/architecture/" class="card-link">Mimariyi Okuyun →</a>
  </div>
  <div class="card">
    <h3>Beta'ya Katılın</h3>
    <p>Test etmekle ilgileniyor musunuz? Erken erişim için bizimle iletişime geçin.</p>
    <a href="/tr/company/contact/" class="card-link">Bize Ulaşın →</a>
  </div>
</div>
