---
title: "Sistem > Yedekleme"
seo_title: "09.7 Sistem > Yedekleme (System > Backup) — Stratum FW Kullanım Kılavuzu"
description: "Kullanıcılar ve gizli anahtarlarla birlikte yedek indirme, AES-256 şifreleme, geri yükleme, satır satır fark (diff) gösteren yapılandırma geçmişi, OPNsense'ten içe aktarma ve SFTP sunucusuna otomatik şifreli yedekleme."
weight: 907
chapter_num: "09.7"
url: "/tr/manual/system-backup/"
aliases:
  - "/tr/manual/09-07-system-backup/"
translationKey: "manual-09-07-system-backup"
---

**System > Backup** yapılandırmanızı üç farklı yolla güvende tutar: bilgisayarınıza indirdiğiniz **yedek dosyası**, doğrudan güvenlik duvarı üzerindeki **yapılandırma geçmişi** ve **bir sunucuya otomatik şifreli yedekleme**.

![System > Backup](/images/manual/backup.png)

## Yedek indirme (Download a backup)

Tüm yapılandırmayı tek bir dosyada toplar.

| Seçenek | Anlamı |
| :--- | :--- |
| **Include panel users** | Kullanıcı adları, roller ve parola özetleri. |
| **Encrypt with a password (AES-256)** | Dosya yalnızca belirlediğiniz parolayla (en az 12 karakter) açılabilir. |
| **Include certificates and secrets** | (Yalnızca şifreli yedeklerde.) OpenVPN CA ve kullanıcı sertifikaları, WireGuard anahtarları, bildirim, SNMP ve MCP gizli bilgileri ile panel sertifikası. Bu seçenekle alınan yedek sayesinde yeni bir güvenlik duvarı **VPN profillerini yeniden dağıtmaya gerek kalmadan** görevi devralır. |

**Download** düğmesine tıklayın. Şifresiz yedek `.json`, şifreli yedek ise `.stbk` uzantılıdır.

> **Yedeği güvenli bir yerde saklayın.** Gizli anahtarlar seçeneği kapalı olsa bile yedek dosyası PPPoE parolası gibi temel bağlantı bilgilerini içerir. Şifreli bir `.stbk` dosyası parolası olmadan - Stratum tarafından bile - kesinlikle geri açılamaz; bu nedenle parolayı yedek dosyasından ayrı ve güvenli bir yerde saklayın.

**Ne zaman yedek alınmalı:** kapsamlı değişikliklerden önce, güncellemelerden önce (güncelleme süreci kendi yedeğini de otomatik alır) ve düzenli aralıklarla - ya da aşağıdaki otomatik uzak yedeklemeyi kurun.

## Yedekten geri yükleme (Restore from a backup)

1. **Choose file** ile bir Stratum FW yedeği (`.json` veya şifreliyse `.stbk`) ya da düz `config.json` seçin.
2. Şifreli dosya için parolasını girin.
3. **Also restore panel users** (anında devreye girer) ve **Also restore certificates and secrets** (mevcut sertifika ve anahtarların üzerine yazar) seçeneklerini belirleyin.
4. **Restore** düğmesine tıklayın.

Yapılandırma **bekleyen değişiklikler (pending changes)** olarak yüklenir: siz **Apply** diyene kadar çalışan sistemde hiçbir şey değişmez - ve Apply sonrasında da geri alma güvencesi çalışır. Uygulamadan önce bekleyen değişiklikleri gözden geçirin.

**Yeni bir donanıma geri yükleme:** arayüzler fiziksel port adlarıyla (`igb0`, `em1` ...) eşleştirilir. Yedekteki port adları yeni donanımda yoksa geri yükleme ekranı hangi portların eksik olduğunu bildirir; Apply öncesinde [**Interfaces > Assignments**](/tr/manual/interfaces-assignments/) sayfasından portları yeni donanıma eşleştirin.

Sertifika ve gizli bilgileri içermeyen bir yedek bunları geri yükleyemez; ayrıca yönetim paneline erişebilen en az bir etkin yönetici içermeyen kullanıcı yedeği reddedilir.

