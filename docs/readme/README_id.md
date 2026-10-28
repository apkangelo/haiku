<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · **Bahasa Indonesia** · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Pengelola kata sandi **offline** untuk Android: fitur-fitur [Bitwarden](https://bitwarden.com) dan
[Keyra](https://github.com/Tahir-1907/Keyra) dalam satu aplikasi seluler, dengan animasi GSAP di mana-mana dan antarmuka
yang bisa dikustomisasi sepenuhnya lewat modul. Tanpa akun, tanpa server, tanpa telemetri: brankas tidak pernah keluar dari
ponsel.

Dikemas sebagai APK dengan [Capacitor](https://capacitorjs.com/), dikompilasi otomatis oleh GitHub Actions, dan dirancang untuk
Galaxy Z Fold (terlipat: bilah tab; terbuka: rel di kiri dan item yang dibuka di kanan).

## Unduh APK

➡️ **[Unduh Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — file ini juga ada di tab
*Releases*, lengkap dengan checksum SHA-256-nya.

1. Unduh `haiku.apk` di ponsel lalu buka.
2. Izinkan instalasi dari sumber ini jika Android memintanya, lalu ketuk **Instal**.
3. Saat pertama dibuka: *Buat brankas saya* (atau *Pulihkan cadangan*).

Semua versi ditandatangani dengan **kunci yang sama** (folder `signing/`): pembaruan terpasang di atas
versi sebelumnya tanpa perlu dicopot, jadi brankas tidak hilang. Kunci ini publik di repositori: pasang Haiku
hanya dari repositori ini. Untuk kunci privat, tambahkan secret `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD`, dan
`HAIKU_KEY_PASSWORD` di *Settings › Secrets › Actions* (mengganti kunci mengharuskan pencopotan: buat
cadangan dulu).

## Apa yang dilakukan Haiku

### Brankas
- **7 jenis item**: login, kartu pembayaran (kartu 3D yang bisa dibalik untuk melihat kode keamanan), identitas, catatan
  aman, kunci SSH (pembuatan Ed25519 dalam format OpenSSH), jaringan Wi-Fi (berbagi lewat kode QR), server (menyalin
  perintah `ssh`/`sftp`).
- Beberapa **alamat per login** dengan aturan pencocokan (domain, host, persis, ekspresi reguler…),
  **kolom kustom** (teks, tersembunyi, ya/tidak), **lampiran** terenkripsi (10 MB), **catatan**.
- **Folder** (warna, urutan dengan seret), **tag** (20 per item, aturan Keyra), **favorit**.
- **Pencarian** tanpa memedulikan aksen dan huruf besar/kecil, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Riwayat**: 20 versi per item, bisa dipulihkan dengan perbandingan sebelum/sesudah; 10 kata sandi lama.
- **Tempat sampah** dengan pembersihan otomatis (7, 30, 90 hari, 1 tahun, atau tidak pernah), **riwayat global** perubahan.
- **Minta ulang kata sandi utama** per item (seperti Bitwarden), **pilih banyak** (pindahkan, favorit,
  tempat sampah), **duplikat**, gestur: geser kanan = salin, geser kiri = favorit, tekan lama = menu.
- **Beberapa brankas** independen, masing-masing dengan kata sandinya sendiri.

### Isi otomatis
- Haiku adalah **layanan isi otomatis Android** (Pengaturan › Isi otomatis): di aplikasi atau
  browser, ketuk kolomnya lalu "Isi dengan Haiku", buka kunci (biometrik, PIN, atau kata sandi) dan pilih
  login-nya. Pencocokan mengikuti aturan Bitwarden (domain, host, `androidapp://paquet`…), dan
  kaitan dengan aplikasi bisa diingat dengan sekali ketuk.
- Kode 2FA dari login disalin otomatis untuk langkah berikutnya.
- Setelah masuk, Android menawarkan untuk **menyimpan** (atau memperbarui) login di Haiku.
- Chrome: *Setelan › Isi otomatis › Gunakan layanan lain*.

### Autentikator (2FA)
- Kode **TOTP** (SHA-1/256/512, 6 atau 8 digit, periode bebas), **HOTP**, dan **Steam Guard**, langsung dengan
  cincin hitung mundur dan kode berikutnya.
- Tambah dengan **memindai kode QR** (kamera atau tangkapan layar), kunci manual, atau tautan `otpauth://`.
- Impor ekspor **Google Authenticator** ("Transfer akun") dan **Aegis**.

### Generator
- Kata sandi (5 hingga 128 karakter, jumlah minimum angka/simbol, tanpa karakter yang membingungkan, simbol sesuai pilihan),
  **frasa sandi** (daftar 7.776 kata dalam bahasa Prancis dan Inggris), **nama pengguna** (kata, acak, alamat
  `moi+xxxx@gmail.com`, domain catch-all), **kode PIN**. Entropi yang tepat dan waktu untuk membobol ditampilkan.
- Riwayat nilai yang dibuat, terenkripsi di dalam brankas.

### Kesehatan brankas
Skor dari 100 dan laporan: kata sandi **lemah**, **dipakai ulang**, **lama** (lebih dari setahun), **kosong**,
**situs http://**, **kartu kedaluwarsa**, dan sesuai permintaan: **kebocoran** (Have I Been Pwned, k-anonimitas: hanya
5 karakter dari hash SHA-1 yang dikirim) dan **2FA tersedia tapi belum aktif** (daftar 2fa.directory).

### Send (berbagi terenkripsi)
Padanan offline dari Bitwarden Send: sebuah teks atau file (15 MB) menjadi **halaman HTML mandiri** yang terenkripsi
(AES-256-GCM, PBKDF2-SHA256 600.000 iterasi), dengan tanggal kedaluwarsa. Penerima membukanya di browser
apa pun dengan kata sandi yang kamu berikan lewat saluran lain.

### Impor / ekspor
| Format | Impor | Ekspor |
|---|---|---|
| Haiku terenkripsi (`.haiku.json`, Argon2id + AES-256-GCM, seluruh brankas) | ✅ | ✅ |
| Brankas Haiku (`.haiku`, cadangan atau ponsel lain) | ✅ | ✅ |
| Bitwarden JSON (teks biasa) | ✅ | ✅ |
| Bitwarden JSON **dilindungi kata sandi** (PBKDF2 atau Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` terenkripsi | ✅ | ✅ (bisa diimpor ulang ke Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Kata Sandi Apple | ✅ | ✅ (CSV browser) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON biasa), Google Authenticator (QR) | ✅ | ✅ (tautan `otpauth://`) |
| Salinan kertas (halaman HTML untuk dicetak, seperti PDF Keyra) | — | ✅ |

Pratinjau sebelum impor (tanpa kata sandi), folder dibuat otomatis, duplikat dilewati, pengingat untuk menghapus
file teks biasa. Jenis khusus Haiku (Wi-Fi, server) tetap utuh saat bolak-balik lewat Bitwarden berkat kolom
`haiku-type`. Ekspor meminta kata sandi utama lagi.

### Keamanan
```text
kata sandi utama ──Argon2id (64 MiB, 3 putaran, 4 jalur)──► KEK
KEK ──AES-256-GCM──► DEK (kunci data acak 256 bit)
DEK ──AES-256-GCM──► isi brankas (JSON terkompresi)
```
- Kata sandi utama tidak pernah disimpan. Mengganti kata sandi hanya membungkus ulang DEK tanpa mengenkripsi ulang semuanya.
- Kekuatan Argon2id bisa diatur: ringan (32 MiB), standar (64 MiB, seperti Keyra), diperkuat (128 MiB · 4 putaran).
- **Kunci pemulihan** gaya Keyra: 32 karakter (base32 Crockford, 150 bit + pemeriksaan), hanya ditampilkan
  sekali; "lupa kata sandi" membuat kunci baru dan membatalkan kunci lama.
- **Biometrik**: DEK dienkripsi dengan kunci AES dari **Android Keystore** yang hanya bisa dipakai setelah
  autentikasi biometrik kuat, dan dibatalkan jika sidik jari baru ditambahkan.
- **Kode PIN** (4 hingga 12 digit) diturunkan dengan Argon2id, dinonaktifkan setelah 5 kesalahan (bisa diatur), opsi "minta lagi
  kata sandi utama setelah mulai ulang".
- **Kunci otomatis** (dari seketika hingga 4 jam, atau tidak pernah), juga di latar belakang; **papan klip** dikosongkan setelah
  30 dtk (bisa diatur) dan ditandai *sensitif* (disembunyikan di pratinjau Android 13+); **layar aman** (tanpa tangkapan layar,
  pratinjau disembunyikan di aplikasi terbaru); pencadangan Android untuk folder aplikasi dinonaktifkan; APK *release*
  (tidak bisa di-debug).
- **Cadangan** otomatis saat mengunci (jika brankas berubah) di `Documents/Haiku/Sauvegardes`, 10 disimpan
  (bisa diatur); cadangan dipulihkan sebagai brankas **baru**, tidak ada yang ditimpa.

### Personalisasi: Studio dan modul
- **9 tema** (Haïku, Sumi-e, Sakura, Neon, Matriks, Surya, Gletser, Kertas, Monokrom), mode gelap, AMOLED,
  terang, dan kaca, dua warna aksen bebas (atau harmoni acak), berbagi tema lewat kode.
- **Latar animasi**: aurora, rasi bintang (bereaksi pada jari), tinta sumi-e (setetes setiap sentuhan), kelopak
  sakura, kisi retro, gelombang, hujan kode; intensitas, tekstur kertas, halo.
- **8 font** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), sudut membulat,
  buram kaca, kerapatan, daftar sebagai kartu / baris / kisi, inisial atau ikon.
- **Animasi GSAP** yang bisa diatur (tidak ada, lembut, normal, *gila*): intro stempel 俳句, ensō yang digoreskan kuas di
  layar kunci dan gembok yang berputar saat membuka, rahasia muncul seperti sedang didekripsi
  (ScrambleText), ledakan partikel (Physics2D), daftar yang menata ulang dirinya (Flip), panel dan baris yang bisa diseret
  (Draggable + Inertia), kartu yang miring secara 3D, transisi halaman pilihan (geser, zoom, balik 3D,
  tirai, buram).
- **Modul**: setiap fitur (2FA, generator, kesehatan, Send, folder, riwayat, tempat sampah, impor/ekspor,
  cadangan) bisa dinyalakan atau dimatikan; **bilah tab** kamu susun sendiri (maksimal 5, urutan dengan seret); **beranda**
  terdiri dari widget yang bisa diatur ulang (haiku hari ini, pencarian, pintasan, favorit, kode 2FA, skor kesehatan,
  item terbaru, generator cepat, statistik, folder, awan tag).
- **Palet perintah** (geser ke bawah di header, atau Ctrl+K di keyboard) dan pintasan keyboard
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Keterbatasan yang diketahui
- Tidak ada sinkronisasi, organisasi, atau akses darurat (itu fitur server Bitwarden): ini adalah
  brankas lokal, seperti Keyra. Untuk pindah dari satu ponsel ke ponsel lain: cadangan `.haiku` atau ekspor terenkripsi.
- Passkey dalam ekspor Bitwarden tidak diimpor: Haiku bukan penyedia passkey.
- Antarmuka hanya dalam bahasa Prancis (Keyra menyediakan 17 bahasa).
- Isi otomatis melewati layar Haiku (tanpa saran langsung di keyboard): satu ketukan lebih banyak,
  tetapi tidak ada rahasia yang pernah disimpan sebagai teks biasa untuk sistem.
- File `.kdbx` tidak dibaca langsung: ekspor dulu ke CSV atau XML dari KeePassXC / KeePass.
- Perkiraan kekuatan dihitung secara offline dan bersifat perkiraan.

## Kompilasi sendiri (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # tes inti (enkripsi, impor/ekspor, 2FA…)
npm run android:add      # hanya jika folder android/ belum ada
npm run sync             # build web + salin ke proyek Android + pengaturan Haiku
npm run open             # membuka Android Studio (atau: cd android && gradlew assembleRelease)
```
APK ditandatangani dengan kunci di `signing/`, sama seperti hasil GitHub Actions: keduanya bisa dipasang saling
menimpa.

## Organisasi kode
```text
src/core/      enkripsi, brankas, pengimpor/pengekspor, TOTP, generator, kekuatan, audit, Send, SSH
src/views/     layar (beranda, brankas, item, edit, 2FA, generator, kesehatan, Send, Studio, modul…)
src/ui/        efek GSAP, latar animasi, tema, komponen, layar kunci
scripts/       pengaturan proyek Android, plugin Java native (biometrik Keystore, layar aman, isi otomatis), ikon
tests/         tes Node (dengan ekspor Keyra sungguhan) dan tes antarmuka Playwright
```

## Kredit
Haiku adalah penulisan ulang independen: ia meniru **fitur** Bitwarden dan Keyra serta membaca/menulis
**format ekspor** keduanya, tanpa memakai ulang kode mereka. Terima kasih kepada [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), daftar kata
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(diturunkan dari Lexique 3.81), serta font Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, dan
JetBrains Mono (SIL Open Font License).
