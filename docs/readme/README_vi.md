<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · **Tiếng Việt** · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Trình quản lý mật khẩu **ngoại tuyến** cho Android: các tính năng của [Bitwarden](https://bitwarden.com) và
[Keyra](https://github.com/Tahir-1907/Keyra) trong một ứng dụng di động, với hiệu ứng GSAP ở khắp nơi và giao diện
tùy biến hoàn toàn theo mô-đun. Không tài khoản, không máy chủ, không thu thập dữ liệu: kho mật khẩu không bao giờ rời khỏi
điện thoại.

Đóng gói thành APK bằng [Capacitor](https://capacitorjs.com/), biên dịch tự động bằng GitHub Actions, và được thiết kế cho
Galaxy Z Fold (khi gập: thanh thẻ; khi mở: thanh điều hướng bên trái và mục đang mở bên phải).

## Tải APK

➡️ **[Tải Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — tệp này cũng có trong thẻ
*Releases*, kèm mã băm SHA-256.

1. Tải `haiku.apk` về điện thoại và mở nó.
2. Cho phép cài đặt từ nguồn này nếu Android yêu cầu, rồi nhấn **Cài đặt**.
3. Ở lần mở đầu tiên: *Tạo kho của tôi* (hoặc *Khôi phục bản sao lưu*).

Mọi phiên bản đều được ký bằng **cùng một khóa** (thư mục `signing/`): bản cập nhật cài đè lên
bản trước mà không cần gỡ, nên không mất kho. Khóa này công khai trong kho mã: chỉ cài Haiku
từ kho mã này. Để dùng khóa riêng, hãy thêm các secret `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` và
`HAIKU_KEY_PASSWORD` trong *Settings › Secrets › Actions* (đổi khóa buộc phải gỡ ứng dụng: hãy
sao lưu trước).

## Haiku làm được gì

### Kho
- **7 loại mục**: thông tin đăng nhập, thẻ thanh toán (thẻ 3D lật được để xem mã bảo mật), danh tính, ghi chú
  bảo mật, khóa SSH (tạo Ed25519 theo định dạng OpenSSH), mạng Wi-Fi (chia sẻ bằng mã QR), máy chủ (sao chép
  lệnh `ssh`/`sftp`).
- Nhiều **địa chỉ cho mỗi thông tin đăng nhập** với quy tắc so khớp (tên miền, máy chủ, chính xác, biểu thức chính quy…),
  **trường tùy chỉnh** (văn bản, ẩn, có/không), **tệp đính kèm** được mã hóa (10 MB), **ghi chú**.
- **Thư mục** (màu, kéo để sắp xếp), **nhãn** (20 nhãn mỗi mục, theo quy tắc của Keyra), **yêu thích**.
- **Tìm kiếm** không phân biệt dấu và hoa thường, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Lịch sử**: 20 phiên bản mỗi mục, khôi phục được với so sánh trước/sau; 10 mật khẩu cũ.
- **Thùng rác** tự dọn (7, 30, 90 ngày, 1 năm hoặc không bao giờ), **lịch sử chung** các thay đổi.
- **Hỏi lại mật khẩu chính** theo từng mục (như Bitwarden), **chọn nhiều** (di chuyển, yêu thích,
  thùng rác), **nhân bản**, cử chỉ: vuốt phải = sao chép, vuốt trái = yêu thích, nhấn giữ = menu.
- **Nhiều kho độc lập**, mỗi kho có mật khẩu riêng.

### Tự động điền
- Haiku là một **dịch vụ tự động điền của Android** (Cài đặt › Tự động điền): trong một ứng dụng hoặc
  trình duyệt, chạm vào ô nhập rồi chọn "Điền bằng Haiku", mở khóa (sinh trắc học, PIN hoặc mật khẩu) và chọn
  thông tin đăng nhập. Việc so khớp theo quy tắc của Bitwarden (tên miền, máy chủ, `androidapp://paquet`…), và
  liên kết với một ứng dụng có thể được ghi nhớ chỉ với một chạm.
- Mã 2FA của thông tin đăng nhập được tự động sao chép cho bước tiếp theo.
- Sau khi đăng nhập, Android đề nghị **lưu** (hoặc cập nhật) thông tin đăng nhập vào Haiku.
- Chrome: *Cài đặt › Tự động điền › Dùng dịch vụ khác*.

### Trình xác thực (2FA)
- Mã **TOTP** (SHA-1/256/512, 6 hoặc 8 chữ số, chu kỳ tùy ý), **HOTP** và **Steam Guard**, hiển thị trực tiếp với
  vòng đếm ngược và mã kế tiếp.
- Thêm bằng cách **quét mã QR** (camera hoặc ảnh chụp màn hình), nhập khóa thủ công hoặc liên kết `otpauth://`.
- Nhập bản xuất của **Google Authenticator** ("Chuyển tài khoản") và của **Aegis**.

### Trình tạo
- Mật khẩu (5 đến 128 ký tự, số lượng chữ số/ký hiệu tối thiểu, loại trừ ký tự dễ nhầm, tự chọn ký hiệu),
  **cụm mật khẩu** (danh sách 7.776 từ bằng tiếng Pháp và tiếng Anh), **tên người dùng** (từ, ngẫu nhiên, địa chỉ
  `moi+xxxx@gmail.com`, tên miền catch-all), **mã PIN**. Hiển thị entropy chính xác và thời gian bẻ khóa.
- Lịch sử các giá trị đã tạo, được mã hóa trong kho.

### Sức khỏe của kho
Điểm trên 100 và báo cáo: mật khẩu **yếu**, **dùng lại**, **cũ** (hơn một năm), **còn thiếu**,
**trang http://**, **thẻ hết hạn**, và theo yêu cầu: **rò rỉ** (Have I Been Pwned, k-ẩn danh: chỉ
5 ký tự của mã băm SHA-1 được gửi đi) và **có hỗ trợ 2FA nhưng chưa bật** (danh sách 2fa.directory).

### Send (chia sẻ mã hóa)
Phiên bản ngoại tuyến của Bitwarden Send: một đoạn văn bản hoặc tệp (15 MB) trở thành một **trang HTML độc lập** được mã hóa
(AES-256-GCM, PBKDF2-SHA256 600.000 vòng lặp), có ngày hết hạn. Người nhận mở nó trong bất kỳ
trình duyệt nào bằng mật khẩu mà bạn gửi qua một kênh khác.

### Nhập / xuất
| Định dạng | Nhập | Xuất |
|---|---|---|
| Haiku mã hóa (`.haiku.json`, Argon2id + AES-256-GCM, toàn bộ kho) | ✅ | ✅ |
| Kho Haiku (`.haiku`, bản sao lưu hoặc điện thoại khác) | ✅ | ✅ |
| Bitwarden JSON (văn bản thường) | ✅ | ✅ |
| Bitwarden JSON **có mật khẩu bảo vệ** (PBKDF2 hoặc Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` mã hóa | ✅ | ✅ (nhập lại được vào Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Mật khẩu Apple | ✅ | ✅ (CSV trình duyệt) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON thường), Google Authenticator (QR) | ✅ | ✅ (liên kết `otpauth://`) |
| Bản giấy (trang HTML để in, giống bản PDF của Keyra) | — | ✅ |

Xem trước khi nhập (không hiển thị mật khẩu), tự tạo thư mục, bỏ qua mục trùng, nhắc xóa
tệp văn bản thường. Các loại riêng của Haiku (Wi-Fi, máy chủ) vẫn giữ nguyên khi đi qua Bitwarden rồi quay lại nhờ trường
`haiku-type`. Khi xuất, ứng dụng sẽ hỏi lại mật khẩu chính.

### Bảo mật
```text
mật khẩu chính ──Argon2id (64 MiB, 3 lượt, 4 luồng)──► KEK
KEK ──AES-256-GCM──► DEK (khóa dữ liệu ngẫu nhiên 256 bit)
DEK ──AES-256-GCM──► nội dung kho (JSON nén)
```
- Mật khẩu chính không bao giờ được lưu. Đổi mật khẩu chỉ bọc lại DEK mà không mã hóa lại toàn bộ.
- Độ mạnh Argon2id tùy chỉnh: nhẹ (32 MiB), tiêu chuẩn (64 MiB, như Keyra), tăng cường (128 MiB · 4 lượt).
- **Khóa khôi phục** kiểu Keyra: 32 ký tự (base32 của Crockford, 150 bit + mã kiểm tra), chỉ hiển thị
  một lần; "quên mật khẩu" sẽ tạo khóa mới và vô hiệu hóa khóa cũ.
- **Sinh trắc học**: DEK được mã hóa bằng một khóa AES của **Android Keystore** chỉ dùng được sau khi
  xác thực sinh trắc học mạnh, và bị vô hiệu nếu thêm vân tay mới.
- **Mã PIN** (4 đến 12 chữ số) được dẫn xuất bằng Argon2id, bị tắt sau 5 lần sai (tùy chỉnh), tùy chọn "hỏi lại
  mật khẩu chính sau khi khởi động lại".
- **Tự khóa** (từ ngay lập tức đến 4 giờ, hoặc không bao giờ), kể cả khi chạy nền; **bộ nhớ tạm** được xóa sau
  30 giây (tùy chỉnh) và đánh dấu *nhạy cảm* (ẩn trong bản xem trước của Android 13+); **màn hình bảo mật** (không chụp màn hình,
  ẩn bản xem trước trong ứng dụng gần đây); tắt sao lưu Android cho thư mục ứng dụng; APK bản *release*
  (không gỡ lỗi được).
- **Sao lưu** tự động khi khóa (nếu kho có thay đổi) vào `Documents/Haiku/Sauvegardes`, giữ 10 bản
  (tùy chỉnh); các bản sao lưu được khôi phục thành một kho **mới**, không ghi đè lên gì cả.

### Cá nhân hóa: Studio và các mô-đun
- **9 giao diện** (Haïku, Sumi-e, Sakura, Neon, Ma trận, Mặt trời, Băng hà, Giấy, Đơn sắc), các chế độ tối, AMOLED,
  sáng và kính, hai màu nhấn tự do (hoặc phối màu ngẫu nhiên), chia sẻ giao diện bằng mã.
- **Hình nền động**: cực quang, chòm sao (phản ứng theo ngón tay), mực sumi-e (một giọt mỗi lần chạm), cánh hoa
  anh đào, lưới retro, gợn sóng, mưa mã; cường độ, vân giấy, quầng sáng.
- **8 phông chữ** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), bo góc,
  độ mờ kính, mật độ, danh sách dạng thẻ / dòng / lưới, chữ cái đầu hoặc biểu tượng.
- **Hiệu ứng GSAP** tùy chỉnh (không, nhẹ nhàng, bình thường, *điên rồ*): màn mở đầu với con dấu 俳句, vòng ensō vẽ bằng bút lông trên
  màn hình khóa và ổ khóa xoay khi mở khóa, hiện thông tin bí mật như đang giải mã
  (ScrambleText), các chùm hạt bung ra (Physics2D), danh sách tự sắp xếp lại (Flip), bảng và dòng có thể kéo
  (Draggable + Inertia), thẻ nghiêng 3D, hiệu ứng chuyển trang tùy chọn (trượt, phóng to, lật 3D,
  rèm, làm mờ).
- **Mô-đun**: mỗi tính năng (2FA, trình tạo, sức khỏe, Send, thư mục, lịch sử, thùng rác, nhập/xuất,
  sao lưu) đều bật hoặc tắt được; **thanh thẻ** do bạn tự sắp (tối đa 5, kéo để sắp xếp); **màn hình chính**
  gồm các widget có thể sắp xếp lại (haiku trong ngày, tìm kiếm, lối tắt, yêu thích, mã 2FA, điểm sức khỏe,
  mục gần đây, trình tạo nhanh, thống kê, thư mục, đám mây nhãn).
- **Bảng lệnh** (vuốt xuống trên thanh tiêu đề, hoặc Ctrl+K trên bàn phím) và phím tắt bàn phím
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Hạn chế đã biết
- Không có đồng bộ, tổ chức hay truy cập khẩn cấp (đó là các tính năng của máy chủ Bitwarden): đây là
  một kho cục bộ, giống Keyra. Để chuyển từ điện thoại này sang điện thoại khác: bản sao lưu `.haiku` hoặc bản xuất mã hóa.
- Khóa truy cập (passkey) trong bản xuất Bitwarden không được nhập: Haiku không phải là nhà cung cấp khóa truy cập.
- Giao diện chỉ có tiếng Pháp (Keyra có 17 ngôn ngữ).
- Tự động điền đi qua màn hình của Haiku (không có gợi ý ngay trên bàn phím): thêm một lần chạm,
  nhưng không bí mật nào được lưu dưới dạng văn bản thường cho hệ thống.
- Tệp `.kdbx` không được đọc trực tiếp: hãy xuất chúng sang CSV hoặc XML từ KeePassXC / KeePass.
- Việc ước tính độ mạnh được tính ngoại tuyến và chỉ mang tính gần đúng.

## Tự biên dịch (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # kiểm thử phần lõi (mã hóa, nhập/xuất, 2FA…)
npm run android:add      # chỉ khi thư mục android/ chưa tồn tại
npm run sync             # build web + sao chép vào dự án Android + cài đặt Haiku
npm run open             # mở Android Studio (hoặc: cd android && gradlew assembleRelease)
```
APK được ký bằng khóa trong `signing/`, giống bản của GitHub Actions: hai bản có thể cài đè
lên nhau.

## Tổ chức mã nguồn
```text
src/core/      mã hóa, kho, bộ nhập/xuất, TOTP, trình tạo, độ mạnh, kiểm tra, Send, SSH
src/views/     màn hình (trang chủ, kho, mục, chỉnh sửa, 2FA, trình tạo, sức khỏe, Send, Studio, mô-đun…)
src/ui/        hiệu ứng GSAP, hình nền động, giao diện, thành phần, màn hình khóa
scripts/       cài đặt dự án Android, plugin Java native (sinh trắc học Keystore, màn hình bảo mật, tự động điền), biểu tượng
tests/         kiểm thử Node (với bản xuất Keyra thật) và kiểm thử giao diện Playwright
```

## Ghi công
Haiku là một bản viết lại độc lập: nó tái hiện các **tính năng** của Bitwarden và Keyra và đọc/ghi
**định dạng xuất** của chúng, nhưng không dùng lại mã nguồn của họ. Cảm ơn [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), danh sách từ
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(dựa trên Lexique 3.81), cùng các phông chữ Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho và
JetBrains Mono (SIL Open Font License).
