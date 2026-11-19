---
title: "Kurulum"
seo_title: "01. Kurulum — Stratum FW Kullanım Kılavuzu"
description: "Donanım ve sanal makine gereksinimleri, kurulum imajını indirme ve doğrulama, diske kurulum, ilk açılış kurulum sihirbazı ve konsol menüsü."
weight: 1
chapter_num: "01"
url: "/tr/manual/installation/"
aliases:
  - "/tr/manual/01-installation/"
translationKey: "manual-01-installation"
---

Bu bölüm sizi boş bir makineden web panelini açabileceğiniz çalışan bir güvenlik duvarına ulaştırır. Sistem gereksinimlerini, iki farklı kurulum imajını, kurulum aracının kendisini, ilk açılışta çalışan kurulum sihirbazını ve sonrasında ekranda göreceğiniz konsol menüsünü kapsar.

## Gereksinimler

Stratum FW 64 bit x86 donanımlarda (`amd64`) çalışır: kompakt ağ cihazları, kişisel bilgisayarlar, kabinet (rack) sunucular ve sanal makineler.

| Kaynak | En düşük (Minimum) | Önerilen |
| :--- | :--- | :--- |
| İşlemci | 64 bit x86 | AES-NI destekli bir işlemci (daha hızlı VPN ve şifreli yedekleme için) |
| Bellek (RAM) | 2 GB | DNS engelleme listeleri ve Web Filtresi ile 4 GB; saldırı tespit sistemi (Suricata) ile 8 GB veya üzeri |
| Disk | 8 GB (kurulum aracı daha küçük diskleri reddeder) | SSD üzerinde 32 GB veya üzeri - günlükler, raporlar ve birden fazla önyükleme ortamı (boot environment) için alan |
| Ağ portları | 1 (WAN) | 2 veya daha fazla: WAN, LAN ve ek ağlar ya da ikinci bir internet hattı için ek portlar |

Bellek değerleri yol göstericidir: Stratum FW daha düşük bellekle çalışmayı reddetmez, ancak saldırı tespit sistemi ve geniş engelleme listeleri belleğe önemli miktarda veri yükler.

**Ağ kartları.** FreeBSD 15.1 tarafından desteklenen ağ kartları sorunsuz çalışır; buna çoğu Intel (`igb`, `ix`, `em`), Realtek, Broadcom ve Chelsio kartı ile yaygın sanallaştırma platformlarının sanal ağ kartları dahildir. Bir güvenlik duvarı için en güvenli tercih Intel ağ kartlarıdır.

**Sanal makineler.** Stratum FW; Proxmox VE, KVM/QEMU, VMware ESXi, Hyper-V ve VirtualBox üzerinde çalışır.

- Kuruluma başlamadan *önce* sanal makineye en az iki ağ kartı ekleyin ve bunları doğru sanal anahtarlara (virtual switch) bağlayın; port atamalarını kurulum sihirbazında yapacaksınız.
- Proxmox ve KVM üzerinde disk ve ağ için **VirtIO** (`vtbd`, `vtnet`), VMware üzerinde ise **VMXNET3** kullanın.
- VPN'lerin donanım tabanlı şifrelemeden yararlanabilmesi için işlemci türünü `host` (veya AES-NI komut setini aktaran başka bir tür) olarak ayarlayın.

> **Not:** Başlamadan önce hangi fiziksel portun WAN (internete veya üst yönlendiriciye bakan taraf) ve hangisinin LAN (iç ağınıza bakan taraf) olacağını planlayın. Kurulum sihirbazı kabloyu taktığınızda ilgili portu otomatik bulabilir, ancak port adlarını bilmek işinizi kolaylaştırır: çoğu Intel tabanlı cihazda portlar kasa üzerindeki sırayla `igb0`, `igb1` ... şeklinde adlandırılır.

## İndirme ve doğrulama

Kurulum imajları, doğrulama özeti dosyasıyla (`SHA256SUMS-<sürüm>`) birlikte stratumfw.com [**İndir**](/tr/download/) sayfasında yayınlanır:

| Dosya | Kullanım amacı |
| :--- | :--- |
| `stratum-fw-<version>-amd64.iso` | Sanal makineler ve CD/DVD'den veya sunucu yönetim kartının (IPMI, iDRAC, iLO) bağladığı sanal ISO üzerinden açılan fiziksel makineler. |
| `stratum-fw-<version>-amd64.img` | Fiziksel makineler için USB bellek kurulumu. |

