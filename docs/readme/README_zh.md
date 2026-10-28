<div align="center">

[English](README_en.md) · **简体中文** · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

适用于 Android 的**离线**密码管理器：把 [Bitwarden](https://bitwarden.com) 和
[Keyra](https://github.com/Tahir-1907/Keyra) 的功能集成到一个手机应用中，处处都有 GSAP 动画，界面
可以通过模块完全自定义。无账号、无服务器、无遥测：密码库永远不会离开
手机。

使用 [Capacitor](https://capacitorjs.com/) 打包为 APK，由 GitHub Actions 自动编译，并专为
Galaxy Z Fold 设计（折叠时：底部标签栏；展开时：左侧导航栏，右侧显示打开的条目）。

## 下载 APK

➡️ **[下载 Haiku（`haiku.apk`）](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** —— 该文件也在
*Releases* 页面，并附有 SHA-256 校验值。

1. 在手机上下载 `haiku.apk` 并打开。
2. 如果 Android 提示，请允许从该来源安装，然后点击**安装**。
3. 首次启动：*创建我的密码库*（或*恢复备份*）。

所有版本都使用**同一个密钥**（`signing/` 文件夹）签名：更新可以直接覆盖安装在
旧版本上，无需卸载，因此不会丢失密码库。该密钥在仓库中是公开的：请只从本仓库
安装 Haiku。如需使用私有密钥，请在 *Settings › Secrets › Actions* 中添加 `HAIKU_KEYSTORE_B64`、`HAIKU_KEYSTORE_PASSWORD` 和
`HAIKU_KEY_PASSWORD` 这几个 secret（更换密钥需要卸载旧版本：请先做好
备份）。

## Haiku 的功能

### 密码库
- **7 种条目类型**：登录信息、银行卡（可翻转的 3D 卡片，用于查看安全码）、身份信息、安全
  笔记、SSH 密钥（生成 OpenSSH 格式的 Ed25519 密钥）、Wi-Fi 网络（通过二维码分享）、服务器（复制
  `ssh`/`sftp` 命令）。
- 每个登录信息可有多个**网址**及匹配规则（域名、主机、精确匹配、正则表达式……），
  **自定义字段**（文本、隐藏、是/否）、加密的**附件**（10 MB）、**备注**。
- **文件夹**（颜色、拖动排序）、**标签**（每个条目 20 个，遵循 Keyra 的规则）、**收藏**。
- **搜索**不区分重音和大小写，支持 `#étiquette`、`#"deux mots"`、`type:carte`、`is:fav`、`has:totp`、`in:dossier`。
- **历史记录**：每个条目保留 20 个版本，可对比修改前后并恢复；保留 10 个旧密码。
- **回收站**自动清理（7、30、90 天、1 年或永不），以及修改的**全局历史**。
- 按条目**重新输入主密码**（与 Bitwarden 相同）、**多选**（移动、收藏、
  回收站）、**复制条目**，手势：向右滑 = 复制，向左滑 = 收藏，长按 = 菜单。
- **多个独立的密码库**，每个都有自己的密码。

### 自动填充
- Haiku 是一个 **Android 自动填充服务**（设置 › 自动填充）：在应用或
  浏览器中，点击输入框后选择"使用 Haiku 填充"，解锁（生物识别、PIN 或密码）并选择
  登录信息。匹配遵循 Bitwarden 的规则（域名、主机、`androidapp://paquet`……），
  与应用的关联可一键记住。
- 登录信息的 2FA 验证码会自动复制，供下一步使用。
- 登录后，Android 会提示将登录信息**保存**（或更新）到 Haiku。
- Chrome：*设置 › 自动填充 › 使用其他服务*。

### 身份验证器（2FA）
- **TOTP** 验证码（SHA-1/256/512，6 或 8 位，任意周期）、**HOTP** 和 **Steam Guard**，实时显示，带
  倒计时圆环和下一个验证码。
- 通过**扫描二维码**（相机或截图）、手动输入密钥或 `otpauth://` 链接添加。
- 导入 **Google Authenticator** 的导出（"转移账号"）以及 **Aegis**。

### 生成器
- 密码（5 到 128 个字符，数字/符号最少个数，排除易混淆字符，可选符号），
  **密码短语**（法语和英语各 7,776 个单词的词表）、**用户名**（单词、随机、地址
  `moi+xxxx@gmail.com`、catch-all 域名）、**PIN 码**。显示精确的熵和破解所需时间。
- 生成过的值的历史记录，加密保存在密码库中。

### 密码库健康
满分 100 的评分和报告：**弱**密码、**重复使用**的密码、**旧**密码（超过一年）、**缺失**的密码、
**http:// 网站**、**过期的银行卡**，以及按需检查：**泄露**（Have I Been Pwned，k-匿名：只发送
SHA-1 哈希的前 5 个字符）和**支持但未启用的 2FA**（2fa.directory 列表）。

### Send（加密分享）
离线版的 Bitwarden Send：一段文字或一个文件（15 MB）会变成一个加密的**独立 HTML 页面**
（AES-256-GCM，PBKDF2-SHA256 600,000 次迭代），并带有过期日期。接收者可在任何
浏览器中用你通过其他渠道告诉他的密码打开。

### 导入 / 导出
| 格式 | 导入 | 导出 |
|---|---|---|
| 加密的 Haiku（`.haiku.json`，Argon2id + AES-256-GCM，整个密码库） | ✅ | ✅ |
| Haiku 密码库（`.haiku`，备份或另一部手机） | ✅ | ✅ |
| Bitwarden JSON（明文） | ✅ | ✅ |
| **密码保护的** Bitwarden JSON（PBKDF2 或 Argon2id） | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| 加密的 Keyra `.mcfexport` | ✅ | ✅（可重新导入 Keyra） |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV、KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera、Firefox、Safari / Apple 密码 | ✅ | ✅（浏览器 CSV） |
| LastPass、1Password、Dashlane、Proton Pass、NordPass（CSV） | ✅ | — |
| Aegis（明文 JSON）、Google Authenticator（二维码） | ✅ | ✅（`otpauth://` 链接） |
| 纸质副本（可打印的 HTML 页面，类似 Keyra 的 PDF） | — | ✅ |

导入前预览（不显示密码），自动创建文件夹，跳过重复项，并提醒删除
明文文件。Haiku 特有的类型（Wi-Fi、服务器）借助一个 `haiku-type` 字段，可以在 Bitwarden 中往返
而不丢失。导出时会再次要求输入主密码。

### 安全
```text
主密码 ──Argon2id（64 MiB，3 轮，4 条并行通道）──► KEK
KEK ──AES-256-GCM──► DEK（随机 256 位数据密钥）
DEK ──AES-256-GCM──► 密码库内容（压缩的 JSON）
```
- 主密码从不存储。修改主密码只会重新包装 DEK，无需重新加密全部内容。
- Argon2id 强度可调：轻量（32 MiB）、标准（64 MiB，与 Keyra 相同）、加强（128 MiB · 4 轮）。
- Keyra 风格的**恢复密钥**：32 个字符（Crockford base32，150 位 + 校验），只显示
  一次；"忘记密码"会创建新密钥并让旧密钥失效。
- **生物识别**：DEK 由 **Android Keystore** 中的 AES 密钥加密，该密钥只有在通过
  强生物识别验证后才能使用，添加新指纹时会失效。
- **PIN 码**（4 到 12 位）通过 Argon2id 派生，连续 5 次错误后停用（可调），可选择"重启后
  重新要求输入主密码"。
- **自动锁定**（从立即到 4 小时，或从不），在后台也会生效；**剪贴板**在
  30 秒后清空（可调）并标记为*敏感*（在 Android 13+ 的预览中隐藏）；**安全屏幕**（禁止截图，
  在最近任务中隐藏预览）；禁用 Android 对应用文件夹的备份；*release* 版 APK
  （不可调试）。
- 锁定时自动**备份**（如果密码库有变化）到 `Documents/Haiku/Sauvegardes`，保留 10 份
  （可调）；备份会作为**新的**密码库恢复，不会覆盖任何内容。

### 个性化：工作室与模块
- **9 种主题**（Haïku、Sumi-e、Sakura、霓虹、矩阵、太阳、冰川、纸张、单色），深色、AMOLED、
  浅色和玻璃模式，两种自由强调色（或随机配色），可通过代码分享主题。
- **动态背景**：极光、星座（随手指变化）、水墨（每次触摸滴下一滴）、樱花
  花瓣、复古网格、波浪、代码雨；强度、纸张纹理、光晕。
- **8 种字体**（Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho、JetBrains Mono），圆角、
  玻璃模糊、密度、列表显示为卡片 / 行 / 网格、首字母或图标。
- 可调的 **GSAP 动画**（无、柔和、正常、*疯狂*）：俳句印章开场、锁屏上用毛笔画出的圆相，
  解锁时转动的锁，像解密一样显示机密内容
  （ScrambleText）、粒子迸发（Physics2D）、自动重排的列表（Flip）、可拖动的面板和行
  （Draggable + Inertia）、3D 倾斜的卡片、可选的页面过渡（滑动、缩放、3D 翻转、
  帘幕、模糊）。
- **模块**：每个功能（2FA、生成器、健康、Send、文件夹、历史、回收站、导入/导出、
  备份）都可开启或关闭；**标签栏**可自由组合（最多 5 个，拖动排序）；**主页**
  由可重新排序的小组件组成（每日俳句、搜索、快捷方式、收藏、2FA 验证码、健康评分、
  最近条目、快速生成器、统计、文件夹、标签云）。
- **命令面板**（在标题栏上向下滑，或用键盘按 Ctrl+K）以及键盘快捷键
  （Ctrl+N、Ctrl+L、Ctrl+G、Ctrl+F、Ctrl+,）。

## 已知限制
- 没有同步、组织和紧急访问（这些是 Bitwarden 服务器端的功能）：这是
  一个本地密码库，和 Keyra 一样。要从一部手机迁移到另一部：使用 `.haiku` 备份或加密导出。
- Bitwarden 导出中的通行密钥（passkey）不会被导入：Haiku 不是通行密钥提供方。
- 界面仅有法语（Keyra 提供 17 种语言）。
- 自动填充需要经过 Haiku 的界面（键盘中没有内联建议）：多点一下，
  但系统从不会以明文保存任何机密。
- 不能直接读取 `.kdbx` 文件：请先在 KeePassXC / KeePass 中将其导出为 CSV 或 XML。
- 强度估算是离线的，只是近似值。

## 自行编译（Windows、Android Studio、JDK 21）
```bash
npm install
npm test                 # 核心测试（加密、导入/导出、2FA……）
npm run android:add      # 仅在 android/ 文件夹不存在时运行
npm run sync             # Web 构建 + 复制到 Android 项目 + Haiku 设置
npm run open             # 打开 Android Studio（或：cd android && gradlew assembleRelease）
```
APK 使用 `signing/` 中的密钥签名，与 GitHub Actions 编译的相同：两者可以互相
覆盖安装。

## 代码结构
```text
src/core/      加密、密码库、导入器/导出器、TOTP、生成器、强度、审计、Send、SSH
src/views/     页面（首页、密码库、条目、编辑、2FA、生成器、健康、Send、工作室、模块……）
src/ui/        GSAP 效果、动态背景、主题、组件、锁屏
scripts/       Android 项目设置、Java 原生插件（Keystore 生物识别、安全屏幕、自动填充）、图标
tests/         Node 测试（使用真实的 Keyra 导出）和 Playwright 界面测试
```

## 致谢
Haiku 是独立重写的作品：它重现了 Bitwarden 和 Keyra 的**功能**，并能读写
它们的**导出格式**，但没有复用它们的代码。感谢 [GSAP](https://gsap.com)（Webflow）、[Lucide](https://lucide.dev)
（ISC）、[hash-wasm](https://github.com/Daninet/hash-wasm)（MIT）、[jsQR](https://github.com/cozmo/jsQR)（Apache-2.0）、
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator)（MIT）、
[EFF](https://www.eff.org/dice) 词表（CC BY 3.0）、[diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
（源自 Lexique 3.81），以及 Sora、Manrope、Outfit、Space Grotesk、Syne、Unbounded、Shippori Mincho 和
JetBrains Mono 字体（SIL Open Font License）。
