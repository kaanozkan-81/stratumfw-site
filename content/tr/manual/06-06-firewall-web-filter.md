---
title: "Güvenlik Duvarı > Web Filtresi"
seo_title: "06.6 Güvenlik Duvarı > Web Filtresi (Firewall > Web Filter) — Stratum FW Kullanım Kılavuzu"
description: "Politikalarla ağ bazında web sitesi kategorilerini engelleme: 56 kategori, Güvenli Arama (Safe Search), isim değiştiren yeni siteler için Anahtar Kelime (Keywords) tespiti, istisnalar ve Blocked görünümü."
weight: 606
chapter_num: "06.6"
url: "/tr/manual/firewall-web-filter/"
aliases:
  - "/tr/manual/06-06-firewall-web-filter/"
translationKey: "manual-06-06-firewall-web-filter"
---

Web Filtresi (Web Filter), seçtiğiniz ağlar için web sitesi kategorilerini - yetişkin içerik, kumar, zararlı yazılım, sosyal ağlar vb. - topluca engeller. Her ağ kendine özgü bir **politika** (policy) alır: misafir Wi-Fi ağı için ayrı bir politika, personel için başka bir politika, sunucular içinse filtresiz erişim tanımlayabilirsiniz.

Filtre doğrudan güvenlik duvarının DNS çözümleyicisi üzerinden çalışır: engellenen bir kategorideki site sorgulandığında çözümleyici *mevcut değil* yanıtı döner ve site açılmaz - hiçbir cihaza sertifika veya ajan kurmadan, HTTPS siteler dahil (şifre çözme / araya girme yapılmadan) tüm cihazlarda çalışır.

> **Not:** Web Filtresi **Unbound** içinde çalışır. Önce **Services > DNS Resolver** altından DNS çözümleyici olarak **Unbound** seçin.

![Firewall > Web Filter](/images/manual/wf-page.png)

## Etkinleştirme

1. **Block site categories with the policies below** anahtarını açın.
2. En az bir politika ekleyin (aşağıya bakınız).
3. **Category list updates** sıklığını seçin ve **Save** düğmesine tıklayın.
4. **Apply changes** ile etkinleştirin. Politikaların kullandığı kategoriler indirilir ve yüklenir.

Web Filtresini açıp kapatmak ve politikalarda değişiklik yapmak **Apply changes** gerektirir; istisnalar (exceptions) ise kaydedildiği anda devreye girer.

## Politikalar (Policies)

**Add policy** düğmesi politika düzenleme sayfasını açar.

| Alan | Anlamı |
| :--- | :--- |
| **Name** | Politika adı, örneğin *Guests* veya *Staff*. |
| **Enabled** | Devre dışı bırakılan bir politika ayarlarını korur ancak hiçbir engelleme yapmaz. |
| **Networks** | Politikanın uygulanacağı iç ağlar ile OpenVPN ve WireGuard istemci ağları. Bir ağ aynı anda yalnızca tek bir politikada yer alabilir. |
| **Safe Search** | Google, Bing ve DuckDuckGo aramalarında müstehcen sonuçları gizler, YouTube'u Kısıtlı Mod'da çalıştırır - kullanıcıların tarayıcıdan kapatabileceği bir ayar değil, doğrudan güvenlik duvarı tarafından zorunlu kılınan bir denetimdir. |
| **Blocked categories** | Seçilen ağlar için engellenecek site kategorileri. |

Yeni seçilen kategoriler Apply işlemi sırasında indirilir. Çok geniş kategoriler ek bellek kullanır; sayfa tahmini bellek ihtiyacını gösterir.

![Bir Web Filtresi politikası](/images/manual/wf-policy.png)

### Kategoriler

| Grup | Kategoriler |
| :--- | :--- |
| **Adult** (Yetişkin ve Sakıncalı) | Adult (full list), Adult (compact list), Mixed adult content, Lingerie, Dating, Sexual education, Gambling, Drugs, Violence and hate, Dangerous material, Sects |
| **Security** (Güvenlik) | Malware, Phishing, Cryptojacking, Stalkerware, DDoS services, Hacking, Dialers, Fake news, Marketingware, Residential proxies |
| **Getting around the filter** (Filtreyi Aşma Araçları) | VPN services, DNS over HTTPS servers, Proxies and anonymizers, URL shorteners, Remote control, Dynamic DNS |
| **Social and communication** (Sosyal ve İletişim) | Social networks, Chat, Forums, Blogs, Webmail |
| **Entertainment** (Eğlence) | Games, Audio and video, Radio, Sports, Manga, Celebrities, News, Astrology |
| **Shopping and money** (Alışveriş ve Finans) | Shopping, Banks, Finance, Cryptocurrency, Job search, Advertising |
| **Downloads and hosting** (İndirme ve Barındırma) | Downloads, File hosting, Warez, Web hosting |
| **Other** (Diğer) | Artificial intelligence, Translation, Cheating, Mobile phones, Cooking, Educational games |

**Kategori seçimi.** Bir misafir ağında herkesin engellenmesini beklediği temel kategorilerle başlayın: **Adult**, **Gambling**, **Malware** ve **Phishing**. Zamanla ihtiyaca göre ekleme yapın; sayfadaki **Blocked** görünümü filtrenin neleri durdurduğunu gösterir.

**Adult (full list)** milyonlarca alan adı içerir ve yüksek miktarda bellek kullanır; **Adult (compact list)** ise çok daha düşük bellek kullanımıyla bilinen başlıca siteleri kapsar ve çoğu güvenlik duvarı için doğru tercihtir.