Her iki imaj da aynı sistemi içerir ve hem ekran + klavye ile **hem de** seri konsol üzerinden aynı anda çalışır (115200 baud, 8N1, birinci seri port). Ekran çıkışı olmayan cihazlar - çoğu donanım güvenlik duvarı kutusu - seri veya USB konsol kablosuyla konsol portu üzerinden kurulur. Kurulan güvenlik duvarı aynı konsol desteğini korur.

İndirilen dosyanın eksiksiz ve değiştirilmemiş olduğunu her zaman doğrulayın. Çıkan sonucu `SHA256SUMS-<version>` dosyasındaki (ve İndir sayfasındaki) ilgili satırla karşılaştırın:

```bash
# Linux
sha256sum stratum-fw-0.36.1-amd64.img

# macOS / FreeBSD
shasum -a 256 stratum-fw-0.36.1-amd64.img

# Windows (PowerShell)
Get-FileHash .\stratum-fw-0.36.1-amd64.img -Algorithm SHA256
```

Değerler uyuşmuyorsa dosyayı yeniden indirin. Özeti (checksum) eşleşmeyen bir imajdan asla kurulum yapmayın.

## USB belleğe yazdırma

Bellekteki tüm veriler silinir.

- **Windows, macOS, Linux:** **balenaEtcher** veya **Rufus** kullanın. Rufus sorarsa *DD image* (DD yansıması) kipini seçin.
- **Linux komut satırı** - `/dev/sdX` yerine kendi USB belleğinizin adını yazın (`lsblk` ile kontrol edin; yanlış aygıtı seçmek o diski siler):

```bash
sudo dd if=stratum-fw-0.36.1-amd64.img of=/dev/sdX bs=4M status=progress conv=fsync
```

- **macOS komut satırı** - belleği `diskutil list` ile bulun, ardından:

```bash
diskutil unmountDisk /dev/diskN
sudo dd if=stratum-fw-0.36.1-amd64.img of=/dev/rdiskN bs=4m
```

## Konsola bağlanma

- **Ekran ve klavye:** bunları bağlayın ve USB bellekten veya ISO'dan önyükleme yapın (önyükleme menüsü tuşu genellikle <kbd>F11</kbd>, <kbd>F12</kbd> veya <kbd>Esc</kbd>'dir; sunucularda yönetim kartı ISO dosyasını doğrudan bağlayabilir).
- **Seri konsol:** konsol kablosunu bağlayın ve **115200 baud, 8 veri biti, eşlik yok (no parity), 1 dur biti, akış denetimi yok (no flow control)** ayarlarıyla bir uçbirim açın:

```bash
# Linux / macOS (aygıt adı kabloya göre değişir)
screen /dev/ttyUSB0 115200
```

  Windows üzerinde PuTTY kullanın: bağlantı türü *Serial*, kablonun COM portu, hız `115200`.

## Kurulum aracı (Installer)

Makine **Welcome to Stratum FW** önyükleme menüsüyle açılır, birkaç saniye sonra kendiliğinden devam eder ve kurulum aracını başlatır:

```
==============================================================
  Stratum FW 0.36.1 - Installer
==============================================================

  1) Install Stratum FW
  2) Shell
  3) Reboot
  4) Power off
```

![Kurulum aracının ana menüsü](/images/manual/install-menu.png)

**1) Install Stratum FW** seçeneğini seçin.

### 1. Hedef diski seçin

Kurulum aracı bulduğu diskleri boyutu ve modeliyle listeler. Önyükleme yaptığınız USB bellek veya CD bu listede gösterilmez.

```
Available disks:
  1) ada0       128 GB  KINGSTON SA400S37128G
  2) da1         32 GB  SanDisk Ultra

Select target disk [1]:
```

Diskin numarasını yazıp <kbd>Enter</kbd> tuşuna basın. 8 GB'tan küçük diskler kabul edilmez.

### 2. Onaylayın

```
  !!! ALL DATA ON ada0 (128 GB) WILL BE ERASED !!!
  Type 'yes' to continue:
```

Tam olarak `yes` yazın. Başka herhangi bir giriş diske dokunmadan menüye geri döner.

### 3. root parolasını belirleyin

