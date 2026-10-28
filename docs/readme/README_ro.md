<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · **Română**

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Manager de parole **offline** pentru Android: funcțiile din [Bitwarden](https://bitwarden.com) și din
[Keyra](https://github.com/Tahir-1907/Keyra) într-o aplicație mobilă, cu animații GSAP peste tot și o interfață
complet personalizabilă prin module. Fără cont, fără server, fără telemetrie: seiful nu părăsește niciodată
telefonul.

Împachetat ca APK cu [Capacitor](https://capacitorjs.com/), compilat automat de GitHub Actions și gândit pentru
Galaxy Z Fold (pliat: bară de file; depliat: bară laterală în stânga și elementul deschis în dreapta).

## Descarcă APK-ul

➡️ **[Descarcă Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — fișierul se află și în
fila *Releases*, împreună cu amprenta sa SHA-256.

1. Descarcă `haiku.apk` pe telefon și deschide-l.
2. Permite instalarea din această sursă dacă Android cere acest lucru, apoi **Instalează**.
3. La prima pornire: *Creează seiful meu* (sau *Restaurează o copie de rezervă*).

Toate versiunile sunt semnate cu **aceeași cheie** (folderul `signing/`): o actualizare se instalează peste
versiunea anterioară fără dezinstalare, deci fără a pierde seiful. Această cheie este publică în depozit: instalează Haiku doar
din acest depozit. Pentru o cheie privată, adaugă secretele `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` și
`HAIKU_KEY_PASSWORD` în *Settings › Secrets › Actions* (schimbarea cheii impune o dezinstalare: fă mai întâi o
copie de rezervă).

## Ce face Haiku

### Seif
- **7 tipuri de elemente**: autentificare, card bancar (card 3D care se întoarce pentru a vedea codul de securitate), identitate, notă
  securizată, cheie SSH (generare Ed25519 în format OpenSSH), rețea Wi-Fi (partajare prin cod QR), server (copierea
  comenzii `ssh`/`sftp`).
- Mai multe **adrese per autentificare** cu regulă de potrivire (domeniu, gazdă, exact, expresie regulată…),
  **câmpuri personalizate** (text, ascuns, da/nu), **atașamente** criptate (10 MB), **note**.
- **Foldere** (culoare, ordine prin glisare), **etichete** (20 per element, regulile din Keyra), **favorite**.
- **Căutare** fără diacritice și fără a ține cont de majuscule, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Istoric**: 20 de versiuni per element, restaurabile cu comparație înainte/după; 10 parole vechi.
- **Coș de gunoi** cu golire automată (7, 30, 90 de zile, 1 an sau niciodată), **istoric global** al modificărilor.
- **Cererea din nou a parolei principale** per element (ca în Bitwarden), **selecție multiplă** (mutare, favorite,
  coș), **duplicare**, gesturi: glisare la dreapta = copiere, la stânga = favorit, apăsare lungă = meniu.
- **Mai multe seifuri** independente, fiecare cu parola sa.

### Completare automată
- Haiku este un **serviciu de completare automată Android** (Setări › Completare automată): într-o aplicație sau într-un
  browser, atinge câmpul apoi „Completează cu Haiku”, deblochează (biometrie, PIN sau parolă) și alege
  autentificarea. Potrivirile urmează regulile Bitwarden (domeniu, gazdă, `androidapp://paquet`…), iar
  asocierea cu o aplicație poate fi memorată cu un singur gest.
- Codul 2FA al autentificării este copiat automat pentru pasul următor.
- După o conectare, Android propune **salvarea** (sau actualizarea) autentificării în Haiku.
- Chrome: *Setări › Completare automată › Folosește alt serviciu*.

### Autentificator (2FA)
- Coduri **TOTP** (SHA-1/256/512, 6 sau 8 cifre, perioadă liberă), **HOTP** și **Steam Guard**, în timp real cu inel de
  numărătoare inversă și codul următor.
- Adăugare prin **scanarea unui cod QR** (cameră sau captură de ecran), cheie manuală sau link `otpauth://`.
- Import din exportul **Google Authenticator** („Transferă conturi”) și din **Aegis**.

### Generator
- Parole (5 până la 128 de caractere, minimum de cifre/simboluri, caractere ambigue excluse, simboluri la alegere),
  **fraze de acces** (liste de 7.776 de cuvinte în franceză și engleză), **nume de utilizator** (cuvinte, aleatoriu, adresă
  `moi+xxxx@gmail.com`, domeniu catch-all), **coduri PIN**. Entropia exactă și timpul de spargere sunt afișate.
- Istoricul valorilor generate, criptat în seif.

### Sănătatea seifului
Scor din 100 și rapoarte: parole **slabe**, **refolosite**, **vechi** (mai mult de un an), **lipsă**,
**site-uri pe http://**, **carduri expirate**, iar la cerere: **scurgeri** (Have I Been Pwned, k-anonimat: se trimit doar
5 caractere din amprenta SHA-1) și **2FA disponibil dar inactiv** (lista 2fa.directory).

### Send (partajare criptată)
Echivalentul offline al Bitwarden Send: un text sau un fișier (15 MB) devine o **pagină HTML autonomă** criptată
(AES-256-GCM, PBKDF2-SHA256 cu 600.000 de iterații), cu dată de expirare. Destinatarul o deschide în orice
browser cu parola pe care i-o transmiți pe alt canal.

### Import / export
| Format | Import | Export |
|---|---|---|
| Haiku criptat (`.haiku.json`, Argon2id + AES-256-GCM, tot seiful) | ✅ | ✅ |
| Seif Haiku (`.haiku`, copie de rezervă sau alt telefon) | ✅ | ✅ |
| Bitwarden JSON (necriptat) | ✅ | ✅ |
| Bitwarden JSON **protejat prin parolă** (PBKDF2 sau Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` criptat | ✅ | ✅ (se reimportă în Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Parole Apple | ✅ | ✅ (CSV de browser) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON necriptat), Google Authenticator (QR) | ✅ | ✅ (linkuri `otpauth://`) |
| Copie pe hârtie (pagină HTML de tipărit, ca PDF-ul din Keyra) | — | ✅ |

Previzualizare înainte de import (fără parole), foldere create automat, duplicate ignorate, memento pentru ștergerea
fișierelor necriptate. Tipurile specifice Haiku (Wi-Fi, server) fac drumul dus-întors prin Bitwarden datorită unui câmp
`haiku-type`. Exportul cere din nou parola principală.

### Securitate
```text
parola principală ──Argon2id (64 MiB, 3 treceri, 4 benzi)──► KEK
KEK ──AES-256-GCM──► DEK (cheie de date aleatorie de 256 de biți)
DEK ──AES-256-GCM──► conținutul seifului (JSON comprimat)
```
- Parola principală nu este stocată niciodată. Schimbarea parolei reîmpachetează DEK fără a recripta totul.
- Rezistență Argon2id reglabilă: ușoară (32 MiB), standard (64 MiB, ca Keyra), întărită (128 MiB · 4 treceri).
- **Cheie de recuperare** în stilul Keyra: 32 de caractere (base32 Crockford, 150 de biți + control), afișată o singură
  dată; „am uitat parola” creează o cheie nouă și o invalidează pe cea veche.
- **Biometrie**: DEK este criptată cu o cheie AES din **Android Keystore** utilizabilă doar după o
  autentificare biometrică puternică, invalidată dacă se adaugă o amprentă nouă.
- **Cod PIN** (4 până la 12 cifre) derivat prin Argon2id, dezactivat după 5 erori (reglabil), opțiunea „cere din nou
  parola principală la repornire”.
- **Blocare automată** (de la imediat la 4 h, sau niciodată), inclusiv în fundal; **clipboard** golit după
  30 s (reglabil) și marcat *sensibil* (ascuns în previzualizarea din Android 13+); **ecran securizat** (fără capturi,
  previzualizare ascunsă în aplicațiile recente); backup-ul Android al folderului aplicației dezactivat; APK *release*
  (nedepanabil).
- **Copii de rezervă** automate la blocare (dacă seiful s-a schimbat) în `Documents/Haiku/Sauvegardes`, 10 păstrate
  (reglabil); se restaurează ca un seif **nou**, nimic nu este suprascris.

### Personalizare: Studio și modulele
- **9 teme** (Haïku, Sumi-e, Sakura, Neon, Matrice, Solar, Ghețar, Hârtie, Monocrom), moduri întunecat, AMOLED,
  luminos și sticlă, două culori de accent libere (sau o armonie aleatorie), partajarea temei prin cod.
- **Fundaluri animate**: auroră, constelație (reacționează la deget), cerneală sumi-e (o picătură la fiecare atingere), petale de
  sakura, grilă retro, unde, ploaie de cod; intensitate, granulație de hârtie, halou.
- **8 fonturi** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), rotunjiri,
  estomparea sticlei, densitate, listă sub formă de carduri / rânduri / grilă, inițiale sau pictograme.
- **Animații GSAP** reglabile (deloc, blânde, normale, *nebunești*): intro cu ștampila 俳句, ensō trasat cu pensula pe
  ecranul de blocare și lacăt care se rotește la deblocare, dezvăluirea secretelor ca o decriptare
  (ScrambleText), jerbe de particule (Physics2D), liste care se reorganizează (Flip), panouri și rânduri de glisat
  (Draggable + Inertia), carduri care se înclină în 3D, tranziții de pagină la alegere (glisare, zoom, răsturnare 3D,
  cortină, estompare).
- **Module**: fiecare funcție (2FA, generator, sănătate, Send, foldere, istoric, coș, import/export,
  copii de rezervă) se activează sau se dezactivează; **bara de file** se compune (cel mult 5, ordine prin glisare); **ecranul principal**
  este format din widgeturi reordonabile (haiku-ul zilei, căutare, scurtături, favorite, coduri 2FA, scor de sănătate,
  recente, generator rapid, statistici, foldere, nor de etichete).
- **Paletă de comenzi** (glisare în jos pe antet, sau Ctrl+K de la tastatură) și scurtături de tastatură
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Limitări cunoscute
- Fără sincronizare, organizații sau acces de urgență (acestea sunt funcții ale serverului Bitwarden): este
  un seif local, ca Keyra. Pentru a trece de pe un telefon pe altul: copie de rezervă `.haiku` sau export criptat.
- Cheile de acces (passkeys) dintr-un export Bitwarden nu sunt importate: Haiku nu este un furnizor de passkeys.
- Interfață doar în franceză (Keyra oferă 17 limbi).
- Completarea automată trece prin ecranul Haiku (fără sugestii în linie în tastatură): o atingere
  în plus, dar niciun secret nu este păstrat vreodată necriptat pentru sistem.
- Fișierele `.kdbx` nu sunt citite direct: exportă-le în CSV sau XML din KeePassXC / KeePass.
- Estimarea robusteții se face offline și este aproximativă.

## Compilează singur (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # testele nucleului (criptare, importuri/exporturi, 2FA…)
npm run android:add      # doar dacă folderul android/ nu există
npm run sync             # build web + copiere în proiectul Android + setările Haiku
npm run open             # deschide Android Studio (sau: cd android && gradlew assembleRelease)
```
APK-ul este semnat cu cheia din `signing/`, la fel ca cel din GitHub Actions: se instalează unul peste
celălalt.

## Organizarea codului
```text
src/core/      criptare, seif, importatori/exportatori, TOTP, generator, robustețe, audit, Send, SSH
src/views/     ecrane (principal, seif, element, editare, 2FA, generator, sănătate, Send, Studio, module…)
src/ui/        efecte GSAP, fundaluri animate, temă, componente, ecran de blocare
scripts/       setările proiectului Android, plugin nativ Java (biometrie Keystore, ecran securizat, completare automată), pictograme
tests/         teste Node (cu un export Keyra real) și test de interfață Playwright
```

## Mulțumiri
Haiku este o rescriere independentă: preia **funcționalitățile** din Bitwarden și Keyra și citește/scrie
**formatele lor de export**, fără a le refolosi codul. Mulțumiri pentru [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), lista de cuvinte
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(derivată din Lexique 3.81), precum și fonturilor Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho și
JetBrains Mono (SIL Open Font License).
