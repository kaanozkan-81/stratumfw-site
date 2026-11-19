---
title: "Stratum Firewall v0.35.0 – v0.35.2: Yerleşik Salt Okunur Yapay Zeka Asistanı, PUB-1 IP Maskeleme, Yerel Ollama ve Akıllı IDS Bildirimleri"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-35-2-ai-assistant"
aliases:
  - /tr/blog/stratum-firewall-v0-35-1-yerlesik-yapay-zeka-asistani-ip-maskeleme-yerel-ollama/
description: "Stratum Firewall v0.35.2; 15 salt okunur MCP aracı, deterministik kural sayımı, PUB-1 genel IP maskeleme, %100 yerel LAN Ollama desteği ve yalnızca açık portlar için bildirim üreten akıllı Suricata IDS filtrelemesini sunuyor."
lead: "FreeBSD 15.1 tabanlı güvenlik duvarınıza doğal dilde tanılama soruları sorun: sıfır yapılandırma değişikliği riski, otomatik genel IP maskeleme, yerel Ollama desteği ve gürültüsüz IDS bildirimleri."
images: ["/images/ai-assistant-ui.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "yapay zeka asistani", "mcp", "ollama", "suricata", "v0.35.2"]
---

Üretim ortamındaki bir güvenlik duvarının içine yapay zeka eklemek, her sistem ve ağ yöneticisinin aklına haklı olarak iki temel soruyu getirir: **Ya model yanlış bir çıkarımla kural değiştirip ağı kilitlerse?** ve **Gerçek WAN IP adreslerimi, tünel durumlarımı veya anahtarlarımı neden bir bulut servisine göndereyim?**

[Stratum Firewall](https://stratumfw.com/tr/) üzerinde yayınladığımız **`v0.35.0` – `v0.35.2`** sürümleriyle gelen **`Diagnostics > AI Assistant` (`Tanılama > Yapay Zeka Asistanı`)**, bu iki riski yalnızca metin tabanlı uyarılarla değil, doğrudan mimari sınırlarla ortadan kaldırırken `v0.35.2` ile kapalı WAN portlarına yönelik internet taramalarının gereksiz bildirim gürültüsünü de kesiyor. Modülün tüm ayrıntılarını [Yerleşik Yapay Zeka Asistanı](https://stratumfw.com/tr/product/ai-assistant/) sayfasında ve tüm değişiklik geçmişini [Sürüm Notları](https://stratumfw.com/tr/resources/changelog/) üzerinde inceleyebilirsiniz.

![Stratum Firewall Yerleşik Salt Okunur Yapay Zeka Asistanı Arayüzü](/images/ai-assistant-ui.jpg)

### 1. Mimari Olarak Salt Okunur: 15 Canlı MCP Tanılama Aracı

Kabuk komutu çalıştıran veya yapılandırma dosyalarına yazma yetkisi olan otomasyon araçlarının aksine, `Diagnostics > AI Assistant` modülünün sistem üzerinde **hiçbir yazma yetkisi yoktur**. `config.json` dosyasını değiştiremez, `pf` kurallarını yeniden yükleyemez, durum tablolarını sıfırlayamaz, `strongSwan` ya da `Unbound` servislerini yeniden başlatamaz. Gerçek yapılandırma değişiklikleri yalnızca yöneticinin kontrolünde, sözdizimi ön testleri ve panel üzerinden **120 saniyelik Safe-Apply** otomatik geri alma güvencesiyle uygulanır.

Asistan, Stratum Firewall'ın Python 3.12 / FastAPI kontrol katmanındaki **15 salt okunur MCP (Model Context Protocol) aracını** kullanarak canlı sistem durumunu okur:

- **Canlı Kural, NAT ve Trafik Analizi:** Etkin `pf` filtre kurallarını, port yönlendirmelerini, giden NAT eşlemelerini, `dummynet` bant genişliği sınırlayıcılarını ve arayüz sayaçlarını inceler.
- **5 Katmanlı Security Guard Görünürlüğü:** [Security Guard](https://stratumfw.com/tr/product/security-guard/) altındaki `63` IP engelleme listesi, `5` RIR GeoIP ülke kuralı, `93` Unbound RPZ DNSBL listesi, `56` UT1 Web Filtresi kategorisi ve `52` Suricata satır içi IPS/IDS kural kategorisindeki olayları tek sorguda ilişkilendirir.
- **Çoklu WAN ve Çok Üreticili VPN Sağlığı:** Hat gecikme ve paket kaybı değerlerini, `strongSwan` IKEv2/IKEv1 Site-to-Site IPsec (`enc0`) tünellerini ve [v0.34.1 Çok Üreticili S2S VPN sürümüyle](https://stratumfw.com/tr/blog/stratum-firewall-v0-34-1-ipsec-wireguard-s2s-vlan-matrisi-kesintisiz-suricata/) gelen FreeBSD çekirdeği içindeki adanmış WireGuard (`wg0`, `wg1+`) el sıkışma durumlarını denetler.
- **Şeffaf Araç Kaynak Gösterimi:** Üretilen her yanıtın hemen altında hangi MCP araçlarının çağrıldığı açıkça listelenir; bir ayar değişikliği gerekiyorsa asistan sizi doğrudan ilgili panel sayfasına yönlendirir.

### 2. Deterministik Kural Matematiği (`v0.35.1`) ve Akıllı IDS Bildirimleri (`v0.35.2`)

Canlı güvenlik duvarlarındaki gerçek saha deneyimiyle `v0.35.0` sonrasında iki önemli mühendislik iyileştirmesi devreye alındı:

- **Önceden Hesaplanmış Kural Sayıları ve Sıfır Sayaç Anlamı (`v0.35.1`):** Uzun JSON kural listelerini dil modeline saydırmak bir eksik/bir fazla hatalarına ve `0` paket sayaçlarının "hatalı kural" sanılmasına yol açabiliyordu. Kural ve NAT araçları artık `all` (toplam), `enabled` (etkin) ve `disabled` (devre dışı) sayılarını Python katmanında kesin tam sayı olarak hesaplar; `0` sayaçlarını ise açıkça `"son Apply/yeniden yükleme işleminden bu yana eşleşmedi"` semantiğiyle işaretler.
- **Yalnızca Gerçekten Açık Portlar İçin IDS Bildirimi (`v0.35.2`):** Suricata bir WAN hattını `pf` filtrelemeden önce izlediği için, güvenlik duvarında kapalı olan portlara gelen rastgele internet taramaları (örneğin UDP `9034` Realtek SDK istismarı, CVE-2021-35394) paketler `pf` tarafından düşürüldüğü halde Telegram ve e-posta uyarısı üretebiliyordu. Stratum FW `v0.35.2`, WAN adreslerinde hangi portların hangi kaynaklar için gerçekten açık olduğunu (port yönlendirmeleri, arayüz grupları dahil WAN geçiş kuralları, WAN yönetim erişimi, OpenVPN, WireGuard ve IPsec) hesaplar ve yalnızca içeriden çıkan, iç istemcilere yönelen ya da açık bir porta gelen uyarıları bildirir. Düşürülen taramalar gizlenmez; IDS ekranında, günlüklerde ve Yapay Zeka Asistanı yanıtlarında **`Port closed`** işaretiyle görünmeye devam eder.

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

Bulut sağlayıcılarında varsayılan olarak açık gelen `PUB-1` maskeleme katmanı, tüm genel IPv4 adreslerini buluta çıkmadan önce `PUB-1`, `PUB-2` gibi anonim takma adlara dönüştürür; yanıt döndüğünde ise panel belleğinde gerçek IP adreslerini tekrar yerine koyar. OpenSSL (`openssl` aracı ve Python `cryptography` paketi) ile üretilen anahtar ve sertifikalar, parolalar, `0600` izinli root dosyaları ve VPN yapılandırmaları MCP katmanına hiçbir koşulda açılmaz.

### 4. %100 Yerel `Ollama` Dahil Beş Sağlayıcı Seçeneği

Stratum Firewall sizi ücretli bir bulut aboneliğine veya aracı sunucuya bağlamaz (`$0/yıl`):

- **Yerel Ağı Terk Etmeyen `Ollama` Desteği:** Yönetim VLAN'ınızda veya yerel GPU sunucunuzda çalışan bir `Ollama` servisine (`Qwen`, `Llama`, `Mistral`, `DeepSeek` vb.) bağlanarak tüm tanılama sürecini kapalı devre ağınızda çalıştırabilirsiniz.
- **Esnek Bulut API Desteği:** İhtiyacınıza göre kendi API anahtarınızla **Anthropic Claude**, **OpenAI**, **DeepSeek** veya **Google Gemini** modellerini `PUB-1` IP maskeleme koruması ve yerleşik **Test** düğmesiyle doğrudan kullanabilirsiniz.

### Doğal Dilde Sorabileceğiniz Örnek Tanılama Soruları

Arıza anında beş farklı sekme arasında gezinmek yerine doğrudan Türkçe veya İngilizce sorabilirsiniz:

1. *"Son bir saatte neler engellendi; gerçekten açık bir WAN portuna yönelik Suricata uyarısı var mı?"*
2. *"Etkin ve devre dışı tüm WAN ile LAN kurallarını özetle; son Apply işleminden beri hiç eşleşmeyen kuralları listele."*
3. *"Site-to-Site IPsec ve çekirdek WireGuard tünellerinin tamamı ayakta mı, el sıkışması geciken bir uç var mı?"*
4. *"WAN2 hattı sağlıklı mı ve iç ağda şüpheli giden trafik üreten bir cihaz görülüyor mu?"*

### `v0.35.2` Sürümünü Deneyin veya Geçiş Yapın

- **Mevcut Kurulumlar:** **`System > Update`** ekranından OpenZFS Önyükleme Ortamı (`bectl`) anlık görüntü güvencesiyle tek tıkla imzalı `v0.35.2` sürümüne yükseltebilirsiniz.
- **Yeni Kurulum ve İnceleme:** Saf FreeBSD 15.1 gücünü modern API kontrolüyle birleştiren kurulum imajlarını [Stratum Firewall İndir](https://stratumfw.com/tr/download/) sayfasından (`SHA256SUMS` doğrulamasıyla) edinebilir, arayüzü [Canlı Demo](https://demo.stratumfw.com) üzerinde test edebilir, diğer çözümlerle [Güvenlik Duvarı Karşılaştırması](https://stratumfw.com/tr/compare/) tablosunda kıyaslayabilir ve sürüm geçmişini [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel) üzerinden takip edebilirsiniz.
