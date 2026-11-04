---
title: "Çoklu WAN Yedekliliği (Multi-WAN Failover) Nedir ve İşletmeniz Neden Buna İhtiyaç Duyar?"
date: 2026-09-16
author: "Stratum Security Team"
description: "Kesilen bir internet bağlantısı bir işletmeye üretkenlik ve gelir kaybı olarak binlerce dolara mal olabilir. Çoklu WAN yedekliliği, bağlantılar arasında otomatik olarak geçiş yaparak sizi çevrimiçi tutar."
tags: ["multi-wan", "failover", "business-continuity", "networking", "is-surekliligi"]
---

İnternete bağlı olan her işletme — yani her işletme — o anı yaşamıştır. Bağlantı kopar. Satış noktası sistemi işlem yapmayı durdurur. Video görüşmesi cümlenin ortasında donar. Güvendiğiniz bulut uygulaması bir hata döndürür. Birisi "İnternet mi kesildi?" der ve herkes ekranlarından başını kaldırıp bakar.

Küçük bir işletme için bu rahatsız edicidir. Çevrimiçi işlem yapan bir işletme için pahalıdır. Telefonda bir müşterisi olan bir işletme için utanç vericidir.

İyi haber şu ki, bu sorunun iyi bilinen bir çözümü var. Buna **çoklu WAN yedekliliği (multi-WAN failover)** denir ve bir işletmenin ağına yapabileceği en uygun maliyetli yatırımlardan biridir. Bu makale ne olduğunu, nasıl çalıştığını ve bir çözümü değerlendirirken nelere dikkat etmeniz gerektiğini açıklar.

## Çoklu WAN yedekliliği nedir?

"WAN", Geniş Alan Ağı (Wide Area Network) anlamına gelir — pratikte ağınız ile internet arasındaki bağlantı demektir. "Çoklu WAN" kurulumu, ağınızın birden fazla internet bağlantısına sahip olduğu anlamına gelir. "Yedeklilik" (failover), bir bağlantı başarısız olduğunda trafiğin otomatik olarak diğerine kaydığı anlamına gelir.

Bir araya getirildiğinde, çoklu WAN yedekliliği, iki veya daha fazla internet bağlantısı kullanma ve bağlantılardan biri kullanılamaz hale geldiğinde ağınızın otomatik olarak aralarında geçiş yapmasını sağlama uygulamasıdır.

Kavram basittir. Uygulama, işlerin ilginçleştiği yerdir.

## Tek bir internet bağlantısı neden bir işletme riskidir?

İşletmenizin tek bir internet bağlantısı varsa (bir fiber hat, bir kablo modem veya bir kurumsal DSL devresi), ağınızın kullanılabilirliğinin tamamen o tek bağlantıya bağlı olduğunu kabul etmişsinizdir. Bu bağımlılığın sonuçları vardır:

- **ISS (İnternet Servis Sağlayıcı) kesintileri nadir değildir.** Fiber kesintileri olur. Yukarı akış yönlendirme sorunları olur. Planlı bakım olur.
- **Donanım arızalanır.** Modemler arızalanır. Kablolar bozulur. Hava koşulları fiziksel altyapıyı etkiler.
- **SLA'nız (Hizmet Seviyesi Anlaşması) kesintiyi önlemez.** Bir SLA size kredi kazandırır. Satış noktası sisteminizi çevrimiçi hale getirmez.
- **Kurtarma zaman alır.** "Hızlı" bir ISS yanıtı bile saatlerle ölçülür, dakikalarla değil.

Kesinti süresinin her dakikasının bir maliyeti vardır. Bazı işletmeler için bu maliyetin ölçülmesi kolaydır — bir restoran kartları işleyemez, bir hukuk firması dava dosyalarına erişemez, bir perakendeci envanteri kontrol edemez. Diğerleri için daha dolaylıdır ama yine de gerçektir: kaybedilen üretkenlik, hayal kırıklığına uğramış müşteriler, çalışanların tıkanıp kalması.

## Yedeklilik (Failover) gerçekte nasıl çalışır?

İyi tasarlanmış bir yedeklilik sisteminin dört bileşeni vardır: algılama, karar verme, geçiş ve kurtarma.

### 1. Algılama
Sistem bir bağlantının gerçekten çalışıp çalışmadığını belirlemelidir. Bu kulağa geldiğinden daha zordur. Bir arayüzün "açık" olması bağlantının işlevsel olduğu anlamına gelmez.

İyi yedeklilik sistemleri birden fazla algılama yöntemi kullanır:
- **Fiziksel bağlantı durumu:** Arayüz açık mı?
- **ICMP (ping) kontrolleri:** Güvenilir uzak ana bilgisayarlara ulaşabiliyor muyuz?
- **HTTP/HTTPS kontrolleri:** Bilinen bir uç noktaya ulaşıp geçerli bir yanıt alabiliyor muyuz?
- **DNS kontrolleri:** Sabit olduğunu bildiğimiz bir adı çözümleyebiliyor muyuz?

### 2. Karar Verme
Bir bağlantı sağlıksız kabul edildiğinde, sistemin ne yapacağına karar vermesi gerekir. İki yaygın mod:
- **Aktif/Pasif yedeklilik:** Bir bağlantı birincil, diğeri yedektir.
- **Yük Dengeleme (Aktif/Aktif):** Trafik mevcut tüm bağlantılara dağıtılır.

### 3. Geçiş
Yedeklilik kararı verildiğinde, trafik gerçekte nasıl hareket eder? Bu, doğru yapılması en zor kısımdır.
İyi tasarlanmış sistemler bunu zarifçe yönetir:
- **Durum bilgili izleme:** Sistem aktif oturumları izler ve mümkün olduğunda bunları korumaya çalışır.
- **Hızlı geçiş:** Geçiş bir veya iki saniye içinde gerçekleşir.

### 4. Kurtarma
Başarısız bağlantı geri geldiğinde ne olur? İki yaklaşım vardır: **otomatik olarak geri dönmek** veya **manuel geçişe kadar yedekte kalmak**.

## Çoklu WAN çözümünde nelere dikkat edilmeli?

Değerlendirirken şunları arayın: Çoklu sağlık kontrolü yöntemleri, yapılandırılabilir eşikler, politikaya dayalı yönlendirme, yük dengeleme modları, oturum koruma ve görünürlük.

## Sonuç

Çoklu WAN yedekliliği, çoğu işletme için en etkili ağ yatırımlarından biridir. İkinci bir bağlantının maliyeti genellikle, kesintinin ilk saatinde kaybettiğiniz para ve itibar tarafından hızla amorti edilir. Bugün ağınızın tek bir kırılgan bağlantıya bağlı olup olmadığını kontrol edin. Cevap "evet" ise, çoklu WAN yedekliliği yapılacaklar listenizin en üstünde olmalıdır.
