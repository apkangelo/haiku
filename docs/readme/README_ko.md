<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · **한국어** · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Android용 **오프라인** 비밀번호 관리자: [Bitwarden](https://bitwarden.com)과
[Keyra](https://github.com/Tahir-1907/Keyra)의 기능을 하나의 모바일 앱에 담았고, 곳곳에 GSAP 애니메이션을 넣었으며, 인터페이스를
모듈 단위로 완전히 바꿀 수 있습니다. 계정도, 서버도, 원격 측정도 없습니다: 보관함은 절대
휴대폰 밖으로 나가지 않습니다.

[Capacitor](https://capacitorjs.com/)로 APK를 만들고, GitHub Actions로 자동 빌드하며,
Galaxy Z Fold에 맞춰 설계했습니다(접었을 때: 탭 바, 펼쳤을 때: 왼쪽 레일과 오른쪽에 열린 항목).

## APK 다운로드

➡️ **[Haiku 다운로드(`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — 이 파일은
*Releases* 탭에도 SHA-256 해시와 함께 있습니다.

1. 휴대폰에서 `haiku.apk`를 다운로드해서 엽니다.
2. Android가 요청하면 이 출처에서의 설치를 허용하고 **설치**를 누릅니다.
3. 처음 실행할 때: *내 보관함 만들기*(또는 *백업 복원*).

모든 버전은 **같은 키**(`signing/` 폴더)로 서명됩니다: 업데이트를 제거 없이 이전 버전 위에
설치할 수 있어 보관함을 잃지 않습니다. 이 키는 저장소에 공개되어 있으니 Haiku는 반드시
이 저장소에서만 설치하세요. 비공개 키를 쓰려면 *Settings › Secrets › Actions*에 `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD`,
`HAIKU_KEY_PASSWORD` 시크릿을 추가하세요(키를 바꾸면 앱을 제거해야 하므로 먼저
백업하세요).

## Haiku가 하는 일

### 보관함
- **7가지 항목 유형**: 로그인, 결제 카드(뒤집어서 보안 코드를 보는 3D 카드), 신원 정보, 보안
  메모, SSH 키(OpenSSH 형식의 Ed25519 키 생성), Wi-Fi 네트워크(QR 코드로 공유), 서버(
  `ssh`/`sftp` 명령 복사).
- 로그인마다 여러 개의 **URI**와 일치 규칙(도메인, 호스트, 정확히 일치, 정규식…),
  **사용자 지정 필드**(텍스트, 숨김, 예/아니요), 암호화된 **첨부 파일**(10MB), **메모**.
- **폴더**(색상, 드래그로 정렬), **태그**(항목당 20개, Keyra 규칙), **즐겨찾기**.
- 악센트와 대소문자를 무시하는 **검색**, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **기록**: 항목당 20개 버전, 변경 전후를 비교하며 복원 가능; 이전 비밀번호 10개 보관.
- 자동으로 비워지는 **휴지통**(7, 30, 90일, 1년 또는 안 함), 변경 사항의 **전체 기록**.
- 항목별 **마스터 비밀번호 재확인**(Bitwarden처럼), **다중 선택**(이동, 즐겨찾기,
  휴지통), **복제**, 제스처: 오른쪽으로 스와이프 = 복사, 왼쪽 = 즐겨찾기, 길게 누르기 = 메뉴.
- 각각 고유한 비밀번호를 가진 **여러 개의 독립된 보관함**.

### 자동 완성
- Haiku는 **Android 자동 완성 서비스**입니다(설정 › 자동 완성): 앱이나
  브라우저에서 입력란을 누르고 "Haiku로 채우기"를 선택한 뒤 잠금을 해제하고(생체 인증, PIN 또는 비밀번호)
  로그인을 고릅니다. 일치 판정은 Bitwarden 규칙을 따르며(도메인, 호스트, `androidapp://paquet`…),
  앱과의 연결은 한 번의 탭으로 기억할 수 있습니다.
- 로그인의 2FA 코드는 다음 단계를 위해 자동으로 복사됩니다.
- 로그인한 뒤 Android가 Haiku에 로그인을 **저장**(또는 업데이트)할지 제안합니다.
- Chrome: *설정 › 자동 완성 › 다른 서비스 사용*.

### 인증기(2FA)
- **TOTP** 코드(SHA-1/256/512, 6자리 또는 8자리, 자유로운 주기), **HOTP**, **Steam Guard**를 실시간으로,
  카운트다운 링과 다음 코드와 함께 표시.
- **QR 코드 스캔**(카메라 또는 스크린샷), 수동 키 입력, `otpauth://` 링크로 추가.
- **Google Authenticator** 내보내기("계정 이전")와 **Aegis**에서 가져오기.

### 생성기
- 비밀번호(5~128자, 숫자/기호 최소 개수, 헷갈리는 문자 제외, 사용할 기호 선택),
  **패스프레이즈**(프랑스어와 영어 7,776단어 목록), **사용자 이름**(단어, 무작위, 주소
  `moi+xxxx@gmail.com`, 캐치올 도메인), **PIN 코드**. 정확한 엔트로피와 해독 시간 표시.
- 생성한 값의 기록은 보관함 안에 암호화되어 저장.

### 보관함 상태
100점 만점 점수와 보고서: **약한**, **재사용된**, **오래된**(1년 이상), **비어 있는** 비밀번호,
**http:// 사이트**, **만료된 카드**, 그리고 요청 시: **유출**(Have I Been Pwned, k-익명성: SHA-1 해시의
5글자만 전송)과 **사용 가능하지만 꺼져 있는 2FA**(2fa.directory 목록).

### Send(암호화 공유)
Bitwarden Send의 오프라인 버전: 텍스트나 파일(15MB)이 암호화된 **독립형 HTML 페이지**가 됩니다
(AES-256-GCM, PBKDF2-SHA256 600,000회 반복), 만료일 포함. 받는 사람은 다른 경로로 전달받은
비밀번호로 어떤 브라우저에서든 열 수 있습니다.

### 가져오기 / 내보내기
| 형식 | 가져오기 | 내보내기 |
|---|---|---|
| 암호화된 Haiku(`.haiku.json`, Argon2id + AES-256-GCM, 보관함 전체) | ✅ | ✅ |
| Haiku 보관함(`.haiku`, 백업 또는 다른 휴대폰) | ✅ | ✅ |
| Bitwarden JSON(평문) | ✅ | ✅ |
| **비밀번호로 보호된** Bitwarden JSON(PBKDF2 또는 Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| 암호화된 Keyra `.mcfexport` | ✅ | ✅(Keyra로 다시 가져오기 가능) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Apple 암호 | ✅ | ✅(브라우저 CSV) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass(CSV) | ✅ | — |
| Aegis(평문 JSON), Google Authenticator(QR) | ✅ | ✅(`otpauth://` 링크) |
| 종이 사본(인쇄용 HTML 페이지, Keyra의 PDF와 비슷) | — | ✅ |

가져오기 전 미리보기(비밀번호 제외), 폴더 자동 생성, 중복 건너뛰기, 평문 파일 삭제
알림. Haiku 고유 유형(Wi-Fi, 서버)은 `haiku-type` 필드 덕분에 Bitwarden을 거쳐 왕복해도
그대로 유지됩니다. 내보낼 때는 마스터 비밀번호를 다시 묻습니다.

### 보안
```text
마스터 비밀번호 ──Argon2id(64 MiB, 3 패스, 4 레인)──► KEK
KEK ──AES-256-GCM──► DEK(무작위 256비트 데이터 키)
DEK ──AES-256-GCM──► 보관함 내용(압축된 JSON)
```
- 마스터 비밀번호는 절대 저장되지 않습니다. 비밀번호를 바꾸면 DEK만 다시 감싸고 전체를 다시 암호화하지 않습니다.
- Argon2id 강도 조절: 가벼움(32 MiB), 표준(64 MiB, Keyra와 동일), 강화(128 MiB · 4 패스).
- Keyra 스타일 **복구 키**: 32자(Crockford base32, 150비트 + 검사), 단 한 번만
  표시; "비밀번호 분실"은 새 키를 만들고 이전 키를 무효화합니다.
- **생체 인증**: DEK는 **Android Keystore**의 AES 키로 암호화되며, 이 키는 강력한 생체 인증을
  통과한 뒤에만 쓸 수 있고 새 지문이 추가되면 무효화됩니다.
- **PIN 코드**(4~12자리)는 Argon2id로 파생, 5번 틀리면 비활성화(조정 가능), "다시 시작한 뒤
  마스터 비밀번호 다시 묻기" 옵션.
- **자동 잠금**(즉시부터 4시간까지, 또는 안 함), 백그라운드에서도 작동; **클립보드**는
  30초 후 지워지고(조정 가능) *민감한 정보*로 표시(Android 13+ 미리보기에서 숨김); **보안 화면**(스크린샷 불가,
  최근 앱에서 미리보기 숨김); 앱 폴더의 Android 백업 비활성화; *release* APK
  (디버깅 불가).
- 잠글 때 자동 **백업**(보관함이 바뀐 경우)을 `Documents/Haiku/Sauvegardes`에 저장, 10개 보관
  (조정 가능); 백업은 **새** 보관함으로 복원되며 아무것도 덮어쓰지 않습니다.

### 개인화: 스튜디오와 모듈
- **9가지 테마**(Haïku, Sumi-e, Sakura, 네온, 매트릭스, 솔라, 빙하, 종이, 모노크롬), 다크, AMOLED,
  라이트, 글래스 모드, 자유로운 강조 색상 2가지(또는 무작위 조합), 코드로 테마 공유.
- **움직이는 배경**: 오로라, 별자리(손가락에 반응), 수묵(터치할 때마다 한 방울), 벚꽃
  꽃잎, 레트로 그리드, 물결, 코드 비; 강도, 종이 질감, 후광.
- **8가지 글꼴**(Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), 모서리 둥글기,
  글래스 흐림, 밀도, 목록을 카드 / 행 / 그리드로, 이니셜 또는 아이콘.
- 조정 가능한 **GSAP 애니메이션**(없음, 부드럽게, 보통, *화려하게*): 俳句 도장 인트로, 잠금 화면에 붓으로 그리는
  엔소와 잠금 해제 때 돌아가는 자물쇠, 해독하듯 나타나는 비밀
  (ScrambleText), 입자 폭발(Physics2D), 스스로 재정렬되는 목록(Flip), 끌어서 옮기는 시트와 행
  (Draggable + Inertia), 3D로 기울어지는 카드, 선택 가능한 페이지 전환(슬라이드, 확대, 3D 뒤집기,
  커튼, 흐림).
- **모듈**: 각 기능(2FA, 생성기, 상태, Send, 폴더, 기록, 휴지통, 가져오기/내보내기,
  백업)을 켜고 끌 수 있고, **탭 바**를 직접 구성하며(최대 5개, 드래그로 정렬), **홈**은
  순서를 바꿀 수 있는 위젯으로 구성됩니다(오늘의 하이쿠, 검색, 바로 가기, 즐겨찾기, 2FA 코드, 상태 점수,
  최근 항목, 빠른 생성기, 통계, 폴더, 태그 클라우드).
- **명령 팔레트**(헤더에서 아래로 스와이프하거나 키보드에서 Ctrl+K)와 키보드 단축키
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## 알려진 제한 사항
- 동기화, 조직, 긴급 접근은 없습니다(이것들은 Bitwarden 서버의 기능입니다): Keyra처럼
  로컬 보관함입니다. 휴대폰을 바꿀 때는 `.haiku` 백업이나 암호화 내보내기를 쓰세요.
- Bitwarden 내보내기에 들어 있는 패스키는 가져오지 않습니다: Haiku는 패스키 공급자가 아닙니다.
- 인터페이스는 프랑스어로만 제공됩니다(Keyra는 17개 언어 지원).
- 자동 완성은 Haiku 화면을 거칩니다(키보드 안의 인라인 제안 없음): 탭이 한 번 더 필요하지만,
  시스템에 비밀이 평문으로 남는 일은 없습니다.
- `.kdbx` 파일은 바로 읽지 못합니다: KeePassXC / KeePass에서 CSV나 XML로 내보내세요.
- 강도 추정은 오프라인으로 계산한 근사치입니다.

## 직접 빌드하기(Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # 핵심 테스트(암호화, 가져오기/내보내기, 2FA…)
npm run android:add      # android/ 폴더가 없을 때만
npm run sync             # 웹 빌드 + Android 프로젝트로 복사 + Haiku 설정
npm run open             # Android Studio 열기(또는: cd android && gradlew assembleRelease)
```
APK는 GitHub Actions에서 만든 것과 마찬가지로 `signing/`의 키로 서명되므로, 서로 덮어써서
설치할 수 있습니다.

## 코드 구성
```text
src/core/      암호화, 보관함, 가져오기/내보내기 모듈, TOTP, 생성기, 강도, 감사, Send, SSH
src/views/     화면(홈, 보관함, 항목, 편집, 2FA, 생성기, 상태, Send, 스튜디오, 모듈…)
src/ui/        GSAP 효과, 움직이는 배경, 테마, 컴포넌트, 잠금 화면
scripts/       Android 프로젝트 설정, 네이티브 Java 플러그인(Keystore 생체 인증, 보안 화면, 자동 완성), 아이콘
tests/         Node 테스트(실제 Keyra 내보내기 사용)와 Playwright 인터페이스 테스트
```

## 크레딧
Haiku는 독자적으로 다시 작성한 앱입니다: Bitwarden과 Keyra의 **기능**을 재현하고 그
**내보내기 형식**을 읽고 쓰지만, 코드는 가져다 쓰지 않았습니다. [GSAP](https://gsap.com)(Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm)(MIT), [jsQR](https://github.com/cozmo/jsQR)(Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator)(MIT),
[EFF](https://www.eff.org/dice) 단어 목록(CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(Lexique 3.81에서 파생), 그리고 Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho,
JetBrains Mono 글꼴(SIL Open Font License)에 감사드립니다.
