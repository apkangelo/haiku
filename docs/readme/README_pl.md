<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · **Polski** · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Menedżer haseł **offline** na Androida: funkcje [Bitwarden](https://bitwarden.com) i
[Keyra](https://github.com/Tahir-1907/Keyra) w jednej aplikacji mobilnej, z animacjami GSAP wszędzie i interfejsem
w pełni konfigurowalnym za pomocą modułów. Bez konta, bez serwera, bez telemetrii: sejf nigdy nie opuszcza
telefonu.

Spakowany jako APK za pomocą [Capacitor](https://capacitorjs.com/), kompilowany automatycznie przez GitHub Actions i zaprojektowany z myślą o
Galaxy Z Fold (po złożeniu: pasek kart; po rozłożeniu: pasek boczny po lewej i otwarty wpis po prawej).

## Pobierz APK

➡️ **[Pobierz Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — plik jest też w
karcie *Releases*, razem z sumą kontrolną SHA-256.

1. Pobierz `haiku.apk` na telefon i otwórz go.
2. Zezwól na instalację z tego źródła, jeśli Android o to poprosi, a potem dotknij **Zainstaluj**.
3. Przy pierwszym uruchomieniu: *Utwórz mój sejf* (lub *Przywróć kopię zapasową*).

Wszystkie wersje są podpisane **tym samym kluczem** (folder `signing/`): aktualizacja instaluje się na
poprzedniej wersji bez odinstalowywania, więc bez utraty sejfu. Ten klucz jest publiczny w repozytorium: instaluj Haiku
wyłącznie z tego repozytorium. Aby użyć prywatnego klucza, dodaj sekrety `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` i
`HAIKU_KEY_PASSWORD` w *Settings › Secrets › Actions* (zmiana klucza wymaga odinstalowania: najpierw zrób
kopię zapasową).

## Co robi Haiku

### Sejf
- **7 typów wpisów**: dane logowania, karta płatnicza (karta 3D do odwrócenia, żeby zobaczyć kod CVV), tożsamość, bezpieczna
  notatka, klucz SSH (generowanie Ed25519 w formacie OpenSSH), sieć Wi-Fi (udostępnianie kodem QR), serwer (kopiowanie
  polecenia `ssh`/`sftp`).
- Kilka **adresów na dane logowania** z regułą dopasowania (domena, host, dokładne, wyrażenie regularne…),
  **własne pola** (tekst, ukryte, tak/nie), szyfrowane **załączniki** (10 MB), **notatki**.
- **Foldery** (kolor, kolejność przeciąganiem), **tagi** (20 na wpis, zasady Keyry), **ulubione**.
- **Wyszukiwanie** bez względu na akcenty i wielkość liter, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Historia**: 20 wersji na wpis, do przywrócenia z porównaniem przed/po; 10 starych haseł.
- **Kosz** z automatycznym opróżnianiem (7, 30, 90 dni, 1 rok lub nigdy), **globalna historia** zmian.
- **Ponowne pytanie o hasło główne** dla wpisu (jak w Bitwarden), **wielokrotny wybór** (przenieś, ulubione,
  kosz), **duplikowanie**, gesty: przesunięcie w prawo = kopiuj, w lewo = ulubione, długie przytrzymanie = menu.
- **Wiele niezależnych sejfów**, każdy z własnym hasłem.

### Autouzupełnianie
- Haiku jest **usługą autouzupełniania Androida** (Ustawienia › Autouzupełnianie): w aplikacji lub
  przeglądarce dotknij pola, a potem „Wypełnij przez Haiku”, odblokuj (biometria, PIN lub hasło) i wybierz
  dane logowania. Dopasowanie stosuje zasady Bitwarden (domena, host, `androidapp://paquet`…), a
  powiązanie z aplikacją można zapamiętać jednym dotknięciem.
- Kod 2FA danych logowania jest automatycznie kopiowany do następnego kroku.
- Po zalogowaniu Android proponuje **zapisanie** (lub aktualizację) danych logowania w Haiku.
- Chrome: *Ustawienia › Autouzupełnianie › Użyj innej usługi*.

### Uwierzytelniacz (2FA)
- Kody **TOTP** (SHA-1/256/512, 6 lub 8 cyfr, dowolny okres), **HOTP** i **Steam Guard**, na żywo z
  pierścieniem odliczania i kolejnym kodem.
- Dodawanie przez **skanowanie kodu QR** (aparat lub zrzut ekranu), ręczny klucz lub link `otpauth://`.
- Import eksportu **Google Authenticator** („Przenieś konta”) i **Aegis**.

### Generator
- Hasła (od 5 do 128 znaków, minimalna liczba cyfr/symboli, bez mylących znaków, wybór symboli),
  **frazy hasłowe** (listy 7776 słów po francusku i angielsku), **nazwy użytkownika** (słowa, losowe, adres
  `moi+xxxx@gmail.com`, domena catch-all), **kody PIN**. Wyświetlana jest dokładna entropia i czas złamania.
- Historia wygenerowanych wartości, zaszyfrowana w sejfie.

### Kondycja sejfu
Wynik na 100 i raporty: hasła **słabe**, **powtórzone**, **stare** (ponad rok), **brakujące**,
**strony http://**, **wygasłe karty**, a na żądanie: **wycieki** (Have I Been Pwned, k-anonimowość: wysyłanych jest tylko
5 znaków skrótu SHA-1) i **dostępne, ale nieaktywne 2FA** (lista 2fa.directory).

### Send (szyfrowane udostępnianie)
Offline’owy odpowiednik Bitwarden Send: tekst lub plik (15 MB) staje się zaszyfrowaną **samodzielną stroną HTML**
(AES-256-GCM, PBKDF2-SHA256 z 600 000 iteracji), z datą wygaśnięcia. Odbiorca otwiera ją w dowolnej
przeglądarce hasłem, które przekazujesz mu innym kanałem.

### Import / eksport
| Format | Import | Eksport |
|---|---|---|
| Szyfrowane Haiku (`.haiku.json`, Argon2id + AES-256-GCM, cały sejf) | ✅ | ✅ |
| Sejf Haiku (`.haiku`, kopia zapasowa lub inny telefon) | ✅ | ✅ |
| Bitwarden JSON (jawny) | ✅ | ✅ |
| Bitwarden JSON **chroniony hasłem** (PBKDF2 lub Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Szyfrowany Keyra `.mcfexport` | ✅ | ✅ (da się zaimportować z powrotem do Keyry) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Hasła Apple | ✅ | ✅ (CSV przeglądarki) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (jawny JSON), Google Authenticator (QR) | ✅ | ✅ (linki `otpauth://`) |
| Kopia papierowa (strona HTML do wydruku, jak PDF Keyry) | — | ✅ |

Podgląd przed importem (bez haseł), foldery tworzone automatycznie, pomijanie duplikatów, przypomnienie o usunięciu
jawnych plików. Typy właściwe dla Haiku (Wi-Fi, serwer) przechodzą tam i z powrotem przez Bitwarden dzięki polu
`haiku-type`. Eksport ponownie pyta o hasło główne.

### Bezpieczeństwo
```text
hasło główne ──Argon2id (64 MiB, 3 przebiegi, 4 tory)──► KEK
KEK ──AES-256-GCM──► DEK (losowy 256-bitowy klucz danych)
DEK ──AES-256-GCM──► zawartość sejfu (skompresowany JSON)
```
- Hasło główne nigdy nie jest przechowywane. Zmiana hasła jedynie ponownie opakowuje DEK, bez szyfrowania wszystkiego od nowa.
- Regulowana odporność Argon2id: lekka (32 MiB), standardowa (64 MiB, jak w Keyrze), wzmocniona (128 MiB · 4 przebiegi).
- **Klucz odzyskiwania** w stylu Keyry: 32 znaki (base32 Crockforda, 150 bitów + suma kontrolna), wyświetlany tylko
  raz; „nie pamiętam hasła” tworzy nowy klucz i unieważnia stary.
- **Biometria**: DEK jest szyfrowany kluczem AES z **Android Keystore**, którego można użyć tylko po
  silnym uwierzytelnieniu biometrycznym; klucz traci ważność po dodaniu nowego odcisku palca.
- **Kod PIN** (od 4 do 12 cyfr) wyprowadzany przez Argon2id, wyłączany po 5 błędach (regulowane), opcja „po ponownym uruchomieniu
  znów pytaj o hasło główne”.
- **Automatyczna blokada** (od natychmiastowej do 4 h lub nigdy), także w tle; **schowek** czyszczony po
  30 s (regulowane) i oznaczany jako *poufny* (ukryty w podglądzie Androida 13+); **bezpieczny ekran** (bez zrzutów ekranu,
  podgląd ukryty w ostatnich aplikacjach); kopia zapasowa Androida folderu aplikacji wyłączona; APK w wersji *release*
  (bez debugowania).
- Automatyczne **kopie zapasowe** przy blokadzie (jeśli sejf się zmienił) w `Documents/Haiku/Sauvegardes`, przechowywanych 10
  (regulowane); przywracają się jako **nowy** sejf, nic nie jest nadpisywane.

### Personalizacja: Studio i moduły
- **9 motywów** (Haïku, Sumi-e, Sakura, Neon, Matrix, Słońce, Lodowiec, Papier, Monochromatyczny), tryby ciemny, AMOLED,
  jasny i szkło, dwa dowolne kolory akcentu (lub losowa harmonia), udostępnianie motywu kodem.
- **Animowane tła**: zorza, gwiazdozbiór (reaguje na palec), tusz sumi-e (kropla przy każdym dotknięciu), płatki
  sakury, siatka retro, fale, deszcz kodu; intensywność, ziarno papieru, poświata.
- **8 czcionek** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), zaokrąglenia,
  rozmycie szkła, gęstość, lista jako karty / wiersze / siatka, inicjały lub ikony.
- Regulowane **animacje GSAP** (brak, łagodne, normalne, *szalone*): intro z pieczęcią 俳句, ensō malowane pędzlem na
  ekranie blokady i zamek, który obraca się przy odblokowaniu, odsłanianie sekretów jak przy deszyfrowaniu
  (ScrambleText), wybuchy cząsteczek (Physics2D), listy, które same się przestawiają (Flip), panele i wiersze do przeciągania
  (Draggable + Inertia), karty przechylające się w 3D, przejścia stron do wyboru (przesunięcie, zoom, obrót 3D,
  kurtyna, rozmycie).
- **Moduły**: każdą funkcję (2FA, generator, kondycja, Send, foldery, historia, kosz, import/eksport,
  kopie zapasowe) można włączyć lub wyłączyć; **pasek kart** komponujesz sam (maksymalnie 5, kolejność przeciąganiem); **ekran główny**
  składa się z widżetów, które można przestawiać (haiku dnia, wyszukiwanie, skróty, ulubione, kody 2FA, wynik kondycji,
  ostatnie, szybki generator, statystyki, foldery, chmura tagów).
- **Paleta poleceń** (przesunięcie w dół na nagłówku lub Ctrl+K na klawiaturze) i skróty klawiszowe
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Znane ograniczenia
- Brak synchronizacji, organizacji i dostępu awaryjnego (to funkcje serwera Bitwarden): to
  lokalny sejf, jak Keyra. Aby przejść z jednego telefonu na drugi: kopia zapasowa `.haiku` lub szyfrowany eksport.
- Klucze dostępu (passkeys) z eksportu Bitwarden nie są importowane: Haiku nie jest dostawcą kluczy dostępu.
- Interfejs tylko po francusku (Keyra oferuje 17 języków).
- Autouzupełnianie przechodzi przez ekran Haiku (bez podpowiedzi wbudowanych w klawiaturę): o jedno dotknięcie więcej,
  ale żaden sekret nigdy nie jest przechowywany jawnie dla systemu.
- Pliki `.kdbx` nie są odczytywane bezpośrednio: wyeksportuj je do CSV lub XML z KeePassXC / KeePass.
- Ocena siły hasła jest liczona offline i przybliżona.

## Samodzielna kompilacja (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # testy rdzenia (szyfrowanie, importy/eksporty, 2FA…)
npm run android:add      # tylko jeśli folder android/ nie istnieje
npm run sync             # build web + kopiowanie do projektu Android + ustawienia Haiku
npm run open             # otwiera Android Studio (lub: cd android && gradlew assembleRelease)
```
APK jest podpisywany kluczem z `signing/`, tak samo jak ten z GitHub Actions: oba instalują się jeden na
drugim.

## Organizacja kodu
```text
src/core/      szyfrowanie, sejf, importery/eksportery, TOTP, generator, siła haseł, audyt, Send, SSH
src/views/     ekrany (główny, sejf, wpis, edycja, 2FA, generator, kondycja, Send, Studio, moduły…)
src/ui/        efekty GSAP, animowane tła, motyw, komponenty, ekran blokady
scripts/       ustawienia projektu Android, natywna wtyczka Java (biometria Keystore, bezpieczny ekran, autouzupełnianie), ikony
tests/         testy Node (z prawdziwym eksportem Keyry) i test interfejsu w Playwright
```

## Podziękowania
Haiku to niezależnie napisana od nowa aplikacja: odtwarza **funkcje** Bitwarden i Keyry oraz czyta/zapisuje
ich **formaty eksportu**, nie wykorzystując ich kodu. Dziękuję [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), liście słów
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(wywodzącej się z Lexique 3.81) oraz czcionkom Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho i
JetBrains Mono (SIL Open Font License).
