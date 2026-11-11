---
title: "REST API, MCP ve SNMP"
description: "Stratum Firewall'ı FastAPI REST API ile otomatikleştirin, Model Context Protocol (MCP) üzerinden yapay zeka asistanlarıyla canlı durumu sorgulayın ve SNMP ile izleyin."
lead: "Yapılandırmayı REST API ile otomatikleştirin, MCP üzerinden yapay zeka asistanlarıyla canlı ağ analizi yapın ve Zabbix, LibreNMS veya PRTG ile izleyin."
---

Stratum Firewall; FreeBSD 15.1 üzerinde **API-öncelikli** Python 3.12 / FastAPI mimarisiyle çalışır ve buna ek olarak yapay zeka asistanları için salt-okunur **Model Context Protocol (MCP)** sunucusu ile yerleşik FreeBSD **SNMP (`bsnmpd`)** ajanını barındırır.

## 1. REST API Kimlik Doğrulama ve Safe-Apply

Tüm yönetim uç noktaları standart HTTPS üzerinden sunulur (`https://<guvenlik-duvari-ip>/api/`):

```bash
# 1. Oturum cerezi (cookie) almak icin giris yapin
curl -k -X POST https://192.168.1.1/login \
  -d "username=admin&password=guvenli_sifreniz" \
  -c cookies.txt

# 2. CSRF basligiyla kimligi dogrulanmis istek gonderin
curl -k -X GET https://192.168.1.1/api/status \
  -b cookies.txt \
  -H "X-Requested-With: stratum"
```

* **RBAC Yetkileri:** **Yönetici** tüm uç noktalarda tam yetkiye sahiptir; **Operatör** okuma ve `/api/diag/*` tanılama işlemlerini çalıştırabilir; **Salt Okunur** hesaplarda yazma istekleri `403 Forbidden` döndürür.
* **Safe-Apply Onayı:** Yapılandırma değişiklikleri uygulandığında bağlantıyı korumak için **60 saniye** içinde `/api/apply/confirm` çağrılmalıdır; aksi halde sistem otomatik geri döner.

## 2. Yapay Zeka Tanılama — MCP Sunucusu (`Sistem → Ayarlar → MCP`, `v0.14.0`)

**Claude, ChatGPT, Gemini veya DeepSeek** gibi MCP destekli yapay zeka istemcilerini güvenlik duvarınıza tam kontrollü şekilde bağlayarak *"son 1 saatte hangi port taramaları engellendi?"* veya *"misafir ağında hangi cihazlar var?"* gibi sorular sorabilirsiniz:

* **%100 Salt Okunur (15+ Araç):** Sistem durumu, arayüzler, WAN gecikme/failover durumu, servisler, güvenlik duvarı ve NAT kuralları, Suricata IPS uyarıları (`ids_alerts`), engelleme özetleri, DHCP kiralamaları ve aktif VPN oturumlarını okur — **MCP üzerinden hiçbir ayar değiştirilemez**.
* **IP Kısıtlamalı Token ve Gizlilik:** Her asistan için ayrı erişim anahtarı (yalnızca özet/hash olarak saklanır) üretebilir ve istediğiniz aracı tek tek kapatabilirsiniz. Parolalar, özel anahtarlar ve VPN profilleri asla paylaşılmaz.

## 3. SNMP İzleme (`Sistem → Ayarlar → SNMP`, `v0.19.0`)

FreeBSD'nin yerel `bsnmpd` servisiyle (**Zabbix, LibreNMS, PRTG, Cacti** uyumlu, salt-okunur `SNMP v1 / v2c`) 64-bit arayüz sayaçlarını (`IF-MIB`), CPU/bellek/disk metriklerini ve `pf` istatistiklerini izleyebilirsiniz. Yalnızca seçilen iç arayüzlerden ve izin verilen IP adreslerinden gelen sorgulara yanıt verir; WAN üzerinde asla açılmaz.
