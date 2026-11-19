---
title: "Güvenlik Duvarı > Takma Adlar"
seo_title: "06.3 Güvenlik Duvarı > Takma Adlar (Firewall > Aliases) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarı kuralları ve NAT için adlandırılmış sunucu, ağ, port, alan adı ve ülke grupları - beş alias türü, otomatik güncellemeler, yeniden adlandırma ve silme."
weight: 603
chapter_num: "06.3"
url: "/tr/manual/firewall-aliases/"
aliases:
  - "/tr/manual/06-03-firewall-aliases/"
translationKey: "manual-06-03-firewall-aliases"
---

Bir takma ad (alias), bir adres veya port grubuna anlamlı bir isim verir. Güvenlik duvarı kuralları ve NAT kayıtları tek tek adresler yerine bu ismi kullanır; böylece:

- kurallar kısa ve okunaklı kalır - uzun bir IP listesi yerine *kaynak `Office_IPs`* yazılır;
- alias içinde yapılan tek bir değişiklik onu kullanan tüm kuralları ve NAT kayıtlarını anında günceller;
- güvenlik duvarı bazı takma adları kendi başına güncel tutar - alan adları düzenli aralıklarla yeniden çözülür, ülke IP listeleri her hafta indirilir.

![Firewall > Aliases](/images/manual/fw-aliases.png)

## Takma ad (Alias) türleri

| Tür | İçerik | Örnek | Kullanıldığı yerler |
| :--- | :--- | :--- | :--- |
| **Hosts** | Tekil IPv4 adresleri | `192.168.1.10`, `192.168.1.11` | Kuralların kaynak ve hedefi, port yönlendirme, giden NAT |
| **Networks** | CIDR biçiminde ağlar (istemci bitleri sıfır) | `10.0.0.0/8`, `192.168.50.0/24` | Hosts ile aynı yerlerde |
| **Ports** | Portlar ve port aralıkları | `443`, `8000:8010` | Kuralların hedef portu, port yönlendirmelerin dış portu |
| **Host names** | Alan adları (ve gerekiyorsa IP adresleri) | `update.example.com`, `203.0.113.10` | Hosts ile aynı yerlerde |
| **Countries** | Ülkeler | Türkiye, Almanya | Hosts ile aynı yerlerde |

Güvenlik duvarı **Host names** ve **Countries** türündeki takma adları kendiliğinden güncel tutar:

- **Host names** kayıtları güvenlik duvarı tarafından her 5 dakikada bir yeniden çözülür. Çözülen bir IP adresi son görüldüğü andan itibaren bir saat boyunca alias içinde tutulur; böylece IP adresleri sık sık değişen servisler (bulut servisleri, CDN'ler) kesintisiz çalışmaya devam eder. Alias başına en fazla 200 alan adı eklenebilir.
- **Countries** kayıtları ilgili ülkelere tahsis edilmiş tüm IP bloklarını beş bölgesel internet kayıt kuruluşundan (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC) her hafta güncelleyerek içerir. Herhangi bir hesap veya lisans anahtarı gerekmez. Alias başına en fazla 100 ülke seçilebilir.

Liste sayfasındaki **Update now** düğmesi alan adlarını ve ülke listelerini anında yeniler - örneğin bu türde yeni bir alias oluşturduktan hemen sonra kullanabilirsiniz.

## Takma ad listesi

Listenin üzerindeki sekmeler kayıtları türüne göre süzer (**All**, **Hosts**, **Networks**, **Ports**, **Host names**, **Countries**). Her satırda takma adın adı (**Name**), türü (**Type**), içeriği (**Entries** - alan adları ve ülkeler için o anda kaç adres tuttukları da gösterilir), açıklaması (**Description**) ve onu kullanan kurallar ile NAT kayıtları (**Used by**) yer alır.

## Takma ad ekleme

**Add alias** düğmesine tıklayın.

| Alan | Anlamı |
| :--- | :--- |
| **Name** | Harfle başlayan, en fazla 31 karakter uzunluğunda harf, rakam ve `_` - örneğin `Office_IPs`, `Web_Ports`. Bir arayüz adı (`lan`, `opt2` ...) veya ayrılmış bir sözcük (`any`, `self`, `bogons`, `private`) olamaz. Kurallar bu takma ada bu isimle başvurur. |
| **Type** | Yukarıdaki beş türden biri. |
| **Entries** | Her satıra bir kayıt girin. **Countries** türünde ise ülkeleri doğrudan listeden seçin. |
| **Description** | İsteğe bağlı açıklama notunuz. |

**Save** düğmesine, ardından liste sayfasında **Apply changes** düğmesine tıklayın.

## Sık kullanılan takma ad örnekleri

| Alias | Tür | Kullanım amacı |
| :--- | :--- | :--- |
| `RFC1918` | Networks: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | *Misafirlerden RFC1918'e giden trafiği reddet* - bir ağı mevcut ve ileride eklenecek tüm özel iç ağlardan uzak tutar. |
| `Office_IPs` | Hosts veya Networks: şube ve iş ortaklarınızın sabit genel IP adresleri | Uzak masaüstü, SSH veya kamera kayıt cihazı için açılan port yönlendirmelerin ve WAN kurallarının izin verilen kaynağı. |
| `Web_Ports` | Ports: `80`, `443` | Web trafiği için iki ayrı kural yerine tek kural yazma. |
| `Turkiye` | Countries: Türkiye | *Rezervasyon sunucusunun port yönlendirmesine yalnızca Türkiye'den erişilebilsin*: port yönlendirmenin izin verilen kaynağı olarak kullanılır. |
| `Backup_Server` | Host names: `backup.example.com` | Uzak sunucunun IP adresi değiştiğinde bile onu otomatik izleyen kural. |

> **İpucu:** Ülke takma adları kaba bir süzgeçtir - bir saldırgan herhangi bir ülkedeki kiralık sunucuyu kullanabilir ve bazı gerçek kullanıcılar yurt dışına seyahat edebilir. Ülke filtrelerini bir servisin tek koruması olarak değil, tarama gürültüsünü ve saldırı yüzeyini azaltmak için kullanın.

## Değiştirme, yeniden adlandırma veya silme

- **Yeniden adlandırma:** bir takma adın ismini değiştirmek onu kullanan tüm güvenlik duvarı kurallarını ve NAT kayıtlarını otomatik günceller.
- **Tür değişikliği:** kullanımda olan bir takma adın türü değiştirilemez - *The type of an alias that is in use cannot be changed.*
- **Silme:** kullanımda olan bir takma ad silinemez; panel nerede kullanıldığını açıkça listeler, örneğin *Alias Office_IPs is in use: WAN rule #2, Port forward #1*. Düzenleme sayfası da aynı listeyi **Used by** altında gösterir.
