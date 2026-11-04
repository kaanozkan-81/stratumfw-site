---
title: "Temel Engellemenin Ötesinde: v0.13.2 ile Proaktif Uyarılar ve Derin Görünürlük"
date: 2026-11-04
author: "Stratum Security Team"
description: "Bir güvenlik duvarı sadece paketleri engellememeli; neler olup bittiğini de size söylemelidir. En son Stratum FW güncellemesindeki yeni WAN sağlığı, VPN görünürlüğü ve proaktif uyarı özelliklerini keşfedin."
tags: ["panel", "uyarilar", "izleme", "vpn", "dhcp"]
---

On yıllardır geleneksel güvenlik duvarları reaktiftir. Ağınızın kenarında sessizce otururlar, kötü trafiği engellerler ve varlıklarını sadece internet koptuğunda veya bir kural bir şeyi bozduğunda hissettirirler.

Eğer İnternet Servis Sağlayıcınız (ISS) korkunç bir gecikme (latency) sağlıyorsa veya bir WireGuard eşi günlerce sessizce bağlantıdan düşmüşse, bir kullanıcı şikayet edene kadar bunu bilemezdiniz.

En son güncellemelerle (**v0.13.1** ve **v0.13.2**), bunu değiştiriyoruz. Stratum FW panelini proaktif bir ağ komuta merkezine dönüştürüyoruz.

![Stratum FW v0.13.2 Paneli](/images/docs/dashboard.png)
*WAN sağlığını, trafiği ve koruma istatistiklerini gösteren yeni Stratum FW paneli.*


## 1. WAN Sağlığı: ISS'niz Gerçekten Sözünü Tutuyor mu?
Belirli bir internet hızı ve güvenilirliği için para ödüyorsunuz. Peki ISS'nin üzerine düşeni yapıp yapmadığını nasıl bileceksiniz?

v0.13.1'deki yeni **WAN Paneli**, **her 30 saniyede bir ağ geçidinize gecikme ve paket kaybını** ölçer ve canlı bir eğriyle görselleştirir. Ayrıca bağlantının ne kadar süredir açık olduğunu, bugünkü toplam indirme/yükleme miktarını ve canlı hızı görebilirsiniz. Eğer birincil fiber hattınız %10 paket kaybıyla bozulmaya başlarsa, bunu panoda anında görürsünüz.

## 2. VPN Görünürlüğü: Aslında Kim Bağlı?
WireGuard ve OpenVPN'i kurmak kolaydır. Ancak düzinelerce uzaktan çalışanınız olduğunda onları yönetmek başka bir hikayedir.

Güncellenen pano artık **bağlı olan her OpenVPN kullanıcısını ve aktif WireGuard eşini** listeliyor. Anında şunları görebilirsiniz:
- Bağlandıkları genel IP adresi.
- Atanan dahili VPN adresleri.
- Ne zaman bağlandıkları (veya son el sıkışmaları).
- Alınan ve gönderilen toplam veri.

Artık bir kullanıcının bağlı olup olmadığını veya ele geçirilmiş bir cihazın arka planda sessizce veri indirip indirmediğini tahmin etmeye gerek yok.

## 3. Proaktif Uyarılar: Kırılmadan Önce Bana Söyle (v0.13.2)
v0.13.2'nin en önemli özelliği yeni **Pano Uyarıları (Alerts)** sistemidir.

Artık Stratum FW'yi kritik olaylar için **Telegram veya E-posta** (SMTP) yoluyla uyarı gönderecek şekilde yapılandırabilirsiniz:
- WAN bağlantısı kesildi veya geri geldi (ve yeni WAN IP adresleri).
- Engellenen girişler (brute-force denemeleri).
- Başarısız uygulamalar veya otomatik geri almalar.
- Disk neredeyse dolu, CPU sıcaklığı yüksek.
- IP/DNS karaliste indirme hataları.

Olaylar panoda bir listede kaydedilir (son 100 tanesi tutulur); zamanını ve e-posta veya Telegram ile gönderilip gönderilmediğini gösterir. İnternet kesilirse, Stratum yedek WAN devreye girdiği anda size uyarı göndermeye çalışacaktır.

## 4. DHCP ve IP Yönetimi
Yine v0.13.2'de pano artık bir DHCP paneli içeriyor. Şunları izleyebilirsiniz:
- Ağ başına aktif kiralamalar.
- Adres havuzunun ne kadarının kullanıldığı (%75'te turuncuya, %90'da kırmızıya döner).
- Statik eşlemelerin sayısı.

Bu, yoğun bir ofis aniden DHCP kiralamalarını tükettiğinde yaşanan klasik "IP adresi alamıyorum" paniğini önler.

## Felsefe: Yalın, Güçlü ve Her Zaman İzleyen
Derin ve eyleme dönüştürülebilir ağ görünürlüğü elde etmek için devasa bir bulut aboneliğine veya kurumsal bir destek sözleşmesine ihtiyacınız yok. Stratum FW hafif kalıyor (10 yıllık bir Intel Atom'da çalışıyor!) ancak artık ağınızı kurumsal bir NGFW'nin gözleriyle izliyor.

İş başında görmeye hazır mısınız? En son ISO'yu indirin veya bir sanal makinede çalıştırın.

---

* [Stratum FW'yi İndir](/tr/download/)
* [Mimariyi İncele](/tr/product/architecture/)
