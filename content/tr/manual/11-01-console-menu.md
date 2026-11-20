---
title: "Konsol Menüsü"
seo_title: "11.1 Konsol Menüsü (Console Menu) — Stratum FW Kullanım Kılavuzu"
description: "VGA veya seri konsol ile SSH üzerindeki yönetim menüsü: durum başlığı, port eşleştirme, IP adresi tanımlama, yönetici parolası sıfırlama, yönetim erişimi, bekleyen değişiklikler, kabuk (shell), yeniden başlatma ve ilk açılış sihirbazı."
weight: 1101
chapter_num: "11.1"
url: "/tr/manual/console-menu/"
aliases:
  - "/tr/manual/11-01-console-menu/"
translationKey: "manual-11-01-console-menu"
---

Konsol menüsü, web yönetim paneline ulaşılamadığı anlarda içeri girmenin doğrudan yoludur: monitör ve klavye (VGA) üzerinde, seri konsolda (WatchGuard M370 gibi donanım cihazlarının konsol portu) veya **SSH** üzerinden birebir aynı şekilde çalışır.

Yerel konsolda (VGA ve seri port) menü kendiliğinden açılır. SSH üzerinden bağlandığınızda ise `root` olarak giriş yaparsınız ([**System > Settings > Admin Access**](/tr/manual/system-settings-general-access/), SSH) ve doğrudan komut kabuğuna düşersiniz - menüyü açmak için `stratum-menu` yazın.

![Konsol menüsü](/images/manual/console-menu.png)

## Durum başlığı (The header)

Menünün en üstünde güvenlik duvarının anlık özeti gösterilir:

```
Stratum FW 0.36.1  -  fw-belek  -  FreeBSD 15.1-RELEASE

 WAN   igb0     203.0.113.10/29        static  up
 LAN   igb1     192.168.1.1/24         static  up
 OPT1  igb2     10.20.0.1/24           static  down

 Web panel: https://192.168.1.1/   https://10.20.0.1/
 WAN management access: closed
```

- Her arayüz kendi **rolü**, **fiziksel portu**, **adresi**, **kipi** (`static`, `dhcp`, `pppoe`) ve **hat durumuyla** (*up*, *no carrier* ya da port bu donanımda mevcut değilse *missing*) listelenir.
- **Web panel**: tarayıcı adres çubuğuna yazılacak adresler (`443` dışında bir port kullanılıyorsa port numarasıyla birlikte). WAN adresi yalnızca WAN yönetim erişimi açıksa listelenir.
- Henüz **Keep changes** ile onaylanmamış bir Apply işlemi varsa üst kısımda *A configuration change is awaiting confirmation (option 7)* uyarısı yer alır.

## Menü seçenekleri

```
  1) Assign interfaces               6) Management access
  2) Set interface IP address        7) Pending change
  3) Reset admin password            8) Shell
  4) Restart web panel               9) Reboot system
  5) Ping host                      10) Halt system
                                     0) Exit
```

### 1) Assign interfaces (Arayüzleri eşleştirme)

Hangi fiziksel ağ portunun `WAN`, `LAN`, `OPT1` ... olacağını belirler. Liste her portu **MAC** adresi, **Link**, **Media** ve mevcut atamasıyla gösterir.

Her rol için port adını (`igb0`, `em1` ...) yazın ya da **otomatik algılama için `a`** tuşlayın: atamak istediğiniz portun kablosunu çıkarıp 30 saniye içinde tekrar takın; bağlantısı gelen (*link up*) port otomatik seçilir. Gövde üzerindeki port etiketleri işletim sistemi adlarıyla örtüşmeyen kutularda çok pratiktir.

**WAN** zorunludur; **LAN** ve ek **OPT** portları isteğe bağlıdır (atlamak için `Enter`). Yeni eşleştirme ekranda gösterilir ve **Proceed?** onayından sonra uygulanır.

Daha önce var olan bir rol yeni portta da mevcut ayarlarını korur. Yeni eklenen bir LAN arayüzüne varsayılan olarak `192.168.1.1/24` adresi ve DHCP sunucusu (`192.168.1.100`-`.199`) verilir.

### 2) Set interface IP address (Arayüz IP adresi belirleme)

Bir arayüz seçin, ardından:

- **WAN**: *Configure WAN IPv4 via DHCP?* - ya da alt ağ ön ekiyle statik IP adresi ve **WAN gateway**;
- **iç arayüzler**: alt ağ ön ekiyle IP adresi (örneğin `192.168.1.1/24`) ve aralığıyla birlikte **DHCP sunucusu** çalıştırılıp çalıştırılmayacağı.

