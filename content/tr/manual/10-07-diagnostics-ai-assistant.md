---
title: "Tanılama > Yapay Zeka Asistanı"
seo_title: "10.7 Tanılama > Yapay Zeka Asistanı (Diagnostics > AI Assistant) — Stratum FW Kullanım Kılavuzu"
description: "Güvenlik duvarına doğal dilde soru sorma: sağlayıcılar (Claude, ChatGPT, DeepSeek, Gemini, yerel Ollama), API anahtarları, genel IP maskeleme (PUB-1), araç adımları, örnek sorular ve gizlilik."
weight: 1007
chapter_num: "10.7"
url: "/tr/manual/diagnostics-ai-assistant/"
aliases:
  - "/tr/manual/10-07-diagnostics-ai-assistant/"
translationKey: "manual-10-07-diagnostics-ai-assistant"
---

Yapay Zeka Asistanı (**AI Assistant**), **bu güvenlik duvarı** hakkındaki sorularınızı kendi cümlelerinizle - *"son bir saatte neler engellendi ve nereden geldi?"*, *"her iki WAN bağlantısı da sağlıklı mı?"* - doğrudan sistemin **gerçek, canlı durumuna** bakarak yanıtlar. Güvenlik duvarını MCP sunucusuyla ([**System > Settings > MCP**](/tr/manual/system-settings-snmp-mcp/)) aynı 16 salt okunur aracı kullanarak okur ve **hiçbir ayarı değiştiremez**: bir değişiklik gerektiğinde panelin hangi sayfasından ve nasıl yapılacağını adım adım açıklar.

Soruyu hangi dilde sorarsanız o dilde yanıt verir.

![Diagnostics > AI Assistant](/images/manual/ai.png)

Yapay Zeka Asistanını yalnızca yöneticiler kullanabilir.

## Ayarlar (Settings)

| Ayar | Anlamı |
| :--- | :--- |
| **Enable the AI assistant** | Varsayılan olarak kapalıdır. |
| **Provider** | **Anthropic (Claude)**, **OpenAI (ChatGPT)**, **DeepSeek**, **Google Gemini** veya **Ollama (local, on your network)**. |
| **Model** | Kullanılacak model adı - örneğin Anthropic için `claude-sonnet-5-5` veya `claude-haiku-5-5`, DeepSeek için `deepseek-chat`, OpenAI hesabınızdaki veya Google AI Studio'daki model adı. Ollama için araç çağırmayı (*tool calling*) destekleyen bir model, örneğin `qwen2.5` veya `llama3.1`. |
| **API key** | Sağlayıcıdan aldığınız API anahtarı (Ollama için gerekmez). Yalnızca `root` erişimli bir dosyada saklanır ve bir daha gösterilmez; mevcut anahtarı korumak için alanı boş bırakın ya da **Remove the saved key** ile kaldırın. |
| **Ollama address** | (Ollama.) Yerel ağınızda Ollama çalıştıran sunucunun adresi, örneğin `http://10.200.0.50:11434`. |
| **Mask public IP addresses (PUB-1, PUB-2 ...) before they are sent** | Varsayılan olarak açıktır - aşağıdaki **Gizlilik** bölümüne bakınız. |
| **Tool steps per question** | Modelin tek bir yanıt üretirken güvenlik duvarını en fazla kaç kez sorgulayabileceği (`1`-`20`, varsayılan `8`). Daha yüksek adım sayısı daha derin analiz sağlar ancak API kullanım maliyetini artırır. |

**Test** sağlayıcıyı, API anahtarını ve modeli doğrular; **Save** kaydeder. Hazır olduğunda sayfada kullanılabilir araç sayısıyla birlikte **Ready** ibaresi görünür.

[**System > Settings > MCP**](/tr/manual/system-settings-snmp-mcp/) altında kapatılan araçlar Yapay Zeka Asistanı tarafından da kullanılamaz.

## Soru sorma (Ask)

Sorunuzu yazıp **Ask** düğmesine basın (`Enter` gönderir, `Shift+Enter` alt satıra geçer). Başlamak için hazır öneriler:

- *Son bir saatte neler engellendi ve nereden geldi?* (*What was blocked in the last hour, and from where?*)
- *Her iki WAN bağlantısı da sağlıklı mı? Paket kaybı veya yüksek gecikme var mı?*
- *Bugünkü saldırı tespit (IDS) uyarılarını özetle. Şüpheli bir iç cihaz var mı?*
- *LAN üzerindeki hangi güvenlik duvarı kuralları şimdiye kadar hiç eşleşmedi?*

Diğer örnek sorular:

- *Şu anda misafir ağında hangi cihazlar bağlı?*
- *VPN'e kimler bağlı?*
- *`192.168.1.50` neden internete çıkamıyor?*
- *Bugün en çok bant genişliği tüketen uygulamalar hangileri ve Uygulama Kontrolü neleri engelledi?*
- *Son 24 saatte en yoğun arayüz hangisiydi?*
- *Bugün yapılandırmada neler değişti?*
- *Web sunucuma port 443 yönlendirmesini nasıl yaparım?*

Her yanıtın altında **hangi araçların kullanıldığını** (üzerine gelindiğinde parametreleriyle birlikte) ve kaç genel IP adresinin maskelendiğini gösteren rozetler yer alır - böylece yanıtın hangi canlı veriye dayandığını şeffaf biçimde görürsünüz.

Sohbet önceki soruları hatırladığı için takip soruları sorabilirsiniz (*"peki hangi ülkelerden?"*). **New chat** yeni bir sohbet başlatır. Sohbetler yalnızca bellekte (son kullanımdan itibaren 1 saat) tutulur ve içerikleri diske günlüklenmez.

## Gizlilik (Privacy)

Bulut tabanlı bir sağlayıcı kullanıldığında **sorunuz ve araçların döndürdüğü veriler ilgili sağlayıcıya gönderilir** - örneğin günlük satırları, iç adresler ve kural adları. Parolalar, özel anahtarlar, jetonlar ve VPN yapılandırmaları ise hiçbir araç tarafından döndürülmez.

- **Mask public IP addresses** seçeneği araç çıktılarındaki her genel (public) IPv4 adresini güvenlik duvarından çıkmadan önce geçici bir takma adla (`PUB-1`, `PUB-2` ...) değiştirir; yanıt size gösterilirken gerçek IP adresleri tekrar yerine konur. İç ağlarınızdaki özel IP adresleri ise olduğu gibi gönderilir.
- Paylaşmak istemediğiniz veri türlerine ait MCP araçlarını [**System > Settings > MCP**](/tr/manual/system-settings-snmp-mcp/) sayfasından kapatabilirsiniz.
- **Ollama** kullanıldığında ise hiçbir veri ağınızın dışına çıkmaz: sorular, veriler ve yanıtlar tamamen kendi yerel sunucunuzda kalır.

## Sağlayıcı seçimi

| Sağlayıcı | Notlar |
| :--- | :--- |
| Anthropic (Claude) | Günlükleri ve güvenlik duvarı kurallarını analiz etmede çok güçlüdür; önerilir. |
| OpenAI, Gemini, DeepSeek | OpenAI uyumlu API uç noktaları üzerinden çalışır. |
| Ollama | Ücretsiz ve %100 yereldir; akıcı hız için GPU'lu bir sunucu ve araç çağırmayı destekleyen bir model gerektirir. Küçük modeller daha sade yanıtlar üretir. |

Bulut sağlayıcıları kullanım başına ücretlendirir; **Tool steps per question** ayarı tek bir sorunun tüketebileceği üst sınırı korur.