**Import from OPNsense** bir OPNsense `config.xml` dosyasını dönüştürür - ayrıntılar için **OPNsense'ten Geçiş** bölümüne bakınız.

## Yapılandırma geçmişi (Configuration history)

Yapılandırma her kaydedildiğinde bir kopyası saklanır (en yeni 30 kayıt). Her kayıt için **Saved** zamanı, **File**, **Size** ve şu işlemler sunulur:

- **Diff** mevcut yapılandırmayla arasındaki farkları satır satır gösterir;
- **Download** o kopyayı dosya olarak indirir;
- **Restore** o kopyayı bekleyen değişiklik olarak yükler - her zamanki gibi Apply ile devreye alınır.

Günler sonra fark edilen hatalı bir değişiklikten geri dönmenin en hızlı yolu yapılandırma geçmişidir.

## Bir sunucuya otomatik yedekleme (Automatic backup to a server)

**Automatic backup to a server > Configure** belirlediğiniz takvimde **SFTP** üzerinden şifreli yedek yükler - bir NAS cihazına (Synology, QNAP, TrueNAS) ya da Linux veya Windows OpenSSH sunucusuna.

![Otomatik yedekleme](/images/manual/backup-auto.png)

### 1. Güvenlik duvarının SSH anahtarı

Güvenlik duvarı uzak sunucuya kendi SSH anahtarıyla bağlanır - sunucu parolası tutulmaz.

1. **Create SSH key** düğmesine tıklayın.
2. Gösterilen açık anahtar satırını **Copy** ile kopyalayın ve uzak sunucudaki yedekleme kullanıcısının `~/.ssh/authorized_keys` dosyasına ekleyin.

İlk bağlantıda uzak sunucunun ana makine anahtarı (*host key*) kaydedilir; sonraki tüm bağlantılarda bu anahtarın eşleşmesi zorunludur - sunucunuzu taklit eden sahte hedeflere karşı koruma sağlar. Uzak sunucu yeniden kurulduysa **Forget the host key** ile eski kaydı sıfırlayın.

### 2. Ayarlar

| Alan | Anlamı |
| :--- | :--- |
| **Upload an encrypted backup to the server on a schedule** | Otomatik yedeklemeyi etkinleştirir. |
| **Server (SFTP)**, **Port** | Sunucu IP adresi veya adı ve port (varsayılan `22`). |
| **User name** | Uzak sunucudaki kullanıcı adı. |
| **Folder** | Kullanıcının ev dizinine göreli veya tam dizin yolu. Klasör sunucuda mevcut olmalıdır. |
| **Schedule**, **On**, **At** | Saatlik, 6 veya 12 saatte bir, günlük, haftalık veya **Off (manual only)**; gün ve saat. |
| **Keep on the server** | Uzak sunucuda tutulacak en yeni yedek sayısı (`1`-`365`, varsayılan `30`); daha eski yedekler otomatik silinir. |
| **Include panel users**, **Include certificates and secrets** | Manuel indirmedekiyle aynıdır; varsayılan olarak her ikisi de açıktır. |
| **Encryption password** | En az 12 karakter. **Bu parolayı ayrıca güvenli bir yerde mutlaka saklayın.** |

**Test connection** sunucuya giriş yapıp hedef klasörü denetler. **Back up now** anında bir yedek yükler. **Save** anında devreye girer (Apply gerekmez).

### Nasıl çalışır?

Uzak sunucu her zaman yalnızca `stratum-<hostname>-<tarih>.stbk` adında **AES-256-GCM** ile şifrelenmiş dosyalar görür. İhtiyaç halinde bu dosyalardan herhangi birini aynı parolayla **System > Backup** üzerinden - aynı cihaza veya yeni bir güvenlik duvarına - geri yükleyebilirsiniz.

**Last backup** son yedeklemenin zamanını ve sonucunu gösterir. Başarısız olan bir yedekleme bir sonraki saat başında yeniden denenir ve [**System > Settings > Notifications**](/tr/manual/system-settings-logging-notifications/) üzerinden bildirilir.
