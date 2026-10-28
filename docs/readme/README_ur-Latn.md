<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · **Roman Urdu** · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Android ke liye **offline** password manager: [Bitwarden](https://bitwarden.com) aur
[Keyra](https://github.com/Tahir-1907/Keyra) ke features ek mobile app mein, har jagah GSAP animations ke saath, aur ek interface
jo modules ke zariye poori tarah customize ho sakta hai. Koi account nahin, koi server nahin, koi telemetry nahin: vault kabhi
phone se bahar nahin jaata.

[Capacitor](https://capacitorjs.com/) ke saath APK mein pack kiya gaya, GitHub Actions se khud-ba-khud build hota hai, aur
Galaxy Z Fold ke liye banaya gaya hai (band: tab bar; khula: baayein taraf rail aur daayein taraf khula hua item).

## APK download karein

➡️ **[Haiku download karein (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — yeh file
*Releases* tab mein bhi hai, apne SHA-256 hash ke saath.

1. `haiku.apk` phone par download karein aur kholein.
2. Agar Android poochhe to is source se installation ki ijazat dein, phir **Install** dabayein.
3. Pehli baar kholne par: *Mera vault banayein* (ya *Backup restore karein*).

Saare versions **ek hi key** (`signing/` folder) se sign kiye gaye hain: update pichhle version ke upar uninstall kiye
baghair install ho jaata hai, is liye vault zaaya nahin hota. Yeh key repository mein public hai: Haiku sirf
isi repository se install karein. Private key ke liye `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` aur
`HAIKU_KEY_PASSWORD` secrets *Settings › Secrets › Actions* mein shamil karein (key badalne par uninstall zaroori hai: pehle
backup le lein).

## Haiku kya karta hai

### Vault
- **7 qism ke items**: login, payment card (3D card jo palat kar security code dikhata hai), shanakht, mehfooz
  note, SSH key (OpenSSH format mein Ed25519 generation), Wi-Fi network (QR code se share), server (
  `ssh`/`sftp` command copy).
- Har login ke liye kai **addresses**, matching rule ke saath (domain, host, exact, regular expression…),
  **custom fields** (text, chhupa hua, haan/nahin), encrypted **attachments** (10 MB), **notes**.
- **Folders** (rang, drag se tarteeb), **tags** (har item par 20, Keyra ke qawaid), **favourites**.
- **Search** jo accents aur chhote/bade huroof ko nazar-andaaz karti hai, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **History**: har item ke 20 versions, pehle/baad ke muqable ke saath restore ho sakte hain; 10 purane passwords.
- **Trash** khud-ba-khud khaali hota hai (7, 30, 90 din, 1 saal ya kabhi nahin), tabdeeliyon ki **mukammal history**.
- Har item ke liye **master password dobara maangna** (Bitwarden ki tarah), **multi-select** (move, favourites,
  trash), **duplicate**, gestures: daayein swipe = copy, baayein = favourite, lamba dabana = menu.
- Kai **alag alag vaults**, har ek ka apna password.

### Autofill
- Haiku ek **Android autofill service** hai (Settings › Autofill): kisi app ya
  browser mein field ko chhuein phir "Haiku se bharein", unlock karein (biometrics, PIN ya password) aur
  login chunein. Matching Bitwarden ke qawaid par chalti hai (domain, host, `androidapp://paquet`…), aur
  kisi app se taalluq ek tap mein yaad rakha ja sakta hai.
- Login ka 2FA code agle qadam ke liye khud-ba-khud copy ho jaata hai.
- Login ke baad Android login ko Haiku mein **save** (ya update) karne ki tajweez deta hai.
- Chrome: *Settings › Autofill › Doosri service istemal karein*.

### Authenticator (2FA)
- **TOTP** codes (SHA-1/256/512, 6 ya 8 hindse, apni marzi ka period), **HOTP** aur **Steam Guard**, live,
  countdown ring aur agle code ke saath.
- **QR code scan** (camera ya screenshot), manual key ya `otpauth://` link se shamil karein.
- **Google Authenticator** export ("Accounts transfer karein") aur **Aegis** import karein.

### Generator
- Passwords (5 se 128 characters, kam-az-kam hindse/symbols, mubham characters khaarij, apni pasand ke symbols),
  **passphrases** (French aur English mein 7,776 alfaaz ki fehristein), **usernames** (alfaaz, random, address
  `moi+xxxx@gmail.com`, catch-all domain), **PIN codes**. Durust entropy aur todne ka waqt dikhaya jaata hai.
- Banaye gaye values ki history, vault mein encrypted.

### Vault ki sehat
100 mein se score aur reports: **kamzor**, **dobara istemal hue**, **purane** (ek saal se zyada), **ghayab** passwords,
**http:// wali sites**, **expire shuda cards**, aur maangne par: **leaks** (Have I Been Pwned, k-anonymity: SHA-1 hash ke sirf
5 characters bheje jaate hain) aur **2FA dastyab magar band** (2fa.directory ki fehrist).

### Send (encrypted sharing)
Bitwarden Send ka offline mutabadil: koi text ya file (15 MB) ek encrypted **khud-mukhtar HTML page** ban jaati hai
(AES-256-GCM, PBKDF2-SHA256 600,000 iterations), expiry date ke saath. Wusool karne wala use kisi bhi
browser mein us password se kholta hai jo aap use kisi doosre zariye se dete hain.

### Import / export
| Format | Import | Export |
|---|---|---|
| Encrypted Haiku (`.haiku.json`, Argon2id + AES-256-GCM, poora vault) | ✅ | ✅ |
| Haiku vault (`.haiku`, backup ya doosra phone) | ✅ | ✅ |
| Bitwarden JSON (plain text) | ✅ | ✅ |
| **Password se mehfooz** Bitwarden JSON (PBKDF2 ya Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Encrypted Keyra `.mcfexport` | ✅ | ✅ (Keyra mein dobara import hota hai) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Apple Passwords | ✅ | ✅ (browser CSV) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (plain JSON), Google Authenticator (QR) | ✅ | ✅ (`otpauth://` links) |
| Kaghazi copy (print ke liye HTML page, Keyra ke PDF ki tarah) | — | ✅ |

Import se pehle preview (passwords ke baghair), folders khud-ba-khud bante hain, duplicates chhod diye jaate hain, plain text files
delete karne ki yaad-dihani. Haiku ki apni qismein (Wi-Fi, server) ek `haiku-type` field ki badaulat
Bitwarden se ho kar wapas aa jaati hain. Export master password dobara maangta hai.

### Security
```text
master password ──Argon2id (64 MiB, 3 passes, 4 lanes)──► KEK
KEK ──AES-256-GCM──► DEK (random 256-bit data key)
DEK ──AES-256-GCM──► vault ka content (compressed JSON)
```
- Master password kabhi store nahin hota. Password badalne se sirf DEK dobara wrap hoti hai, sab kuchh dobara encrypt nahin hota.
- Argon2id ki mazbooti adjust ho sakti hai: halki (32 MiB), standard (64 MiB, Keyra ki tarah), mazboot (128 MiB · 4 passes).
- Keyra jaisi **recovery key**: 32 characters (Crockford base32, 150 bits + check), sirf ek
  baar dikhayi jaati hai; "password bhool gaya" nayi key banata hai aur purani ko radd kar deta hai.
- **Biometrics**: DEK ek **Android Keystore** AES key se encrypt hoti hai jo sirf mazboot biometric
  authentication ke baad istemal ho sakti hai, aur naya fingerprint shamil hone par radd ho jaati hai.
- **PIN code** (4 se 12 hindse) Argon2id se derive hota hai, 5 ghaltiyon ke baad band (adjust ho sakta hai), option "restart par
  master password dobara maangein".
- **Auto-lock** (fauran se 4 ghante tak, ya kabhi nahin), background mein bhi; **clipboard**
  30 s ke baad saaf (adjust ho sakta hai) aur *sensitive* mark (Android 13+ ke preview mein chhupa); **mehfooz screen** (koi screenshot nahin,
  recent apps mein preview chhupa); app folder ka Android backup band; *release* APK
  (debug nahin ho sakta).
- Lock karne par (agar vault badla ho) `Documents/Haiku/Sauvegardes` mein khud-ba-khud **backups**, 10 rakhe jaate hain
  (adjust ho sakta hai); yeh **naye** vault ke taur par restore hote hain, kuchh overwrite nahin hota.

### Customization: Studio aur modules
- **9 themes** (Haïku, Sumi-e, Sakura, Neon, Matrix, Solar, Glacier, Paper, Monochrome), dark, AMOLED,
  light aur glass modes, do azaad accent rang (ya random harmony), code ke zariye theme share karein.
- **Animated backgrounds**: aurora, constellation (ungli par react karti hai), sumi-e syahi (har touch par ek boond), sakura ki
  pattiyan, retro grid, lehrein, code ki baarish; shiddat, kaghaz ka grain, halo.
- **8 fonts** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), golaai,
  glass blur, density, list cards / lines / grid ki shakal mein, initials ya icons.
- Adjust hone wali **GSAP animations** (koi nahin, narm, normal, *zabardast*): 俳句 stamp wala intro, lock screen par brush se
  bana ensō aur unlock par ghoomta hua taala, secrets ka decrypt jaisa reveal
  (ScrambleText), particles ke phawware (Physics2D), khud tarteeb badalti lists (Flip), drag hone wale panels aur rows
  (Draggable + Inertia), 3D mein jhukte cards, apni pasand ke page transitions (slide, zoom, 3D flip,
  pardah, blur).
- **Modules**: har feature (2FA, generator, sehat, Send, folders, history, trash, import/export,
  backups) on ya off hota hai; **tab bar** aap khud banate hain (zyada se zyada 5, drag se tarteeb); **home**
  dobara tarteeb hone wale widgets se bani hai (aaj ka haiku, search, shortcuts, favourites, 2FA codes, sehat ka score,
  haaliya, fauri generator, statistics, folders, tag cloud).
- **Command palette** (header par neeche swipe karein, ya keyboard par Ctrl+K) aur keyboard shortcuts
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Maloom hadood
- Sync, organizations ya emergency access nahin (yeh Bitwarden server ke features hain): yeh
  Keyra ki tarah ek local vault hai. Ek phone se doosre par jaane ke liye: `.haiku` backup ya encrypted export.
- Bitwarden export ki passkeys import nahin hoteen: Haiku passkey provider nahin hai.
- Interface sirf French mein (Keyra 17 zubaanein deta hai).
- Autofill Haiku ki screen se guzarta hai (keyboard mein inline suggestions nahin): ek tap zyada,
  lekin koi secret kabhi system ke liye plain text mein nahin rakha jaata.
- `.kdbx` files seedha nahin parhi jaateen: unhein KeePassXC / KeePass se CSV ya XML mein export karein.
- Mazbooti ka andaza offline aur taqreebi hai.

## Khud build karein (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # core tests (encryption, imports/exports, 2FA…)
npm run android:add      # sirf agar android/ folder maujood na ho
npm run sync             # web build + Android project mein copy + Haiku settings
npm run open             # Android Studio kholta hai (ya: cd android && gradlew assembleRelease)
```
APK `signing/` ki key se sign hota hai, bilkul GitHub Actions wale ki tarah: dono ek doosre ke upar
install ho jaate hain.

## Code ki tarteeb
```text
src/core/      encryption, vault, importers/exporters, TOTP, generator, mazbooti, audit, Send, SSH
src/views/     screens (home, vault, item, edit, 2FA, generator, sehat, Send, Studio, modules…)
src/ui/        GSAP effects, animated backgrounds, theme, components, lock screen
scripts/       Android project settings, native Java plugin (Keystore biometrics, mehfooz screen, autofill), icons
tests/         Node tests (asli Keyra export ke saath) aur Playwright interface test
```

## Credits
Haiku ek azaad rewrite hai: yeh Bitwarden aur Keyra ke **features** dobara banata hai aur
un ke **export formats** parhta/likhta hai, un ka code dobara istemal kiye baghair. Shukriya [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), alfaaz ki fehrist
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(Lexique 3.81 se akhaz), aur fonts Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho aur
JetBrains Mono (SIL Open Font License) ka.
