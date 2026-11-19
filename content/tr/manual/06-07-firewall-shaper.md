---
title: "Güvenlik Duvarı > Bant Genişliği (Shaper)"
seo_title: "06.7 Güvenlik Duvarı > Bant Genişliği (Firewall > Shaper) — Stratum FW Kullanım Kılavuzu"
description: "FreeBSD dummynet ile iç ağ başına (tüm ağ ortak veya cihaz başına ayrı) indirme ve yükleme hız sınırları ve Captive Portal cihaz limitleriyle birlikte çalışma mantığı."
weight: 607
chapter_num: "06.7"
url: "/tr/manual/firewall-shaper/"
aliases:
  - "/tr/manual/06-07-firewall-shaper/"
translationKey: "manual-06-07-firewall-shaper"
---

Bant Genişliği Yöneticisi (**Shaper**), bir iç ağın her iki yönde ne kadar bant genişliği kullanabileceğini sınırlar. Tipik kullanım senaryoları şunlardır:

- misafir Wi-Fi ağının toplam kullanımı hiçbir zaman 100 Mbit/s değerini aşmaz; böylece personel ağı için her zaman kapasite kalır;
- her misafir telefonu en fazla 20 Mbit/s hız alır; böylece tek bir indirme işlemi diğer herkesi yavaşlatmaz;
- kamera ağının yükleme (upload) trafiği internet hattının taşıyabileceği kapasitenin altında tutulur.

Hız sınırları paket filtresi üzerinden doğrudan FreeBSD'nin **dummynet** altyapısını kullanır. Güvenlik duvarı kurallarınızda başka hiçbir değişiklik yapmanız gerekmez.

![Firewall > Shaper](/images/manual/fw-shaper.png)

## Hız sınırı tanımlama

**Firewall > Shaper** sayfası tüm iç ağları tek bir tabloda listeler. Her ağ için şu sütunlar yer alır:

| Sütun | Anlamı |
| :--- | :--- |
| **Network** | İç ağ arayüzü (her arayüz için bir sınır satırı bulunur). |
| **On** | Hız sınırını etkinleştirir. |
| **Download (Mbit/s)** | Ağdaki cihazların aldığı (indirdiği) trafik hızı. Boş bırakılırsa veya `0` girilirse bu yönde sınır uygulanmaz. Ondalık değerler desteklenir (`0.1` ile `100000` arası). |
| **Upload (Mbit/s)** | Ağdaki cihazların gönderdiği (yüklediği) trafik hızı. Boş bırakılırsa veya `0` girilirse sınır uygulanmaz. |
| **Applies to** | **Whole network** - ağdaki tüm cihazlar sınırı ortaklaşa paylaşır; örneğin ofis VLAN ağı toplamda hiçbir zaman 255 Mbit/s üstüne çıkmaz. **Each device** - ağdaki her cihaz bu sınırı tek başına alır; örneğin misafir telefonu başına 20 Mbit/s. |
| **Note** | Sınır için açıklama notunuz. |

Bir sınır etkinleştirildiğinde en az bir yön (Download veya Upload) dolu olmalıdır. **Save** düğmesine tıklayın, ardından **Apply changes** ve **Keep changes** ile onaylayın.

## Sınırların çalışma davranışı

- **Download** ağdaki cihazların aldığı trafiği, **upload** ise cihazların gönderdiği trafiği ifade eder - yön her zaman ağdaki cihazların bakış açısından okunur.
- **İki yerel ağ arasındaki trafik de sınıra dahildir.** Sunucu ağından sınırlandırılmış bir misafir ağına kopyalanan dosya da misafir ağının hız sınırına tabi tutulur.
- **Each device** seçeneği ağdaki her IP adresine belirlenen hızda bağımsız bir kuyruk verir; sınır değeri ağdaki cihaz sayısına göre büyümez veya küçülmez. **Whole network** ise tüm cihazlar arasında tek bir kuyruğu paylaştırır.
- Sınırlar kota değil anlık hız (rate) limitidir: günlük veya aylık veri tüketimi sayılmaz.

> **İpucu:** Paylaşılan bir internet hattının her zaman düşük gecikmeli kalması için yoğun ağlara hattın gerçek kapasitesinin biraz altında (örneğin ölçülen hızın %90'ı kadar) bir *Whole network* sınırı tanımlayın. Kuyruk servis sağlayıcının modeminde şişmek yerine güvenlik duvarı üzerinde yönetildiğinde herkes için gecikme (ping) düşük kalır.

## Captive Portal ile birlikte kullanım

Captive Portal kendi cihaz başına hız sınırlarını tanımlayabilir - bölgenin (zone) varsayılan indirme ve yükleme hız sınırları ya da bir RADIUS sunucusunun kullanıcı için gönderdiği hız değerleri (**Services > Captive Portal**). Oturum açmış bir cihaz için portalın belirlediği hız sınırı, o ağın Shaper sınırının yerini alır. Davranışın kolay öngörülebilir olması için belirli bir ağda iki yöntemden yalnızca birini tercih edin.

## Sınırları çalışırken izleme

[Kontrol panelindeki](/tr/manual/dashboard/) **Bandwidth limits** kartı tanımlı her sınırı o ağın anlık kullanım hızıyla birlikte gösterir. **Diagnostics > Traffic Graphs** ise bir arayüzün zaman içindeki trafik grafiğini sunar.
