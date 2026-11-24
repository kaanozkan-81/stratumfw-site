---
title: "Servisler > DNS Çözümleyici"
seo_title: "07.3 Servisler > DNS Çözümleyici (Services > DNS Resolver) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarının DNS servisi: yerel alan adı, üst DNS sunucuları, dnsmasq veya Unbound seçimi, yönlendirici veya kök çözümleyici modu, DNSSEC, DNS sorgularını güvenlik duvarına zorlama, ana makine ve alan adı geçersiz kılmaları."
weight: 703
chapter_num: "07.3"
url: "/tr/manual/services-dns-resolver/"
aliases:
  - "/tr/manual/07-03-services-dns-resolver/"
translationKey: "manual-07-03-services-dns-resolver"
---

Ağlarınızdaki cihazlar alan adlarını güvenlik duvarına sorar; güvenlik duvarı ise ya kendi yerel kayıtlarından yanıt verir ya da internete sorar. **Services > DNS Resolver** bu sürecin nasıl işleyeceğini yönetir.

DNS yapılandırmasını doğru kurgulamak göründüğünden çok daha önemlidir: DNS engelleme listeleri, Web Filtresi (**Keywords** dahil), [**Uygulama Kontrolü**](/tr/manual/firewall-application-control/) engelleme sayfası ve Web Activity görünürlüğünün tamamı güvenlik duvarının çözümleyicisi üzerinden çalışır; başka bir DNS sunucusuna soru soran bir cihaz DNS tabanlı filtrelerin ve engelleme sayfasının dışında kalır.

![Services > DNS Resolver](/images/manual/dns.png)

## Çözümleyici ayarları (Resolver settings)

| Ayar | Anlamı |
| :--- | :--- |
| **Local domain** | İç ağlarınızın yerel alan adı (varsayılan `lan`). DHCP istemcilerine ve statik eşlemelere `isim.<alanadı>` biçiminde erişilir ve bu alan adına yönelik sorgular hiçbir zaman güvenlik duvarının dışına çıkmaz. **System > Settings > General** altındaki ayarla aynıdır. |
| **Upstream DNS servers** | Güvenlik duvarının sorgu yönlendireceği üst DNS sunucuları; virgülle ayrılmış en fazla dört adres. Varsayılan `1.1.1.1, 8.8.8.8`. Unbound kullanıldığında IP adresi biçiminde olmalıdır. |
| **Use only these upstream servers** | Güvenlik duvarının başka kaynaklardan (örneğin DHCP WAN hattından) öğreneceği DNS sunucularını yok sayar. Varsayılan olarak açıktır. |
| **DNS resolver** | **dnsmasq** - DNS ve DHCP tek serviste birleşir; veya **Unbound** - doğrulama yapan gelişmiş çözümleyici (`dnsmasq` arka planda yalnızca DHCP dağıtmaya devam eder). |
| **Unbound mode** | **Forwarder** - sorguları yukarıdaki üst DNS sunucularına iletir (en az bir üst sunucu gerekir); **Resolver** - aracı bir DNS sağlayıcısı kullanmadan doğrudan internetin kök (root) sunucularına sorar. |
| **Validate DNSSEC signatures** | (Unbound.) DNSSEC imzası hatalı olan yanıtları reddeder - imzalı alan adları için sahte DNS yanıtlarına karşı koruma sağlar. |

### dnsmasq mı yoksa Unbound mı?

| Özellik | dnsmasq | Unbound |
| :--- | :--- | :--- |
| İç ağlar için DNS ve yerel cihaz adları | evet | evet |
| DNS engelleme listeleri, Web Filtresi ve Keywords, Uygulama Kontrolü engelleme sayfası, Web Activity | hayır | **evet** |
| DNSSEC doğrulaması | hayır | evet |
| Doğrudan kök sunuculardan çözümleme (aracı DNS sağlayıcısız) | hayır | evet |
| Bellek kullanımı | çok düşük | daha yüksek (aktif engelleme listeleri ve Web Filtresi kategorileriyle artar) |

Filtreleme yapmasını istediğiniz her güvenlik duvarında **Unbound** seçin. `dnsmasq` yalnızca yönlendirme yapan sade kurulumlar için en hafif seçenektir. Her iki durumda da DHCP dağıtımını `dnsmasq` sürdürdüğü için çözümleyici değiştirmek ağdaki cihazların IP adreslerine dokunmaz.

**Forwarder mı yoksa Resolver mı?** *Forwarder* modunda güvenlik duvarı her sorguyu üst DNS sunucularına iletir - hızlıdır ancak üst DNS sağlayıcısı sorgulanan alan adlarını görür. *Resolver* modunda ise güvenlik duvarı yetkili isim sunucularına doğrudan kendisi sorar - arada üçüncü taraf DNS sağlayıcısı yoktur; yalnızca ilk kez sorulan bir alan adının çözülmesi birkaç milisaniye daha uzun sürebilir.

**Save settings**, **Apply changes** ve **Keep changes** düğmelerine tıklayın.

## DNS sorgularını güvenlik duvarına zorlama (Force DNS through the firewall)

