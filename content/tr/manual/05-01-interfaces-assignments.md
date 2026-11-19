---
title: "Arayüzler > Atamalar"
seo_title: "05.1 Arayüzler > Atamalar (Interfaces > Assignments) — Stratum FW Kullanım Kılavuzu"
description: "Ağ portları ve rolleri: WAN ve LAN'ı başka bir porta taşıma, iç ağ veya ek internet hattı olarak OPT arayüzleri ekleme, IP adresleri, PPPoE, MTU ve arayüz kaldırma."
weight: 401
chapter_num: "05.1"
url: "/tr/manual/interfaces-assignments/"
aliases:
  - "/tr/manual/05-01-interfaces-assignments/"
translationKey: "manual-05-01-interfaces-assignments"
---

Bir *arayüz* (interface), güvenlik duvarının parçası olduğu bir ağdır. Bir **rolü** (*WAN*, *LAN*, *OPT1*, *OPT2* ...) vardır ve bir **ağ portunu** kullanır: `igb2` gibi fiziksel bir port ya da `igb1.20` gibi bir port üzerindeki VLAN. Paneldeki diğer her şey - güvenlik duvarı kuralları, DHCP, DNS, VPN erişimi, NAT - fiziksel porta değil, doğrudan bu role bağlıdır. Bu sayede bir ağı kurallarına hiç dokunmadan başka bir fiziksel porta taşıyabilirsiniz.

**Interfaces > Assignments** sayfası atanmış arayüzleri ve henüz boşta olan ağ portlarını listeler.

![Interfaces > Assignments](/images/manual/if-assignments.png)

## Arayüz listesi

**Assigned interfaces** tablosu her arayüz için bir satır gösterir:

| Sütun | Anlamı |
| :--- | :--- |
| **Interface** | Verdiğiniz açıklama (büyük) ve rol (küçük) - örneğin *GUESTS* / *OPT2*. OPT arayüzlerinde ayrıca türün **WAN** mı yoksa **LAN** mı olduğu gösterilir. **you are here** etiketi, tarayıcınızın panele ulaştığı arayüzü işaretler. Gri renkli satır devre dışı bırakılmış bir arayüzdür. |
| **Network port** | Arayüzün kullandığı port; buradaki seçiciden değiştirilebilir (aşağıya bakınız). |
| **Link** | Kablonun takılı ve bağlantının aktif olup olmadığı. |
| **IPv4** | Arayüzün IP adresi (ve nasıl alındığı). |
| **MAC address** | Portun donanım (MAC) adresi. |
| **Actions** | **Edit**; OPT arayüzleri için ayrıca **Remove**. |

**Available network ports** tablosu henüz hiçbir arayüz tarafından kullanılmayan fiziksel portları ve VLAN'ları MAC adresi, hat durumu (link) ve hız (media) bilgisiyle listeler. Liste her 10 saniyede bir kendiliğinden yenilenir; böylece kabloyu takıp hangi portun aktifleştiğini anında görebilirsiniz.

## WAN, LAN ve OPT rolleri

| Rol | Tür | Açıklama |
| :--- | :--- | :--- |
| **WAN** | WAN | Ana internet bağlantısı. Varsayılan rotayı (default route) taşır; devre dışı bırakılamaz ve kaldırılamaz. |
| **LAN** | LAN | Ana iç ağ. *Anti-lockout* kuralı bu ağın panele her zaman ulaşmasını sağlar. Kaldırılamaz, yalnızca başka bir porta taşınabilir. |
| **OPT1**, **OPT2** ... | LAN veya WAN | Eklenen diğer tüm ağlar. Arayüzü eklerken türünü siz seçersiniz. |

Seçilen tür güvenlik duvarının o ağa nasıl davranacağını belirler:

