---
title: "Yanıp Sönen Işıkların Ötesinde: Stratum FW ile Gerçek Ağ Görünürlüğü"
date: 2026-11-04
author: "Stratum Security Team"
description: "Geleneksel güvenlik duvarları sadece bir şeyler ters gittiğinde yanıp söner. Yeni v0.13.1 panel güncellemesiyle Stratum FW, gerçekten ihtiyacınız olan bağlamı sunar."
tags: ["panel", "izleme", "wan", "vpn", "guncelleme"]
---

Eğer daha önce geleneksel bir kurumsal güvenlik duvarı yönettiyseniz, o hissi bilirsiniz. Sunucu odasına girersiniz ve karşınızda yanıp sönen bir LED duvarı vardır. Biri yeşil, diğeri turuncu yanıp söner. Ama *neden* yanıp söndüğünü bilmezsiniz.

WAN bağlantısı mı koptu? Sadece yüksek paket kaybı mı yaşıyor? VPN tüneli açık ama aslında kimse bağlı değil mi?

**Stratum FW**'yi inşa ettik çünkü ağ güvenliğinin bir tahmin oyunu olmaması gerektiğine inanıyoruz. Ve en son güncellemelerimizle (**v0.13.0** ve **v0.13.1**), yanıp sönen ışıkların yerini eyleme dönüştürülebilir zekayla değiştiren tamamen yenilenmiş bir pano sunuyoruz.

## Yeni Pano (v0.13.0)

Stratum FW'ye giriş yaptığınızda artık sadece CPU ve bellek kullanımıyla karşılaşmıyorsunuz. Yeni pano, ağınız için bir komuta merkezi.

- **Durum Şeridi:** Ana bilgisayar adınızı, sürümünüzü, güncelleme durumunuzu ve her WAN bağlantısını canlı indirme/yükleme hızlarıyla anında görün.
- **Temel Rakamlar:** CPU, bellek, disk ve sıcaklık — 5 dakikalık eğrilerle ve uyarı seviyelerinde renk değiştiren göstergelerle.
- **Koruma Metrikleri:** IP karalisteleriniz tarafından son bir saatte tam olarak kaç paketin engellendiğini ve hangi kuralların işin ağır kısmını yaptığını görün.
- **En Çok Engellenen Kaynaklar:** Ağınıza en çok vurmaya çalışan IP'lerin ve portların canlı bir listesi.

## v0.13.1 Güncellemesi: WAN Sağlığı ve VPN Bağlamı

v0.13.0 panosu devasa bir sıçramaydı. Ancak hemen ardından, sistem yöneticilerinin özlemini çektiği derin bağlamı ekleyen **v0.13.1** ile geldik.

### WAN Sağlık Paneli
ISS'niz gerçekten ödediğiniz hızı sağlıyor mu? Yeni WAN paneli, **her 30 saniyede bir ağ geçidinize gecikme ve paket kaybını** canlı bir eğriyle ölçer. Ayrıca bağlantının ne kadar süredir açık olduğunu, bugünkü toplam indirme/yükleme miktarını ve canlı hızı görebilirsiniz.

### VPN Görünürlüğü
WireGuard veya OpenVPN'i kurmak bir şeydir, onu gerçekten kimin kullandığını bilmek başka bir şeydir. Pano artık bağlı olan her OpenVPN kullanıcısını ve aktif WireGuard eşini şunlarla gösterir:
- Bağlandıkları genel IP adresi.
- Atanan VPN adresleri.
- Ne zaman bağlandıkları (veya son el sıkışmaları).
- Alınan ve gönderilen toplam veri.

### Otomatik Güncelleme Kontrolleri
Ayrıca hayat kalitesini artıran bir özellik ekledik: Panoyu açmak artık arka planda yazılım güncellemelerini kontrol ediyor (en fazla 5 dakikada bir), tıpkı pfSense gibi. Yeni bir sürüm varsa bir rozet göreceksiniz.


## Sunucu Odasındaki Görünmez Kahraman: Kaynak Verimliliği

Geleneksel güvenlik duvarlarının gizli maliyetinden bahsetmemiz gerekiyor.

