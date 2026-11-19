---
title: "Arayüzler > Sanal IP'ler"
seo_title: "05.5 Arayüzler > Sanal IP'ler (Interfaces > Virtual IPs) — Stratum FW Kullanım Kılavuzu"
description: "Bir arayüz üzerindeki ek IP adresleri: port yönlendirme ve giden NAT için WAN üzerinde birden fazla genel IP adresi veya bir iç ağda ikinci adres kullanımı."
weight: 505
chapter_num: "05.5"
url: "/tr/manual/interfaces-virtual-ips/"
aliases:
  - "/tr/manual/05-05-interfaces-virtual-ips/"
translationKey: "manual-05-05-interfaces-virtual-ips"
---

Sanal IP (Virtual IP), bir arayüz üzerine eklenen ek bir IP adresidir (*IP alias*). Güvenlik duvarı bu adreste de tıpkı arayüzün kendi ana adresindeymiş gibi yanıt verir.

Tipik kullanım senaryoları:

- **Birden fazla genel (public) IP adresi bloğu.** Servis sağlayıcınız size bir `/29` bloğu (beş kullanılabilir adres) verdiğinde WAN arayüzü bunlardan birini kullanır; diğerleri sanal IP olarak tanımlanır. Böylece ikinci bir genel adres farklı bir iç sunucuya yönlendirilebilir veya bir iç ağın tamamı internete kendisine ait ayrı bir genel IP adresiyle çıkabilir.
- **İç ağda ikinci bir adres,** örneğin ağdaki cihazlar eski bir ağ geçidi adresinden yenisine kademeli olarak geçirilirken geçiş süresince her iki adresi de aktif tutmak için.

## Sanal IP listesi

**Interfaces > Virtual IPs** sayfası tanımlı her sanal IP'yi adresi (**Address**), bağlı olduğu arayüz (**Interface**), açıklaması (**Description**), onu kullanan NAT kayıtları (**Used by**) ve durumu (**State**: aktif veya Apply edilene kadar *pending*) ile birlikte gösterir.

![Interfaces > Virtual IPs](/images/manual/vip-list.png)

## Sanal IP ekleme

**Add virtual IP** düğmesine tıklayın.

| Ayar | Anlamı |
| :--- | :--- |
| **Interface** | Ek adresi alacak arayüz. Bu arayüzün kendine ait bir IPv4 adresi (sabit veya DHCP) bulunmalıdır. |
| **Address** | Eklenecek adres, örneğin `203.0.113.11`. Arayüzün kendi alt ağının ağ (network) veya yayın (broadcast) adresi olamaz, başka bir arayüzün zaten kullandığı bir adres olamaz ve her adres yalnızca bir kez eklenebilir. |
| **Prefix** | Yalnızca arayüzün kendi alt ağı **dışındaki** bir adres eklenirken kullanılır: o diğer ağın önek uzunluğu (8 ile 32 arası). Arayüzün kendi alt ağı içindeki bir adres ise ne girerseniz girin FreeBSD kuralı gereği her zaman tekil adres (`/32`) olarak eklenir. |
| **Description** | İsteğe bağlı açıklama notunuz, örneğin *Mail server address*. |

**Save**, **Apply changes** ve **Keep changes** düğmelerine tıklayın. Arayüzün (veya tüm ağ yapılandırmasının) yeniden başlatılması sanal IP'leri silmez: güvenlik duvarı hem servis yenilemelerinde hem de yeniden başlatma sonrasında sanal IP'leri kendiliğinden tekrar ekler.

## Sanal IP'nin kullanıldığı yerler

| Nerede | Nasıl kullanılır |
| :--- | :--- |
| **Port yönlendirme** (**Firewall > NAT**) | Yönlendirmenin hedef adresi olarak sanal IP'yi seçin: *o* adrese ve porta gelen bağlantılar ilgili iç sunucuya iletilir. Seçilmezse port yönlendirme arayüzün kendi ana adresini dinler. |
| **Giden NAT** (**Firewall > NAT**, manuel veya hibrit kip) | Bir iç ağın veya sunucunun trafiğini sanal IP'ye çevirerek internete kendine ait genel adresten çıkmasını sağlayın - örneğin çıkış IP adresinin DNS kayıtlarıyla (SPF/PTR) eşleşmesi gereken bir e-posta sunucusu için. |
| **1:1 NAT** (**Firewall > NAT**, 1:1) | Bir genel IP adresini her iki yönde tek bir iç sunucuyla birebir eşleştirin. 1:1 NAT için kullanılan tekil bir genel adres ilgili WAN üzerinde sanal IP olarak tanımlanmış olmalıdır. |
| **Güvenlik duvarı kuralları** | Güvenlik duvarının kendi adresleri (kurallardaki *This firewall* seçimi) sanal IP'leri de kapsar. Bunlara neyin ulaşabileceğine WAN kuralları ve yönetim erişimi ayarları karar verir. |

> **Not:** WAN üzerindeki bir sanal IP, güvenlik duvarının genel bir IP adresidir. Tıpkı ana WAN adresinde olduğu gibi bir port yönlendirme veya WAN izin kuralı açılmadıkça dışarıdan hiçbir trafik bu adrese ulaşamaz.

## Değiştirme veya silme

- Bir port yönlendirme veya giden NAT kuralının kullandığı sanal IP'nin adresi değiştirilemez ve bu kayıt silinemez: panel ilgili kayıtları ismen belirtir - *This virtual IP is used by NAT - change it there first: Port forward Mail*. Önce o NAT kayıtlarını güncelleyin.
- Düzenleme sayfası adresi kullanan kayıtları **Used by** altında listeler.

En fazla 256 sanal IP tanımlanabilir.
