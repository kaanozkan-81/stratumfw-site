---
title: "Güvenlik Duvarı > İndirme Kontrolü (Download Control)"
seo_title: "06.9 Güvenlik Duvarı > İndirme Kontrolü ve Gizlenmiş Program Tespiti — Stratum FW Kullanım Kılavuzu"
description: "İç ağlarda HTTP ve HTTPS dosya indirmelerini denetleme ve engelleme: port 3128 Squid vekil sunucu, Inspection CA (.cer / .crt), ilk baytlardan gerçek dosya türü tespiti, .pdf/.jpg kılıflı gizlenmiş .exe yakalama, Monitor/Block modları, Bypass ve Always allowed listeleri ile Downloads sekmesi."
weight: 609
chapter_num: "06.9"
url: "/tr/manual/firewall-download-control/"
aliases:
  - "/tr/manual/06-09-firewall-download-control/"
translationKey: "manual-06-09-firewall-download-control"
---

**Firewall > Download Control** (İndirme Kontrolü), seçtiğiniz iç ağlardaki cihazların HTTP ve HTTPS üzerinden indirdiği dosyaları denetler; indirilen dosyaları kaydeder, başka bir uzantı altına gizlenmiş programları (örneğin adı `.pdf` veya `.jpg` yapılmış bir `.exe` dosyasını) içeriğinden yakalar ve istenmeyen dosya türlerini kullanıcı bilgisayarına ulaşmadan engeller.

![Firewall > Download Control > Settings](/images/manual/dlctl-settings.png)

## Nasıl çalışır?

1. **Port 3128 üzerinde web vekil sunucusu (Squid):** Stratum FW, işaretlediğiniz iç ağlarda kendi arayüz IP adresi üzerinde (`<arayüz IP>:3128`) **Squid** web vekil sunucusunu çalıştırır. Cihazlar vekil sunucuyu kullanacak şekilde ayarlandığında (örneğin bir Windows etki alanında **Group Policy** ile; gerekli adımlar doğrudan panelde listelenir) denetime girer. Vekil sunucuya yönlendirilmeyen cihazlar denetlenmez.
2. **Inspection CA ile HTTPS denetimi:** Vekil sunucu HTTPS indirmelerini incelemek için [**System > Certificates > Inspection CA**](/tr/manual/system-certificates/#inspection-ca) altında yer alan ve **Block Page CA**'dan tamamen ayrı olan kendi sertifika otoritesini kullanır. Sertifikayı `.cer` (Windows ve Group Policy için) veya `.crt` (PEM) biçiminde indirip vekil sunucuyu kullanan cihazlara yükleyin.
3. **Sadece uzantıya değil dosyanın ilk baytlarına bakar:** Karar verilirken **dosya adı**, karşı sunucunun bildirdiği **MIME türü** ve **dosyanın kendi ilk baytları** birlikte değerlendirilir. Bir web sayfasının normal bileşenleri (sayfa içi betikler, resimler, sıkıştırılmış sayfalar) dosya indirmesi olarak sayılmaz.
4. **Kesintisiz geçiş güvencesi (Fail-open):** Dosya denetleyicisi herhangi bir nedenle durursa indirmeler kesilmeden devam eder; bilgisayarlardaki uç nokta antivirüs yazılımları kendi kontrollerini sürdürür.

## Dosya türleri ve gizlenmiş programlar

| Ayar | Tanınan ve denetlenen içerikler |
| :--- | :--- |
| **Disguised programs (Gizlenmiş programlar)** | Başka bir dosya uzantısı arkasına gizlenmiş çalıştırılabilir dosyaları — örneğin `fatura.pdf` veya `fotograf.jpg` adı verilmiş bir `.exe` programını — ilk baytlarındaki içerikten tanır. Aşağıda **hangi dosya türleri seçili olursa olsun** engellenebilir. |
| **Programs and installers** *(varsayılan)* | Windows, Linux ve macOS programları (`.exe`, `.dll`, ELF, Mach-O), `MSI`, `MSIX`, `APK` ve `JAR` paketleri. |
| **Scripts and shortcuts** *(varsayılan)* | İndirilen PowerShell, toplu iş (`.bat`/`.cmd`), kabuk betikleri ve kısayollar. |
| **Archives** | `ZIP`, `RAR`, `7z`, `CAB` arşiv dosyaları. |
| **Disk images** | Disk ve kurulum kalıpları (`.iso`, `.img`, `.dmg` vb.). |
| **Documents** | `PDF` ve Microsoft Office dosyaları. |
| **Video and audio** | İndirilen video ve ses dosyaları. |

## Monitor veya Block modu

- **Monitor (İzle):** Nelerin engelleneceğini kaydeder ancak hiçbir dosyayı engellemez. İndirme Kontrolünü yeni bir ağda devreye alırken iş akışlarını görmek için **ilk birkaç gün Monitor moduyla başlamanız** önerilir.
- **Block (Engelle):** Engellenen dosyayı indirmenin engellendiğini bildiren bir tarayıcı sayfasıyla değiştirir; bu sayfa [**Engelleme Sayfası (Block page)**](/tr/manual/firewall-application-control/#engelleme-sayfası-block-page) tasarımını kullanır.

## Bypass listesi ve Her zaman izinli (Always allowed)

| Liste | Davranış | Tipik kullanım alanları |
| :--- | :--- | :--- |
| **Bypass list** | Bu sitelerin HTTPS bağlantısı vekil sunucu tarafından **hiç açılmaz** ve dosyaları **denetlenmez**. | Bankalar, kamu kurumları (`gov.tr`), işletim sistemi güncellemeleri, güvenlik/antivirüs yazılımları ve yalnızca kendi sabitlenmiş sertifikasını (*certificate pinning*) kabul eden uygulamalar. **Varsayılan bypass listesi tek tıklamayla** eklenebilir. |
| **Always allowed** | Bu sitelerden yapılan indirmeler **denetlenir ve Downloads sekmesine kaydedilir**, ancak **hiçbir zaman engellenmez**. | Kurum içi yazılım dağıtım sunucuları veya güvenilir iş ortağı portalları (yükleyicileri engellemeden indirme kaydı tutmak istediğiniz adresler). |

## Downloads sekmesi ve gizlilik

![Firewall > Download Control > Downloads](/images/manual/dlctl-downloads.png)

**Downloads** sekmesi HTTP ve HTTPS üzerinden kaydedilen indirmeleri en yeniden eskiye doğru listeler:

- **Sütunlar:** **Time** (saat), **Device** (cihaz), **Site**, **File** (dosya adı), **Type** (gerçek dosya türü ve gizlenmiş program uyarısı), **Size** (boyut) ve **Result** (*Allowed*, *Always allowed*, *Would block* veya *Blocked*).
- **Arama ve filtreleme:** Sonuca, dosya türüne, ağa veya döneme göre filtreleyebilir; dosya adı, site veya cihaz IP'sine göre arama yapabilirsiniz.
- **Saklama süresi:** Kayıtlar güvenlik duvarı üzerinde **1 ila 365 gün** boyunca tutulur (varsayılan **30 gün**).
- **Yalnızca yöneticilere özel ve denetim kayıtlı:** Bu sayfayı yalnızca **Yöneticiler (Administrators)** açabilir ve indirme kayıtlarına yapılan her bakış [**Diagnostics > System Logs**](/tr/manual/diagnostics-system-logs/) altına kaydedilir.
