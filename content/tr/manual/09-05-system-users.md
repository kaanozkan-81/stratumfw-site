---
title: "Sistem > Kullanıcılar"
seo_title: "09.5 Sistem > Kullanıcılar (System > Users) — Stratum FW Kullanım Kılavuzu"
description: "Web paneli ve VPN hesapları: RBAC rolleri, yalnızca VPN hesapları, sabit VPN adresleri, parolalar, hesabı kapatma, kendi hesap sayfanız ve kurtarma kodlarıyla iki aşamalı doğrulama (2FA)."
weight: 905
chapter_num: "09.5"
url: "/tr/manual/system-users/"
aliases:
  - "/tr/manual/09-05-system-users/"
translationKey: "manual-09-05-system-users"
---

Ekibinizdeki her kişiye kendine ait bir kullanıcı hesabı verin: böylece günlük kayıtları kimin neyi değiştirdiğini açıkça gösterir ve ayrılan bir çalışanın erişimi diğer herkesin parolasını değiştirmeye gerek kalmadan tek tıkla kapatılabilir.

Bir hesap **web panelinde** oturum açabilir, **OpenVPN** ile bağlanabilir ya da her ikisini birden yapabilir.

![System > Users](/images/manual/users.png)

## Kullanıcı listesi

Tablo her hesabın **User**, **Role**, **Status**, **VPN** erişimi, **Last login** ve **Last failed attempt** bilgilerini; düzenleme, OpenVPN profilini indirme ve silme işlemleriyle birlikte listeler.

Kullanıcı değişiklikleri **Apply** gerekmeden anında devreye girer. Bir kullanıcının parolasını değiştirmek veya hesabını devre dışı bırakmak o kullanıcının tüm açık oturumlarını derhal sonlandırır.

## Kullanıcı ekleme veya düzenleme

**Add user** alanları:

| Alan | Anlamı |
| :--- | :--- |
| **User name** | Harfle başlayan 2-32 karakter; harf, rakam, `.`, `_` ve `-`. Oluşturulduktan sonra değiştirilemez. |
| **Full name / description** | Örneğin *Resepsiyon - gece vardiyası*. |
| **Role** | Aşağıdaki yetki rollerinden biri. |
| **Account enabled** | Devre dışı bırakılan kullanıcı giriş yapamaz ve açık oturumları anında kapanır. Hesap ve geçmişi silinmeden korunur. |
| **May sign in to the web panel** | Yalnızca VPN kullanacak hesaplar için kapatılır. |
| **May connect with OpenVPN** | Kullanıcıya kişisel bir istemci sertifikası üretir. OpenVPN sunucusunun açık olması gerekir ([**VPN > OpenVPN**](/tr/manual/vpn-openvpn/)). |
| **Fixed VPN address** | İsteğe bağlı: her bağlantıda aynı VPN IP adresini verir - güvenlik duvarı kurallarında tekil olarak bu kullanıcının adresine izin vermek için kullanılır. |
| **Password**, **Repeat password** | En az 10 karakter. Mevcut kullanıcıyı düzenlerken parolayı değiştirmemek için boş bırakın. |

### Roller (Roles)

| Rol | Yetkileri |
| :--- | :--- |
| **Administrator** | Tüm yetkiler. |
| **Operator** | Sayfaları görüntüler, tanılama araçlarını kullanır (ping, traceroute, açık bağlantıları sonlandırma, Wake-on-LAN, ARP temizleme) ve ön büro işlemlerini yürütür (Captive Portal cihaz bağlantısını kesme ve yerel portal kullanıcılarını yönetme). Yapılandırma değişikliği yapamaz. |
| **Read-only** | Yalnızca görüntüleme yetkisi. |

Kimin ne yaptığını gösteren veya gizli anahtarlara erişim sağlayan bazı sayfalar yalnızca yöneticilere açıktır: **Users**, **Backup**, **Update**, **Power**, **Admin Access** / **SNMP** / **MCP** ayarları, **Web Activity**, Web Filtresi raporu, engelleme listelerinin **Blocked** raporu, **Packet Capture**, IPsec günlüğü, **AI Assistant** ve OpenVPN profilleri ile WireGuard yapılandırmalarının indirilmesi.