- **LAN** (iç ağ): kendi güvenlik duvarı kurallarına sahiptir, DHCP sunucusu çalıştırabilir, DNS çözümleyici tarafından yanıtlanır ve internete çıkan trafiği çevrilir (giden NAT). Yeni eklenen LAN tipindeki bir OPT arayüzü **güvenlik duvarı kuralı olmadan** başlar - **Firewall > Rules** altından kural ekleyene kadar bu ağdan hiçbir trafik geçmez.
- **WAN** (ek internet bağlantısı): giden NAT alır ve ana WAN ile aynı korumaya sahiptir - bir kural izin vermedikçe dışarıdan gelen trafik engellenir. Varsayılan rota ana WAN üzerinde kalır; trafik yedeklilik (failover), yük paylaşımı veya politika tabanlı yönlendirme (**Interfaces > Gateways**, **Firewall > Rules**) ile ek WAN üzerinden gönderilir.

## Yeni arayüz ekleme

1. **Available network ports** tablosunda ilgili portun yanındaki **Add interface** düğmesine tıklayın. Bir VLAN portu, onu [**Interfaces > VLANs**](/tr/manual/interfaces-vlans/) sayfasında oluşturduktan sonra burada görünür.
2. **Interface type** seçin: **WAN** (internet bağlantısı) veya **LAN** (kendi istemcileri ve DHCP sunucusu olan iç ağ). Seçilen tür daha sonra değiştirilemez - değiştirmek için arayüzü kaldırıp yeniden eklemeniz gerekir.
3. Ayarları doldurun (sonraki bölüm) ve **Add interface** düğmesine tıklayın.
4. Üst çubuktan **Apply changes** ve ardından **Keep changes** düğmesine tıklayın.

Yeni arayüz sıradaki boş OPT numarasını alır.

LAN tipinde bir arayüz ekledikten sonra:

- ilgili sekmeden güvenlik duvarı kurallarını ekleyin (**Firewall > Rules**) - o zamana kadar bu ağdan gelen her şey engellenir;
- ağdaki cihazların otomatik IP alması gerekiyorsa DHCP sunucusunu açın (**Services > DHCP Server**).

## Arayüz ayarları

Bir arayüz satırında **Edit** düğmesine tıklayın. Panele o arayüz üzerinden ulaşıyorsanız üstteki uyarı kutusu, bağlantıyı kesen bir değişikliğin **Apply changes** sonrasında 120 saniye içinde otomatik geri alınacağını hatırlatır.

![Bir arayüzü düzenleme](/images/manual/if-edit.png)

| Ayar | Anlamı |
| :--- | :--- |
| **Description** | Panel genelinde gösterilen görünen ad; en fazla 40 karakter, örneğin *Guests* veya *Fibre*. Boş bırakılırsa rol adı (*OPT2*) görünür. |
| **Status** | **Interface is enabled**. Devre dışı bırakılan bir arayüz ayarlarını korur ancak trafik taşımaz; kuralları ve DHCP sunucusu kullanılmaz. WAN her zaman etkindir. |
| **IPv4 configuration** | Arayüzün IP adresini nasıl alacağı - aşağıya bakınız. |
| **MTU** | Bu ağdaki en büyük paket boyutu. Boş bırakılırsa varsayılan değer kullanılır: 1500 (PPPoE için 1492). İzin verilen aralık: 576 ile 9216 arası. Yalnızca geçerli bir neden varsa değiştirin (servis sağlayıcı talimatı veya anahtarları destekleyen bir depolama ağında jumbo frame kullanımı gibi). |

**IPv4 configuration** seçenekleri:

| Tür | Ayarlar | Notlar |
| :--- | :--- | :--- |
| **Static** | **IPv4 address / prefix** - güvenlik duvarının bu ağdaki kendi adresi, örneğin `192.168.20.1/24` (önek `/30` veya daha geniş olmalıdır); WAN arayüzlerinde ek olarak aynı ağ içindeki servis sağlayıcı yönlendiricisi (**Gateway**). | İç ağlar için standart seçimdir. |
| **DHCP** | Ek ayar yok. | Adres başka bir DHCP sunucusundan alınır. Ek bir WAN arayüzünde alınan kiranın ağ geçidi yedeklilik, yanıt trafiği ve politika yönlendirmesi için kullanılır; varsayılan rotayı veya DNS sunucularını tek başına devralmaz. |
| **PPPoE** | **Username**, **Password**, **Service name** (yalnızca sağlayıcı isterse). | Yalnızca WAN tipinde kullanılabilir. Bağlantı ayrı bir `pppoe` arayüzü olarak görünür; kurallar ve NAT onu otomatik izler. Parola yalnızca `root` erişimli bir dosyada saklanır ve bir daha gösterilmez - mevcut parolayı korumak için alanı boş bırakın. |
| **None** | Ek ayar yok. | IP adresi atanmaz: yalnızca VLAN taşıyan veya ilerisi için hazırlanan portlarda kullanılır. |

### Bir arayüzün alt ağı değiştiğinde

Bir iç arayüzün IP adresi başka bir alt ağa değiştirildiğinde DHCP ayarları da onunla birlikte taşınır:

- Mevcut DHCP aralığı yeni alt ağa uymuyorsa otomatik olarak yeni ağa (bir `/24` ağında `.100 - .199` aralığına) taşınır ve panel bunu bildirir.
- Sabit DHCP eşleştirmeleri (static mappings) cihaz kısmını korur (`192.168.1.50` adresi `192.168.20.50` olur). Bir eşleştirme yeni ağa taşınamıyorsa değişiklik reddedilir - önce **Services > DHCP Server** altından ilgili kaydı düzenleyin veya silin.
- Arayüz artık sabit (static) bir adrese sahip değilse DHCP sunucusu kapatılır.

*OPT2 ağı* gibi arayüz rolünü kullanan güvenlik duvarı kuralları yeni alt ağı otomatik olarak izler. Eski IP adreslerinin elle açıkça yazıldığı kurallar veya takma adlar (aliases) ise elle güncellenmelidir.

## Bir arayüzü başka bir porta taşıma

**Assigned interfaces** tablosundaki her satırda bir **Network port** seçicisi bulunur. Başka bir port seçip **Save assignments** düğmesine tıklayın. Arayüzün tüm yapılandırması - IP adresi, güvenlik duvarı kuralları, DHCP sunucusu - yeni porta taşınır. Her port aynı anda yalnızca tek bir arayüz tarafından kullanılabilir.

Tipik kullanım senaryoları: fiziksel bir portun arızalanması, cihazın farklı port adlarına sahip yeni bir donanım modeliyle değiştirilmesi veya bir ağın fiziksel porttan VLAN yapısına geçirilmesi.

> **Uyarı:** Üzerinden bağlı olduğunuz arayüzü başka bir porta taşımak, kabloyu yeni porta takana kadar bağlantınızı keser. 120 saniye içinde **Keep changes** düğmesine tıklayamazsanız değişiklik otomatik olarak geri alınır - bu nedenle ya kabloyu hızla yeni porta taşıyın ya da değişikliği başka bir arayüzden veya konsoldan (menü seçeneği **1) Assign interfaces**) yapın.

## Bir arayüzü kaldırma

OPT arayüzleri satırlarındaki **Remove** düğmesiyle kaldırılabilir (onaylamak için aynı satırda tekrar tıklayın). Arayüzün kendi güvenlik duvarı kuralları ve DHCP sunucusu da onunla birlikte silinir; port yeniden boşa çıkar.

Başka bir bileşenin hâlâ kullandığı bir arayüzü kaldırmaya çalıştığınızda panel bunu reddeder ve nerede kullanıldığını listeler: örneğin *OPT2 is still used by: LAN rule #3, Port forward #1*. Önce bu kayıtları değiştirin veya silin. WAN ve LAN kaldırılamaz; yalnızca başka bir porta taşınabilir.