```
Root password (console and SSH, min. 8 characters):
Repeat password:
```

Bu parola sistemin SSH üzerinden oturum açmak için kullanılan `root` hesabına aittir. Web panelinin parolası **değildir** - web paneli parolası ilk açılıştaki kurulum sihirbazında belirlenir. Uzun, benzersiz bir parola seçin ve güvenle saklayın.

### 4. Kurulum işlemi

Kurulum aracı bu aşamada kendi başına çalışır; internetten hiçbir şey indirilmez, tüm bileşenler doğrudan kurulum medyasından yüklenir:

```
Installing Stratum FW 0.36.1 to ada0 (log: /tmp/stratum-install.log)

  Partitioning ada0                              ok
  Creating ZFS pool                              ok
  Installing FreeBSD base (offline)              ok
  Configuring system                             ok
  Installing Stratum FW packages                 ok
  Stratum system integration                     ok
  Setting root password                          ok
  Installing boot loaders (BIOS + UEFI)          ok
  Finalizing                                     ok

Installation complete. Remove the install media before rebooting.
Reboot now? [Y/n]:
```

İşlem birkaç dakika sürer. USB belleği çıkarın (veya sanal ISO medyasını ayırın), ardından yeniden başlatmak için <kbd>Enter</kbd> tuşuna basın.

Herhangi bir adım başarısız olursa kurulum aracı **Installation FAILED at:** başlığıyla ilgili adımı ve günlük dosyasının son satırlarını gösterir, ardından menüye döner. Yeniden denemeden önce tüm günlüğü (`/tmp/stratum-install.log`) okumak için **2) Shell** seçeneğini kullanabilirsiniz. En sık karşılaşılan nedenler arızalı bir disk veya başka bir sistemin RAID ya da ZFS havuzu tarafından hâlâ kullanımda olan bir disktir.

### Kurulum aracının oluşturduğu bölümler

| Bölüm (Partition) | Boyut | Amaç |
| :--- | :--- | :--- |
| `freebsd-boot` | 512 KB | Klasik BIOS makineler için önyükleme kodu |
| `efi` | 260 MB | UEFI makineler için önyükleyici |
| `freebsd-swap` | 2 GB | Takas (Swap) alanı |
| `freebsd-zfs` | kalan alan | Sıkıştırmalı `zroot` ZFS havuzu |

Disk hem BIOS hem de UEFI makinelerde önyükleme yapabilir. Sistem `zroot/ROOT/default` önyükleme ortamında yaşar; sonraki her güncelleme bunun yanına yeni bir önyükleme ortamı ekler (bkz. **System > Update** bölümü).

## İlk açılış: kurulum sihirbazı

İlk açılışta tüm konsollarda şu ekran görünür:

```
Welcome to Stratum FW 0.36.1

This wizard assigns network ports, sets IP addresses, WAN access
and the web panel administrator password.

Press Enter to start the setup wizard...
```

Kullandığınız konsolda <kbd>Enter</kbd> tuşuna basın. Makinede hem ekran hem seri konsol bağlıysa sihirbaz ilk <kbd>Enter</kbd> basılan konsolda çalışır; diğer konsol işlem tamamlanana kadar bekler.

### Adım 1 - Arayüzleri atayın

Sihirbaz ağ portlarını MAC adresleri, hat (link) durumu ve hız bilgisiyle listeler:

```
 Port     MAC                Link         Media                   Assigned
 igb0     00:90:0b:aa:bb:01  active       Ethernet autoselect     
 igb1     00:90:0b:aa:bb:02  no carrier   Ethernet autoselect     
 igb2     00:90:0b:aa:bb:03  no carrier   Ethernet autoselect     

WAN interface (name or 'a' for auto-detect):
```

Her rol için port adını yazın veya sihirbazın otomatik bulması için `a` girin: kablo takılıysa çıkarın ve 30 saniye içinde yeniden takın; bağlantısı aktifleşen port otomatik seçilir.

| Rol | Zorunlu mu | Açıklama |
| :--- | :--- | :--- |
| **WAN** | evet | İnternet tarafı: modeme, servis sağlayıcı yönlendiricisine veya üst ağa bakan port. |
| **LAN** | hayır (atlamak için <kbd>Enter</kbd>) | İç ağınız. Web paneline buradan erişilir. |
| **OPT1**, **OPT2** ... | hayır | Ek ağlar (misafir, sunucular, kameralar vb.) veya ikinci bir internet hattı. İsim yazmadan <kbd>Enter</kbd> tuşuna basana kadar sihirbaz sormaya devam eder. |

