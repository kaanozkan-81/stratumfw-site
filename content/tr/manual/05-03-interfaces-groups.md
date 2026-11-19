---
title: "Arayüzler > Gruplar"
seo_title: "05.3 Arayüzler > Gruplar (Interfaces > Groups) — Stratum FW Kullanım Kılavuzu"
description: "Arayüz grupları: birden fazla arayüzün ortak kullandığı ve her üyenin kendi kurallarından önce değerlendirilen güvenlik duvarı kuralları."
weight: 503
chapter_num: "05.3"
url: "/tr/manual/interfaces-groups/"
aliases:
  - "/tr/manual/05-03-interfaces-groups/"
translationKey: "manual-05-03-interfaces-groups"
---

Bir arayüz grubu (interface group), güvenlik duvarı kurallarını ortaklaşa kullanabilmeleri için birden fazla arayüzü tek bir isim altında toplar. Grup üzerinde bir kez yazılan kural, gruptaki tüm üye arayüzlere uygulanır.

Tipik kullanım senaryoları:

- **Çok sayıda VLAN, tek ortak politika.** Yirmi ayrı misafir ve departman VLAN'ının tamamının yönetim ağından uzak tutulması gerekiyorsa, yirmi ayrı kopya yerine `Vlans` adlı bir grup üzerine tek bir *block* (engelleme) kuralı yazılır.
- **Birden fazla WAN hattı.** Her internet bağlantısında geçerli olması gereken bir kural - örneğin gelen bir VPN portuna izin vermek - WAN arayüzlerini içeren bir grup üzerinde tanımlanır.

## Grup kuralları nasıl değerlendirilir

Oluşturulan her grup **Firewall > Rules** sayfasında kendine ait bir sekme alır. Grubun kuralları, üye arayüzlerin kendi kurallarından **önce**, listedeki sırayla değerlendirilir - tıpkı pfSense ve OPNsense'te olduğu gibi. Eşleşen ilk kural kararı verir.

```
Paket OPT3 arayüzüne gelir (Vlans grubunun bir üyesi)
  1. Vlans grubunun kuralları    <- önce burası denetlenir
  2. OPT3'ün kendi kuralları     <- yalnızca hiçbir grup kuralı eşleşmezse bakılır
  3. varsayılan: engellenir (blocked)
```

Bu nedenle ortak **block** (engelleme) kurallarını grup üzerine koyun (böylece herhangi bir üyenin kendi "izin ver" kuralına ulaşmadan önce trafiği durdururlar); ağdan ağa farklılaşan izin kurallarını ise arayüzlerin kendi sekmelerinde tutun.

> **Not:** Bir grup üzerinde *LAN ağı* (*the network of LAN*) veya *OPT2 adresi* (*the address of OPT2*) şeklinde yazılan kural yine tam olarak o ağları ifade eder - "her üyenin kendi ağı" anlamına gelmez. Her üye arayüzün kendi ağını eşleştirmek için grup kuralında kaynak olarak *any* kullanın veya tüm üye ağları içeren bir takma ad (alias) tanımlayın.

## Grup listesi

**Interfaces > Groups** sayfası mevcut grupları üyeleri (**Members**), açıklaması (**Description**) ve sahip oldukları kural sayısı (**Rules**) ile birlikte listeler.

![Interfaces > Groups](/images/manual/if-groups.png)

## Grup oluşturma veya düzenleme

**Add group** düğmesine (veya mevcut bir grupta **Edit** bağlantısına) tıklayın.

| Ayar | Anlamı |
| :--- | :--- |
| **Name** | Harfle başlayan, 1 ile 15 karakter uzunluğunda harf, rakam veya `_` - örneğin `Vlans` ya da `Guest_Nets`. **Firewall > Rules** sayfasındaki grup sekmesinin adıdır. Bir arayüz adı (`lan`, `opt2` ...) veya mevcut bir takma ad (alias) ile aynı olamaz. |
| **Description** | İsteğe bağlı açıklama, en fazla 80 karakter. |
| **Members** | Grupta yer alacak arayüzler - en az bir arayüz seçilmelidir. Bir arayüz birden fazla gruba üye olabilir. |

Kaydedin, ardından **Firewall > Rules** sayfasında grubun sekmesine geçerek kurallarını ekleyin ve **Apply changes** ile etkinleştirin.

Bir arayüz sistemden kaldırıldığında üyesi olduğu gruplardan kendiliğinden çıkarılır.

## Grup silme

Üzerinde hâlâ kural bulunan bir grup silinemez: *The group has 2 rule(s). Delete them on Firewall > Rules > Vlans first.* Bu kısıtlama bilinçli olarak konmuştur - grubu doğrudan silmek o kuralları ve onlarla birlikte örneğin misafir ağlarını yönetim ağınızdan uzak tutan engelleme kuralını sessizce ortadan kaldırırdı.

## Sınırlar

- En fazla 32 grup oluşturulabilir.
- Trafiği seçilen belirli bir WAN hattı üzerinden gönderen kurallar (politika tabanlı yönlendirme / policy routing), bir grup üzerinde yalnızca grubun tüm üyeleri iç ağ (LAN tipinde) arayüzleriyse kullanılabilir.
