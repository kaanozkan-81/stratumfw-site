---
title: "Komut Satırı"
seo_title: "11.2 Komut Satırı (Command Line) — Stratum FW Kullanım Kılavuzu"
description: "stratum-config komutu (show, validate, render, diff, 180 sn geri almalı apply, confirm, rollback, status) ve diğer tüm stratum-* yönetim araçları, FreeBSD servisleri ve sistem dosya yolları."
weight: 1102
chapter_num: "11.2"
url: "/tr/manual/command-line/"
aliases:
  - "/tr/manual/11-02-command-line/"
translationKey: "manual-11-02-command-line"
---

Stratum FW'deki her işlem web panelinden yapılabilir. Komut satırı (CLI) sorun giderme, otomasyon ve web paneline ulaşılamayan durumlar içindir. [Konsol menüsündeki](/tr/manual/console-menu/) **8) Shell** seçeneğiyle veya SSH üzerinden açabilirsiniz; tüm komutlar `root` yetkisi gerektirir.

> **Yapılandırmayı elle değil, panelden düzenleyin.** Üretilen servis dosyaları (`/etc/pf.conf`, `dnsmasq`, `Unbound`, `OpenVPN` ...) her Apply işleminde baştan yazılır - bu dosyalarda doğrudan yapılan değişiklikler silinir. Sistemdeki tek gerçek doğruluk kaynağı `/usr/local/etc/stratum/config.json` dosyasıdır.

## stratum-config

`stratum-config` aracı `config.json` tanımını çalışan sisteme dönüştürür. Web panelindeki **Apply**, **Keep changes** ve otomatik geri alma mekanizmasının tamamı arka planda bu aracı kullanır.

| Komut | İşlevi |
| :--- | :--- |
| `stratum-config show` | `config.json` içeriğini ekrana yazdırır. |
| `stratum-config validate` | `config.json` dosyasını denetler; varsa doğrulama hatalarını listeler. |
| `stratum-config render pf` | Üretilecek `pf` kural kümesini ekrana basar (ayrıca `dnsmasq`, PPPoE parolası maskelenmiş olarak `mpd5` veya `rc.conf` ayarları için `rc` verilebilir). Çalışan sistemde hiçbir şeyi değiştirmez. |
| `stratum-config diff` | Üretilecek yapılandırmayı çalışan sistemle karşılaştırır: dosya bazında satır farkları, `rc.conf` değişiklikleri, yeniden başlatılacak arayüz ve servisler ile bekleyen değişiklik sayısı. Hiçbir şeyi değiştirmez. |
| `stratum-config apply` | `config.json` yapılandırmasını uygular ve onaylanmazsa **180 saniye sonra otomatik geri alır**. `--timeout <s>` farklı bir süre belirler; `--no-rollback` geri sayım olmadan doğrudan uygular. |
| `stratum-config confirm` | Son uygulanan değişikliği kalıcı yapar (paneldeki **Keep changes** karşılığı). |
| `stratum-config rollback` | Son Apply öncesindeki duruma hemen şimdi geri döner. |
| `stratum-config status` | Onay bekleyen bir Apply olup olmadığını, ne kadar önce uygulandığını ve geri alma sayacının çalışıp çalışmadığını gösterir. |

Aynı anda yalnızca tek bir `apply`, `confirm` veya `rollback` çalışabilir; önceki işlem onay beklerken yeni bir `apply` reddedilir.

**Tipik kullanım - bir değişikliğin ne yapacağını uygulamadan önce görme:**

```
stratum-config diff | less
stratum-config render pf | less
```

**Komut satırından uygulama** (örneğin panel erişilemezken SSH üzerinden):

```
stratum-config apply
# her seyin calistigini kontrol edin, ardindan 180 sn icinde:
stratum-config confirm
```

Yaptığınız değişiklik SSH bağlantınızı keserse hiçbir şey yapmayın: süre dolduğunda önceki çalışan durum kendiliğinden geri döner.

> `stratum-config import` çalışan ham bir sistemden ilk `config.json` dosyasını üretir. Yalnızca kurulum sırasında bir kez kullanılır; yapılandırılmış bir güvenlik duvarında çalıştırmayın.

Her Apply işleminin ayrıntılı günlüğü `/var/log/stratum-apply.log` dosyasına yazılır ([**Diagnostics > System Logs > Configuration**](/tr/manual/diagnostics-system-logs/)).

## Diğer araçlar

Liste indiren veya servis yöneten her sayfanın arkasında bir komut satırı aracı bulunur. Bu araçların `status` alt komutları JSON çıktısı üretir.

