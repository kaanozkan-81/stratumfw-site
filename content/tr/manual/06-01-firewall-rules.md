---
title: "Güvenlik Duvarı > Kurallar"
seo_title: "06.1 Güvenlik Duvarı > Kurallar (Firewall > Rules) — Stratum FW Kullanım Kılavuzu"
description: "Arayüz ve grup bazında güvenlik duvarı kuralları: kuralların değerlendirilme sırası, sistem kuralları, tüm kural alanları, ayırıcılar, eşleşme sayaçları, zamanlamalar, politika yönlendirme ve en iyi uygulamalar."
weight: 601
chapter_num: "06.1"
url: "/tr/manual/firewall-rules/"
aliases:
  - "/tr/manual/06-01-firewall-rules/"
translationKey: "manual-06-01-firewall-rules"
---

Güvenlik duvarı kuralları hangi trafiğin geçebileceğine karar verir. **Firewall > Rules** sayfasında her arayüzün - ve her arayüz grubunun - kendine ait ayrı bir sekmesi ve kural listesi bulunur.

![Firewall > Rules](/images/manual/fw-rules.png)

## Kurallar nasıl değerlendirilir

- **Kurallar bir arayüze giren trafiğe uygulanır.** *LAN* sekmesindeki bir kural, LAN tarafından güvenlik duvarına giren paketlere - yani LAN içindeki cihazların başlattığı bağlantılara - bakar. Misafir ağındaki cihazların nereye ulaşabileceğini kontrol etmek için kuralları ulaşmak istedikleri ağın sekmesine değil, misafir ağının kendi sekmesine yazın.
- **Yukarıdan aşağıya değerlendirilir, ilk eşleşen kural kazanır.** Koşulları uyan ilk kural kararı verir; altındaki kurallara bakılmaz.
- **Yanıt paketlerine otomatik izin verilir.** Güvenlik duvarı geçişine izin verdiği her bağlantıyı durum tablosunda (*state*) izler; izin verilmiş bir bağlantının dönüş paketleri ayrı bir kurala gerek kalmadan geçer.
- **Diğer her şey engellenir.** Her kural listesinin en sonunda *Default deny* (varsayılan engelleme) bulunur: hiçbir kuralın izin vermediği trafik engellenir ve günlüğe kaydedilir.
- **Listelerin değerlendirilme sırası:** önce sistem kuralları, ardından arayüzün üyesi olduğu grupların kuralları, sonra arayüzün kendi kuralları ve en sonda varsayılan engelleme (*default deny*).

```
Paket GUEST arayüzüne gelir (OPT2, Vlans grubunun üyesi)
  1. sistem kuralları (gri)         anti-lockout, DHCP, port yönlendirmeleri ...
  2. Vlans grubunun kuralları       <- ortak kurallarınız
  3. OPT2 (GUEST) kuralları         <- bu ağa özel kurallarınız
  4. varsayılan engelleme           engellenir ve günlüğe yazılır
```

## Kural listesi

| Sütun | Anlamı |
| :--- | :--- |
| (tutamak) | Kuralı tutamağından sürükleyerek sırasını değiştirin. |
| **On** | Anahtar kuralı etkinleştirir veya devre dışı bırakır. |
| **Action** | Pass (izin ver), Block (sessizce engelle) veya Reject (reddet); ek bir simge eşleşen paketlerin günlüğe yazıldığını gösterir. |
| **Protocol**, **Source**, **Destination**, **Port** | Kuralın neyi eşleştirdiği. Bir adresin başındaki `!` işareti *belirtilen hariç her şey* anlamına gelir. |
| **Description** | Yazdığınız açıklama metni; zamanlama varsa zamanlama rozeti (aktifken yeşil) ve özel ağ geçidi seçilmişse ağ geçidi rozeti (*via WAN2*) ile birlikte gösterilir. |
| **Hits** | Kuralların son yüklenmesinden bu yana (her Apply işlemi kuralları yeniden yükler) kuralla eşleşen paket ve bayt sayısı. Üzerine geldiğinizde açık bağlantı (state) sayısını gösterir. |
| **Actions** | Yukarı / aşağı taşıma, düzenleme, silme. |

