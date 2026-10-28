<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · **日本語** · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Android 用の**オフライン**パスワードマネージャー：[Bitwarden](https://bitwarden.com) と
[Keyra](https://github.com/Tahir-1907/Keyra) の機能を 1 つのモバイルアプリにまとめ、あらゆる場所に GSAP アニメーションを使い、インターフェースは
モジュール単位で完全にカスタマイズできます。アカウントなし、サーバーなし、テレメトリーなし：保管庫が
スマホの外に出ることはありません。

[Capacitor](https://capacitorjs.com/) で APK 化し、GitHub Actions で自動ビルド。
Galaxy Z Fold 向けに設計しています（折りたたみ時：タブバー、展開時：左にレール、右に開いた項目）。

## APK のダウンロード

➡️ **[Haiku をダウンロード（`haiku.apk`）](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — このファイルは
*Releases* タブにも SHA-256 ハッシュ付きで置いてあります。

1. スマホで `haiku.apk` をダウンロードして開きます。
2. Android に求められたらこの提供元からのインストールを許可し、**インストール**をタップします。
3. 初回起動時：*保管庫を作成*（または*バックアップを復元*）。

すべてのバージョンは**同じ鍵**（`signing/` フォルダー）で署名されています：アップデートはアンインストールせずに
前のバージョンに上書きでき、保管庫も失われません。この鍵はリポジトリで公開されているので、Haiku は
このリポジトリからのみインストールしてください。非公開の鍵を使う場合は、*Settings › Secrets › Actions* にシークレット `HAIKU_KEYSTORE_B64`、`HAIKU_KEYSTORE_PASSWORD`、
`HAIKU_KEY_PASSWORD` を追加します（鍵を変えるとアンインストールが必要になるため、先に
バックアップを取ってください）。

## Haiku でできること

### 保管庫
- **7 種類の項目**：ログイン、決済カード（裏返してセキュリティコードを見られる 3D カード）、ID、セキュア
  メモ、SSH 鍵（OpenSSH 形式の Ed25519 鍵を生成）、Wi-Fi ネットワーク（QR コードで共有）、サーバー（
  `ssh`/`sftp` コマンドをコピー）。
- ログインごとに複数の **URI** と一致ルール（ドメイン、ホスト、完全一致、正規表現……）、
  **カスタムフィールド**（テキスト、非表示、はい/いいえ）、暗号化された**添付ファイル**（10 MB）、**メモ**。
- **フォルダー**（色、ドラッグで並べ替え）、**タグ**（項目ごとに 20 個、Keyra のルール）、**お気に入り**。
- アクセントや大文字小文字を区別しない**検索**、`#étiquette`、`#"deux mots"`、`type:carte`、`is:fav`、`has:totp`、`in:dossier`。
- **履歴**：項目ごとに 20 バージョン、変更前後を比較して復元可能。過去のパスワードを 10 件保持。
- 自動で空になる**ゴミ箱**（7、30、90 日、1 年、または無期限）、変更の**全体履歴**。
- 項目ごとの**マスターパスワード再入力**（Bitwarden と同様）、**複数選択**（移動、お気に入り、
  ゴミ箱）、**複製**、ジェスチャー：右スワイプ = コピー、左スワイプ = お気に入り、長押し = メニュー。
- 独立した**複数の保管庫**、それぞれに専用のパスワード。

### 自動入力
- Haiku は **Android の自動入力サービス**です（設定 › 自動入力）：アプリや
  ブラウザで入力欄をタップし、「Haiku で入力」を選んでロックを解除（生体認証、PIN、またはパスワード）し、
  ログインを選びます。一致判定は Bitwarden のルールに従い（ドメイン、ホスト、`androidapp://paquet`……）、
  アプリとの関連付けはワンタップで記憶できます。
- ログインの 2FA コードは次のステップのために自動でコピーされます。
- サインイン後、Android がログインを Haiku に**保存**（または更新）するよう提案します。
- Chrome：*設定 › 自動入力 › 別のサービスを使用*。

### 認証システム（2FA）
- **TOTP** コード（SHA-1/256/512、6 桁または 8 桁、任意の周期）、**HOTP**、**Steam Guard** をリアルタイム表示し、
  カウントダウンのリングと次のコードも表示。
- **QR コードのスキャン**（カメラまたはスクリーンショット）、手動の鍵入力、`otpauth://` リンクで追加。
- **Google Authenticator** のエクスポート（「アカウントを移行」）と **Aegis** からのインポート。

### ジェネレーター
- パスワード（5〜128 文字、数字/記号の最小数、紛らわしい文字の除外、使う記号の選択）、
  **パスフレーズ**（フランス語と英語の 7,776 語リスト）、**ユーザー名**（単語、ランダム、アドレス
  `moi+xxxx@gmail.com`、キャッチオールドメイン）、**PIN コード**。正確なエントロピーと解読にかかる時間を表示。
- 生成した値の履歴は保管庫内に暗号化して保存。

### 保管庫の健全性
100 点満点のスコアとレポート：**弱い**、**使い回し**、**古い**（1 年以上）、**未設定**のパスワード、
**http:// のサイト**、**期限切れのカード**、さらに必要に応じて：**漏えい**（Have I Been Pwned、k-匿名性：送信されるのは
SHA-1 ハッシュの 5 文字だけ）と**利用可能なのに無効な 2FA**（2fa.directory のリスト）。

### Send（暗号化共有）
Bitwarden Send のオフライン版：テキストやファイル（15 MB）が暗号化された**自己完結型の HTML ページ**になります
（AES-256-GCM、PBKDF2-SHA256 600,000 回反復）、有効期限付き。受信者は、別の手段で伝えたパスワードを使って
どのブラウザでも開けます。

### インポート / エクスポート
| 形式 | インポート | エクスポート |
|---|---|---|
| 暗号化された Haiku（`.haiku.json`、Argon2id + AES-256-GCM、保管庫全体） | ✅ | ✅ |
| Haiku 保管庫（`.haiku`、バックアップまたは別のスマホ） | ✅ | ✅ |
| Bitwarden JSON（平文） | ✅ | ✅ |
| **パスワード保護された** Bitwarden JSON（PBKDF2 または Argon2id） | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| 暗号化された Keyra `.mcfexport` | ✅ | ✅（Keyra に再インポート可能） |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV、KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera、Firefox、Safari / Apple パスワード | ✅ | ✅（ブラウザ CSV） |
| LastPass、1Password、Dashlane、Proton Pass、NordPass（CSV） | ✅ | — |
| Aegis（平文 JSON）、Google Authenticator（QR） | ✅ | ✅（`otpauth://` リンク） |
| 紙のコピー（印刷用 HTML ページ、Keyra の PDF のようなもの） | — | ✅ |

インポート前のプレビュー（パスワードは表示しない）、フォルダーの自動作成、重複のスキップ、平文ファイルを
削除するようリマインド。Haiku 独自の種類（Wi-Fi、サーバー）は `haiku-type` フィールドのおかげで Bitwarden を経由しても
失われません。エクスポート時にはマスターパスワードを再度求めます。

### セキュリティ
```text
マスターパスワード ──Argon2id（64 MiB、3 パス、4 レーン）──► KEK
KEK ──AES-256-GCM──► DEK（ランダムな 256 ビットのデータ鍵）
DEK ──AES-256-GCM──► 保管庫の中身（圧縮 JSON）
```
- マスターパスワードは決して保存されません。パスワードを変えても DEK を包み直すだけで、全体を再暗号化する必要はありません。
- Argon2id の強度は調整可能：軽量（32 MiB）、標準（64 MiB、Keyra と同じ）、強化（128 MiB · 4 パス）。
- Keyra 風の**回復キー**：32 文字（Crockford base32、150 ビット + チェック）、表示は
  1 回だけ。「パスワードを忘れた」で新しいキーを作ると古いキーは無効になります。
- **生体認証**：DEK は **Android Keystore** の AES 鍵で暗号化され、この鍵は
  強力な生体認証のあとでしか使えず、新しい指紋が追加されると無効になります。
- **PIN コード**（4〜12 桁）は Argon2id で派生、5 回失敗すると無効化（調整可能）、「再起動後に
  マスターパスワードを再度求める」オプション。
- **自動ロック**（即時から 4 時間、または無効）、バックグラウンドでも作動。**クリップボード**は
  30 秒後に消去（調整可能）し、*機密*として扱う（Android 13 以降のプレビューで非表示）。**セキュアな画面**（スクリーンショット不可、
  最近使ったアプリでプレビューを非表示）。アプリのフォルダーの Android バックアップを無効化。*release* 版の APK
  （デバッグ不可）。
- ロック時の自動**バックアップ**（保管庫に変更があった場合）を `Documents/Haiku/Sauvegardes` に保存、10 件保持
  （調整可能）。バックアップは**新しい**保管庫として復元され、何も上書きされません。

### カスタマイズ：スタジオとモジュール
- **9 種類のテーマ**（Haïku、Sumi-e、Sakura、ネオン、マトリックス、ソーラー、グレイシャー、ペーパー、モノクローム）、ダーク、AMOLED、
  ライト、ガラスの各モード、2 つの自由なアクセントカラー（またはランダムな配色）、コードでのテーマ共有。
- **動く背景**：オーロラ、星座（指に反応）、墨（タッチのたびに一滴）、桜の
  花びら、レトロなグリッド、波紋、コードの雨。強さ、紙の質感、光の輪。
- **8 種類のフォント**（Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho、JetBrains Mono）、角の丸み、
  ガラスのぼかし、密度、カード / 行 / グリッドのリスト表示、頭文字またはアイコン。
- 調整可能な **GSAP アニメーション**（なし、控えめ、標準、*ド派手*）：俳句の判子のイントロ、ロック画面に筆で描かれる
  円相とロック解除で回る錠前、解読するように現れる秘密情報
  （ScrambleText）、パーティクルの噴出（Physics2D）、並べ替わるリスト（Flip）、ドラッグできるシートや行
  （Draggable + Inertia）、3D で傾くカード、選べるページ遷移（スライド、ズーム、3D フリップ、
  カーテン、ぼかし）。
- **モジュール**：各機能（2FA、ジェネレーター、健全性、Send、フォルダー、履歴、ゴミ箱、インポート/エクスポート、
  バックアップ）をオン / オフでき、**タブバー**は自由に構成（最大 5 つ、ドラッグで並べ替え）。**ホーム**は
  並べ替え可能なウィジェットで構成（今日の俳句、検索、ショートカット、お気に入り、2FA コード、健全性スコア、
  最近の項目、クイックジェネレーター、統計、フォルダー、タグクラウド）。
- **コマンドパレット**（ヘッダーで下にスワイプ、またはキーボードで Ctrl+K）とキーボードショートカット
  （Ctrl+N、Ctrl+L、Ctrl+G、Ctrl+F、Ctrl+,）。

## 既知の制限
- 同期、組織、緊急アクセスはありません（これらは Bitwarden サーバーの機能です）：Keyra と同じく
  ローカルの保管庫です。スマホを替えるときは `.haiku` バックアップか暗号化エクスポートを使います。
- Bitwarden のエクスポートに含まれるパスキーはインポートされません：Haiku はパスキープロバイダーではありません。
- インターフェースはフランス語のみです（Keyra は 17 言語に対応）。
- 自動入力は Haiku の画面を経由します（キーボード内のインライン候補はなし）：1 タップ増えますが、
  システムに秘密が平文で残ることはありません。
- `.kdbx` ファイルは直接読み込めません：KeePassXC / KeePass から CSV か XML にエクスポートしてください。
- 強度の推定はオフラインで行う近似値です。

## 自分でビルドする（Windows、Android Studio、JDK 21）
```bash
npm install
npm test                 # コアのテスト（暗号化、インポート/エクスポート、2FA……）
npm run android:add      # android/ フォルダーがない場合のみ
npm run sync             # Web ビルド + Android プロジェクトへのコピー + Haiku の設定
npm run open             # Android Studio を開く（または：cd android && gradlew assembleRelease）
```
APK は GitHub Actions のものと同じく `signing/` の鍵で署名されるので、互いに上書き
インストールできます。

## コードの構成
```text
src/core/      暗号化、保管庫、インポーター/エクスポーター、TOTP、ジェネレーター、強度、監査、Send、SSH
src/views/     画面（ホーム、保管庫、項目、編集、2FA、ジェネレーター、健全性、Send、スタジオ、モジュール……）
src/ui/        GSAP エフェクト、動く背景、テーマ、コンポーネント、ロック画面
scripts/       Android プロジェクトの設定、ネイティブ Java プラグイン（Keystore 生体認証、セキュア画面、自動入力）、アイコン
tests/         Node のテスト（本物の Keyra エクスポートを使用）と Playwright のインターフェーステスト
```

## クレジット
Haiku は独自に書き直したものです：Bitwarden と Keyra の**機能**を再現し、
それらの**エクスポート形式**を読み書きしますが、コードは流用していません。[GSAP](https://gsap.com)（Webflow）、[Lucide](https://lucide.dev)
（ISC）、[hash-wasm](https://github.com/Daninet/hash-wasm)（MIT）、[jsQR](https://github.com/cozmo/jsQR)（Apache-2.0）、
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator)（MIT）、
[EFF](https://www.eff.org/dice) の単語リスト（CC BY 3.0）、[diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
（Lexique 3.81 から派生）、そして Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho、
JetBrains Mono の各フォント（SIL Open Font License）に感謝します。
