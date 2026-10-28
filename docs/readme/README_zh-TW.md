<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · **繁體中文** · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Android 上的**離線**密碼管理器：把 [Bitwarden](https://bitwarden.com) 和
[Keyra](https://github.com/Tahir-1907/Keyra) 的功能整合進一個手機 App，到處都有 GSAP 動畫，介面
可以透過模組完全自訂。沒有帳號、沒有伺服器、沒有遙測：保險庫永遠不會離開
手機。

以 [Capacitor](https://capacitorjs.com/) 封裝為 APK，由 GitHub Actions 自動編譯，並專為
Galaxy Z Fold 設計（摺疊時：底部分頁列；展開時：左側導覽列，右側顯示開啟的項目）。

## 下載 APK

➡️ **[下載 Haiku（`haiku.apk`）](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** —— 這個檔案也在
*Releases* 分頁，並附有 SHA-256 雜湊值。

1. 在手機上下載 `haiku.apk` 並開啟。
2. 若 Android 要求，請允許從此來源安裝，然後點選**安裝**。
3. 第一次啟動：*建立我的保險庫*（或*還原備份*）。

所有版本都以**同一把金鑰**（`signing/` 資料夾）簽署：更新可以直接覆蓋安裝在
舊版上，不必解除安裝，因此不會遺失保險庫。這把金鑰在儲存庫中是公開的：請只從本儲存庫
安裝 Haiku。若要使用私人金鑰，請在 *Settings › Secrets › Actions* 中新增 `HAIKU_KEYSTORE_B64`、`HAIKU_KEYSTORE_PASSWORD` 和
`HAIKU_KEY_PASSWORD` 這幾個 secret（更換金鑰必須解除安裝：請先做好
備份）。

## Haiku 的功能

### 保險庫
- **7 種項目類型**：登入資訊、付款卡（可翻轉的 3D 卡片，用來查看安全碼）、身分、安全
  筆記、SSH 金鑰（產生 OpenSSH 格式的 Ed25519 金鑰）、Wi-Fi 網路（以 QR 碼分享）、伺服器（複製
  `ssh`/`sftp` 指令）。
- 每筆登入資訊可有多個**網址**與比對規則（網域、主機、完全相符、規則運算式……），
  **自訂欄位**（文字、隱藏、是/否）、加密的**附件**（10 MB）、**筆記**。
- **資料夾**（顏色、拖曳排序）、**標籤**（每個項目 20 個，依循 Keyra 的規則）、**我的最愛**。
- **搜尋**不分重音與大小寫，支援 `#étiquette`、`#"deux mots"`、`type:carte`、`is:fav`、`has:totp`、`in:dossier`。
- **歷史紀錄**：每個項目保留 20 個版本，可比對前後差異並還原；保留 10 個舊密碼。
- **垃圾桶**自動清除（7、30、90 天、1 年或永不），以及變更的**整體歷史紀錄**。
- 依項目**重新輸入主密碼**（和 Bitwarden 一樣）、**多選**（移動、我的最愛、
  垃圾桶）、**複製項目**，手勢：往右滑 = 複製，往左滑 = 我的最愛，長按 = 選單。
- **多個獨立的保險庫**，各有自己的密碼。

### 自動填入
- Haiku 是 **Android 自動填入服務**（設定 › 自動填入）：在 App 或
  瀏覽器中，點選欄位後選「使用 Haiku 填入」，解鎖（生物辨識、PIN 碼或密碼）並選擇
  登入資訊。比對遵循 Bitwarden 的規則（網域、主機、`androidapp://paquet`……），
  與 App 的關聯可一鍵記住。
- 登入資訊的 2FA 驗證碼會自動複製，供下一步使用。
- 登入後，Android 會提議把登入資訊**儲存**（或更新）到 Haiku。
- Chrome：*設定 › 自動填入 › 使用其他服務*。

### 驗證器（2FA）
- **TOTP** 驗證碼（SHA-1/256/512，6 或 8 位數，任意週期）、**HOTP** 和 **Steam Guard**，即時顯示，附
  倒數圓環與下一組驗證碼。
- 以**掃描 QR 碼**（相機或螢幕截圖）、手動輸入金鑰或 `otpauth://` 連結新增。
- 匯入 **Google Authenticator** 的匯出（「轉移帳戶」）以及 **Aegis**。

### 產生器
- 密碼（5 到 128 個字元，數字/符號最少數量，排除易混淆字元，可選符號），
  **密碼片語**（法文與英文各 7,776 字的字表）、**使用者名稱**（單字、隨機、位址
  `moi+xxxx@gmail.com`、catch-all 網域）、**PIN 碼**。顯示精確的熵值與破解所需時間。
- 產生過的值的歷史紀錄，加密存放在保險庫中。

### 保險庫健康度
滿分 100 的分數與報告：**弱**密碼、**重複使用**、**過舊**（超過一年）、**缺少**的密碼、
**http:// 網站**、**過期的付款卡**，以及依需求檢查：**外洩**（Have I Been Pwned，k-匿名：只送出
SHA-1 雜湊的前 5 個字元）和**支援卻未啟用的 2FA**（2fa.directory 清單）。

### Send（加密分享）
離線版的 Bitwarden Send：一段文字或一個檔案（15 MB）會變成加密的**獨立 HTML 頁面**
（AES-256-GCM，PBKDF2-SHA256 600,000 次迭代），並設有到期日。收件人可在任何
瀏覽器中，用你透過其他管道告訴他的密碼開啟。

### 匯入 / 匯出
| 格式 | 匯入 | 匯出 |
|---|---|---|
| 加密的 Haiku（`.haiku.json`，Argon2id + AES-256-GCM，整個保險庫） | ✅ | ✅ |
| Haiku 保險庫（`.haiku`，備份或另一支手機） | ✅ | ✅ |
| Bitwarden JSON（明文） | ✅ | ✅ |
| **受密碼保護的** Bitwarden JSON（PBKDF2 或 Argon2id） | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| 加密的 Keyra `.mcfexport` | ✅ | ✅（可再匯入 Keyra） |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV、KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera、Firefox、Safari / Apple 密碼 | ✅ | ✅（瀏覽器 CSV） |
| LastPass、1Password、Dashlane、Proton Pass、NordPass（CSV） | ✅ | — |
| Aegis（明文 JSON）、Google Authenticator（QR 碼） | ✅ | ✅（`otpauth://` 連結） |
| 紙本副本（可列印的 HTML 頁面，類似 Keyra 的 PDF） | — | ✅ |

匯入前預覽（不顯示密碼），自動建立資料夾，略過重複項目，並提醒刪除
明文檔案。Haiku 特有的類型（Wi-Fi、伺服器）透過 `haiku-type` 欄位，可經由 Bitwarden 來回
匯出匯入而不遺失。匯出時會再次要求主密碼。

### 安全性
```text
主密碼 ──Argon2id（64 MiB，3 輪，4 條通道）──► KEK
KEK ──AES-256-GCM──► DEK（隨機 256 位元資料金鑰）
DEK ──AES-256-GCM──► 保險庫內容（壓縮的 JSON）
```
- 主密碼從不儲存。變更密碼只會重新包裝 DEK，不必重新加密全部內容。
- 可調整 Argon2id 強度：輕量（32 MiB）、標準（64 MiB，同 Keyra）、強化（128 MiB · 4 輪）。
- Keyra 風格的**復原金鑰**：32 個字元（Crockford base32，150 位元 + 檢查碼），只顯示
  一次；「忘記密碼」會建立新金鑰並使舊金鑰失效。
- **生物辨識**：DEK 由 **Android Keystore** 的 AES 金鑰加密，此金鑰只有在通過
  強式生物辨識驗證後才能使用，新增指紋時即失效。
- **PIN 碼**（4 到 12 位數）以 Argon2id 衍生，錯誤 5 次後停用（可調整），可選擇「重新啟動後
  再次要求主密碼」。
- **自動鎖定**（從立即到 4 小時，或永不），背景中也會生效；**剪貼簿**在
  30 秒後清除（可調整）並標示為*敏感*（在 Android 13+ 的預覽中隱藏）；**安全畫面**（禁止截圖，
  在最近使用的 App 中隱藏預覽）；停用 Android 對 App 資料夾的備份；*release* 版 APK
  （無法偵錯）。
- 鎖定時自動**備份**（若保險庫有變更）到 `Documents/Haiku/Sauvegardes`，保留 10 份
  （可調整）；備份會以**新的**保險庫還原，不會覆寫任何內容。

### 個人化：工作室與模組
- **9 種主題**（Haïku、Sumi-e、Sakura、霓虹、矩陣、太陽、冰河、紙張、單色），深色、AMOLED、
  淺色與玻璃模式，兩種自由強調色（或隨機配色），可用代碼分享主題。
- **動態背景**：極光、星座（隨手指反應）、水墨（每次觸碰滴下一滴）、櫻花
  花瓣、復古格線、波紋、程式碼雨；強度、紙張紋理、光暈。
- **8 種字型**（Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho、JetBrains Mono），圓角、
  玻璃模糊、密度、清單以卡片 / 列 / 格狀顯示、首字母或圖示。
- 可調整的 **GSAP 動畫**（無、柔和、一般、*瘋狂*）：俳句印章開場、在鎖定畫面上以毛筆畫出圓相，
  解鎖時轉動的鎖，以解密效果顯示機密內容
  （ScrambleText）、粒子迸發（Physics2D）、自動重新排序的清單（Flip）、可拖曳的面板與列
  （Draggable + Inertia）、3D 傾斜的卡片、可自選的頁面轉場（滑動、縮放、3D 翻轉、
  布幕、模糊）。
- **模組**：每項功能（2FA、產生器、健康度、Send、資料夾、歷史紀錄、垃圾桶、匯入/匯出、
  備份）都可開啟或關閉；**分頁列**可自由組合（最多 5 個，拖曳排序）；**首頁**
  由可重新排列的小工具組成（每日俳句、搜尋、捷徑、我的最愛、2FA 驗證碼、健康分數、
  最近項目、快速產生器、統計、資料夾、標籤雲）。
- **指令面板**（在標題列上往下滑，或用鍵盤按 Ctrl+K）以及鍵盤快速鍵
  （Ctrl+N、Ctrl+L、Ctrl+G、Ctrl+F、Ctrl+,）。

## 已知限制
- 沒有同步、組織和緊急存取（這些是 Bitwarden 伺服器的功能）：這是
  本機保險庫，就像 Keyra。要從一支手機換到另一支：使用 `.haiku` 備份或加密匯出。
- Bitwarden 匯出中的通行金鑰（passkey）不會匯入：Haiku 不是通行金鑰提供者。
- 介面僅有法文（Keyra 提供 17 種語言）。
- 自動填入會經過 Haiku 的畫面（鍵盤中沒有內嵌建議）：多點一下，
  但系統從不會以明文保存任何機密。
- 無法直接讀取 `.kdbx` 檔案：請先從 KeePassXC / KeePass 匯出為 CSV 或 XML。
- 強度估算為離線計算，僅供參考。

## 自行編譯（Windows、Android Studio、JDK 21）
```bash
npm install
npm test                 # 核心測試（加密、匯入/匯出、2FA……）
npm run android:add      # 只在 android/ 資料夾不存在時執行
npm run sync             # Web 建置 + 複製到 Android 專案 + Haiku 設定
npm run open             # 開啟 Android Studio（或：cd android && gradlew assembleRelease）
```
APK 以 `signing/` 中的金鑰簽署，與 GitHub Actions 編譯的相同：兩者可以互相
覆蓋安裝。

## 程式碼結構
```text
src/core/      加密、保險庫、匯入器/匯出器、TOTP、產生器、強度、稽核、Send、SSH
src/views/     畫面（首頁、保險庫、項目、編輯、2FA、產生器、健康度、Send、工作室、模組……）
src/ui/        GSAP 效果、動態背景、主題、元件、鎖定畫面
scripts/       Android 專案設定、Java 原生外掛（Keystore 生物辨識、安全畫面、自動填入）、圖示
tests/         Node 測試（使用真實的 Keyra 匯出）與 Playwright 介面測試
```

## 致謝
Haiku 是獨立重寫的作品：它重現了 Bitwarden 與 Keyra 的**功能**，並能讀寫
它們的**匯出格式**，但沒有沿用它們的程式碼。感謝 [GSAP](https://gsap.com)（Webflow）、[Lucide](https://lucide.dev)
（ISC）、[hash-wasm](https://github.com/Daninet/hash-wasm)（MIT）、[jsQR](https://github.com/cozmo/jsQR)（Apache-2.0）、
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator)（MIT）、
[EFF](https://www.eff.org/dice) 字表（CC BY 3.0）、[diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
（衍生自 Lexique 3.81），以及 Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho 和
JetBrains Mono 字型（SIL Open Font License）。
