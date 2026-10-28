<div align="center">

**English** · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

**Offline** password manager for Android: the features of [Bitwarden](https://bitwarden.com) and
[Keyra](https://github.com/Tahir-1907/Keyra) in one mobile app, with GSAP animations everywhere and an interface
you can fully customize with modules. No account, no server, no telemetry: the vault never leaves
the phone.

Packaged as an APK with [Capacitor](https://capacitorjs.com/), built automatically by GitHub Actions, and designed for the
Galaxy Z Fold (folded: tab bar; unfolded: rail on the left and the open item on the right).

## Download the APK

➡️ **[Download Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — the file is also in the
*Releases* tab, with its SHA-256 checksum.

1. Download `haiku.apk` on the phone and open it.
2. Allow installation from this source if Android asks, then tap **Install**.
3. On first launch: *Create my vault* (or *Restore a backup*).

Every version is signed with **the same key** (`signing/` folder): an update installs over the
previous one without uninstalling, so without losing the vault. This key is public in the repository: only install Haiku
from this repository. For a private key, add the secrets `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` and
`HAIKU_KEY_PASSWORD` in *Settings › Secrets › Actions* (changing the key requires uninstalling: make a
backup first).

## What Haiku does

### Vault
- **7 item types**: login, payment card (3D card you flip to see the security code), identity, secure
  note, SSH key (Ed25519 generation in OpenSSH format), Wi-Fi network (shared via QR code), server (copies the
  `ssh`/`sftp` command).
- Several **URIs per login** with a match rule (domain, host, exact, regular expression…),
  **custom fields** (text, hidden, yes/no), encrypted **attachments** (10 MB), **notes**.
- **Folders** (color, drag to reorder), **tags** (20 per item, Keyra's rules), **favorites**.
- **Search** ignoring accents and case, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **History**: 20 versions per item, restorable with a before/after comparison; 10 previous passwords.
- **Trash** with automatic purge (7, 30, 90 days, 1 year or never), **global history** of changes.
- **Master password re-prompt** per item (like Bitwarden), **multi-select** (move, favorites,
  trash), **duplicate**, gestures: swipe right = copy, swipe left = favorite, long press = menu.
- **Multiple independent vaults**, each with its own password.

### Autofill
- Haiku is an **Android autofill service** (Settings › Autofill): in an app or a
  browser, tap the field then "Fill with Haiku", unlock (biometrics, PIN or password) and pick
  the login. Matching follows Bitwarden's rules (domain, host, `androidapp://paquet`…), and
  the link to an app can be remembered in one tap.
- The login's 2FA code is copied automatically for the next step.
- After you sign in, Android offers to **save** (or update) the login in Haiku.
- Chrome: *Settings › Autofill › Use another service*.

### Authenticator (2FA)
- **TOTP** codes (SHA-1/256/512, 6 or 8 digits, any period), **HOTP** and **Steam Guard**, live with a
  countdown ring and the next code.
- Add by **scanning a QR code** (camera or screenshot), manual key or `otpauth://` link.
- Import from a **Google Authenticator** export ("Transfer accounts") and from **Aegis**.

### Generator
- Passwords (5 to 128 characters, minimum digits/symbols, ambiguous characters excluded, symbols of your choice),
  **passphrases** (7,776-word lists in French and English), **usernames** (words, random, address
  `moi+xxxx@gmail.com`, catch-all domain), **PIN codes**. Exact entropy and time to crack shown.
- History of generated values, encrypted in the vault.

### Vault health
Score out of 100 and reports: **weak**, **reused**, **old** (more than a year) and **missing** passwords,
**http:// sites**, **expired cards**, and on demand: **breaches** (Have I Been Pwned, k-anonymity: only
5 characters of the SHA-1 hash are sent) and **2FA available but not enabled** (2fa.directory list).

### Send (encrypted sharing)
The offline equivalent of Bitwarden Send: a text or a file (15 MB) becomes an encrypted **self-contained HTML page**
(AES-256-GCM, PBKDF2-SHA256 600,000 iterations), with an expiry date. The recipient opens it in any
browser with the password you give them through another channel.

### Import / export
| Format | Import | Export |
|---|---|---|
| Encrypted Haiku (`.haiku.json`, Argon2id + AES-256-GCM, whole vault) | ✅ | ✅ |
| Haiku vault (`.haiku`, backup or another phone) | ✅ | ✅ |
| Bitwarden JSON (plain text) | ✅ | ✅ |
| **Password-protected** Bitwarden JSON (PBKDF2 or Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Encrypted Keyra `.mcfexport` | ✅ | ✅ (re-imports into Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Apple Passwords | ✅ | ✅ (browser CSV) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (plain JSON), Google Authenticator (QR) | ✅ | ✅ (`otpauth://` links) |
| Paper copy (HTML page to print, like Keyra's PDF) | — | ✅ |

Preview before import (without the passwords), folders created automatically, duplicates skipped, reminder to delete
plain-text files. Haiku's own types (Wi-Fi, server) survive a round trip through Bitwarden thanks to a
`haiku-type` field. Exporting asks for the master password again.

### Security
```text
master password ──Argon2id (64 MiB, 3 passes, 4 lanes)──► KEK
KEK ──AES-256-GCM──► DEK (random 256-bit data key)
DEK ──AES-256-GCM──► vault contents (compressed JSON)
```
- The master password is never stored. Changing it re-wraps the DEK without re-encrypting everything.
- Adjustable Argon2id strength: light (32 MiB), standard (64 MiB, like Keyra), hardened (128 MiB · 4 passes).
- Keyra-style **recovery key**: 32 characters (Crockford base32, 150 bits + checksum), shown only
  once; "forgot password" creates a new key and invalidates the old one.
- **Biometrics**: the DEK is encrypted by an **Android Keystore** AES key that can only be used after a
  strong biometric authentication, and is invalidated if a new fingerprint is added.
- **PIN code** (4 to 12 digits) derived with Argon2id, disabled after 5 failed attempts (adjustable), option to "ask for the
  master password again after a restart".
- **Auto-lock** (from immediately to 4 h, or never), in the background too; **clipboard** cleared after
  30 s (adjustable) and marked *sensitive* (hidden in the Android 13+ preview); **secure screen** (no screenshots,
  preview hidden in recent apps); Android backup of the app folder disabled; *release* APK
  (not debuggable).
- Automatic **backups** on lock (if the vault changed) in `Documents/Haiku/Sauvegardes`, 10 kept
  (adjustable); they restore as a **new** vault, nothing is overwritten.

### Customization: the Studio and modules
- **9 themes** (Haïku, Sumi-e, Sakura, Neon, Matrix, Solar, Glacier, Paper, Monochrome), dark, AMOLED,
  light and glass modes, two free accent colors (or a random harmony), theme sharing by code.
- **Animated backgrounds**: aurora, constellation (reacts to your finger), sumi-e ink (a drop at every touch), sakura
  petals, retro grid, waves, code rain; intensity, paper grain, halo.
- **8 fonts** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), corner rounding,
  glass blur, density, list as cards / rows / grid, initials or icons.
- Adjustable **GSAP animations** (none, gentle, normal, *wild*): 俳句 stamp intro, ensō brushed onto
  the lock screen and a lock that turns when unlocking, decryption-style secret reveal
  (ScrambleText), particle bursts (Physics2D), lists that reorder themselves (Flip), sheets and rows you drag
  (Draggable + Inertia), cards that tilt in 3D, page transitions of your choice (slide, zoom, 3D flip,
  curtain, blur).
- **Modules**: each feature (2FA, generator, health, Send, folders, history, trash, import/export,
  backups) can be turned on or off; you compose the **tab bar** (5 at most, drag to reorder); the **home screen**
  is made of reorderable widgets (haiku of the day, search, shortcuts, favorites, 2FA codes, health score,
  recent items, quick generator, statistics, folders, tag cloud).
- **Command palette** (swipe down on the header, or Ctrl+K on a keyboard) and keyboard shortcuts
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Known limitations
- No sync, organizations or emergency access (those are Bitwarden server features): it's
  a local vault, like Keyra. To move from one phone to another: `.haiku` backup or encrypted export.
- Passkeys in a Bitwarden export are not imported: Haiku is not a passkey provider.
- Interface in French only (Keyra offers 17 languages).
- Autofill goes through Haiku's screen (no inline suggestions in the keyboard): one more tap,
  but no secret is ever kept in plain text for the system.
- `.kdbx` files are not read directly: export them to CSV or XML from KeePassXC / KeePass.
- The strength estimate is offline and approximate.

## Build it yourself (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # core tests (encryption, imports/exports, 2FA…)
npm run android:add      # only if the android/ folder doesn't exist yet
npm run sync             # web build + copy into the Android project + Haiku settings
npm run open             # opens Android Studio (or: cd android && gradlew assembleRelease)
```
The APK is signed with the key in `signing/`, just like the GitHub Actions one: each installs over
the other.

## Code layout
```text
src/core/      encryption, vault, importers/exporters, TOTP, generator, strength, audit, Send, SSH
src/views/     screens (home, vault, item, edit, 2FA, generator, health, Send, Studio, modules…)
src/ui/        GSAP effects, animated backgrounds, theme, components, lock screen
scripts/       Android project settings, native Java plugin (Keystore biometrics, secure screen, autofill), icons
tests/         Node tests (with a real Keyra export) and a Playwright interface test
```

## Credits
Haiku is an independent rewrite: it reproduces the **features** of Bitwarden and Keyra and reads/writes
their **export formats**, without reusing their code. Thanks to [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), the
[EFF](https://www.eff.org/dice) word list (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(derived from Lexique 3.81), and to the Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho and
JetBrains Mono fonts (SIL Open Font License).
