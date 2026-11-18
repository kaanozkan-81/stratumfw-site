---
title: "Stratum Firewall v0.35.1: Yerleşik Salt Okunur Yapay Zeka Asistanı, PUB-1 IP Maskeleme ve Yerel Ollama"
date: 2026-11-18
author: "Stratum Security"
translationKey: "v0-35-1-ai-assistant"
description: "Stratum Firewall v0.35.1; 14 salt okunur MCP aracı, deterministik kural sayımı, bulut modelleri için otomatik PUB-1 genel IP maskeleme ve %100 yerel LAN Ollama desteğiyle Tanılama > Yapay Zeka Asistanı modülünü sunuyor."
lead: "FreeBSD 15.1 tabanlı güvenlik duvarınıza doğal dilde tanılama soruları sorun: sıfır yapılandırma değişikliği riski, bulut sorgularında otomatik genel IP maskeleme ve verinin ağ dışına hiç çıkmadığı yerel Ollama desteği."
images: ["/images/ai-assistant-ui.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "yapay zeka asistani", "mcp", "ollama", "gizlilik", "v0.35.1"]
---

Üretim ortamındaki bir güvenlik duvarının içine yapay zeka eklemek, her sistem ve ağ yöneticisinin aklına haklı olarak iki temel soruyu getirir: **Ya model yanlış bir çıkarımla kural değiştirip ağı kilitlerse?** ve **Gerçek WAN IP adreslerimi, tünel durumlarımı veya anahtarlarımı neden bir bulut servisine göndereyim?**

[Stratum Firewall](https://stratumfw.com/tr/) üzerinde yayınladığımız **`v0.35.0` ve `v0.35.1`** sürümleriyle gelen **`Tanılama > Yapay Zeka Asistanı` (`Diagnostics > AI Assistant`)**, bu iki riski yalnızca metin tabanlı uyarılarla değil, doğrudan mimari sınırlarla ortadan kaldıracak şekilde tasarlandı. Modülün tüm teknik ayrıntılarını [Yerleşik Yapay Zeka Asistanı](https://stratumfw.com/tr/product/ai-assistant/) sayfasında ve tüm değişiklik geçmişini [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) üzerinde inceleyebilirsiniz.

![Stratum Firewall Yerleşik Salt Okunur Yapay Zeka Asistanı Arayüzü](/images/ai-assistant-ui.jpg)

### 1. Mimari Olarak Salt Okunur: 14 Canlı MCP Tanılama Aracı

Kabuk komutu çalıştıran veya yapılandırma dosyalarına yazma yetkisi olan otomasyon araçlarının aksine, `Tanılama > Yapay Zeka Asistanı` modülünün sistem üzerinde **hiçbir yazma yetkisi yoktur**. `config.json` dosyasını değiştiremez, `pf` kurallarını yeniden yükleyemez, durum tablolarını sıfırlayamaz, `strongSwan` ya da `Unbound` servislerini yeniden başlatamaz.

Bunun yerine asistan, Stratum Firewall'ın Python 3.12 / FastAPI kontrol katmanındaki **14 salt okunur MCP (Model Context Protocol) aracını** kullanarak canlı sistem durumunu okur:

- **Canlı Kural, NAT ve Trafik Analizi:** Etkin `pf` filtre kurallarını, port yönlendirmelerini, giden NAT eşlemelerini, `dummynet` bant genişliği sınırlayıcılarını ve arayüz sayaçlarını inceler.
- **5 Katmanlı Security Guard Görünürlüğü:** [Security Guard](https://stratumfw.com/tr/product/security-guard/) altındaki `63` IP engelleme listesi, `5` RIR GeoIP ülke kuralı, `93` Unbound RPZ DNSBL listesi, `56` UT1 Web Filtresi kategorisi ve `52` Suricata satır içi IPS kural kategorisindeki engellemeleri tek sorguda ilişkilendirir.
- **Çoklu WAN ve Çok Üreticili VPN Sağlığı:** Hat gecikme ve paket kaybı değerlerini, `strongSwan` IKEv2/IKEv1 Site-to-Site IPsec (`enc0`) tünellerini ve [v0.34.1 Çok Üreticili S2S VPN sürümüyle](https://stratumfw.com/tr/blog/stratum-firewall-v0-34-1-ipsec-wireguard-s2s-vlan-matrisi-kesintisiz-suricata/) gelen FreeBSD çekirdeği içindeki adanmış WireGuard (`wg0`, `wg1+`) el sıkışma durumlarını denetler.
- **Şeffaf Araç Kaynak Gösterimi:** Üretilen her yanıtın hemen altında hangi MCP araçlarının çağrıldığı açıkça listelenir; bir ayar değişikliği gerekiyorsa asistan sizi doğrudan ilgili panel sayfasına yönlendirir.

### 2. Deterministik Kural Matematiği ve Net Sayaç Anlamlandırması (`v0.35.1`)

Çok sayıda VLAN, WAN ve VPN kuralı barındıran gerçek güvenlik duvarlarında dil modellerinin uzun JSON kural listelerini saymaya çalışması zaman zaman bir eksik veya bir fazla sayım hatalarına yol açabilir; ayrıca modeller `0` paket eşleşme sayacını hatalı biçimde "bozuk kural" olarak yorumlayabilir.

**Stratum Firewall v0.35.1**, bu iki durumu model henüz yanıt üretmeden önce Python katmanında deterministik olarak çözer:

- **Önceden Hesaplanmış Kural ve NAT Toplamları:** Kural ve NAT okuma araçları artık `all` (toplam), `enabled` (etkin) ve `disabled` (devre dışı) sayılarını kontrol katmanında kesin tam sayı olarak hesaplayıp modele sunar.
- **Açık Sıfır Sayaç Anlamı:** `0` değerindeki her paket/bayt sayacı, model bağlamında açıkça `"son yükleme/uygulama (Apply) işleminden bu yana eşleşmedi"` semantiğiyle işaretlenir. DeepSeek ile yapılan canlı doğrulama testlerinde `pf` çekirdek sayaçları ile asistan yanıtları arasında bire bir tutarlılık doğrulandı.

### 3. `PUB-1` Genel IP Maskeleme, Sır İzolasyonu ve Yalnızca RAM Bellek

Gerçek WAN IP adreslerini, uzak şube IPsec uç noktalarını veya kimlik bilgilerini üçüncü taraf bulut API'lerine göndermek kurumsal güvenlik ilkelerine aykırıdır. [Güvenlik ve Doğrulama](https://stratumfw.com/tr/product/security/) sayfasında detaylandırıldığı üzere Stratum Firewall dört katı gizlilik sınırı uygular:

![Stratum Firewall PUB-1 IP Maskeleme ve Yerel Ollama Mimarisi](/images/ai-assistant-privacy.jpg)

| Gizlilik ve Güvenlik Katmanı | Yerel LAN `Ollama` Modu | Bulut LLM Modu (`Claude`, `OpenAI`, `DeepSeek`, `Gemini`) |
| :--- | :--- | :--- |
| **Ağ Veri Yolu** | %100 yerel LAN sorgusu; ağ dışına tek bayt veri çıkmaz | Seçtiğiniz sağlayıcıya şifreli HTTPS çağrısı |
| **Genel (Public) IP Koruması** | Tamamen yerel ağınızın içinde kalır | Gönderim öncesi otomatik `PUB-1`, `PUB-2`... olarak maskelenir; ekranda gerçek IP'ye çevrilir |
| **Parolalar ve `0600` Anahtarlar** | MCP araçlarına ve model bağlamına asla açılmaz | MCP araçlarına ve model bağlamına asla açılmaz |
| **Sohbet Saklama Süresi** | Yalnızca RAM üzerinde (1 saat ömür, diske veya loga yazılmaz) | Yalnızca RAM üzerinde (1 saat ömür, diske veya loga yazılmaz) |
| **Yetki Kapsamı** | Yalnızca `Administrators` (Yönetici) rolü | Yalnızca `Administrators` (Yönetici) rolü |

Bulut sağlayıcılarında varsayılan olarak açık gelen `PUB-1` maskeleme katmanı, tüm genel IPv4/IPv6 adreslerini buluta çıkmadan önce `PUB-1`, `PUB-2` gibi anonim belirteçlere dönüştürür; yanıt döndüğünde ise panel belleğinde gerçek IP adreslerini tekrar yerine koyar. Parolalar, `0600` izinli kök (root) anahtar dosyaları, API jetonları, TOTP gizli anahtarları ve WireGuard/IPsec özel anahtarları MCP katmanına hiçbir koşulda dahil edilmez.

### 4. %100 Yerel `Ollama` Dahil Beş Sağlayıcı Seçeneği

Stratum Firewall sizi ücretli bir bulut aboneliğine veya aracı sunucuya bağlamaz (`$0/yıl`):

- **Yerel Ağı Terk Etmeyen `Ollama` Desteği:** Yönetim VLAN'ınızda veya yerel GPU sunucunuzda çalışan bir `Ollama` servisine (`Qwen`, `Llama`, `Mistral`, `DeepSeek` vb.) bağlanarak tüm tanılama sürecini kapalı devre ağınızda çalıştırabilirsiniz.
- **Esnek Bulut API Desteği:** İhtiyacınıza göre kendi API anahtarınızla **Anthropic Claude**, **OpenAI**, **DeepSeek** veya **Google Gemini** modellerini `PUB-1` IP maskeleme korumasıyla doğrudan kullanabilirsiniz.

### Doğal Dilde Sorabileceğiniz Örnek Tanılama Soruları

Arıza anında beş farklı sekme arasında gezinmek yerine doğrudan Türkçe veya İngilizce sorabilirsiniz:

1. *"VLAN 30 üzerindeki istemciler neden 10.20.0.15 sunucusunun 443 portuna erişemiyor, ilk hangi kural veya engelleme listesi devreye giriyor?"*
2. *"Etkin ve devre dışı tüm WAN ile LAN kurallarını özetle; son Apply işleminden beri hiç paket eşleşmeyen kuralları listele."*
3. *"Site-to-Site IPsec ve çekirdek WireGuard tünellerinin tamamı ayakta mı, el sıkışması geciken bir uç var mı?"*
4. *"Son günlüklerde en çok engelleme yapan Security Guard IP listeleri ve Suricata IPS kategorileri hangileri?"*

### `v0.35.1` Sürümünü Deneyin veya Geçiş Yapın

- **Mevcut Kurulumlar:** **`System > Update`** ekranından OpenZFS Önyükleme Ortamı (`bectl`) anlık görüntü güvencesiyle tek tıkla imzalı `v0.35.1` sürümüne yükseltebilirsiniz.
- **Yeni Kurulum ve İnceleme:** Saf FreeBSD 15.1 gücünü modern API kontrolüyle birleştiren kurulum imajlarını [Stratum Firewall İndir](https://stratumfw.com/tr/download/) sayfasından (`SHA256SUMS` doğrulamasıyla) edinebilir, arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde test edebilir, diğer çözümlerle [Güvenlik Duvarı Karşılaştırması](https://stratumfw.com/tr/compare/) tablosunda kıyaslayabilir ve sürüm takibini [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel) üzerinden yapabilirsiniz.
