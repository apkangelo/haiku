<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · **Deutsch** · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

**Offline**-Passwortmanager für Android: die Funktionen von [Bitwarden](https://bitwarden.com) und
[Keyra](https://github.com/Tahir-1907/Keyra) in einer mobilen App, mit GSAP-Animationen überall und einer Oberfläche,
die sich über Module vollständig anpassen lässt. Kein Konto, kein Server, keine Telemetrie: Der Tresor verlässt nie
das Handy.

Mit [Capacitor](https://capacitorjs.com/) als APK verpackt, automatisch von GitHub Actions gebaut und für das
Galaxy Z Fold gedacht (zugeklappt: Tab-Leiste; aufgeklappt: Leiste links und geöffneter Eintrag rechts).

## APK herunterladen

➡️ **[Haiku herunterladen (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — die Datei liegt auch im
Tab *Releases*, mit ihrer SHA-256-Prüfsumme.

1. Lade `haiku.apk` aufs Handy herunter und öffne sie.
2. Erlaube die Installation aus dieser Quelle, falls Android fragt, und tippe dann auf **Installieren**.
3. Beim ersten Start: *Meinen Tresor erstellen* (oder *Sicherung wiederherstellen*).

Alle Versionen sind mit **demselben Schlüssel** signiert (Ordner `signing/`): Ein Update wird ohne Deinstallation über die
vorherige Version installiert, der Tresor geht also nicht verloren. Dieser Schlüssel ist im Repository öffentlich: Installiere Haiku
nur aus diesem Repository. Für einen privaten Schlüssel lege die Secrets `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` und
`HAIKU_KEY_PASSWORD` unter *Settings › Secrets › Actions* an (ein Schlüsselwechsel erfordert eine Deinstallation: Mach vorher eine
Sicherung).

## Was Haiku kann

### Tresor
- **7 Eintragstypen**: Zugangsdaten, Zahlungskarte (3D-Karte zum Umdrehen, um die Prüfnummer zu sehen), Identität, sichere
  Notiz, SSH-Schlüssel (Ed25519-Erzeugung im OpenSSH-Format), WLAN-Netz (Teilen per QR-Code), Server (kopiert den
  Befehl `ssh`/`sftp`).
- Mehrere **Adressen pro Zugang** mit Erkennungsregel (Domain, Host, exakt, regulärer Ausdruck…),
  **eigene Felder** (Text, verborgen, ja/nein), verschlüsselte **Anhänge** (10 MB), **Notizen**.
- **Ordner** (Farbe, Reihenfolge per Ziehen), **Tags** (20 pro Eintrag, Regeln von Keyra), **Favoriten**.
- **Suche** ohne Akzente und Groß-/Kleinschreibung, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Verlauf**: 20 Versionen pro Eintrag, wiederherstellbar mit Vorher-/Nachher-Vergleich; 10 alte Passwörter.
- **Papierkorb** mit automatischer Leerung (7, 30, 90 Tage, 1 Jahr oder nie), **globaler Verlauf** der Änderungen.
- **Erneute Abfrage des Master-Passworts** pro Eintrag (wie bei Bitwarden), **Mehrfachauswahl** (verschieben, Favoriten,
  Papierkorb), **Duplizieren**, Gesten: nach rechts wischen = kopieren, nach links = Favorit, lange drücken = Menü.
- **Mehrere unabhängige Tresore**, jeder mit eigenem Passwort.

### Automatisches Ausfüllen
- Haiku ist ein **Android-Autofill-Dienst** (Einstellungen › Automatisches Ausfüllen): In einer App oder einem
  Browser tippst du auf das Feld, dann auf „Mit Haiku ausfüllen“, entsperrst (Biometrie, PIN oder Passwort) und wählst
  den Zugang. Die Zuordnung folgt den Regeln von Bitwarden (Domain, Host, `androidapp://paquet`…), und
  die Verknüpfung mit einer App lässt sich mit einem Tippen merken.
- Der 2FA-Code des Zugangs wird für den nächsten Schritt automatisch kopiert.
- Nach einer Anmeldung bietet Android an, den Zugang in Haiku zu **speichern** (oder zu aktualisieren).
- Chrome: *Einstellungen › Automatisches Ausfüllen › Anderen Dienst verwenden*.

### Authenticator (2FA)
- **TOTP**-Codes (SHA-1/256/512, 6 oder 8 Ziffern, freie Periode), **HOTP** und **Steam Guard**, live mit
  Countdown-Ring und nächstem Code.
- Hinzufügen per **QR-Code-Scan** (Kamera oder Screenshot), manuellem Schlüssel oder `otpauth://`-Link.
- Import des **Google Authenticator**-Exports („Konten übertragen“) und von **Aegis**.

### Generator
- Passwörter (5 bis 128 Zeichen, Mindestanzahl an Ziffern/Symbolen, ohne verwechselbare Zeichen, Symbole nach Wahl),
  **Passphrasen** (Listen mit 7.776 Wörtern auf Französisch und Englisch), **Benutzernamen** (Wörter, zufällig, Adresse
  `moi+xxxx@gmail.com`, Catch-all-Domain), **PIN-Codes**. Genaue Entropie und Knackzeit werden angezeigt.
- Verlauf der erzeugten Werte, verschlüsselt im Tresor.

### Tresor-Zustand
Punktzahl von 100 und Berichte: **schwache**, **mehrfach verwendete**, **alte** (über ein Jahr) und **fehlende** Passwörter,
**http://-Seiten**, **abgelaufene Karten** und auf Anfrage: **Datenlecks** (Have I Been Pwned, k-Anonymität: Es werden nur
5 Zeichen des SHA-1-Hashs gesendet) und **verfügbare, aber nicht aktivierte 2FA** (Liste von 2fa.directory).

### Send (verschlüsseltes Teilen)
Das Offline-Gegenstück zu Bitwarden Send: Ein Text oder eine Datei (15 MB) wird zu einer verschlüsselten **eigenständigen HTML-Seite**
(AES-256-GCM, PBKDF2-SHA256 mit 600.000 Iterationen), mit Ablaufdatum. Der Empfänger öffnet sie in jedem
Browser mit dem Passwort, das du ihm über einen anderen Kanal gibst.

### Import / Export
| Format | Import | Export |
|---|---|---|
| Verschlüsseltes Haiku (`.haiku.json`, Argon2id + AES-256-GCM, gesamter Tresor) | ✅ | ✅ |
| Haiku-Tresor (`.haiku`, Sicherung oder anderes Handy) | ✅ | ✅ |
| Bitwarden JSON (Klartext) | ✅ | ✅ |
| **Passwortgeschütztes** Bitwarden JSON (PBKDF2 oder Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Verschlüsseltes Keyra `.mcfexport` | ✅ | ✅ (lässt sich wieder in Keyra importieren) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Apple Passwörter | ✅ | ✅ (Browser-CSV) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (Klartext-JSON), Google Authenticator (QR) | ✅ | ✅ (`otpauth://`-Links) |
| Papierkopie (HTML-Seite zum Drucken, wie das PDF von Keyra) | — | ✅ |

Vorschau vor dem Import (ohne die Passwörter), automatisch angelegte Ordner, übersprungene Duplikate, Erinnerung, die
Klartextdateien zu löschen. Die Haiku-eigenen Typen (WLAN, Server) überstehen dank eines Felds `haiku-type`
den Hin- und Rückweg über Bitwarden. Der Export fragt erneut nach dem Master-Passwort.

### Sicherheit
```text
Master-Passwort ──Argon2id (64 MiB, 3 Durchläufe, 4 Lanes)──► KEK
KEK ──AES-256-GCM──► DEK (zufälliger 256-Bit-Datenschlüssel)
DEK ──AES-256-GCM──► Tresorinhalt (komprimiertes JSON)
```
- Das Master-Passwort wird nie gespeichert. Ein Passwortwechsel verpackt den DEK neu, ohne alles neu zu verschlüsseln.
- Einstellbare Argon2id-Stärke: leicht (32 MiB), Standard (64 MiB, wie Keyra), verstärkt (128 MiB · 4 Durchläufe).
- **Wiederherstellungsschlüssel** wie bei Keyra: 32 Zeichen (Crockford-Base32, 150 Bit + Prüfsumme), wird nur
  einmal angezeigt; „Passwort vergessen“ erzeugt einen neuen Schlüssel und macht den alten ungültig.
- **Biometrie**: Der DEK wird mit einem AES-Schlüssel aus dem **Android Keystore** verschlüsselt, der nur nach einer
  starken biometrischen Authentifizierung nutzbar ist und ungültig wird, wenn ein neuer Fingerabdruck hinzukommt.
- **PIN-Code** (4 bis 12 Ziffern), per Argon2id abgeleitet, nach 5 Fehlversuchen deaktiviert (einstellbar), Option „nach einem Neustart
  erneut nach dem Master-Passwort fragen“.
- **Automatische Sperre** (sofort bis 4 Std. oder nie), auch im Hintergrund; **Zwischenablage** wird nach
  30 s geleert (einstellbar) und als *vertraulich* markiert (in der Vorschau von Android 13+ verborgen); **sicherer Bildschirm** (keine Screenshots,
  Vorschau in den letzten Apps verborgen); Android-Sicherung des App-Ordners deaktiviert; *Release*-APK
  (nicht debugbar).
- Automatische **Sicherungen** beim Sperren (wenn sich der Tresor geändert hat) in `Documents/Haiku/Sauvegardes`, 10 werden aufbewahrt
  (einstellbar); sie werden als **neuer** Tresor wiederhergestellt, nichts wird überschrieben.

### Anpassung: das Studio und die Module
- **9 Themes** (Haïku, Sumi-e, Sakura, Neon, Matrix, Solar, Gletscher, Papier, Monochrom), Modi dunkel, AMOLED,
  hell und Glas, zwei frei wählbare Akzentfarben (oder eine zufällige Harmonie), Teilen des Themes per Code.
- **Animierte Hintergründe**: Polarlicht, Sternbild (reagiert auf den Finger), Sumi-e-Tinte (ein Tropfen bei jeder Berührung), Sakura-
  Blüten, Retro-Gitter, Wellen, Code-Regen; Intensität, Papierkorn, Lichthof.
- **8 Schriften** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), Eckenrundung,
  Glasunschärfe, Dichte, Liste als Karten / Zeilen / Raster, Initialen oder Symbole.
- Einstellbare **GSAP-Animationen** (keine, sanft, normal, *verrückt*): Intro mit 俳句-Stempel, mit dem Pinsel gezeichnetes Ensō auf
  dem Sperrbildschirm und ein Schloss, das sich beim Entsperren dreht, Enthüllung der Geheimnisse wie beim Entschlüsseln
  (ScrambleText), Partikelexplosionen (Physics2D), Listen, die sich neu ordnen (Flip), ziehbare Panels und Zeilen
  (Draggable + Inertia), Karten, die sich in 3D neigen, Seitenübergänge nach Wahl (Schieben, Zoom, 3D-Drehung,
  Vorhang, Unschärfe).
- **Module**: Jede Funktion (2FA, Generator, Zustand, Send, Ordner, Verlauf, Papierkorb, Import/Export,
  Sicherungen) lässt sich ein- oder ausschalten; die **Tab-Leiste** stellst du selbst zusammen (höchstens 5, Reihenfolge per Ziehen); die **Startseite**
  besteht aus anordenbaren Widgets (Haiku des Tages, Suche, Verknüpfungen, Favoriten, 2FA-Codes, Zustandswert,
  zuletzt verwendet, Schnellgenerator, Statistiken, Ordner, Tag-Wolke).
- **Befehlspalette** (auf der Kopfzeile nach unten wischen oder Strg+K auf der Tastatur) und Tastenkürzel
  (Strg+N, Strg+L, Strg+G, Strg+F, Strg+,).

## Bekannte Einschränkungen
- Keine Synchronisierung, keine Organisationen und kein Notfallzugriff (das sind Funktionen des Bitwarden-Servers): Es ist
  ein lokaler Tresor, wie Keyra. Um von einem Handy aufs andere zu wechseln: `.haiku`-Sicherung oder verschlüsselter Export.
- Passkeys aus einem Bitwarden-Export werden nicht importiert: Haiku ist kein Passkey-Anbieter.
- Oberfläche nur auf Französisch (Keyra bietet 17 Sprachen).
- Das automatische Ausfüllen läuft über den Bildschirm von Haiku (keine Inline-Vorschläge in der Tastatur): ein Tippen mehr,
  aber kein Geheimnis wird jemals im Klartext für das System aufbewahrt.
- `.kdbx`-Dateien werden nicht direkt gelesen: Exportiere sie aus KeePassXC / KeePass als CSV oder XML.
- Die Stärkeschätzung erfolgt offline und ist ungefähr.

## Selbst kompilieren (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # Tests des Kerns (Verschlüsselung, Importe/Exporte, 2FA…)
npm run android:add      # nur, wenn der Ordner android/ noch nicht existiert
npm run sync             # Web-Build + Kopie ins Android-Projekt + Haiku-Einstellungen
npm run open             # öffnet Android Studio (oder: cd android && gradlew assembleRelease)
```
Die APK wird mit dem Schlüssel aus `signing/` signiert, genau wie die von GitHub Actions: Beide lassen sich übereinander
installieren.

## Aufbau des Codes
```text
src/core/      Verschlüsselung, Tresor, Importer/Exporter, TOTP, Generator, Stärke, Audit, Send, SSH
src/views/     Bildschirme (Start, Tresor, Eintrag, Bearbeiten, 2FA, Generator, Zustand, Send, Studio, Module…)
src/ui/        GSAP-Effekte, animierte Hintergründe, Theme, Komponenten, Sperrbildschirm
scripts/       Einstellungen des Android-Projekts, natives Java-Plugin (Keystore-Biometrie, sicherer Bildschirm, Autofill), Symbole
tests/         Node-Tests (mit einem echten Keyra-Export) und ein Playwright-Oberflächentest
```

## Danksagungen
Haiku ist eine eigenständige Neuentwicklung: Es übernimmt die **Funktionen** von Bitwarden und Keyra und liest/schreibt
deren **Exportformate**, ohne ihren Code wiederzuverwenden. Danke an [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), die Wortliste von
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(abgeleitet von Lexique 3.81) sowie an die Schriften Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho und
JetBrains Mono (SIL Open Font License).
