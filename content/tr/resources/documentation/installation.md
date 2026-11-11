---
title: "Kurulum Rehberi (.iso ve .img)"
description: "Stratum Firewall'ı önyüklenebilir VGA ISO (.iso) veya başsız Seri Konsol USB (.img) kurulum imajıyla fiziksel donanımınıza ya da sanal makinenize kurun."
lead: "Çevrimdışı ZFS kurulum imajlarımızı kullanarak Stratum Firewall'ı fiziksel cihazınıza veya sanal altyapınıza dakikalar içinde kurun."
aliases: ["/tr/resources/documentation/building-from-source/"]
---

Stratum Firewall, [İndir](/tr/download/) sayfasında doğrudan kuruluma hazır **FreeBSD 15.1-RELEASE (`amd64`)** işletim sistemi imajları olarak sunulur. Kurulum sihirbazı hem UEFI hem de klasik BIOS sistemlerle uyumlu GPT bölümlemesini, **ZFS kök dosya sistemini** ve atomik **ZFS Önyükleme Ortamlarını (Boot Environments)** tamamen çevrimdışı olarak yapılandırır; kurulum sırasında internet bağlantısı gerekmez.

> **Kurulumdan önce arayüzü görmek ister misiniz?** Medya yazdırmadan önce [Canlı Tarayıcı Demosu](https://demo.stratumfw.com) (`demo` / `Stratum2026`) üzerinden çalışan gerçek bir FreeBSD 15.1 sistemini inceleyebilirsiniz.

## 1. Donanım ve Sanal Makine Gereksinimleri

| Kaynak | Minimum | Önerilen (Suricata IPS / DNSBL / Çoklu WAN) |
| :--- | :--- | :--- |
| **Mimari** | 64-bit x86 (`amd64`) | AES-NI destekli 64-bit x86 (`amd64`) |
| **Bellek (RAM)** | 2 GB | 4 GB – 8 GB (ZFS önbelleği, Unbound RPZ ve Suricata IPS için) |
| **Depolama** | 16 GB SSD / NVMe / Sanal Disk | 32 GB+ SSD / NVMe (son 5 ZFS önyükleme ortamı ve günlükler için) |
| **Ağ Kartı (NIC)** | 2 Port (`WAN` + `LAN`) | 2+ Intel veya VirtIO ağ kartı (`igb`, `ix`, `em`, `vtnet`) |

## 2. Doğru Kurulum İmajını Seçme

[İndir](/tr/download/) sayfasında iki farklı kurulum dosyası yer alır:

* **VGA ISO Kurulum İmajı (`stratum-fw-0.27.1-amd64.iso`):** Monitör ve klavye bağlı fiziksel sunucular ile tüm sanallaştırma platformları (**Proxmox VE**, **VMware ESXi**, **KVM / QEMU**, **Hyper-V**) için standart ISO imajıdır.
* **Seri Konsol USB İmajı (`stratum-fw-0.27.1-amd64.img`):** Ekran çıkışı olmayan (headless) donanım güvenlik duvarı cihazları (`COM1` / `ttyu0` üzerinde `115200 baud, 8N1`, RJ45/USB konsol kabloları ve IPMI Serial-over-LAN) için doğrudan USB belleğe yazdırılan ham disk imajıdır.

Kurulan sistem, hangi medyadan kurulduysa (`VGA` veya `Serial`) o konsol modunu otomatik olarak korur.

## 3. SHA-256 Doğrulaması ve USB Belleğe Yazdırma

İndirdiğiniz dosyanın bütünlüğünü [İndir](/tr/download/) sayfasındaki `SHA-256` özetiyle karşılaştırın:

```bash
# Linux veya macOS
sha256sum stratum-fw-*-amd64.*

# Windows PowerShell
Get-FileHash .\stratum-fw-*-amd64.* -Algorithm SHA256
```

İmajı USB belleğe yazdırmak için:

* **Windows / macOS / Linux Arayüz:** **BalenaEtcher** veya **Rufus** (sorulursa *DD Yansıma modu*) ile `.img` veya `.iso` dosyasını doğrudan USB belleğinize yazdırın.
* **Linux / FreeBSD / macOS Komut Satırı:**
  ```bash
  # /dev/sdX kismini kendi USB surucu yolunuzla degistirin!
  sudo dd if=stratum-fw-0.27.1-amd64.img of=/dev/sdX bs=4M status=progress conv=fsync
  ```

## 4. Sanal Makine İpuçları (Proxmox / KVM / ESXi)

* **Disk ve Ağ Sürücüleri:** Proxmox/KVM üzerinde en yüksek hat hızı için **VirtIO Block** (`vtbd0`) ve **VirtIO Network** (`vtnet0`, `vtnet1`); VMware ESXi üzerinde `VMXNET3` veya `E1000e` seçin.
* **İşlemci Türü:** OpenVPN, WireGuard ve şifreli yedeklemelerde donanımsal şifreleme hızlandırması için CPU tipini `host` (veya `AES` destekli profil) yapın.

## 5. Kurulum ve İlk Açılış Konsol Sihirbazı

1. Cihazı kurulum medyasından başlatın. Seri konsol (`.img`) cihazlarında terminal programınızı (`screen`, `minicom` veya PuTTY) **`115200 8N1`** hızında bağlayın.
2. Kurulum ekranında hedef diski seçin; sihirbaz diski **ZFS** (`zroot/ROOT/default`) olarak biçimlendirir ve kurulumu çevrimdışı tamamlar.
3. Kurulum bittiğinde USB/ISO medyasını çıkarıp sistemi yeniden başlatın.
4. İlk açılışta **Stratum Konsol Sihirbazı (`stratum-menu`)** üzerinden:
   * Fiziksel ağ portlarını `WAN`, `LAN` ve isteğe bağlı `OPT` arayüzlerine atayın,
   * `LAN` IP adresini (varsayılan `192.168.1.1/24`, DHCP açık) ve `WAN` bağlantı tipini (`DHCP`, `Statik` veya `PPPoE`) belirleyin,
   * Saat dilimini ve yönetici (`admin`) parolasını tanımlayın.

Ardından yerel ağdaki bir bilgisayardan `https://192.168.1.1/` adresine bağlanarak [İlk Yapılandırma](/tr/resources/documentation/initial-configuration/) adımlarına geçebilir ve **Sistem → Güncelleme** menüsünden en son sürüme (**v0.29.0**) tek tıkla yükseltebilirsiniz.
