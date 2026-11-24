---
title: "Sistem > Ayarlar: SNMP ve MCP"
seo_title: "09.4 Sistem > Ayarlar: SNMP ve MCP — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarını SNMP (LibreNMS, Zabbix, PRTG ...) ile izleme ve yapay zeka asistanlarının MCP sunucusu üzerinden sistem durumunu salt okunur sorgulaması: jetonlar, izinli adresler, 16 MCP aracı ve istemci kurulumu."
weight: 904
chapter_num: "09.4"
url: "/tr/manual/system-settings-snmp-mcp/"
aliases:
  - "/tr/manual/09-04-system-settings-snmp-mcp/"
translationKey: "manual-09-04-system-settings-snmp-mcp"
---

## SNMP

![System > Settings > SNMP](/images/manual/set-snmp.png)

Ağ izleme sistemleri - LibreNMS, Zabbix, PRTG, Observium, Cacti - güvenlik duvarını SNMP üzerinden okur: 64-bit trafik sayaçlarıyla tüm arayüzler, işlemci, bellek, diskler, çalışan süreçler ve `pf` istatistikleri.

| Ayar | Anlamı |
| :--- | :--- |
| **Answer SNMP v1 / v2c queries (read-only)** | SNMP ajanını etkinleştirir. |
| **Community** | 8-64 karakter; harf, rakam veya `. _ ~ -`. Parola gibi çalışır: `public` yerine uzun ve rastgele bir değer seçin. Yalnızca `root` erişimli saklanır ve bir daha gösterilmez; mevcut değeri korumak için alanı boş bırakın. |
| **Allowed from** | Her satıra bir IP adresi veya ağ - kendi izleme sunucunuz. Boş bırakılırsa seçilen iç arayüzlerdeki her cihaz sorgulayabilir. |
| **Answer on** | SNMP ajanının yanıt vereceği iç arayüzler. **WAN arayüzlerinde hiçbir zaman yanıt vermez.** |
| **Location**, **Contact** | İzleme sistemine gösterilen konum ve iletişim bilgisi. |

**Apply** sonrasında devreye girer. SNMP üzerinden güvenlik duvarında hiçbir ayar değiştirilemez (salt okunurdur).

Yararlı OID ağaçları:

| OID | İçeriği |
| :--- | :--- |
| `1.3.6.1.2.1.2`, `1.3.6.1.2.1.31` | Arayüzler (`IF-MIB`) ve 64-bit trafik sayaçları |
| `1.3.6.1.2.1.25` | İşlemci, bellek, diskler ve süreçler (`HOST-RESOURCES`) |
| `1.3.6.1.4.1.12325.1.200` | `pf` paket filtresi istatistikleri |

İzleme sunucusundan sınamak için:

```
snmpwalk -v2c -c <community> <firewall LAN adresi> 1.3.6.1.2.1.1
```

> **Not:** İç ağlarınızda bulunmayan (örneğin bulutta çalışan) bir izleme sunucusu SNMP servisine doğrudan ulaşamaz. Sunucuyu bir VPN tüneliyle ([**VPN > WireGuard S2S**](/tr/manual/vpn-wireguard-s2s/) veya [**IPsec**](/tr/manual/vpn-ipsec/)) bağlayarak güvenlik duvarının iç IP adresini sorgulamasını sağlayın.

## MCP (Model Context Protocol)

![System > Settings > MCP](/images/manual/set-mcp.png)

**Model Context Protocol (MCP)**, dış bir yapay zeka asistanının - MCP istemcisi üzerinden Claude, ChatGPT, Gemini veya DeepSeek - güvenlik duvarına *"son bir saatte neler engellendi?"* veya *"misafir ağında hangi cihazlar var?"* gibi sorular sormasına ve güvenlik duvarının gerçek anlık durumuyla yanıt vermesine olanak tanır.

**Her şey %100 salt okunurdur**: hiçbir araç güvenlik duvarında değişiklik yapamaz. Parolalar, özel anahtarlar, gizli jetonlar ve VPN yapılandırma dosyaları hiçbir zaman döndürülmez. Panelin yerleşik **Yapay Zeka Asistanı** ([**Diagnostics > AI Assistant**](/tr/manual/diagnostics-ai-assistant/)) da birebir aynı araçları kullanır.

### Etkinleştirme