> **Uyarı:** LAN atanmazsa WAN üzerinden yönetim erişimini açana kadar güvenlik duvarı yalnızca konsoldan yönetilebilir. Özel bir nedeniniz yoksa mutlaka bir LAN atayın.

Sihirbaz yapılan atamayı gösterir ve **Proceed?** diye sorar; `y` yazın (veya doğrudan <kbd>Enter</kbd> tuşuna basın).

Tüm bu atamaları daha sonra değiştirebilirsiniz: ek portlar ve VLAN'lar panelde **Interfaces > Assignments** ve **Interfaces > VLANs** altından eklenir.

### Adım 2 - IP adresleri

Sihirbaz sırayla atanan her arayüz için adres ayarlarını sorar.

**WAN**

```
Configure WAN IPv4 via DHCP? [Y/n]:
```

- WAN adresini otomatik alıyorsa (yönlendirici kipindeki modem, servis sağlayıcı yönlendiricisi veya kurumsal üst ağ) varsayılan **Yes** seçeneğini kullanın.
- Sabit IP adresi için **No** seçin: adresi önek uzunluğuyla birlikte girin (örneğin `203.0.113.10/29`), ardından aynı ağ içinde yer alan ağ geçidini (gateway) yazın.

PPPoE bağlantısı (köprü kipindeki fiber veya DSL modemden doğrudan bağlantı) bu adımda ayarlanmaz: şimdilik DHCP seçin ve kurulum sonrasında panelde (**Interfaces > Assignments** altından WAN arayüzünde) PPPoE olarak yapılandırın.

**LAN ve OPT arayüzleri**

```
LAN IPv4 address with prefix (e.g. 192.168.1.1/24) [192.168.1.1/24]:
Enable DHCP server on LAN? [Y/n]:
DHCP range start [192.168.1.100]:
DHCP range end [192.168.1.199]:
```

- Girilen adres, güvenlik duvarının o ağdaki kendi adresi ve ağdaki cihazların varsayılan ağ geçididir. LAN için `192.168.1.1/24` önerilir; bu alt ağ üst tarafta zaten kullanılıyorsa (örneğin WAN tarafındaki servis sağlayıcı modeminde) `10.10.10.1/24` gibi farklı bir ağ seçin.
- DHCP sunucusu ağdaki cihazlara otomatik IP adresi dağıtır. LAN için açık, OPT arayüzleri için kapalı olarak önerilir. Dağıtım aralığı ilgili alt ağın içinde olmalı ve güvenlik duvarının kendi IP adresini içermemelidir.

### Adım 3 - WAN üzerinden yönetim erişimi

```
Management access from WAN
Allows the web panel (HTTPS) and SSH to be reached from the WAN side.
Recommended: No - manage the firewall from LAN.
Allow web panel and SSH access from WAN? [y/N]:
```

Güvenlik duvarı bir laboratuvar ortamında veya başka bir güvenlik duvarının arkasında LAN olmadan çalışmıyorsa **No** yanıtını verin. İhtiyaç duyarsanız daha sonra **System > Settings > Admin Access** altından (yalnızca belirlediğiniz IP adresleriyle sınırlandırarak) veya konsol menüsünden açabilirsiniz.

### Adım 4 - Saat dilimi

```
Timezone or part of a name [Europe/Istanbul]:
```

Saat dilimini veya şehir adının bir kısmını (`berlin`, `new_york`, `istanbul`) yazın; birden fazla eşleşme varsa sihirbaz bunları listeler. Günlükler, zamanlamalar ve raporlar bu saat dilimini kullanır.

### Adım 5 - Web paneli yöneticisi

```
Web panel administrator
New password for 'admin' (min. 10 characters):
Repeat password:
```

Bu parola, **Administrator** rolüne sahip `admin` panel kullanıcısının parolasıdır. Ek kullanıcılar daha sonra panel üzerinden (**System > Users**) eklenir.

### Adım 6 - Uygulama ve onaylama

