---
title: "Güvenlik Duvarı > NAT"
seo_title: "06.2 Güvenlik Duvarı > NAT (Firewall > NAT) — Stratum FW Kullanım Kılavuzu"
description: "Ağ adresi çevirisi (NAT): iç sunuculara port yönlendirme, giden NAT kipleri ve kuralları, 1:1 NAT ve NAT yansıması (NAT reflection)."
weight: 602
chapter_num: "06.2"
url: "/tr/manual/firewall-nat/"
aliases:
  - "/tr/manual/06-02-firewall-nat/"
translationKey: "manual-06-02-firewall-nat"
---

NAT (ağ adresi çevirisi), paketler güvenlik duvarından geçerken kaynak veya hedef IP adreslerini yeniden yazar. **Firewall > NAT** sayfasında üç sekme bulunur:

| Sekme | İşlevi | Tipik kullanım |
| :--- | :--- | :--- |
| **Port Forward** | Güvenlik duvarının bir portuna gelen bağlantıları içerideki bir sunucuya iletir. | Dışarıdan bir web sunucusuna, kamera kayıt cihazına veya uzak masaüstüne erişim sağlama. |
| **Outbound** | Bir WAN hattından internete çıkan trafiğin kaynak adresini yeniden yazar. | Tüm iç ağ cihazlarının tek bir WAN adresini paylaşması (otomatik); bir sunucunun kendine ait genel IP adresiyle çıkması. |
| **1:1** | Bir genel IP adresini içerideki bir sunucuyla her iki yönde birebir eşleştirir. | Tüm gelen ve giden trafikte kendine ait genel IP adresine ihtiyaç duyan sunucular. |

## Port yönlendirme (Port forwards)

Port yönlendirme içerideki bir servisi dış dünyadan erişilebilir kılar: güvenlik duvarı üzerindeki **dış porta** (**external port**) gelen bağlantılar içerideki **hedef** (**target**) sunucuya ve porta iletilir.

![Firewall > NAT > Port Forward](/images/manual/nat-forward.png)

**Her port yönlendirme kendi geçiş izni kuralını otomatik alır** - ilgili arayüzün [**Firewall > Rules**](/tr/manual/firewall-rules/) sekmesinde gri renkli bir sistem kuralı olarak görünür. Ayrıca elle bir izin kuralı yazmanız gerekmez.

### Port yönlendirme ekleme

**Add port forward** düğmesine tıklayın.

| Alan | Anlamı |
| :--- | :--- |
| **Interface** | Trafiğin geldiği arayüz - normalde WAN. |
| **Destination address** | Arayüzün kendi adresi veya sanal IP adreslerinden biri ([**Interfaces > Virtual IPs**](/tr/manual/interfaces-virtual-ips/)) - böylece farklı genel IP adresleri aynı portu içerideki farklı sunuculara yönlendirebilir. |
| **Protocol** | TCP, UDP veya TCP/UDP. |
| **External port** | **Port or range** (`8443`, `5000:5010`) veya **Port alias**: dış dünyanın bağlandığı port. |
| **Target IP address** | Trafiği alacak iç sunucunun IP adresi. |
| **Target port** | İç sunucu üzerindeki port. Boş bırakılırsa dış port ile aynı kabul edilir. Port aralığı girildiğinde aralık bu porttan başlayacak şekilde kaydırılır. Bir port takma adıyla (port alias) birlikte farklı bir hedef port kullanılamaz. |
| **Allowed source** | **Any**, tek bir adres veya ağ, bir alias ya da bir arayüz ağı; *belirtilen hariç herkes* için **Invert match** seçeneğiyle. Mümkün olan her durumda kaynağı sınırlandırın: tüm internete açık bırakılan bir servis birkaç saat içinde tarayıcı botlar tarafından bulunur. |
| **Description** | Açıklama notunuz. |
| **Log connections that match** | Yönlendirme üzerinden geçen her bağlantıyı günlüğe kaydeder. |
| **Port forward is enabled** | Kaydı silmeden geçici olarak kapatmanızı sağlar. |

Örnek: **External port** `8443`, **Target IP address** `192.168.1.10`, **Target port** `443` - dışarıdan `https://<genel IP adresiniz>:8443` adresini açan bir ziyaretçi LAN içindeki web sunucusuna ulaşır.

**Save**, **Apply changes** ve **Keep changes** düğmelerine tıklayın.

### Sıralama ve ayırıcılar

İlk eşleşen yönlendirme kuralı uygulanır; kayıtları oklarla veya sürükleme tutamağından taşıyabilirsiniz. Ayırıcılar (separators) işleyişi değiştirmeden listeyi görsel olarak gruplar. En üstteki gri kayıtlar Stratum FW'nin diğer ayarlardan ürettiği yönlendirmelerdir: örneğin güvenlik duvarının kendi DNS servisini kullanması zorunlu tutulan ağlar için *Force DNS* (**Services > DNS Resolver**, **Diagnostics > Web Activity**).

### Kaynak adresini sınırlandırma