| Araç | Alt komutlar |
| :--- | :--- |
| `stratum-update` | `check` - güncelleme sunucusunu sorgular; `run` - kurar (yedek, ZFS önyükleme ortamı, paket yükseltme, doğrulama); `status`; `prune` - eski önyükleme ortamlarını temizler. |
| `stratum-cert` | `show`; `selfsigned <ad\|ip> ...` - yeni kendinden imzalı sertifika; `install <cert.pem> <key.pem>`; `restore` - bir önceki sertifikaya döner. |
| `stratum-passwd [kullanıcı]` | Web paneli kullanıcısının parolasını değiştirir (varsayılan `admin`). |
| `stratum-ipbl` | `update [--force]`, `check <ip>` - bir IP adresinin hangi engelleme listesinde yer aldığını gösterir, `status`. |
| `stratum-dnsbl` | `update [--force]`, `check <alanadı>` - bir alan adını hangi DNS engelleme listesinin durdurduğunu gösterir, `status`. |
| `stratum-webfilter` | `update [--force]`, `check <alanadı>` - alan adının kategorilerini ve hangi politikaların engellediğini gösterir, `status`. |
| `stratum-ids` | `update [--force]` - ET Open kurallarını indirip yükler, `build`, `status`. |
| `stratum-openvpn` | `status` - bağlı istemciler; `profile <kullanıcı>` - kullanıcının `.ovpn` profilini yazdırır; `revoke <kullanıcı>`. |
| `stratum-backup` | `run` - şimdi otomatik uzak yedek al; `test` - SFTP bağlantısını sına; `list` - sunucudaki yedekleri listele. |
| `stratum-notify` | `test telegram` veya `test email`. |
| `stratum-gwmon status` | Ağ geçidi ölçümleri (gecikme, kayıp, durum). |
| `stratum-menu` | Konsol menüsü; `--setup` ilk açılış sihirbazını yeniden çalıştırır. |

Örnekler:

```
stratum-dnsbl check doubleclick.net
stratum-ipbl check 198.51.100.23
stratum-webfilter check example.com
stratum-update check
```

## Servisler (Services)

Stratum servisleri standart FreeBSD `rc.d` servisleridir:

| Servis | Karşılığı |
| :--- | :--- |
| `stratum` | Web yönetim paneli |
| `nginx` | Web paneli ve Captive Portal önündeki HTTPS sunucusu |
| `stratum_gwmon` | Ağ geçidi izleyicisi |
| `stratum_fwlog` | Güvenlik duvarı günlük toplayıcısı |
| `stratum_traffic` | Trafik grafiği toplayıcısı |
| `stratum_webact` | Web Etkinliği (Web Activity) toplayıcısı |
| `stratum_ipbl`, `stratum_ids` | IP engelleme listeleri ve Suricata saldırı tespit servisi |
| `stratum_wireguard`, `stratum_wgs2s`, `stratum_ipsec` | Çekirdek WireGuard ve IPsec servisleri |
| `stratum_captive` | Captive Portal servisi |
| `stratum_netextra` | Sanal IP'ler (VIP) ve statik rotalar |

```
service stratum status
service stratum restart
```

Paneldeki [**Services > Service Status**](/tr/manual/services-service-status/) sayfası da aynı servisleri gösterir ve yeniden başlatır. Servisleri elle farklı parametrelerle başlatmak yerine paneli veya Apply işlemini tercih edin; elle değiştirilen durum bir sonraki Apply sırasında yapılandırmaya göre geri düzeltilir.

## Dosya ve dizin yapısı

| Yol | İçeriği |
| :--- | :--- |
| `/usr/local/etc/stratum/config.json` | Ana yapılandırma dosyası (gizli parola/anahtar içermez) |
| `/usr/local/etc/stratum/` | Yalnızca `root` erişimli gizli dosyalar (anahtarlar, parolalar, jetonlar) ve panel sertifikası |
| `/usr/local/etc/stratum/backups/` | Yerel yapılandırma geçmişi ([**System > Backup**](/tr/manual/system-backup/)) |
| `/var/db/stratum/` | İndirilmiş engelleme/kategori listeleri, istatistikler, Web Activity ve Captive Portal oturumları |
| `/var/log/stratum.log` | Web paneli günlüğü |
| `/var/log/stratum-apply.log` | Her Apply işleminin günlüğü |
| `/var/log/stratum-update.log` | Sistem güncellemelerinin günlüğü |
| `/var/log/messages`, `/var/log/auth.log` | Genel sistem ve kimlik doğrulama günlükleri |
