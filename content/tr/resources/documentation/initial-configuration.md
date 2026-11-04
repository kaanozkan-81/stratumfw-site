---
title: "İlk Yapılandırma"
lead: "İlk önyüklemeden sonra güvenlik duvarınızı ayarlayın — arayüzler, yönlendirme ve yönetici erişimi."
---

Stratum panelini başarıyla kurup ilk kez oturum açtığınızda, sistem çalışıyor demektir ancak henüz trafiği yönlendirmeye hazır değildir. Bu kılavuz, güvenlik duvarınızı güvenli bir şekilde devreye almak için gereken temel adımları açıklar.

## 1. Varsayılan Kimlik Bilgilerini Değiştirin

Güvenlik için, panele ilk kez giriş yaptığınızda (konsolda yazan varsayılan `admin` bilgilerini kullanarak), şifrenizi değiştirmeniz zorunlu tutulur.

- Güçlü ve benzersiz bir şifre kullanın (en az 12 karakter).
- Değiştirildikten sonra sistem panelin geri kalanının kilidini açar.

## 2. Arayüz Atamaları (WAN ve LAN)

**Arayüzler → Atamalar** (Interfaces → Assignments) bölümüne gidin. Stratum FreeBSD `pf` motorunu kullandığı için, kurallar uygulanmadan önce arayüzlere belirli roller atanmalıdır.

- **WAN:** Modeminize veya ISS'nize bağlı fiziksel arayüzü seçin.
- **LAN:** Dahili ağ anahtarınıza (switch) bağlı fiziksel arayüzü seçin.
- **OPT:** (İsteğe bağlı) DMZ, VLAN'lar veya ikincil bir WAN için ek arayüzleri atayın.

**Kaydet**'e tıklayın. Bu kritik bir değişiklik olduğu için **Güvenli Uygulama (Safe-Apply)** mekanizması devreye girecektir. 60 saniye içinde bağlantıyı onaylamanız gerekir, aksi takdirde sistem kilitlenmeyi önlemek için değişiklikleri otomatik olarak geri alır.

## 3. WAN Yapılandırması

**Arayüzler → WAN** bölümüne gidin.

Güvenlik duvarının genel IP adresini nasıl alacağını yapılandırmalısınız:
- **DHCP:** (En yaygın) Güvenlik duvarı ISS'nizden otomatik olarak bir IP ister.
- **Statik IP:** ISS'nizin sağladığı IP, Alt Ağ Maskesi ve Ağ Geçidi'ni girin.
- **PPPoE:** ISS'nizin sağladığı kullanıcı adı ve şifreyi girin (DSL/Fiber için yaygındır).

Değişiklikleri uygulayın. WAN arayüzünün "Up" (Çalışıyor) durumunda olduğunu ve bir IP adresi aldığını doğrulamak için **Pano**'yu (Dashboard) kontrol edin.

## 4. LAN ve DHCP Kurulumu

**Servisler → DHCP** bölümüne gidin.

Varsayılan olarak Stratum, cihazların (dizüstü bilgisayarlar, telefonlar) otomatik olarak bir IP adresi alıp internete erişebilmesi için LAN arayüzünde bir DHCP sunucusu çalıştırır.

- LAN alt ağınızı tanımlayın (örn. `192.168.1.1/24`).
- DHCP havuzunu tanımlayın (örn. `192.168.1.100` ile `192.168.1.200` arası).
- DNS sunucularınızı ayarlayın (genellikle güvenlik duvarının LAN IP'si).

## 5. Rol Tabanlı Erişim Kontrolü (RBAC)

Stratum ekipler için tasarlanmıştır. Diğer yöneticileri eklemek için **Sistem → Kullanıcılar** bölümüne gidin. `admin` hesabını asla paylaşmamalısınız.

Bireysel kullanıcılar oluşturun ve onlara üç rolden birini atayın:
- **Yönetici (Admin):** Güvenlik duvarı kurallarını, VPN'leri ve sistem ayarlarını yapılandırmaya tam erişim.
- **Operatör:** Panoları görüntüleyebilir, teşhisleri çalıştırabilir (ping/traceroute) ve aktif durumları yönetebilir ancak güvenlik duvarı kurallarını değiştiremez.
- **Salt Okunur:** Panoyu ve günlükleri görüntüleyebilir ancak hiçbir değişiklik yapamaz.

## 6. Tehdit İstihbaratını Etkinleştirin

Ağ artık canlı olduğuna göre onu koruyun. **Güvenlik → Karalisteler** bölümüne gidin.

- Bilinen kötü amaçlı IP'leri otomatik olarak `pf` tablolarına indirmek için **IP Karalistelerini** etkinleştirin.
- Çözümleyici düzeyinde reklamları, izleyicileri ve kötü amaçlı yazılım alan adlarını engellemek için **DNSBL**'yi etkinleştirin.

## Sonraki Adımlar

Güvenlik duvarınız artık trafiği yönlendiriyor ve ağınızı koruyor. Buradan itibaren şunları keşfedebilirsiniz:

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>VPN'i Ayarlayın</h3>
    <p>Güvenli uzaktan erişim için WireGuard'ı yapılandırın.</p>
    <a href="/tr/resources/documentation/wireguard-vpn/" class="card-link">WireGuard Kılavuzu →</a>
  </div>
  <div class="card">
    <h3>Güvenlik Duvarında Uzmanlaşın</h3>
    <p>Güvenli Uygulama (Safe-Apply) kullanarak karmaşık pf kurallarının nasıl yazılacağını öğrenin.</p>
    <a href="/tr/resources/documentation/pf-rules/" class="card-link">pf Kuralları Kılavuzu →</a>
  </div>
</div>