Sihirbaz ayarları tıpkı web panelindeki gibi uygular (bkz. Giriş bölümündeki [**Bir değişiklik sisteme nasıl ulaşır**](/tr/manual/introduction/#how-a-change-reaches-the-system)): yapılandırma önce denetlenir, etkinleştirilir ve kalıcı olması için size 120 saniye süre tanınır:

```
New settings are active. If you do not answer within 120 seconds they are reverted automatically.
Keep the new settings? (n restores the previous settings) [Y/n]:
```

Ayarları korumak için <kbd>Enter</kbd> tuşuna basın. Ön denetim başarısız olursa hiçbir şey değiştirilmez ve sihirbaz nedenlerini listeler; aşağıdaki konsol menüsüyle düzeltip yeniden deneyebilirsiniz.

## Konsol menüsü

Kurulum sihirbazı tamamlandıktan sonra - ve o andan itibaren konsol her açıldığında - ekranda güvenlik duvarının özeti ve konsol menüsü gösterilir:

```
Stratum FW 0.36.1  -  stratum  -  FreeBSD 15.1-RELEASE

 WAN   igb0     203.0.113.10/29        static  up
 LAN   igb1     192.168.1.1/24         static  up

 Web panel: https://192.168.1.1/
 WAN management access: closed

  1) Assign interfaces               6) Management access
  2) Set interface IP address        7) Pending change
  3) Reset admin password            8) Shell
  4) Restart web panel               9) Reboot system
  5) Ping host                      10) Halt system
  0) Exit

Enter an option:
```

![Konsol menüsü](/images/manual/install-console-menu.png)

Üst satırlar her arayüzü portu, adresi, çalışma kipi ve hat durumuyla birlikte listeler; hemen altında web panelinin erişim adresi yer alır.

| Seçenek | İşlevi |
| :--- | :--- |
| **1) Assign interfaces** | Sihirbazda olduğu gibi portları yeniden WAN, LAN ve OPT rollerine atar. |
| **2) Set interface IP address** | Bir arayüzün IP adresini (ve DHCP aralığını) değiştirir. |
| **3) Reset admin password** | Bir panel kullanıcısı için (varsayılan `admin`) yeni parola belirler. Telefon kaybı gibi durumlar için kullanıcının iki aşamalı girişini (2FA) kapatma seçeneği de sunar. |
| **4) Restart web panel** | Sayfa yüklenmediğinde web panelini ve web sunucusunu yeniden başlatır. |
| **5) Ping host** | İnternet bağlantısını (varsayılan `1.1.1.1`) veya ağınızdaki bir hedefi test eder. |
| **6) Management access** | Panel ve SSH erişim durumunu gösterir; WAN erişimini açıp kapatır; veya bir ayar nedeniyle kilitlendiğinizde yönetim erişimini varsayılanlara (port 443, tüm iç ağlar, anti-lockout açık, SSH açık) sıfırlar. |
| **7) Pending change** | Onay bekleyen bir değişikliği kalıcı yapar veya geri alır - örneğin erişimi kaybetmeden hemen önce panelden uyguladığınız bir değişiklik için. |
| **8) Shell** | `root` yetkisiyle komut kabuğu açar. Menüye dönmek için `exit` yazın. |
| **9) Reboot system** / **10) Halt system** | Onay aldıktan sonra sistemi yeniden başlatır veya kapatır. |

Konsol menüsünden yapılan değişiklikler de sihirbazda olduğu gibi önce denetlenir, uygulanır ve onaylanır.

> **Uyarı:** Güvenlik duvarının sahada her zaman onarılabilmesi için fiziksel konsol otomatik olarak `root` yetkisiyle oturum açar ve bu menüyü getirir. Konsola fiziksel erişimi olan herkes güvenlik duvarı üzerinde tam kontrole sahiptir: cihazı kilitli bir sistem odasında veya kabinette tutun ve seri konsol kablosunu bir anahtar gibi koruyun.

## Sıradaki adımlar

LAN üzerindeki bir bilgisayardan **Web panel** satırında gösterilen adresi (örneğin `https://192.168.1.1/`) açın ve `admin` olarak giriş yapın. Sertifika güvenlik duvarının kendisi tarafından oluşturulduğu için tarayıcı ilk açılışta bir kez güvenlik uyarısı gösterir; yeni kurulumda bu beklenen bir durumdur.

**2 Initial configuration** (İlk yapılandırma) bölümüyle devam edin: ilk oturum açma, internet bağlantısı, DNS ve ilk güvenlik duvarı kuralları.