Bir cihaz DHCP'den aldığı DNS sunucusunu yok sayıp doğrudan `8.8.8.8` adresine soru sorabilir - kendi ağ ayarından, bir uygulamanın gömülü DNS adresinden ya da filtreleri aşmak amacıyla. **Force DNS through the firewall** seçtiğiniz ağlar için bu kapıyı kapatır:

- Bu ağlardan çıkan tüm standart DNS sorguları (port 53), cihaz hangi sunucuya sorarsa sorsun şeffaf biçimde güvenlik duvarının çözümleyicisine yönlendirilir - cihaz farkı hissetmez.
- Bu ağlardan çıkan DNS over TLS (port 853) trafiği engellenir; böylece cihazlar standart DNS'e geri döner.
- Bu ayarın kendisi ek bir günlük kaydı tutmaz.

**Except DNS servers:** cihazların doğrudan sorgulamasına izin verilen DNS sunucularını içeren bir takma ad (alias) - örneğin istemcilerin kendi DNS sunucusu olarak doğrudan erişmesi gereken Active Directory etki alanı denetleyicileri (domain controller).

[**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/) altında **Send all DNS to the firewall** etkinse sayfa bunu belirtir: bu durumda zorunlu yönlendirme tüm iç ağlara uygulanır.

DNS over HTTPS (DoH) normal HTTPS trafiğiyle aynı göründüğü için şeffaf yönlendirilemez. Genel DoH sunucularını DNS engelleme listelerindeki ([**Firewall > Blocklists**](/tr/manual/firewall-blocklists/)) **DoH** kategorisiyle veya Web Filtresindeki *DNS over HTTPS servers* kategorisiyle engelleyin.

Oluşturulan yönlendirme kuralları [**Firewall > NAT > Port Forward**](/tr/manual/firewall-nat/) sayfasında gri *Force DNS* satırları olarak görünür.

## Ana makine geçersiz kılmaları (Host overrides)

Bir *host override* kaydı tam olarak belirttiğiniz tek bir alan adına seçtiğiniz IP adresiyle yanıt verilmesini sağlar.

Tipik kullanım senaryoları:

- genel DNS kaydı olmayan bir iç sunucu - örneğin `nas.office.local`;
- dışarıda genel IP adresine çözülen bir adın içerideki cihazlar için doğrudan yerel IP adresine çözülmesi - örneğin `cloud.example.com` adresinin WAN IP'si yerine `192.168.1.20` döndürmesi. Bu yöntem NAT yansımasından ([**Firewall > NAT**](/tr/manual/firewall-nat/)) çok daha temizdir.

**Add host override** alanları:

| Alan | Anlamı |
| :--- | :--- |
| **Host** | Adın ilk bölümü, örneğin `nas` (harf, rakam ve kısa çizgi). Boş bırakılabilir: boş bırakıldığında doğrudan alan adının kendisine yanıt verir. |
| **Domain** | Adın kalan bölümü, örneğin `office.local`. |
| **IP address** | Döndürülecek IP adresi. |
| **Full name** | Bilgi amaçlı tam ad önizlemesi: `nas.office.local`. |
| **Description** | Açıklama notunuz. |

Yalnızca tam eşleşen ad etkilenir; aynı alan adındaki diğer kayıtlar normal biçimde çözülmeye devam eder. Her tam ad için bir kayıt tanımlanabilir.

## Alan adı geçersiz kılmaları (Domain overrides)

Bir *domain override* kaydı belirli bir alan adına - ve tüm alt alan adlarına - gelen her sorguyu seçtiğiniz başka bir DNS sunucusuna iletir.

Tipik kullanım senaryoları:

- bir **Active Directory** etki alanı: `corp.example.local` sorgularını etki alanı denetleyicisine (DC) göndererek etki alanına üye bilgisayarların servislerini bulmasını sağlamak;
- **VPN üzerindeki karşı şube adları**: `branch.lan` sorgularını şube ofisin DNS sunucusuna yönlendirmek.

**Add domain override** alanları:

| Alan | Anlamı |
| :--- | :--- |
| **Domain** | Alt alan adları dahil yönlendirilecek alan adı. |
| **DNS server** | Bu alan adına yanıt veren sunucunun IPv4 adresi. |
| **Description** | Açıklama notunuz. |

Geçersiz kılma kayıtlarındaki değişiklikler **Apply changes** sonrasında etkinleşir. Cihazlar eski yanıtları birkaç dakika önbellekte tutabilir.

## DNS çözümlemeyi sınama

- **Diagnostics > Ping / Traceroute / DNS** aracı bir alan adını doğrudan güvenlik duvarının çözümleyicisi üzerinden sorgular.
- [**Firewall > Blocklists**](/tr/manual/firewall-blocklists/) ve [**Firewall > Web Filter**](/tr/manual/firewall-web-filter/) sayfalarının her birinde bir alan adının engellenip engellenmediğini ve nedenini gösteren sorgulama araçları yer alır.
- İstemci bir cihazda `nslookup <alanadı>` komutu çalıştırıldığında yanıt veren sunucu olarak güvenlik duvarı görünmelidir.