Geçtiğimiz günlerde **Intel Atom C2558** (2014'ten kalma, mütevazı bir 4 çekirdekli işlemci) donanımlı bir cihaza Stratum FW kurdum. Stratum FW altında, bu donanım IP karalistelerini, DNS filtrelemeyi ve WireGuard'ı çalıştırırken bile **%1 CPU** ve **%13 RAM** tüketimiyle boşta bekliyor.

Oysa bundan önce, tam olarak aynı donanım pfSense çalıştırıyordu. Gelişmiş özellikler aktif olmasa bile pfSense, sadece temel güvenlik duvarı görevlerini yerine getirmeye çalışırken sık sık CPU'yu sonuna kadar kullanıyor, çöküyor ve hata raporları oluşturuyordu.

Neden kaynak tüketimi arasında bu kadar devasa bir uçurum var?

**1. PHP/XML Vergisi vs. FastAPI/JSON**
pfSense, PHP tabanlı eski bir mimari ve devasa bir `config.xml` dosyası kullanır. Bir ayarı değiştirdiğinizde veya bir karalisteyi güncellediğinizde, tüm XML dosyasını baştan yazar, ayrıştırır ve çekirdekte tam bir `filter reload` (kural yeniden yüklemesi) tetikler. Zayıf bir CPU'da bu yıkıcı bir etki yaratır.
Stratum FW ise Python/FastAPI ve temiz bir `config.json` kullanır. Sadece değişiklikleri işler ve yerel olarak uygular.

**2. Karaliste Güncellemeleri Doğru Yapıldığında**
pfBlockerNG (pfSense), listeleri ayrıştırmak için Python betikleri kullanır, bunları SQLite'a yazar ve güvenlik duvarını yeniden yükler.
Stratum FW çok daha hafif bir yaklaşım benimser: listeleri indirir ve `pfctl -T replace` komutunu kullanarak `pf` çekirdek tablosunu atomik olarak değiştirir. Güvenlik duvarı yeniden yüklemesi yok, XML ayrıştırma yok, sıfır CPU sıçraması. İşte panelde 225.000'den fazla engellenen alan adını görüp de sistemin kılını bile kıpırdatmamasının nedeni bu.

**3. Arka Plan Şişkinliği Yok**
Stratum sadece ihtiyacı olanı çalıştırır. Gereksiz arka plan servisleri, sürekli yapılandırma kontrol döngüleri yoktur.

**Gelecekte daha da ağırlaşacak mı?**
Evet, tıpkı Suricata gibi derin paket incelemesi (DPI) eklemek her platformda CPU tüketir. Ancak bildirimsel mimarimiz sayesinde Stratum FW, geleneksel güvenlik duvarlarının ağırlığının çok küçük bir kısmında kalmaya devam edecek.

## Kaputun Altında: FreeBSD 15 Log Hatasının Düzeltilmesi
Bu güncelleme sadece yeni özelliklerle ilgili değildi. **v0.13.1**'de kritik bir hatayı da düzelttik. FreeBSD 15'te `pf`, günlüklerine kural numaralarını yazma şeklini değiştirdi (örn. "3..1/0"). Bu, güvenlik duvarı günlüklerimizin yanlış kural adları göstermesine neden oluyordu. Bu durumu çözmek için günlük ayrıştırıcımızı tamamen yeniden yazdık. Güvenlik Duvarı Günlüğü, pano ve uzak syslog artık tekrar tam doğru kuralı gösteriyor.

## Kendiniz görmeye hazır mısınız?
Her zaman istediğimiz güvenlik duvarını inşa ediyoruz. Hattın üzerinde tam olarak ne olduğunu bize söyleyen, tescilli CLI'ler veya bulut panoları arkasına saklanmayan güvenlik duvarı.

En son v0.13.1 ISO'yu indirin ve bir sanal makinede çalıştırın. Veya web sitemizdeki mimariyi inceleyin.

---

* [Stratum FW'yi İndir](/tr/download/)
* [Mimariyi İncele](/tr/product/architecture/)