PPPoE, VLAN'lar ve diğer tüm ayrıntılı ayarlar web panelinden yapılır.

### 3) Reset admin password (Yönetici parolasını sıfırlama)

Bir web paneli kullanıcısı için (varsayılan `admin`) en az 10 karakterli yeni parola belirler. Kullanıcının iki aşamalı doğrulaması (2FA) açıksa menü onu da kapatmayı teklif eder (kaybolan telefon durumları için).

Parolanızı unuttuğunuzda panele geri dönmenin yolu budur. Fiziksel konsolun başındaki herkes bu işlemi yapabileceği için güvenlik duvarını her zaman kilitli bir sistem odasında bulundurun.

### 4) Restart web panel (Web panelini yeniden başlatma)

Güvenlik duvarı trafiği yönlendirmeye devam ettiği halde yönetim paneli yanıt vermediğinde panel servisini ve `nginx` sunucusunu yeniden başlatır.

### 5) Ping host

Bir hedefe (varsayılan `1.1.1.1`) dört adet ping gönderir - internet erişiminin en hızlı testidir.

### 6) Management access (Yönetim erişimi)

```
 1) Open or close WAN access
 2) Reset management access to the defaults
    (port 443, all internal networks, anti-lockout on, SSH on with password; ...)
```

[**System > Settings > Admin Access**](/tr/manual/system-settings-general-access/) üzerinde yapılan bir hatadan sonra geri dönme yoludur. **2** numaralı seçenek varsayılanları geri yükler; WAN erişim durumu ve kayıtlı SSH anahtarları olduğu gibi korunur.

### 7) Pending change (Bekleyen değişiklik)

Onay bekleyen bir Apply işlemi varken değişikliği gösterir ve *Keep the pending change?* diye sorar - **y** kalıcı yapar, **n** (varsayılan) anında önceki ayarlara döner. Web panelinde yaptığınız bir değişiklik bağlantınızı kestiğinde geri sayım süresinin bitmesini beklemek yerine buradan hemen geri alabilirsiniz.

### 8) Shell (Komut kabuğu)

[Bir sonraki bölümdeki](/tr/manual/command-line/) komutları çalıştırmak için `root` kabuğu (`/bin/sh`) açar. Menüye dönmek için `exit` yazın.

### 9) Reboot system, 10) Halt system

Onay sonrasında sistemi yeniden başlatır veya kapatır.

### 0) Exit

Menüden çıkar. Yerel konsolda yeni oturum açma istemi gelir; SSH bağlantısında ise kabuğa dönersiniz.

## Konsolda yapılan değişikliklerin güvencesi

**1**, **2** ve **6** numaralı seçenekler yaptıkları değişikliği tıpkı web panelindeki gibi güvenli geri alma korumasıyla uygular: yeni ayarlar hemen devreye girer, ancak **120 saniye içinde yanıt vermezseniz otomatik olarak geri alınır**. Kalıcı yapmak için *Keep the new settings?* sorusuna **y** (`Enter`), geri almak için **n** yanıtını verin.

Her değişiklik uygulanmadan önce doğrulanır; geçersiz bir ayar reddedilir ve mevcut duruma dokunulmaz.

## İlk açılış sihirbazı (The first-boot wizard)

Kurulum sonrasındaki ilk açılışta - henüz hiçbir yapılandırma yokken - tüm konsollar *Welcome to Stratum FW* karşılama ekranını gösterir. Kullandığınız konsolda **Enter** tuşuna basın; diğer konsollar sihirbaz tamamlanana kadar bekler.

Sihirbaz sırasıyla şu adımlardan geçirir:

1. **Assign interfaces** - 1 numaralı seçenekle aynı.
2. **Addresses** - atanan her arayüzün IP adresleri (2 numaralı seçenekle aynı).
3. **Management access from WAN** - önerilen *No*: güvenlik duvarını LAN üzerinden yönetin. LAN arayüzü atanmamışsa ve WAN erişimi kapalıysa güvenlik duvarı yalnızca konsoldan yönetilebilir.
4. **Time zone** - şehir adının bir kısmını yazın, örneğin `istanbul`.
5. **Web panel administrator** - `admin` kullanıcısı için en az 10 karakterli parola.

Ardından yapılandırma uygulanır ve üst başlığında web panelinin adresi yazan konsol menüsü açılır. Kalan adımlar web panelinden tamamlanır - [**İlk Yapılandırma**](/tr/manual/initial-configuration/) bölümüne bakınız.

Sihirbazı daha sonra yeniden çalıştırmak isterseniz kabukta `stratum-menu --setup` komutunu verebilirsiniz (port eşleştirmesinden başlayarak süreci baştan başlatır).
