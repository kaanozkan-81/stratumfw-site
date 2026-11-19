---
title: "Güvenlik Duvarı > Zamanlamalar"
seo_title: "06.4 Güvenlik Duvarı > Zamanlamalar (Firewall > Schedules) — Stratum FW Kullanım Kılavuzu"
description: "Zamana bağlı güvenlik duvarı kuralları: gün ve saat aralıkları tanımlama, izin ve engelleme kurallarında zamanlama kullanma, gece yarısını aşan aralıklar ve süre bittiğinde açık bağlantılara ne olduğu."
weight: 604
chapter_num: "06.4"
url: "/tr/manual/firewall-schedules/"
aliases:
  - "/tr/manual/06-04-firewall-schedules/"
translationKey: "manual-06-04-firewall-schedules"
---

Bir zamanlama (schedule), bir güvenlik duvarı kuralını belirli gün ve saatlerle sınırlandırır. Zamanlamaya bağlı bir kural yalnızca zamanlamanın aktif olduğu saatlerde var olur; bu saatlerin dışında kural hiç yazılmamış gibi davranılır.

Tipik kullanım senaryoları:

- misafir Wi-Fi ağının yalnızca 07:00 ile 23:00 saatleri arasında internete çıkması;
- mesai saatleri içinde sosyal medya erişiminin engellenmesi;
- bir tedarikçinin uzaktan destek erişimine yalnızca hafta içi günlerde izin verilmesi.

## Zamanlama listesi

**Firewall > Schedules** sayfası tanımlı her zamanlamayı adı (**Name**), gün ve saat aralıkları (**Times**), şu anda aktif olup olmadığı (**Now**), onu kullanan kurallar (**Used by**) ve açıklaması (**Description**) ile birlikte gösterir.

![Firewall > Schedules](/images/manual/fw-schedules.png)

## Zamanlama ekleme

**Add schedule** düğmesine tıklayın.

| Alan | Anlamı |
| :--- | :--- |
| **Name** | Harfle başlayan, en fazla 31 karakter uzunluğunda harf, rakam, `-` ve `_` - örneğin `Work_Hours`. Yeniden adlandırıldığında onu kullanan kurallar korunur. |
| **Description** | İsteğe bağlı açıklama notunuz. |
| **Time ranges** | Her biri kendi **günlerini** ve **başlangıç** ile **bitiş** saatini (`SS:DD`) içeren bir veya daha fazla zaman aralığı. **Add time range** yeni bir aralık satırı ekler. |

Sistem saati tanımlanan aralıklardan **herhangi birinin** içindeyken kural devrededir.

- **Bitiş 24:00** gece yarısına kadar anlamına gelir.
- **Gece yarısını aşan bir pencere iki ayrı aralık olarak yazılır** - örneğin Cuma *22:00-24:00* ve Cumartesi *00:00-06:00*.
- Farklı günlerdeki farklı saatler ayrı aralıklar olarak eklenir: *Pzt-Cum 08:00-18:00* ve *Cmt 09:00-13:00*.

**Save** düğmesine tıklayın. Düzenleme sayfası zamanlamayı kullanan kuralları **Used by** altında listeler.

## Bir kuralda zamanlama kullanma

İlgili kuralı düzenleyin ([**Firewall > Rules**](/tr/manual/firewall-rules/)) ve **Schedule** alanından zamanlamayı seçin. Kural listesinde zamanlama, açıklamanın yanında bir rozet olarak görünür - zamanlama aktifken yeşil, kural devre dışıyken gri renktedir.

Zamanlamanın etkisi kuralın işlem türüne göre değişir:

| Kural türü | Zamanlama süresince | Zamanlama dışında |
| :--- | :--- | :--- |
| **Pass** | Trafiğe izin verilir. | Kural yok sayılır: altındaki kurallar - veya varsayılan engelleme - karar verir. Kuralın izin verdiği açık bağlantılar **zamanlama sona erdiğinde kapatılır**. |
| **Block / Reject** | Trafik engellenir. | Kural yok sayılır: trafik bu kural tarafından engellenmez. |

Örnek - LAN üzerinde mesai saatlerinde sosyal medyanın engellenmesi:

| # | İşlem | Kaynak | Hedef | Zamanlama |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Reject | LAN net | `Social_Media` (alan adı alias kaydı) | `Work_Hours` |
| 2 | Pass | LAN net | any | Always |

## Zamanlama hassasiyeti

- Kurallar bir aralığın başlangıç veya bitiş saatinden itibaren bir dakika içinde güncellenir.
- Saatler güvenlik duvarının kendi saat dilimine (**System > Settings > General**) göredir. Yanlış saat dilimi veya sistem saati tüm zamanlamaları kaydırır - bir zamanlama erken veya geç devreye giriyorsa önce sistem saatini kontrol edin.
- Zamanlamalarda ve onları hangi kuralların kullandığında yapılan değişiklikler **Apply changes** sonrasında etkinleşir.

## Zamanlama silme

Kuralların hâlâ kullandığı bir zamanlama silinemez: panel kaç kuralın ve hangi kuralların bu zamanlamayı kullandığını bildirir. Önce o kurallarda zamanlamayı **Always** (veya başka bir zamanlama) olarak değiştirin.