Panel, sistemde yönetim paneline erişebilen hiçbir etkin yönetici bırakmayacak değişiklikleri reddeder; ayrıca kendi hesabınızı devre dışı bırakamaz, silemez, rolünü düşüremez veya panel yetkisini kaldıramazsınız - bunun için başka bir yöneticiden işlem yapmasını isteyin.

### Yalnızca VPN hesapları (VPN-only accounts)

Yalnızca uzaktan VPN bağlantısına ihtiyaç duyan personel için **May sign in to the web panel** seçeneğini kapatıp **May connect with OpenVPN** seçeneğini açın. Kullanıcı kendi kullanıcı adı, parolası ve kişisel profil dosyasıyla (`.ovpn`) VPN'e bağlanır ancak yönetim panelini açamaz.

Profil dosyasını kullanıcı listesinden veya [**VPN > OpenVPN**](/tr/manual/vpn-openvpn/) sayfasından indirin. **VPN yetkisini kaldırmak, kullanıcıyı devre dışı bırakmak veya silmek sertifikayı anında iptal eder** - eski profil dosyaları derhal geçersiz kalır.

## Hesabınız (Your account)

Her sayfanın sağ üst köşesindeki kullanıcı adına tıklandığında açılan **Your account** ekranında her kullanıcı (rolü ne olursa olsun) şunları yapabilir:

- kendi kullanıcı adını ve rolünü görüntülemek (rol ve açıklama yalnızca yöneticiler tarafından değiştirilir);
- mevcut parolasını girerek **parolasını değiştirmek** - bulunduğunuz tarayıcı oturumu açık kalır, diğer tüm cihazlardaki oturumlarınız kapatılır;
- **iki aşamalı doğrulamayı (2FA)** kurmak.

![Hesabınız](/images/manual/account.png)

## İki aşamalı doğrulama (Two-factor sign-in)

Parola doğrulandıktan sonra giriş ekranı telefonunuzdaki kimlik doğrulama uygulamasından (Google veya Microsoft Authenticator, 1Password, Bitwarden vb.) 6 haneli tek kullanımlık kodu ister. Böylece tek başına çalınmış bir parola giriş yapmak için yeterli olmaz.

Özellikle yönetim paneline dış ağlardan erişilebilen kurulumlarda **her yönetici hesabı için şiddetle önerilir**.

### Kurulum adımları

1. **Your account > Set up two-factor sign-in** düğmesine tıklayın ve parolanızı girin.
2. QR kodu kimlik doğrulama uygulamanızla okutun (veya anahtarı elle girin).
3. Uygulamanın ürettiği 6 haneli kodu yazıp **Switch on** düğmesine tıklayın.
4. Ekranda gösterilen **10 adet kurtarma kodunu (recovery codes) hemen kaydedin** - bu kodlar yalnızca bir kez gösterilir. Telefonunuz yanınızda olmadığında her kod tek seferlik giriş sağlar. Kodları parola yöneticisinde saklayın veya yazdırıp güvenli bir yere kaldırın.
5. **I have saved them** düğmesine tıklayın.

İki aşamalı doğrulamayı açmak diğer tüm cihazlardaki açık oturumlarınızı sonlandırır.

### Sonraki yönetim işlemleri

- **Your account** sayfası kaç adet kullanılmamış kurtarma kodunuz kaldığını gösterir. **New recovery codes** (parolanız ve güncel 6 haneli kodla) eski kodları iptal edip yeni bir 10'lu set üretir.
- **Switch off** parolanız ve güncel 6 haneli kodla iki aşamalı doğrulamayı kapatır.
- **Telefon kayboldu ve kurtarma kodu kalmadıysa:** başka bir yönetici **System > Users** sayfasından ilgili kullanıcıyı açıp **Switch off two-factor sign-in** düğmesine tıklayabilir. Ardından kullanıcı parolasıyla giriş yapıp 2FA kurulumunu yeniler.

> **Sistemdeki tek yönetici olarak dışarıda mı kaldınız?** Fiziksel konsol menüsündeki **3) Reset admin password** seçeneği yönetim paneli için yeni parola belirler.
