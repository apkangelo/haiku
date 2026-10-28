<div align="center">

[English](README_en.md) · [简体中文](README_zh.md) · [繁體中文](README_zh-TW.md) · [日本語](README_ja.md) · [한국어](README_ko.md) · [Türkçe](README_tr.md) · [Русский](README_ru.md) · [Tiếng Việt](README_vi.md) · [ไทย](README_th.md) · [Deutsch](README_de.md) · [Español](README_es.md) · [Français](../../README.md) · [Українська](README_uk.md) · [Polski](README_pl.md) · **Português (Brasil)** · [العربية](README_ar.md) · [فارسی](README_fa.md) · [Bahasa Indonesia](README_id.md) · [Italiano](README_it.md)
· [Ελληνικά](README_el.md) · [മലയാളം](README_ml.md) · [Roman Urdu](README_ur-Latn.md) · [Română](README_ro.md)

</div>

<p align="center"><img src="../../scripts/icons/icon-512.png" width="120" alt="Haiku"></p>

# Haiku

Gerenciador de senhas **offline** para Android: os recursos do [Bitwarden](https://bitwarden.com) e do
[Keyra](https://github.com/Tahir-1907/Keyra) em um app para celular, com animações GSAP por toda parte e uma interface
totalmente personalizável por módulos. Sem conta, sem servidor, sem telemetria: o cofre nunca sai
do celular.

Empacotado como APK com [Capacitor](https://capacitorjs.com/), compilado automaticamente pelo GitHub Actions e pensado para o
Galaxy Z Fold (dobrado: barra de abas; aberto: trilho à esquerda e o item aberto à direita).

## Baixar o APK

➡️ **[Baixar o Haiku (`haiku.apk`)](https://github.com/apk-fold/haiku/raw/main/haiku.apk)** — o arquivo também está na
aba *Releases*, com sua soma SHA-256.

1. Baixe `haiku.apk` no celular e abra.
2. Permita a instalação a partir desta fonte se o Android pedir e toque em **Instalar**.
3. Na primeira abertura: *Criar meu cofre* (ou *Restaurar um backup*).

Todas as versões são assinadas com **a mesma chave** (pasta `signing/`): uma atualização é instalada por cima da
anterior sem desinstalar, portanto sem perder o cofre. Essa chave é pública no repositório: instale o Haiku
somente a partir deste repositório. Para uma chave privada, adicione os secrets `HAIKU_KEYSTORE_B64`, `HAIKU_KEYSTORE_PASSWORD` e
`HAIKU_KEY_PASSWORD` em *Settings › Secrets › Actions* (trocar de chave exige desinstalar: faça antes um
backup).

## O que o Haiku faz

### Cofre
- **7 tipos de item**: login, cartão de pagamento (cartão 3D que vira para mostrar o código de segurança), identidade, nota
  segura, chave SSH (geração Ed25519 no formato OpenSSH), rede Wi-Fi (compartilhamento por QR code), servidor (cópia do
  comando `ssh`/`sftp`).
- Vários **endereços por login** com regra de detecção (domínio, host, exato, expressão regular…),
  **campos personalizados** (texto, oculto, sim/não), **anexos** criptografados (10 MB), **notas**.
- **Pastas** (cor, ordem arrastando), **etiquetas** (20 por item, regras do Keyra), **favoritos**.
- **Busca** sem acentos nem maiúsculas, `#étiquette`, `#"deux mots"`, `type:carte`, `is:fav`, `has:totp`, `in:dossier`.
- **Histórico**: 20 versões por item, restauráveis com comparação antes/depois; 10 senhas antigas.
- **Lixeira** com limpeza automática (7, 30, 90 dias, 1 ano ou nunca), **histórico geral** das alterações.
- **Pedir de novo a senha mestra** por item (como no Bitwarden), **seleção múltipla** (mover, favoritos,
  lixeira), **duplicar**, gestos: deslizar para a direita = copiar, para a esquerda = favorito, toque longo = menu.
- **Vários cofres** independentes, cada um com sua senha.

### Preenchimento automático
- O Haiku é um **serviço de preenchimento automático do Android** (Configurações › Preenchimento automático): em um app ou um
  navegador, toque no campo e depois em "Preencher com o Haiku", desbloqueie (biometria, PIN ou senha) e escolha
  o login. As correspondências seguem as regras do Bitwarden (domínio, host, `androidapp://paquet`…), e
  a associação com um app pode ser lembrada com um toque.
- O código 2FA do login é copiado automaticamente para a próxima etapa.
- Depois de entrar, o Android oferece **salvar** (ou atualizar) o login no Haiku.
- Chrome: *Configurações › Preenchimento automático › Usar outro serviço*.

### Autenticador (2FA)
- Códigos **TOTP** (SHA-1/256/512, 6 ou 8 dígitos, período livre), **HOTP** e **Steam Guard**, ao vivo com anel de
  contagem regressiva e o próximo código.
- Adicionar **escaneando um QR code** (câmera ou captura de tela), chave manual ou link `otpauth://`.
- Importação da exportação do **Google Authenticator** ("Transferir contas") e do **Aegis**.

### Gerador
- Senhas (de 5 a 128 caracteres, mínimos de dígitos/símbolos, caracteres ambíguos excluídos, símbolos à escolha),
  **frases-senha** (listas de 7.776 palavras em francês e em inglês), **nomes de usuário** (palavras, aleatório, endereço
  `moi+xxxx@gmail.com`, domínio catch-all), **códigos PIN**. Entropia exata e tempo para quebrar exibidos.
- Histórico dos valores gerados, criptografado no cofre.

### Saúde do cofre
Pontuação de 0 a 100 e relatórios: senhas **fracas**, **reutilizadas**, **antigas** (mais de um ano), **ausentes**,
**sites em http://**, **cartões vencidos**, e sob demanda: **vazamentos** (Have I Been Pwned, k-anonimato: só saem
5 caracteres do hash SHA-1) e **2FA disponível mas não ativado** (lista do 2fa.directory).

### Send (compartilhamento criptografado)
O equivalente offline do Bitwarden Send: um texto ou um arquivo (15 MB) vira uma **página HTML autônoma** criptografada
(AES-256-GCM, PBKDF2-SHA256 com 600.000 iterações), com data de validade. O destinatário abre em qualquer
navegador com a senha que você passa por outro canal.

### Importação / exportação
| Formato | Importação | Exportação |
|---|---|---|
| Haiku criptografado (`.haiku.json`, Argon2id + AES-256-GCM, cofre inteiro) | ✅ | ✅ |
| Cofre Haiku (`.haiku`, backup ou outro celular) | ✅ | ✅ |
| Bitwarden JSON (sem criptografia) | ✅ | ✅ |
| Bitwarden JSON **protegido por senha** (PBKDF2 ou Argon2id) | ✅ | ✅ |
| Bitwarden CSV | ✅ | ✅ |
| Keyra `.mcfexport` criptografado | ✅ | ✅ (pode ser reimportado no Keyra) |
| Keyra CSV | ✅ | ✅ |
| KeePassXC CSV, KeePass 2 XML | ✅ | — |
| Chrome / Edge / Brave / Opera, Firefox, Safari / Senhas da Apple | ✅ | ✅ (CSV de navegador) |
| LastPass, 1Password, Dashlane, Proton Pass, NordPass (CSV) | ✅ | — |
| Aegis (JSON sem criptografia), Google Authenticator (QR) | ✅ | ✅ (links `otpauth://`) |
| Cópia em papel (página HTML para imprimir, como o PDF do Keyra) | — | ✅ |

Prévia antes da importação (sem as senhas), pastas criadas automaticamente, duplicatas ignoradas, lembrete para apagar
os arquivos sem criptografia. Os tipos próprios do Haiku (Wi-Fi, servidor) fazem a ida e volta pelo Bitwarden graças a um campo
`haiku-type`. A exportação pede de novo a senha mestra.

### Segurança
```text
senha mestra ──Argon2id (64 MiB, 3 passagens, 4 vias)──► KEK
KEK ──AES-256-GCM──► DEK (chave de dados aleatória de 256 bits)
DEK ──AES-256-GCM──► conteúdo do cofre (JSON compactado)
```
- A senha mestra nunca é armazenada. Trocar a senha só reembala a DEK, sem recriptografar tudo.
- Resistência do Argon2id ajustável: leve (32 MiB), padrão (64 MiB, como o Keyra), reforçada (128 MiB · 4 passagens).
- **Chave de recuperação** no estilo do Keyra: 32 caracteres (base32 de Crockford, 150 bits + verificação), exibida uma única
  vez; "esqueci a senha" cria uma nova chave e invalida a anterior.
- **Biometria**: a DEK é criptografada por uma chave AES do **Android Keystore** que só pode ser usada após uma
  autenticação biométrica forte, invalidada se uma nova digital for adicionada.
- **Código PIN** (de 4 a 12 dígitos) derivado com Argon2id, desativado após 5 erros (ajustável), opção "pedir de novo a
  senha mestra depois de reiniciar".
- **Bloqueio automático** (de imediato a 4 h, ou nunca), em segundo plano também; **área de transferência** limpa após
  30 s (ajustável) e marcada como *sensível* (oculta na prévia do Android 13+); **tela segura** (sem capturas de tela,
  prévia oculta nos apps recentes); backup do Android da pasta do app desativado; APK *release*
  (não depurável).
- **Backups** automáticos ao bloquear (se o cofre mudou) em `Documents/Haiku/Sauvegardes`, 10 mantidos
  (ajustável); eles são restaurados como um **novo** cofre, nada é sobrescrito.

### Personalização: o Studio e os módulos
- **9 temas** (Haïku, Sumi-e, Sakura, Neon, Matrix, Solar, Geleira, Papel, Monocromático), modos escuro, AMOLED,
  claro e vidro, duas cores de destaque livres (ou uma harmonia aleatória), compartilhamento de tema por código.
- **Fundos animados**: aurora, constelação (reage ao dedo), tinta sumi-e (uma gota a cada toque), pétalas de
  sakura, grade retrô, ondas, chuva de código; intensidade, granulado de papel, halo.
- **8 fontes** (Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho, JetBrains Mono), arredondamento,
  desfoque do vidro, densidade, lista em cards / linhas / grade, iniciais ou pictogramas.
- **Animações GSAP** ajustáveis (nenhuma, suaves, normais, *malucas*): intro com o carimbo 俳句, ensō traçado a pincel na
  tela de bloqueio e cadeado que gira ao desbloquear, revelação dos segredos como se fossem descriptografados
  (ScrambleText), explosões de partículas (Physics2D), listas que se reorganizam (Flip), painéis e linhas que se arrastam
  (Draggable + Inertia), cards que se inclinam em 3D, transições de página à escolha (deslizar, zoom, giro 3D,
  cortina, desfoque).
- **Módulos**: cada recurso (2FA, gerador, saúde, Send, pastas, histórico, lixeira, importação/exportação,
  backups) pode ser ligado ou desligado; a **barra de abas** é montada por você (no máximo 5, ordem arrastando); o **início**
  é feito de widgets reordenáveis (haiku do dia, busca, atalhos, favoritos, códigos 2FA, pontuação de saúde,
  recentes, gerador rápido, estatísticas, pastas, nuvem de etiquetas).
- **Paleta de comandos** (deslizar para baixo no cabeçalho, ou Ctrl+K no teclado) e atalhos de teclado
  (Ctrl+N, Ctrl+L, Ctrl+G, Ctrl+F, Ctrl+,).

## Limitações conhecidas
- Sem sincronização, organizações nem acesso de emergência (são recursos do servidor Bitwarden): é
  um cofre local, como o Keyra. Para passar de um celular para outro: backup `.haiku` ou exportação criptografada.
- As passkeys de uma exportação do Bitwarden não são importadas: o Haiku não é um provedor de passkeys.
- Interface somente em francês (o Keyra oferece 17 idiomas).
- O preenchimento automático passa pela tela do Haiku (sem sugestões embutidas no teclado): um toque
  a mais, mas nenhum segredo fica guardado sem criptografia para o sistema.
- Arquivos `.kdbx` não são lidos diretamente: exporte-os para CSV ou XML no KeePassXC / KeePass.
- A estimativa de força é feita offline e é aproximada.

## Compilar você mesmo (Windows, Android Studio, JDK 21)
```bash
npm install
npm test                 # testes do núcleo (criptografia, importações/exportações, 2FA…)
npm run android:add      # só se a pasta android/ não existir
npm run sync             # build web + cópia para o projeto Android + configurações do Haiku
npm run open             # abre o Android Studio (ou: cd android && gradlew assembleRelease)
```
O APK é assinado com a chave de `signing/`, assim como o do GitHub Actions: um pode ser instalado por cima do
outro.

## Organização do código
```text
src/core/      criptografia, cofre, importadores/exportadores, TOTP, gerador, força, auditoria, Send, SSH
src/views/     telas (início, cofre, item, edição, 2FA, gerador, saúde, Send, Studio, módulos…)
src/ui/        efeitos GSAP, fundos animados, tema, componentes, tela de bloqueio
scripts/       configurações do projeto Android, plugin nativo em Java (biometria Keystore, tela segura, preenchimento automático), ícones
tests/         testes em Node (com uma exportação real do Keyra) e teste de interface com Playwright
```

## Créditos
O Haiku é uma reescrita independente: ele reproduz os **recursos** do Bitwarden e do Keyra e lê/grava
os **formatos de exportação** deles, sem reaproveitar o código. Obrigado ao [GSAP](https://gsap.com) (Webflow), [Lucide](https://lucide.dev)
(ISC), [hash-wasm](https://github.com/Daninet/hash-wasm) (MIT), [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), à lista de palavras da
[EFF](https://www.eff.org/dice) (CC BY 3.0), ao [diceware-wordlists-fr](https://github.com/mbelivo/diceware-wordlists-fr)
(derivado do Lexique 3.81) e às fontes Sora, Manrope, Outfit, Space Grotesk, Syne, Unbounded, Shippori Mincho e
JetBrains Mono (SIL Open Font License).
