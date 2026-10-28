<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · **Türkçe** · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Android için **çevrimdışı** parola yöneticisi: [Bitwarden](https://bitwarden.com) ve
[Keyra](https://github.com/Tahir-1907/Keyra) özellikleri tek bir mobil uygulamada, her yerde GSAP animasyonları ve
modüllerle tamamen kişiselleştirilebilen bir arayüzle. Hesap yok, sunucu yok, telemetri yok: kasa asla
telefondan çıkmaz.

[Capacitor](https://capacitorjs.com/) ile APK olarak paketlendi, GitHub Actions ile otomatik derlenir ve
Galaxy Z Fold için tasarlandı (katlıyken: sekme çubuğu; açıkken: solda ray, sağda açık öğe).

## APK'yı indir

➡️ **[Haiku'yu indir (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — dosya ayrıca
SHA-256 özetiyle birlikte *Releases* sekmesinde de var.

1. `haiku.apk` dosyasını telefona indir ve aç.
2. Android isterse bu kaynaktan yüklemeye izin ver, ardından **Yükle**'ye dokun.
3. İlk açılışta: *Kasamı oluştur* (ya da *Yedeği geri yükle*).

Tüm sürümler **aynı anahtarla** (`signing/` klasörü) imzalanır: bir güncelleme kaldırmadan önceki sürümün
üzerine kurulur, yani kasa kaybolmaz. Bu anahtar depoda herkese açıktır: Haiku'yu yalnızca
bu depodan yükle. Özel bir anahtar için *Settings › Secrets › Actions* bölümüne `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` ve
`HAIKU_KEY_PASSWORD` gizli değerlerini ekle (anahtar değiştirmek uygulamayı kaldırmayı gerektirir: önce
yedek al).

## Haiku neler yapar

### Kasa
- **7 öğe türü**: oturum bilgisi, ödeme kartı (güvenlik kodunu görmek için çevrilen 3B kart), kimlik, güvenli
  not, SSH anahtarı (OpenSSH biçiminde Ed25519 üretimi), Wi-Fi ağı (QR kodla paylaşım), sunucu (
  `ssh`/`sftp` komutunu kopyalar).
- Her oturum bilgisi için eşleşme kurallı birden çok **adres** (alan adı, sunucu, tam eşleşme, düzenli ifade…),
  **özel alanlar** (metin, gizli, evet/hayır), şifreli **ekler** (10 MB), **notlar**.
- **Klasörler** (renk, sürükleyerek sıralama), **etiketler** (öğe başına 20, Keyra kuralları), **favoriler**.
- Aksan ve büyük/küçük harf duyarsız **arama**, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Geçmiş**: öğe başına 20 sürüm, önce/sonra karşılaştırmasıyla geri yüklenebilir; 10 eski parola.
- Otomatik temizlenen **çöp kutusu** (7, 30, 90 gün, 1 yıl ya da asla), değişikliklerin **genel geçmişi**.
- Öğe bazında **ana parolayı yeniden sorma** (Bitwarden'daki gibi), **çoklu seçim** (taşı, favoriler,
  çöp kutusu), **çoğaltma**, hareketler: sağa kaydır = kopyala, sola = favori, uzun bas = menü.
- Her biri kendi parolasına sahip **birden çok bağımsız kasa**.

### Otomatik doldurma
- Haiku bir **Android otomatik doldurma servisidir** (Ayarlar › Otomatik doldurma): bir uygulamada ya da
  tarayıcıda alana dokun, ardından "Haiku ile doldur", kilidi aç (biyometri, PIN ya da parola) ve
  oturum bilgisini seç. Eşleşmeler Bitwarden kurallarını izler (alan adı, sunucu, `androidapp://paquet`…) ve
  bir uygulamayla ilişki tek dokunuşla hatırlanabilir.
- Oturum bilgisinin 2FA kodu bir sonraki adım için otomatik kopyalanır.
- Giriş yaptıktan sonra Android, oturum bilgisini Haiku'ya **kaydetmeyi** (ya da güncellemeyi) önerir.
- Chrome: *Ayarlar › Otomatik doldurma › Başka bir hizmet kullan*.

### Kimlik doğrulayıcı (2FA)
- **TOTP** kodları (SHA-1/256/512, 6 ya da 8 hane, serbest süre), **HOTP** ve **Steam Guard**, canlı olarak,
  geri sayım halkası ve sonraki kodla.
- **QR kod tarayarak** (kamera ya da ekran görüntüsü), elle anahtar girerek ya da `otpauth://` bağlantısıyla ekleme.
- **Google Authenticator** dışa aktarımını ("Hesapları aktar") ve **Aegis**'i içe aktarma.

### Üretici
- Parolalar (5 ila 128 karakter, en az rakam/sembol sayısı, karışabilen karakterler hariç, seçilebilir semboller),
  **parola cümleleri** (Fransızca ve İngilizce 7.776 kelimelik listeler), **kullanıcı adları** (kelimeler, rastgele, adres
  `moi+xxxx@gmail.com`, catch-all alan adı), **PIN kodları**. Kesin entropi ve kırılma süresi gösterilir.
- Üretilen değerlerin geçmişi, kasada şifreli olarak saklanır.

### Kasa sağlığı
100 üzerinden puan ve raporlar: **zayıf**, **tekrar kullanılan**, **eski** (bir yıldan fazla), **eksik** parolalar,
**http:// siteler**, **süresi dolmuş kartlar** ve istek üzerine: **sızıntılar** (Have I Been Pwned, k-anonimlik: yalnızca
SHA-1 özetinin 5 karakteri gönderilir) ve **mevcut ama etkin olmayan 2FA** (2fa.directory listesi).

### Send (şifreli paylaşım)
Bitwarden Send'in çevrimdışı karşılığı: bir metin ya da dosya (15 MB) şifreli bir **bağımsız HTML sayfasına** dönüşür
(AES-256-GCM, PBKDF2-SHA256 600.000 yineleme), son kullanma tarihiyle. Alıcı onu herhangi bir
tarayıcıda, başka bir kanaldan ilettiğin parolayla açar.

### İçe / dışa aktarma
| Biçim | İçe aktarma | Dışa aktarma |
|---|---|---|
| Şifreli Haiku (`.haiku.json`, Argon2id + AES-256-GCM, tüm kasa) | ✅ | ✅ |
| Haiku kasası (`.haiku`, yedek ya da başka bir telefon) | ✅ | ✅ |
| Bitwarden JSON (düz metin) | ✅ | ✅ |
| **Parola korumalı** Bitwarden JSON (PBKDF2 ya da Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Şifreli Keyra `.mcfexport` | ✅ | ✅ (Keyra'ya yeniden aktarılabilir) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Apple Parolalar | ✅ | ✅ (tarayıcı CSV) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (düz JSON), Google Authenticator (QR) | ✅ | ✅ (`otpauth://` bağlantıları) |
| Kâğıt kopya (yazdırılacak HTML sayfası, Keyra'nın PDF'i gibi) | — | ✅ |

İçe aktarmadan önce önizleme (parolalar olmadan), klasörler otomatik oluşturulur, kopyalar atlanır, düz metin
dosyaları silme hatırlatması. Haiku'ya özgü türler (Wi-Fi, sunucu) bir `haiku-type` alanı sayesinde Bitwarden üzerinden
gidip gelse de kaybolmaz. Dışa aktarma ana parolayı yeniden sorar.

### Güvenlik
```text
ana parola ──Argon2id (64 MiB, 3 geçiş, 4 kanal)──► KEK
KEK ──AES-256-GCM──► DEK (rastgele 256 bitlik veri anahtarı)
DEK ──AES-256-GCM──► kasa içeriği (sıkıştırılmış JSON)
```
- Ana parola asla saklanmaz. Parolayı değiştirmek tüm veriyi yeniden şifrelemeden yalnızca DEK'i yeniden sarar.
- Ayarlanabilir Argon2id gücü: hafif (32 MiB), standart (64 MiB, Keyra gibi), güçlendirilmiş (128 MiB · 4 geçiş).
- Keyra tarzı **kurtarma anahtarı**: 32 karakter (Crockford base32, 150 bit + denetim), yalnızca
  bir kez gösterilir; "parolamı unuttum" yeni bir anahtar oluşturur ve eskisini geçersiz kılar.
- **Biyometri**: DEK, yalnızca güçlü bir biyometrik doğrulamadan sonra kullanılabilen bir **Android Keystore** AES anahtarıyla
  şifrelenir ve yeni bir parmak izi eklenirse geçersiz olur.
- Argon2id ile türetilen **PIN kodu** (4 ila 12 hane), 5 hatadan sonra devre dışı (ayarlanabilir), "yeniden başlatmadan sonra
  ana parolayı tekrar sor" seçeneği.
- **Otomatik kilit** (anında ile 4 sa arası ya da asla), arka planda da; **pano**
  30 sn sonra temizlenir (ayarlanabilir) ve *hassas* olarak işaretlenir (Android 13+ önizlemesinde gizli); **güvenli ekran** (ekran görüntüsü yok,
  son uygulamalarda önizleme gizli); uygulama klasörünün Android yedeği kapalı; *release* APK
  (hata ayıklanamaz).
- Kilitlenince otomatik **yedekler** (kasa değiştiyse) `Documents/Haiku/Sauvegardes` klasörüne, 10 tanesi saklanır
  (ayarlanabilir); **yeni** bir kasa olarak geri yüklenirler, hiçbir şeyin üzerine yazılmaz.

### Kişiselleştirme: Stüdyo ve modüller
- **9 tema** (Haïku, Sumi-e, Sakura, Neon, Matris, Güneş, Buzul, Kâğıt, Tek renk), koyu, AMOLED,
  açık ve cam modları, iki serbest vurgu rengi (ya da rastgele bir uyum), temayı kodla paylaşma.
- **Hareketli arka planlar**: kutup ışıkları, takımyıldız (parmağa tepki verir), sumi-e mürekkebi (her dokunuşta bir damla), sakura
  yaprakları, retro ızgara, dalgalar, kod yağmuru; yoğunluk, kâğıt dokusu, hale.
- **8 yazı tipi** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), köşe yuvarlaklığı,
  cam bulanıklığı, yoğunluk, kart / satır / ızgara şeklinde liste, baş harfler ya da simgeler.
- Ayarlanabilir **GSAP animasyonları** (yok, yumuşak, normal, *çılgın*): 俳句 mühürlü giriş, kilit ekranında fırçayla çizilen
  ensō ve kilit açılırken dönen kilit, şifre çözülür gibi beliren gizli bilgiler
  (ScrambleText), parçacık patlamaları (Physics2D), kendini yeniden düzenleyen listeler (Flip), sürüklenebilen paneller ve satırlar
  (Draggable + Inertia), 3B eğilen kartlar, seçilebilen sayfa geçişleri (kaydırma, yakınlaştırma, 3B çevirme,
  perde, bulanıklık).
- **Modüller**: her özellik (2FA, üretici, sağlık, Send, klasörler, geçmiş, çöp kutusu, içe/dışa aktarma,
  yedekler) açılıp kapatılabilir; **sekme çubuğunu** sen oluşturursun (en fazla 5, sürükleyerek sıralama); **ana ekran**
  yeniden sıralanabilir widget'lardan oluşur (günün haikusu, arama, kısayollar, favoriler, 2FA kodları, sağlık puanı,
  son öğeler, hızlı üretici, istatistikler, klasörler, etiket bulutu).
- **Komut paleti** (başlıkta aşağı kaydır ya da klavyede Ctrl+K) ve klavye kısayolları
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Bilinen sınırlamalar
- Senkronizasyon, organizasyonlar ve acil erişim yok (bunlar Bitwarden sunucusunun özellikleri): Keyra gibi
  yerel bir kasadır. Bir telefondan diğerine geçmek için: `.haiku` yedeği ya da şifreli dışa aktarma.
- Bir Bitwarden dışa aktarımındaki geçiş anahtarları (passkey) içe aktarılmaz: Haiku bir geçiş anahtarı sağlayıcısı değildir.
- Arayüz yalnızca Fransızca (Keyra 17 dil sunuyor).
- Otomatik doldurma Haiku'nun ekranından geçer (klavyede satır içi öneri yok): bir dokunuş fazla,
  ama hiçbir sır sistem için asla düz metin olarak tutulmaz.
- `.kdbx` dosyaları doğrudan okunmaz: onları KeePassXC / KeePass'ten CSV ya da XML olarak dışa aktar.
- Güç tahmini çevrimdışıdır ve yaklaşıktır.

## Kendin derle (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # çekirdek testleri (şifreleme, içe/dışa aktarma, 2FA…)
npm run android:add      # yalnızca android/ klasörü yoksa
npm run sync             # web derlemesi + Android projesine kopyalama + Haiku ayarları
npm run open             # Android Studio'yu açar (ya da: cd android && gradlew assembleRelease)
```
APK, GitHub Actions'ınki gibi `signing/` içindeki anahtarla imzalanır: ikisi birbirinin üzerine
kurulabilir.

## Kodun düzeni
```text
src/core/      şifreleme, kasa, içe/dışa aktarıcılar, TOTP, üretici, güç, denetim, Send, SSH
src/views/     ekranlar (ana ekran, kasa, öğe, düzenleme, 2FA, üretici, sağlık, Send, Stüdyo, modüller…)
src/ui/        GSAP efektleri, hareketli arka planlar, tema, bileşenler, kilit ekranı
scripts/       Android projesi ayarları, yerel Java eklentisi (Keystore biyometrisi, güvenli ekran, otomatik doldurma), simgeler
tests/         Node testleri (gerçek bir Keyra dışa aktarımıyla) ve Playwright arayüz testi
```

## Teşekkürler
Haiku bağımsız bir yeniden yazımdır: Bitwarden ve Keyra'nın **özelliklerini** yeniden üretir ve
onların **dışa aktarma biçimlerini** okuyup yazar, ama kodlarını kullanmaz. [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT),
[EFF](https://www.eff.org/dice) kelime listesi (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(Lexique 3.81'den türetilmiştir) ve Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho ile
JetBrains Mono yazı tiplerine (SIL Open Font License) teşekkürler.
