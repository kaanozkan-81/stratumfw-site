---
translationKey: "migrating-from-pfsense"
title: "pfSense'ten Stratum FW'ye Sorunsuz Geçiş Rehberi"
date: 2026-11-05
description: "Eski ve hantal pfSense arayüzünden sıkıldınız mı? Modern, yapay zeka destekli ve hafif Stratum FW'ye nasıl geçeceğinizi adım adım anlatıyoruz. Yedekleme, kurulum ve geri dönüş adımları."
tags: ["pfSense", "Geçiş", "Firewall", "Stratum FW", "OPNsense Alternatifi"]
categories: ["Rehber", "Güvenlik"]
author: "Stratum Security"
---

Ağınızı yıllardır pfSense ile yönetiyorsunuz. Belki de artık PHP tabanlı o eski arayüzden, hantal menülerden ve yavaş yüklenen sayfalardan sıkıldınız. Ya da sadece daha modern, API tabanlı ve **yapay zeka entegrasyonu** olan bir güvenlik duvarı arıyorsunuz.

İşte bu noktada **Stratum FW** devreye giriyor. FreeBSD'nin gücünü modern bir mimariyle birleştiren Stratum FW, pfSense'ten geçişi son derece kolaylaştırıyor. İşte adım adım geçiş rehberiniz:

## 1. Adım: Mevcut pfSense Yapılandırmanızı Yedekleyin
Geçişe başlamadan önce, mevcut pfSense kurulumunuzun yedeğini aldığınızdan emin olun. pfSense panelinde **Diagnostics > Backup & Restore** yolunu izleyerek XML formatında bir yedek alın. Bu, olası bir sorunda eski sisteminize geri dönmenizi sağlar.

*(Not: Stratum FW, pfSense XML formatını doğrudan içe aktaramaz, ancak yapılandırma ayarlarınızı manuel olarak taşıyabilir veya sıfırdan daha temiz bir kurulum yapabilirsiniz.)*

## 2. Adım: Stratum FW'yi İndirin ve Kurun
Stratum FW, hafif bir FreeBSD imajı olarak sunulur. [İndirme sayfamızdan](/tr/download/) en son sürümü (v0.16.0) indirin. Kurulum adımları, pfSense kurulumuna oldukça benzerdir:
*   ISO dosyasını USB'ye yazdırın (VGA veya Serial sürüm).
*   Sunucunuzu USB'den başlatın.
*   Konsol sihirbazını takip ederek WAN ve LAN arayüzlerinizi atayın.

## 3. Adım: Temel Yapılandırmayı Yapın
Stratum FW kurulduktan sonra, modern web paneline (HTTPS) giriş yapın. Karşınıza çıkan arayüz, pfSense'ten çok daha modern ve hızlıdır (FastAPI tabanlı).
*   **Arayüzler:** WAN ve LAN'ı pfSense'teki gibi atayın.
*   **DHCP ve DNS:** Aynı IP havuzlarını ve DNS sunucularını yapılandırın.
*   **Güvenlik Duvarı Kuralları:** pfSense'teki kurallarınızı gözden geçirin ve Stratum FW'nin modern kural arayüzüne (sürükle-bırak destekli) aktarın.

## 4. Adım: Yapay Zeka (MCP) ile Tanışın
Stratum FW'yi pfSense'ten ayıran en büyük özellik, **MCP (Model Context Protocol)** entegrasyonudur. 

Panelde **System > Settings > MCP** yolunu izleyerek bir erişim belirteci (token) oluşturun. Bu token ile ChatGPT veya Claude gibi yapay zeka asistanlarınızı güvenlik duvarınıza bağlayabilirsiniz. Örneğin, bir yapay zekaya *"Son 1 saatte güvenlik duvarı günlüğünde neler var?"* diye sorabilirsiniz. Yapay zeka sizin için anında özet çıkaracaktır.

## 5. Adım: Geri Dönüş Planı
Yeni sisteminizden memnun kalmazsanız, pfSense yedeğinizi geri yükleyerek eski sisteminize dönebilirsiniz. Stratum FW, ZFS önyükleme ortamları (boot environments) kullanır; bu sayede bir güncelleme veya değişiklik sorun yaratırsa, tek tıkla eski sürüme dönebilirsiniz.

## Sonuç
pfSense, yıllardır ağları koruyan saygın bir araçtır. Ancak **Stratum FW**, geleceğin güvenlik ihtiyaçlarına (yapay zeka, API, modern arayüz) cevap veren yeni nesil bir alternatiftir. 

Geçiş sürecinde aklınıza takılan bir şey olursa, [dokümantasyon sayfamızdan](/tr/resources/documentation/) veya [GitHub sayfamızdan](https://github.com/kaanozkan-81/stratum-panel) bize ulaşabilirsiniz. Tüm yenilikleri takip etmek için [Sürüm Notları](/tr/resources/changelog/) sayfamıza göz atmayı unutmayın.