1. **Let AI assistants read the firewall's state** anahtarını açın.
2. **Server address** bilgisini not edin: `https://<firewall>/mcp` (taşıma katmanı: *streamable HTTP*).
3. **Access tokens** altında bir **Name** girin (örneğin *Kaan - laptop*) ve isteğe bağlı olarak jetonu kullanabilecek kaynak adresleri **Allowed from** alanına yazın. Boş bırakılırsa yönetim paneline erişebilen her adres ([**Admin Access**](/tr/manual/system-settings-general-access/)) kullanabilir.
4. **Create token** düğmesine tıklayın ve üretilen jetonu **hemen kopyalayın** - yalnızca bir kez gösterilir. Sayfa ayrıca jetonun içine yerleştirildiği hazır istemci yapılandırma örneklerini de sunar.

Jetonlar yalnızca özet (hash) olarak saklanır; en fazla 20 jeton oluşturulabilir. Tablo her jetonun başlangıcını, izinli adresleri, **Last used** (zaman ve IP adresi) ve **Calls** sayısını gösterir. **Revoke** bir jetonu anında geçersiz kılar. Hatalı jeton denemeleri başarısız giriş sayılır ve oturum açma korumasında olduğu gibi IP adresini geçici olarak engeller.

### Araçlar (Tools)

16 aracın her biri tek tek kapatılabilir - **bir aracın döndürdüğü her veri kullandığınız yapay zeka sağlayıcısına iletilir**, bu nedenle paylaşmak istemediğiniz veri türlerini kapatabilirsiniz.

| Araç | Döndürdüğü bilgi |
| :--- | :--- |
| System status | Sistem adı, sürümler, çalışma süresi, CPU ve yük, bellek, disk |
| Interfaces | Rolü, fiziksel portu, bağlantı durumu, adresi ve anlık hızıyla tüm arayüzler |
| WAN status | Her WAN hattı için ağ geçidi, gecikme ve paket kaybı |
| Services | Servisler ve her birinin çalışıp çalışmadığı |
| Firewall log | Eşleşen kuralla birlikte son güvenlik duvarı günlük kayıtları ve süzgeçler |
| Blocked traffic summary | Son bir saatlik engelleme özeti: toplamlar, en çok engellenen kaynaklar, hedefler ve portlar |
| Firewall rules | Seçilen bir arayüzün veya grubun kuralları (otomatik kurallar dahil, sırasıyla) |
| NAT rules | Port yönlendirme ve giden NAT kuralları |
| DHCP leases | Ağ bazında aktif kiralamalar ve statik eşlemeler |
| VPN sessions | Bağlı OpenVPN kullanıcıları ve aktif WireGuard eşleri |
| Alerts | Son sistem olayları: WAN geçişleri, engellenen girişler, başarısız Apply, geri almalar, güncellemeler ... |
| Configuration changes | Son yapılandırma uygulamaları, onaylar ve geri almalar |
| Blocklists | IP ve DNS engelleme listeleri, kayıt sayıları, güncellemeler ve hatalar |
| Intrusion detection alerts | Suricata durumu ve son saldırı tespit uyarıları |
| Traffic statistics | Arayüz başına ortalama/tepe hız ve veri miktarı (son saat veya son gün) |
| Application usage | Uygulama Kontrolü üzerinden en çok veri kullanan uygulamalar, kategoriler ve cihazlar ile etkin kurallar ve engelledikleri bağlantılar |

**Save** anahtarı ve seçili araçları kaydeder; anında geçerli olur.

### Bir MCP istemcisi bağlama

**Claude Code:**

```
claude mcp add --transport http stratum https://<firewall>/mcp --header "Authorization: Bearer <token>"
```

**Gemini CLI** (`~/.gemini/settings.json`):

```json
{ "mcpServers": { "stratum": { "httpUrl": "https://<firewall>/mcp",
                               "headers": { "Authorization": "Bearer <token>" } } } }
```

**Diğer istemciler:** URL `https://<firewall>/mcp`, HTTP başlığı `Authorization: Bearer <token>`.

Yönetim paneli sertifikası siz değiştirmediyseniz kendinden imzalıdır (*self-signed*); bu nedenle MCP istemcilerinin sertifikaya güvenmesi gerekir: panel sertifikasını doğrudan [**System > Certificates**](/tr/manual/system-certificates/) sayfasından tek tıkla indirin (`stratum.crt` olarak) ve Node.js tabanlı istemcileri (Claude Code, Gemini CLI) `NODE_EXTRA_CA_CERTS=/yol/stratum.crt` ortam değişkeniyle başlatın.