**Getting around the filter** grubu - VPN servisleri, DNS-over-HTTPS sunucuları ve web vekil sunucuları - filtreyi aşmaya çalışan bir kullanıcının ilk deneyeceği yöntemlerdir. Filtrenin delinmemesi gereken ağlarda bu grubu da engelleyin.

Kategoriler günlük olarak güncellenen Université Toulouse Capitole UT1 kara listelerinden (CC BY-SA 4.0) ve Block List Project kaynaklarından derlenir.

## Filtrenin aşılmasını zorlaştırma

Filtre DNS üzerinden çalıştığı için başka bir DNS sunucusuna soru soran cihaz filtrelenmez. Bu açık kapıları kapatmak için:

1. Filtrelenen ağların **DNS sorgularını güvenlik duvarına zorunlu yönlendirin** (**Services > DNS Resolver** veya [**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/) altındaki *Send all DNS to the firewall*): dışarıdaki başka bir DNS sunucusuna giden sorgular şeffaf biçimde güvenlik duvarına çevrilir.
2. **"Getting around the filter" kategorilerini engelleyin** - VPN servisleri, DNS over HTTPS sunucuları, vekil sunucular.
3. İsteğe bağlı olarak DNS engelleme listelerindeki ([**Firewall > Blocklists**](/tr/manual/firewall-blocklists/)) **DoH** kategorisini de açarak tarayıcıların şifreli DNS kullanmasını önleyin.

DNS tabanlı hiçbir filtre listelerin henüz tanımadığı özel bir VPN sunucusuna bağlanan kullanıcıyı tek başına durduramaz; ancak bu üç adım birlikte uygulandığında pratikte neredeyse tüm aşma girişimlerini engeller.

## Anahtar Kelimeler — İsme Göre Yeni Siteler (Keywords)

![Firewall > Web Filter > Keywords](/images/manual/wf-keywords.png)

Genel kategori listeleri günde bir kez güncellenirken yasa dışı bahis, kumar ve taklit siteler çoğu zaman saat içinde bile yeni numaralı alan adlarına (`site1234.com`, `site1235.com` ...) geçer. **Keywords** sekmesi, ağdaki herhangi bir cihaz güvenlik duvarının Unbound DNS çözümleyicisine sorduğu anda bu yeni alan adlarını içlerindeki marka veya kelimeyle (örneğin `bahis` ya da `casino`) anında yakalar:

| Çalışma Modu | Anahtar kelimeyle eşleşen yeni bir alan adı görüldüğünde ne olur? |
| :--- | :--- |
| **List them only** *(varsayılan)* | Yakalanan yeni siteyi eşleşen anahtar kelime, saat ve ilk sorgulayan cihazla birlikte listeler; tek tıkla **engelleyebilir** (**Your own list** içine ekler), **yok sayabilir** (*Ignore*) veya **İstisnalara** (*Exception*) ekleyebilirsiniz. |
| **Block them at once** | Alan adını doğrudan **Your own list** (Kendi listeniz) kategorisine ekler ve bu kategoriyi seçen tüm Web Filtresi politikalarında saniyeler içinde engeller (politikada engelleme sayfası açıksa özel sayfayı da gösterir). |

- **Yalnızca sitenin kendi adı sayılır:** Alt alan adları (*subdomains*) veya üst düzey alan adı uzantıları (`.com`, `.bet` vb.) eşleşme tetiklemez; **Exceptions** (İstisnalar) her zaman önceliklidir.
- **Web Activity kapalıyken sıfır kayıtla çalışır:** [**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/) kapalı olsa bile çalışır; toplayıcı DNS sorgularını yalnızca anahtar kelimeler için izler ve diske hiçbir gezinme geçmişi kaydetmez.

## İstisnalar (Exceptions)

**Exceptions**: her satıra bir alan adı yazılır; alt alan adlarına da otomatik izin verilir. İstisna listesindeki bir alan adı Web Filtresi tarafından hiçbir zaman engellenmez (DNS engelleme listeleri ise geçerli olmaya devam eder). Her koşulda açılması gereken siteler - örneğin genel bir kategorinin yanlışlıkla içerdiği kendi kurumsal siteniz veya iş ortağınız - için kullanın. **Save exceptions** düğmesine tıkladığınızda anında devreye girer.

## Bir siteyi sorgulama (Check a site)

**Check a site** aracı bir alan adının hangi kategorilerde yer aldığını, hangi politikaların onu engellediğini ve bir istisna kaydının izin verip vermediğini gösterir - *"misafir Wi-Fi ağında bu site neden açılmıyor?"* sorusunun en hızlı yanıtıdır.

## Kategori güncellemeleri

| Ayar | Anlamı |
| :--- | :--- |
| **Category list updates** | 6 saatte bir, 12 saatte bir, günlük, haftalık veya **Off (manual updates only)**. |
| **On**, **At** | Güncelleme günü (haftalıkta) ve saati. |

**Update lists now** listeleri hemen günceller. Başarısız bir indirme önceki çalışan listeyi korur.

## Engellenenler görünümü (Blocked)

Sayfadaki **Blocked** kartı Web Filtresinin **bugün**, **son 7 günde** veya **son 30 günde** hangi kategoriden ne kadar engelleme yaptığını ve en çok engellenen siteleri gösterir. **Web Activity** açık olduğunda ([**Diagnostics > Web Activity**](/tr/manual/diagnostics-web-activity/)) engellemeler cihaz bazında da sayılır.
