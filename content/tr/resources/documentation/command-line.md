---
title: "Komut Satırı"
lead: "Stratum Firewall'ı çalıştıran temel FreeBSD araçlarında uzmanlaşın."
---

Stratum paneli, güvenlik duvarınızı yönetmek için kapsamlı bir arayüz sunsa da, ileri düzey kullanıcılar sıklıkla SSH üzerinden temel FreeBSD sistemiyle etkileşime girecektir. Stratum işletim sistemini sizden saklamaz; onun üzerine inşa eder.

Stratum Firewall'ı yönetirken bilmeniz gereken temel komut satırı araçları şunlardır.

## 1. pfctl (Packet Filter Control)

`pfctl` komutu, `pf` güvenlik duvarına arayüzdür. Kuralları kontrol etmek, durumları görüntülemek ve istatistikleri görmek için kullanabilirsiniz.

Aktif kural setini görüntüleyin:

~~
sudo pfctl -sr
~~

Aktif bağlantıları (durumları) görüntüleyin:

~~
sudo pfctl -ss
~~

Güvenlik duvarı istatistiklerini gösterin:

~~
sudo pfctl -si
~~

Bir yapılandırma dosyasını sözdizimi hataları için test edin:

~~
sudo pfctl -nf /etc/pf.conf
~~

## 2. ifconfig (Arayüz Yapılandırması)

Ağ arayüzlerinizin durumunu görüntülemek, IP adreslerini kontrol etmek ve MAC adreslerini görmek için `ifconfig` kullanın.

Tüm arayüzleri listeleyin:

~~
ifconfig -a
~~

Belirli bir arayüz için ayrıntıları gösterin (örn. em0):

~~
ifconfig em0
~~

## 3. netstat ve sockstat

Bu araçlar, ağ bağlantılarını teşhis etmek ve hangi hizmetlerin hangi portlarda dinlediğini görmek için gereklidir.

Dinleyen TCP/UDP portlarını gösterin:

~~
sockstat -4 -l
~~

Yönlendirme tablosunu gösterin:

~~
netstat -rn
~~

## 4. service (Hizmet Yönetimi)

FreeBSD, rc.d betiklerini yönetmek için `service` komutunu kullanır.

Bir hizmetin durumunu kontrol edin (örn. WireGuard):

~~
sudo service stratum_wireguard status
~~

Paneli yeniden başlatın (arka uç değişiklikleri uygulamak için):

~~
sudo service stratum-panel restart
~~

## 5. tcpdump (Paket Yakalama)

Derinlemesine paket analizi için `tcpdump` yerleşiktir. Bunu arayüzün Teşhisler sayfasından da çalıştırabilirsiniz.

~~
sudo tcpdump -i em0 -n port 80
~~

## Sonraki Adımlar

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Dosya ve Yol Referansı</h3>
    <p>Stratum'un yapılandırmasını ve günlüklerini nerede sakladığını öğrenin.</p>
    <a href="/tr/resources/documentation/file-paths/" class="card-link">Dosya Referansı →</a>
  </div>
  <div class="card">
    <h3>Yardıma mı İhtiyacınız Var?</h3>
    <p>Takıldıysanız ekiple iletişime geçin.</p>
    <a href="/tr/company/contact/" class="card-link">Bize Ulaşın →</a>
  </div>
</div>