Bu sayfada yapılan her değişiklik - kural ekleme, düzenleme, taşıma, açma veya kapatma - anında yapılandırmaya kaydedilir ve **Apply changes** ile etkinleşir.

### Sistem kuralları (gri)

Kendi kurallarınızın üzerinde gri satırlar halinde Stratum FW'nin diğer sayfalardaki ayarlardan otomatik ürettiği kurallar görünür. Bunlar bu sayfadan düzenlenemez; açıklama sütunu hangi ayardan geldiklerini belirtir. Arayüze bağlı olarak şunlar yer alır:

| Sistem kuralı | Geldiği ayar |
| :--- | :--- |
| Anti-lockout: LAN'dan web paneli ve SSH erişimi | **System > Settings > Admin Access** (yalnızca LAN) |
| Bu arayüzden yönetim erişimi / yönetim erişimi yok | **System > Settings > Admin Access** |
| WAN üzerinden yönetim erişimi | **System > Settings > Admin Access** |
| DHCP sunucusu, NTP sunucusu | **Services > DHCP Server**, **Services > NTP** |
| SNMP aracısı | **System > Settings > SNMP** |
| Yayın (broadcast) ve çoklu yayın (multicast) engelleme | Hiçbir zaman yönlendirilmeyen yerel gürültü trafiği (günlüğe yazılıp yazılmaması: **System > Settings > Firewall**) |
| Bogon (ayrılmış) ağları ve özel ağları engelleme | **System > Settings > Firewall** (WAN tipindeki arayüzler) |
| Ping (echo request) yanıtı | **System > Settings > Firewall** (WAN tipindeki arayüzler) |
| Port forward: ... | [**Firewall > NAT**](/tr/manual/firewall-nat/) - her port yönlendirme kendi izin kuralını otomatik alır |
| VPN ve captive portal kuralları | İlgili VPN sayfaları ve **Services > Captive Portal** |

## Kural ekleme veya düzenleme

**Add rule** düğmesine (veya mevcut bir kuralın düzenleme simgesine) tıklayın.

![Bir kuralı düzenleme](/images/manual/fw-rule-edit.png)

| Alan | Anlamı |
| :--- | :--- |
| **Action** | **Pass** - trafiğe izin ver. **Block** - paketi sessizce düşür. **Reject** - paketi düşür ve gönderene bildir (TCP reset veya ICMP mesajı ile); böylece bağlantılar zaman aşımı beklemek yerine anında hata döndürür. |
| **Protocol** | **Any**, **TCP**, **UDP**, **TCP/UDP** veya **ICMP** (ping dahil tüm ICMP türleri). Hedef port belirtmek için TCP, UDP veya TCP/UDP seçilmelidir. |
| **Interface** | Kuralın ait olduğu arayüz veya grup sekmesi. |
| **Source**, **Destination** | **Any** (herhangi biri); **Single host or network** (`192.168.1.10`, `10.0.0.0/8`); **Alias** - tanımlı bir IP/ağ takma adı (**Firewall > Aliases**); **Interface network** - bir arayüzün kendi ağı, örneğin *LAN net* (tüm LAN istemcileri; arayüz adresi değişirse otomatik güncellenir); **This firewall** - sanal IP'ler dahil güvenlik duvarının üzerindeki tüm adresler. **Invert match** seçilenin dışındaki her şey anlamına gelir. |
| **Destination port** | **Any**, **Port or range** (`443`, `8000:8100`) veya **Port alias**. Kaynak port filtrelemesi nadiren yararlı olduğu için sunulmaz. |
| **Schedule** | **Always** (her zaman) veya **Firewall > Schedules** altından bir zamanlama: kural yalnızca zamanlamanın aktif olduğu saatlerde devrededir; diğer saatlerde altındaki kurallar karar verir. |
| **Gateway** | (Birden fazla WAN olduğunda iç arayüzlerdeki *Pass* kurallarında görünür.) Trafiği seçilen WAN hattı üzerinden çıkarır - bkz. [**Interfaces > Gateways**](/tr/manual/interfaces-gateways/). |
| **Description** | En fazla 200 karakter. Kuralın *neden* var olduğunu yazın - bir yıl sonra kimse tek başına IP adresinden hatırlamayacaktır. |
| **Log packets that match this rule** | Eşleşen paketler **Diagnostics > Firewall Log** sayfasında ve günlük raporlarında görünür. Her şeyi değil, incelemeniz gereken trafiği günlüğe yazdırın: yoğun trafikli bir *Pass* kuralını günlüğe yazdırmak diski hızla doldurur. |
| **Rule is enabled** | Devre dışı bırakılan kural listede kalır ancak hiçbir işlem yapmaz. |
| **Position** | Yeni kural için: **At the bottom of the list** (listenin sonuna) veya **At the top of the list** (listenin başına). |

