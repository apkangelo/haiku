<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · **Italiano**
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Gestore di password **offline** per Android: le funzioni di [Bitwarden](https://bitwarden.com) e di
[Keyra](https://github.com/Tahir-1907/Keyra) in un'app mobile, con animazioni GSAP ovunque e un'interfaccia
interamente personalizzabile a moduli. Nessun account, nessun server, nessuna telemetria: la cassaforte non lascia mai
il telefono.

Impacchettato come APK con [Capacitor](https://capacitorjs.com/), compilato automaticamente da GitHub Actions e pensato per il
Galaxy Z Fold (chiuso: barra delle schede; aperto: barra laterale a sinistra e scheda aperta a destra).

## Scaricare l'APK

➡️ **[Scarica Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — il file è anche nella
scheda *Releases*, con il relativo hash SHA-256.

1. Scarica `haiku.apk` sul telefono e aprilo.
2. Consenti l'installazione da questa origine se Android lo chiede, poi tocca **Installa**.
3. Al primo avvio: *Crea la mia cassaforte* (o *Ripristina un backup*).

Tutte le versioni sono firmate con **la stessa chiave** (cartella `signing/`): un aggiornamento si installa sopra la
precedente senza disinstallare, quindi senza perdere la cassaforte. Questa chiave è pubblica nel repository: installa Haiku
solo da questo repository. Per una chiave privata, aggiungi i secret `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` e
`HAIKU_KEY_PASSWORD` in *Settings › Secrets › Actions* (cambiare chiave richiede una disinstallazione: fai prima un
backup).

## Cosa fa Haiku

### Cassaforte
- **7 tipi di elementi**: login, carta di pagamento (carta 3D da girare per vedere il codice di sicurezza), identità, nota
  protetta, chiave SSH (generazione Ed25519 in formato OpenSSH), rete Wi-Fi (condivisione tramite codice QR), server (copia del
  comando `ssh`/`sftp`).
- Più **indirizzi per login** con regola di riconoscimento (dominio, host, esatto, espressione regolare…),
  **campi personalizzati** (testo, nascosto, sì/no), **allegati** cifrati (10 MB), **note**.
- **Cartelle** (colore, ordine trascinando), **tag** (20 per elemento, regole di Keyra), **preferiti**.
- **Ricerca** senza accenti né maiuscole, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Cronologia**: 20 versioni per elemento, ripristinabili con confronto prima/dopo; 10 vecchie password.
- **Cestino** con svuotamento automatico (7, 30, 90 giorni, 1 anno o mai), **cronologia globale** delle modifiche.
- **Richiesta della password principale** per singolo elemento (come in Bitwarden), **selezione multipla** (sposta, preferiti,
  cestino), **duplicazione**, gesti: scorri a destra = copia, a sinistra = preferito, pressione prolungata = menu.
- **Più casseforti** indipendenti, ognuna con la propria password.

### Compilazione automatica
- Haiku è un **servizio di compilazione automatica Android** (Impostazioni › Compilazione automatica): in un'app o in un
  browser, tocca il campo e poi «Compila con Haiku», sblocca (biometria, PIN o password) e scegli
  il login. Le corrispondenze seguono le regole di Bitwarden (dominio, host, `androidapp://paquet`…), e
  l'associazione con un'app può essere memorizzata con un tocco.
- Il codice 2FA del login viene copiato automaticamente per il passaggio successivo.
- Dopo un accesso, Android propone di **salvare** (o aggiornare) il login in Haiku.
- Chrome: *Impostazioni › Compilazione automatica › Usa un altro servizio*.

### Autenticatore (2FA)
- Codici **TOTP** (SHA-1/256/512, 6 o 8 cifre, periodo libero), **HOTP** e **Steam Guard**, in tempo reale con
  anello del conto alla rovescia e codice successivo.
- Aggiunta tramite **scansione di un codice QR** (fotocamera o screenshot), chiave manuale o link `otpauth://`.
- Importazione dell'export di **Google Authenticator** («Trasferisci account») e di **Aegis**.

### Generatore
- Password (da 5 a 128 caratteri, minimi di cifre/simboli, caratteri ambigui esclusi, simboli a scelta),
  **passphrase** (liste di 7776 parole in francese e in inglese), **nomi utente** (parole, casuale, indirizzo
  `moi+xxxx@gmail.com`, dominio catch-all), **codici PIN**. Entropia esatta e tempo di violazione mostrati.
- Cronologia dei valori generati, cifrata nella cassaforte.

### Salute della cassaforte
Punteggio su 100 e rapporti: password **deboli**, **riutilizzate**, **vecchie** (più di un anno), **mancanti**,
**siti in http://**, **carte scadute**, e su richiesta: **violazioni** (Have I Been Pwned, k-anonimato: escono solo
5 caratteri dell'hash SHA-1) e **2FA disponibile ma non attiva** (lista 2fa.directory).

### Send (condivisione cifrata)
L'equivalente offline di Bitwarden Send: un testo o un file (15 MB) diventa una **pagina HTML autonoma** cifrata
(AES-256-GCM, PBKDF2-SHA256 con 600.000 iterazioni), con data di scadenza. Il destinatario la apre in qualsiasi
browser con la password che gli comunichi tramite un altro canale.

### Importazione / esportazione
| Formato | Importazione | Esportazione |
|---|---|---|
| Haiku cifrato (`.haiku.json`, Argon2id + AES-256-GCM, tutta la cassaforte) | ✅ | ✅ |
| Cassaforte Haiku (`.haiku`, backup o altro telefono) | ✅ | ✅ |
| Bitwarden JSON (in chiaro) | ✅ | ✅ |
| Bitwarden JSON **protetto da password** (PBKDF2 o Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` cifrato | ✅ | ✅ (si reimporta in Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Password di Apple | ✅ | ✅ (CSV del browser) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON in chiaro), Google Authenticator (QR) | ✅ | ✅ (link `otpauth://`) |
| Copia cartacea (pagina HTML da stampare, come il PDF di Keyra) | — | ✅ |

Anteprima prima dell'importazione (senza le password), cartelle create automaticamente, doppioni ignorati, promemoria per eliminare
i file in chiaro. I tipi propri di Haiku (Wi-Fi, server) fanno andata e ritorno tramite Bitwarden grazie a un campo
`haiku-type`. L'esportazione richiede di nuovo la password principale.

### Sicurezza
```text
password principale ──Argon2id (64 MiB, 3 passaggi, 4 corsie)──► KEK
KEK ──AES-256-GCM──► DEK (chiave dati casuale a 256 bit)
DEK ──AES-256-GCM──► contenuto della cassaforte (JSON compresso)
```
- La password principale non viene mai memorizzata. Cambiarla reimpacchetta la DEK senza ricifrare tutto.
- Robustezza di Argon2id regolabile: leggera (32 MiB), standard (64 MiB, come Keyra), rinforzata (128 MiB · 4 passaggi).
- **Chiave di recupero** in stile Keyra: 32 caratteri (base32 di Crockford, 150 bit + controllo), mostrata una sola
  volta; «password dimenticata» crea una nuova chiave e invalida la vecchia.
- **Biometria**: la DEK è cifrata da una chiave AES dell'**Android Keystore** utilizzabile solo dopo
  un'autenticazione biometrica forte, e invalidata se viene aggiunta una nuova impronta.
- **Codice PIN** (da 4 a 12 cifre) derivato con Argon2id, disattivato dopo 5 errori (regolabile), opzione «richiedi di nuovo la
  password principale dopo il riavvio».
- **Blocco automatico** (da immediato a 4 h, o mai), anche in background; **appunti** svuotati dopo
  30 s (regolabile) e contrassegnati come *sensibili* (nascosti nell'anteprima di Android 13+); **schermo protetto** (niente screenshot,
  anteprima nascosta nelle app recenti); backup Android della cartella dell'app disattivato; APK *release*
  (non debuggabile).
- **Backup** automatici al blocco (se la cassaforte è cambiata) in `Documents/Haiku/Sauvegardes`, 10 conservati
  (regolabile); si ripristinano come una **nuova** cassaforte, nulla viene sovrascritto.

### Personalizzazione: lo Studio e i moduli
- **9 temi** (Haïku, Sumi-e, Sakura, Neon, Matrix, Solare, Ghiacciaio, Carta, Monocromatico), modalità scura, AMOLED,
  chiara e vetro, due colori d'accento liberi (o un'armonia casuale), condivisione del tema tramite codice.
- **Sfondi animati**: aurora, costellazione (reagisce al dito), inchiostro sumi-e (una goccia a ogni tocco), petali di
  sakura, griglia retrò, onde, pioggia di codice; intensità, grana della carta, alone.
- **8 caratteri** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), arrotondamenti,
  sfocatura del vetro, densità, elenco a schede / righe / griglia, iniziali o pittogrammi.
- **Animazioni GSAP** regolabili (nessuna, delicate, normali, *pazze*): intro con il timbro 俳句, ensō tracciato a pennello sulla
  schermata di blocco e lucchetto che gira allo sblocco, rivelazione dei segreti come in una decifrazione
  (ScrambleText), esplosioni di particelle (Physics2D), elenchi che si riordinano (Flip), pannelli e righe da trascinare
  (Draggable + Inertia), schede che si inclinano in 3D, transizioni di pagina a scelta (scorrimento, zoom, rotazione 3D,
  sipario, sfocatura).
- **Moduli**: ogni funzione (2FA, generatore, salute, Send, cartelle, cronologia, cestino, importazione/esportazione,
  backup) si attiva o si disattiva; la **barra delle schede** si compone a piacere (5 al massimo, ordine trascinando); la **home**
  è fatta di widget riordinabili (haiku del giorno, ricerca, scorciatoie, preferiti, codici 2FA, punteggio di salute,
  recenti, generatore rapido, statistiche, cartelle, nuvola di tag).
- **Tavolozza dei comandi** (scorri in basso sull'intestazione, o Ctrl+K da tastiera) e scorciatoie da tastiera
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Limiti noti
- Niente sincronizzazione, organizzazioni né accesso di emergenza (sono funzioni del server Bitwarden): è
  una cassaforte locale, come Keyra. Per passare da un telefono all'altro: backup `.haiku` o esportazione cifrata.
- Le passkey di un export Bitwarden non vengono importate: Haiku non è un fornitore di passkey.
- Interfaccia solo in francese (Keyra offre 17 lingue).
- La compilazione automatica passa dalla schermata di Haiku (nessun suggerimento in linea nella tastiera): un tocco
  in più, ma nessun segreto viene mai conservato in chiaro per il sistema.
- I file `.kdbx` non vengono letti direttamente: esportali in CSV o XML da KeePassXC / KeePass.
- La stima della robustezza è offline e approssimativa.

## Compilarlo da solo (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # test del nucleo (cifratura, importazioni/esportazioni, 2FA…)
npm run android:add      # solo se la cartella android/ non esiste
npm run sync             # build web + copia nel progetto Android + impostazioni di Haiku
npm run open             # apre Android Studio (oppure: cd android && gradlew assembleRelease)
```
L'APK è firmato con la chiave di `signing/`, come quello di GitHub Actions: si installano l'uno sopra
l'altro.

## Organizzazione del codice
```text
src/core/      cifratura, cassaforte, importatori/esportatori, TOTP, generatore, robustezza, audit, Send, SSH
src/views/     schermate (home, cassaforte, scheda, modifica, 2FA, generatore, salute, Send, Studio, moduli…)
src/ui/        effetti GSAP, sfondi animati, tema, componenti, schermata di blocco
scripts/       impostazioni del progetto Android, plugin nativo Java (biometria Keystore, schermo protetto, compilazione automatica), icone
tests/         test Node (con un vero export di Keyra) e test d'interfaccia Playwright
```

## Crediti
Haiku è una riscrittura indipendente: riprende le **funzionalità** di Bitwarden e di Keyra e legge/scrive
i loro **formati di esportazione**, senza riutilizzarne il codice. Grazie a [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), alla lista di parole
[EFF](https://www.eff.org/dice) (CC BY 3.0), a [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(derivata da Lexique 3.81) e ai caratteri Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho e
JetBrains Mono (SIL Open Font License).
