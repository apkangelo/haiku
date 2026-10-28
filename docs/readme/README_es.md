<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · **Español** · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · [Português (Brasil)](README_pt-BR.md) · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Gestor de contraseñas **sin conexión** para Android: las funciones de [Bitwarden](https://bitwarden.com) y de
[Keyra](https://github.com/Tahir-1907/Keyra) en una app móvil, con animaciones GSAP por todas partes y una interfaz
totalmente personalizable por módulos. Sin cuenta, sin servidor, sin telemetría: la bóveda nunca sale
del móvil.

Empaquetado como APK con [Capacitor](https://capacitorjs.com/), compilado automáticamente por GitHub Actions y pensado para el
Galaxy Z Fold (plegado: barra de pestañas; desplegado: riel a la izquierda y ficha abierta a la derecha).

## Descargar el APK

➡️ **[Descargar Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — el archivo también está en la
pestaña *Releases*, con su suma SHA-256.

1. Descarga `haiku.apk` en el móvil y ábrelo.
2. Permite la instalación desde este origen si Android lo pide y pulsa **Instalar**.
3. En el primer inicio: *Crear mi bóveda* (o *Restaurar una copia de seguridad*).

Todas las versiones se firman con **la misma clave** (carpeta `signing/`): una actualización se instala encima de la
anterior sin desinstalar, así que sin perder la bóveda. Esta clave es pública en el repositorio: instala Haiku
solo desde este repositorio. Para una clave privada, añade los secrets `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` y
`HAIKU_KEY_PASSWORD` en *Settings › Secrets › Actions* (cambiar de clave obliga a desinstalar: haz antes una
copia de seguridad).

## Qué hace Haiku

### Bóveda
- **7 tipos de elementos**: inicio de sesión, tarjeta bancaria (tarjeta 3D que se gira para ver el código de seguridad), identidad, nota
  segura, clave SSH (generación Ed25519 en formato OpenSSH), red Wi-Fi (compartir por código QR), servidor (copia del
  comando `ssh`/`sftp`).
- Varias **direcciones por inicio de sesión** con regla de detección (dominio, host, exacta, expresión regular…),
  **campos personalizados** (texto, oculto, sí/no), **adjuntos** cifrados (10 MB), **notas**.
- **Carpetas** (color, orden arrastrando), **etiquetas** (20 por elemento, reglas de Keyra), **favoritos**.
- **Búsqueda** sin acentos ni mayúsculas, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Historial**: 20 versiones por elemento, restaurables con comparación antes/después; 10 contraseñas antiguas.
- **Papelera** con vaciado automático (7, 30, 90 días, 1 año o nunca), **historial global** de cambios.
- **Volver a pedir la contraseña maestra** por elemento (como Bitwarden), **selección múltiple** (mover, favoritos,
  papelera), **duplicar**, gestos: deslizar a la derecha = copiar, a la izquierda = favorito, pulsación larga = menú.
- **Varias bóvedas** independientes, cada una con su contraseña.

### Autocompletado
- Haiku es un **servicio de autocompletado de Android** (Ajustes › Autocompletado): en una app o un
  navegador, toca el campo y luego «Rellenar con Haiku», desbloquea (biometría, PIN o contraseña) y elige
  el inicio de sesión. Las coincidencias siguen las reglas de Bitwarden (dominio, host, `androidapp://paquet`…), y
  la asociación con una app se puede recordar con un toque.
- El código 2FA del inicio de sesión se copia automáticamente para el paso siguiente.
- Tras iniciar sesión, Android propone **guardar** (o actualizar) el inicio de sesión en Haiku.
- Chrome: *Configuración › Autocompletar › Usar otro servicio*.

### Autenticador (2FA)
- Códigos **TOTP** (SHA-1/256/512, 6 u 8 cifras, periodo libre), **HOTP** y **Steam Guard**, en directo con anillo de
  cuenta atrás y código siguiente.
- Añadir **escaneando un código QR** (cámara o captura de pantalla), clave manual o enlace `otpauth://`.
- Importación de la exportación de **Google Authenticator** («Transferir cuentas») y de **Aegis**.

### Generador
- Contraseñas (de 5 a 128 caracteres, mínimos de cifras/símbolos, caracteres ambiguos excluidos, símbolos a elegir),
  **frases de contraseña** (listas de 7776 palabras en francés y en inglés), **nombres de usuario** (palabras, aleatorio, dirección
  `moi+xxxx@gmail.com`, dominio catch-all), **códigos PIN**. Se muestran la entropía exacta y el tiempo de descifrado.
- Historial de los valores generados, cifrado en la bóveda.

### Salud de la bóveda
Puntuación sobre 100 e informes: contraseñas **débiles**, **reutilizadas**, **antiguas** (más de un año), **ausentes**,
**sitios en http://**, **tarjetas caducadas**, y bajo demanda: **filtraciones** (Have I Been Pwned, k-anonimato: solo salen
5 caracteres del hash SHA-1) y **2FA disponible pero sin activar** (lista de 2fa.directory).

### Send (compartir cifrado)
El equivalente sin conexión de Bitwarden Send: un texto o un archivo (15 MB) se convierte en una **página HTML autónoma** cifrada
(AES-256-GCM, PBKDF2-SHA256 con 600 000 iteraciones), con fecha de caducidad. El destinatario la abre en cualquier
navegador con la contraseña que le pasas por otro canal.

### Importar / exportar
| Formato | Importar | Exportar |
|---|---|---|
| Haiku cifrado (`.haiku.json`, Argon2id + AES-256-GCM, toda la bóveda) | ✅ | ✅ |
| Bóveda Haiku (`.haiku`, copia de seguridad u otro móvil) | ✅ | ✅ |
| Bitwarden JSON (sin cifrar) | ✅ | ✅ |
| Bitwarden JSON **protegido con contraseña** (PBKDF2 o Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` cifrado | ✅ | ✅ (se vuelve a importar en Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Contraseñas de Apple | ✅ | ✅ (CSV de navegador) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON sin cifrar), Google Authenticator (QR) | ✅ | ✅ (enlaces `otpauth://`) |
| Copia en papel (página HTML para imprimir, como el PDF de Keyra) | — | ✅ |

Vista previa antes de importar (sin las contraseñas), carpetas creadas automáticamente, duplicados ignorados, recordatorio de borrar
los archivos sin cifrar. Los tipos propios de Haiku (Wi-Fi, servidor) hacen el viaje de ida y vuelta por Bitwarden gracias a un campo
`haiku-type`. La exportación vuelve a pedir la contraseña maestra.

### Seguridad
```text
contraseña maestra ──Argon2id (64 MiB, 3 pasadas, 4 vías)──► KEK
KEK ──AES-256-GCM──► DEK (clave de datos aleatoria de 256 bits)
DEK ──AES-256-GCM──► contenido de la bóveda (JSON comprimido)
```
- La contraseña maestra nunca se guarda. Cambiarla vuelve a envolver la DEK sin volver a cifrarlo todo.
- Resistencia de Argon2id ajustable: ligera (32 MiB), estándar (64 MiB, como Keyra), reforzada (128 MiB · 4 pasadas).
- **Clave de recuperación** al estilo de Keyra: 32 caracteres (base32 de Crockford, 150 bits + control), mostrada una sola
  vez; «contraseña olvidada» crea una clave nueva e invalida la anterior.
- **Biometría**: la DEK se cifra con una clave AES del **Android Keystore** utilizable solo tras una
  autenticación biométrica fuerte, e invalidada si se añade una huella nueva.
- **Código PIN** (de 4 a 12 cifras) derivado con Argon2id, desactivado tras 5 errores (ajustable), opción «volver a pedir la
  contraseña maestra tras reiniciar».
- **Bloqueo automático** (de inmediato a 4 h, o nunca), también en segundo plano; **portapapeles** vaciado tras
  30 s (ajustable) y marcado como *sensible* (oculto en la vista previa de Android 13+); **pantalla segura** (sin capturas,
  vista previa oculta en las apps recientes); copia de seguridad de Android de la carpeta de la app desactivada; APK *release*
  (no depurable).
- **Copias de seguridad** automáticas al bloquear (si la bóveda ha cambiado) en `Documents/Haiku/Sauvegardes`, se guardan 10
  (ajustable); se restauran como una bóveda **nueva**, no se sobrescribe nada.

### Personalización: el Estudio y los módulos
- **9 temas** (Haïku, Sumi-e, Sakura, Neón, Matrix, Solar, Glaciar, Papel, Monocromo), modos oscuro, AMOLED,
  claro y cristal, dos colores de acento libres (o una armonía al azar), compartir el tema mediante un código.
- **Fondos animados**: aurora, constelación (reacciona al dedo), tinta sumi-e (una gota a cada toque), pétalos de
  sakura, cuadrícula retro, ondas, lluvia de código; intensidad, grano de papel, halo.
- **8 fuentes** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), redondeo de esquinas,
  desenfoque del cristal, densidad, lista en tarjetas / filas / cuadrícula, iniciales o pictogramas.
- **Animaciones GSAP** ajustables (ninguna, suaves, normales, *locas*): intro con el sello 俳句, ensō trazado a pincel en
  la pantalla de bloqueo y cerradura que gira al desbloquear, revelado de los secretos como si se descifraran
  (ScrambleText), estallidos de partículas (Physics2D), listas que se reorganizan (Flip), paneles y filas que se arrastran
  (Draggable + Inertia), tarjetas que se inclinan en 3D, transiciones de página a elegir (deslizar, zoom, giro 3D,
  telón, desenfoque).
- **Módulos**: cada función (2FA, generador, salud, Send, carpetas, historial, papelera, importar/exportar,
  copias de seguridad) se activa o desactiva; la **barra de pestañas** se compone a tu gusto (5 como máximo, orden arrastrando); el **inicio**
  está hecho de widgets reordenables (haiku del día, búsqueda, atajos, favoritos, códigos 2FA, puntuación de salud,
  recientes, generador rápido, estadísticas, carpetas, nube de etiquetas).
- **Paleta de comandos** (deslizar hacia abajo en la cabecera, o Ctrl+K con teclado) y atajos de teclado
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Limitaciones conocidas
- Sin sincronización, organizaciones ni acceso de emergencia (son funciones del servidor de Bitwarden): es
  una bóveda local, como Keyra. Para pasar de un móvil a otro: copia de seguridad `.haiku` o exportación cifrada.
- Las claves de acceso (passkeys) de una exportación de Bitwarden no se importan: Haiku no es un proveedor de passkeys.
- Interfaz solo en francés (Keyra ofrece 17 idiomas).
- El autocompletado pasa por la pantalla de Haiku (sin sugerencias en línea en el teclado): un toque
  más, pero ningún secreto se guarda nunca sin cifrar para el sistema.
- Los archivos `.kdbx` no se leen directamente: expórtalos a CSV o XML desde KeePassXC / KeePass.
- La estimación de robustez se hace sin conexión y es aproximada.

## Compilarlo tú mismo (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # pruebas del núcleo (cifrado, importaciones/exportaciones, 2FA…)
npm run android:add      # solo si la carpeta android/ no existe
npm run sync             # build web + copia al proyecto Android + ajustes de Haiku
npm run open             # abre Android Studio (o: cd android && gradlew assembleRelease)
```
El APK se firma con la clave de `signing/`, igual que el de GitHub Actions: los dos se instalan uno encima del
otro.

## Organización del código
```text
src/core/      cifrado, bóveda, importadores/exportadores, TOTP, generador, robustez, auditoría, Send, SSH
src/views/     pantallas (inicio, bóveda, ficha, edición, 2FA, generador, salud, Send, Estudio, módulos…)
src/ui/        efectos GSAP, fondos animados, tema, componentes, pantalla de bloqueo
scripts/       ajustes del proyecto Android, plugin nativo en Java (biometría Keystore, pantalla segura, autocompletado), iconos
tests/         pruebas en Node (con una exportación real de Keyra) y prueba de interfaz con Playwright
```

## Créditos
Haiku es una reescritura independiente: reproduce las **funciones** de Bitwarden y de Keyra y lee/escribe
sus **formatos de exportación**, sin reutilizar su código. Gracias a [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), la lista de palabras
[EFF](https://www.eff.org/dice) (CC BY 3.0), [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(derivada de Lexique 3.81), y a las fuentes Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho y
JetBrains Mono (SIL Open Font License).