**Save**, ardından **Apply changes** ve **Keep changes** düğmelerine tıklayın.

### Block mu, Reject mi seçilmeli?

- **İç ağlarda** **Reject** kullanın: cihazlar ve kullanıcılar uzun süre zaman aşımı beklemek yerine anında bağlantı reddi yanıtı alır.
- **WAN tarafında** **Block** kullanın: dışarıdan tarama yapan botlar hiçbir yanıt alamaz ve güvenlik duvarı hakkında bilgi edinemez.

## Ayırıcılar (Separators)

**Add separator** düğmesi listeye renkli ve başlıklı bir ayırıcı satır ekler - örneğin *Sunucular*, *Yazıcılar*, *Geçici Kurallar*. Ayırıcılar tıpkı kurallar gibi oklarla veya sürüklenerek taşınabilir, yeniden adlandırılabilir, rengi değiştirilebilir veya silinebilir. Yalnızca listeyi görsel olarak gruplarlar; kuralların işleyişini etkilemezler.

## En iyi uygulama önerileri

- **Özel kuralları genel kuralların üstüne koyun.** Bir istisna kuralı (*yazıcının yazdırma sunucusuna erişmesine izin ver*), istisnası olduğu genel kuralın (*misafirlerin LAN'a erişimini engelle*) üzerinde yer almalıdır.
- **İnternete izin vermeden önce iç ağları engelleyin.** Tipik bir misafir ağı kural dizilimi:

  | # | İşlem | Kaynak | Hedef | Port | Açıklama |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | 1 | Pass | GUEST net | This firewall | 53 (TCP/UDP) | Güvenlik duvarından DNS |
  | 2 | Reject | GUEST net | RFC1918 (alias: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) | any | İç ağlara erişim yok |
  | 3 | Pass | GUEST net | any | any | İnternet erişimi |

  2 numaralı kural misafirleri mevcut ve ileride eklenecek tüm özel iç ağlardan uzak tutar; 3 numaralı kural ise geri kalan trafiğin internete çıkmasına izin verir.
- **"This firewall" hedefini unutmayın.** Hedefi *any* olan bir misafir izin kuralı güvenlik duvarının kendi adreslerine de geçiş izni verir - **Admin Access** altından hangi ağların panele ulaşabileceği sınırlandırılmamışsa buna web paneli de dahildir.
- **Takma adlar (Aliases) kullanın:** birden fazla kuralda geçen adres ve portları alias olarak tanımlayın; değişiklik gerektiğinde her kuralı tek tek düzenlemek yerine yalnızca alias kaydını güncelleyin.
- **Arayüz grupları kullanın:** birden fazla arayüzün paylaştığı ortak kuralları grup üzerinde toplayın ([**Interfaces > Groups**](/tr/manual/interfaces-groups/)).
- **Eşleşme sayaçlarını (Hits) ve günlükleri kontrol edin:** sayacı hiç artmayan bir kural yanlış sırada duruyor veya yanlış kaynak/hedef içeriyor olabilir; **Diagnostics > Firewall Log** paketi hangi kuralın engellediğini gösterir.