**Allowed source** alanı bir takma ada (örneğin şube ve iş ortaklarınızın sabit IP'lerini içeren `Office_IPs` alias kaydına) ayarlanan bir port yönlendirmeye yalnızca o adreslerden ulaşılabilir; diğer herkese port tamamen kapalıymış gibi davranılır. Yalnızca belirli noktaların erişmesi gereken servisler (uzak masaüstü, NVR kayıt cihazı, SSH sunucusu) için tek başına uygulanabilecek en etkili koruma budur.

### NAT yansıması (NAT reflection)

LAN içindeki cihazlar bir port yönlendirmenin *genel* (public) adresini açtığında (örneğin WAN adresinize işaret eden `https://bulut.ornek.com`) varsayılan olarak iç sunucuya ulaşamaz. Bunun çalışması için **System > Settings > Firewall** altındaki **NAT reflection: local devices can use the public address of a port forward** seçeneğini açabilirsiniz. Mümkün olduğunda daha temiz alternatif ise **Services > DNS Resolver** altından yerel cihazlar için ilgili alan adını doğrudan sunucunun iç IP adresine çözen bir DNS kaydı (override) eklemektir.

## Giden NAT (Outbound NAT)

Giden NAT, güvenlik duvarından internete çıkan trafiğin kaynak IP adresini yeniden yazar; böylece özel (private) IP adresli iç ağ cihazları WAN arayüzünün genel IP adresini paylaşabilir.

![Firewall > NAT > Outbound](/images/manual/nat-outbound.png)

### Çalışma kipi (Mode)

| Kip | Anlamı |
| :--- | :--- |
| **Automatic** | Tüm iç arayüzlerin ağları ve arkalarında yönlendirilen alt ağlar ([**Interfaces > Routes**](/tr/manual/interfaces-routes/)), çıktıkları her WAN hattının adresine otomatik çevrilir. Varsayılan kiptir ve neredeyse her kurulum için doğrudur. |
| **Hybrid** | Önce sizin yazdığınız özel kurallar, ardından otomatik kurallar değerlendirilir. Genel yapıyı otomatik tutarken belirli sunucular için istisna tanımlamak istediğinizde kullanılır. |
| **Manual** | Yalnızca sizin yazdığınız kurallar kullanılır. Listelemediğiniz hiçbir ağ çevrilmez ve internete çıkamaz. Liste boşken Manual kipine geçerseniz mevcut bağlantıların aniden kesilmemesi için otomatik kurallar listeye kopyalanır. |
| **Disabled** | Hiçbir giden adres çevirisi yapılmaz - genel veya yönlendirilmiş IP blokları arasında çalışan yönlendiriciler içindir. |

İstediğiniz kipi seçip **Save mode** düğmesine tıklayın. Alttaki **Default rules (automatic)** tablosu otomatik kipin ürettiği kuralları gösterir ve arayüzler değiştikçe kendiliğinden güncellenir.

### Giden NAT kuralları (Hybrid ve Manual)

| Alan | Anlamı |
| :--- | :--- |
| **Interface** | Trafiğin dışarı çıktığı arayüz - normalde WAN. |
| **Source**, **Destination** | Any, tek bir sunucu veya ağ, bir alias ya da bir arayüz ağı (**Invert match** destekli). |
| **Translate to** | **Interface address** - arayüzün mevcut IP adresi; **Specific address** - sabit bir genel IP adresi, örneğin ek IP bloğunuzdan tanımladığınız bir sanal IP. |
| **Description**, **Rule is enabled** | Standart açıklama ve etkinleştirme alanları. |

Kurallar yukarıdan aşağıya değerlendirilir; ilk eşleşen kural kazanır.

Tipik kullanım: bir e-posta sunucusunun DNS (SPF/PTR) kayıtlarında belirtilen genel IP adresinden çıkış yapması gerekiyorsa, o adres için bir sanal IP ekleyin, giden NAT kipini **Hybrid** yapın ve kaynak olarak e-posta sunucusunu, hedef çeviri adresi olarak sanal IP'yi seçen bir kural ekleyin. Diğer tüm cihazlar ana WAN adresini kullanmaya devam eder.

## 1:1 NAT

1:1 NAT bir genel IP adresini içerideki bir adresle her iki yönde eşleştirir: iç sunucu dışarıya bu genel adresle çıkar ve o genel adrese gelen trafik doğrudan iç sunucuya yönlendirilir.

| Alan | Anlamı |
| :--- | :--- |
| **Interface** | Genel IP adresinin ait olduğu WAN arayüzü. |
| **External address** | Tek bir adres veya servis sağlayıcının size yönlendirdiği bir alt ağ (`203.0.113.16/29`). Tekil bir adres kullanılıyorsa WAN üzerinde sanal IP olarak tanımlanmış olmalıdır. WAN arayüzünün kendi ana adresi 1:1 NAT için kullanılamaz. |
| **Internal address** | Tek bir iç sunucu veya dış ağ ile aynı büyüklükte bir iç ağ (`203.0.113.16/29` için `192.168.1.16/29`). |
| **Description**, **Entry is enabled** | Standart açıklama ve etkinleştirme alanları. |

Listedeki **Incoming** sütunu iç adrese giden kaç adet WAN izin kuralı bulunduğunu, hiç kural yoksa *closed* (kapalı) durumunu gösterir.

> **Uyarı:** 1:1 NAT tek başına dışarıdan hiçbir portu açmaz. Sunucunun dış dünyaya sunması gereken portlar için WAN sekmesinde hedefi sunucunun **iç (internal)** adresi olan *Pass* kuralları ekleyin.

Belirli portlar için tanımlı port yönlendirmeleri 1:1 NAT'a göre önceliklidir; giden trafikte ise sunucu ana WAN adresi yerine kendi 1:1 genel adresini kullanır. İkinci bir WAN hattı üzerindeki 1:1 eşleştirmede sunucunun giden trafiği yalnızca o WAN hattından çıktığında ilgili genel adresi alır - bunun için sunucunun bağlı olduğu iç arayüzdeki kuralına ağ geçidi (politika yönlendirmesi) ekleyin.
