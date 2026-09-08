# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## O que é

App do motoboy do **gestou. cardápio**. O entregador pareia o celular com a loja por um
código de 6 letras, inicia o turno (GPS em background) e recebe as entregas atribuídas a
ele — com rota no Google Maps, telefone do cliente e quanto cobrar. Os botões
"Saí pra entrega" e "Entreguei" movem o pedido no kanban da loja.

Projeto e commits em **português (pt-BR)**. A v1.0.4 existiu só pra corrigir mojibake —
cuidado com encoding ao editar strings acentuadas.

## Arquitetura

**Todo o app é `www/index.html`** — um único arquivo com HTML, CSS e JS inline. Não há
bundler, framework, build step do lado web, testes nem linter. Editar esse arquivo *é*
mexer no app.

Capacitor 7 empacota esse HTML como app nativo:

- `www/` → fonte da verdade do web app.
- `android/` → projeto nativo, versionado no git.
- `ios/` → projeto nativo, **ainda não commitado** (adicionado depois do v1.0.5).
- `android/app/src/main/assets/public/` e `ios/App/App/public/` → cópias **geradas** por
  `cap sync`. Nunca edite; edite `www/` e sincronize.

Estado do app vive em `localStorage` (`ge_token`, `ge_store`, `ge_courier`) — não há
backend local nem banco.

### API

Backend externo: `https://pdv360-api.onrender.com` (repo `pdv360-api`), rotas
`/courier-app/*`, autenticadas pelo header `X-Courier-Token`.

| Rota | Uso |
| --- | --- |
| `POST /courier-app/claim` | troca o código de pareamento por um token permanente |
| `POST /courier-app/ping` | posição GPS (throttle de 10s no cliente); devolve `open_orders` |
| `GET /courier-app/orders` | entregas do motoboy (polling de 15s enquanto visível) |
| `POST /courier-app/orders/:id/dispatched` \| `/delivered` | move o pedido no kanban |

Qualquer `401` desloga: para o turno, limpa o `localStorage` e volta pra tela de pareamento.

### GPS em background

Plugin `@capacitor-community/background-geolocation`. O plugin é resolvido **na hora do
uso** (`getBG()`), não no load — evita corrida com a injeção do runtime do Capacitor.

As permissões de localização do Android **não estão** no `AndroidManifest.xml` do app;
vêm do manifest do plugin por merge (`ACCESS_FINE/COARSE_LOCATION`,
`FOREGROUND_SERVICE_LOCATION`, `POST_NOTIFICATIONS`). No iOS ficam no
`ios/App/App/Info.plist` (as duas chaves `NSLocation*UsageDescription` + `UIBackgroundModes: location`).

Passar `backgroundMessage` no `addWatcher` é o que liga o modo background nas duas
plataformas — no iOS o plugin então chama `requestAlwaysAuthorization()` e liga
`allowsBackgroundLocationUpdates`. Mas o comportamento visível difere, e os textos da
UI são escolhidos por `plat()` / `IOS` em `www/index.html`:

| | Android | iOS |
| --- | --- | --- |
| Permissão que basta | "durante o uso do app" | precisa chegar em **Sempre** |
| Como o usuário chega lá | um diálogo só | iOS dá "ao usar o app" primeiro e oferece o upgrade pra Sempre depois de um tempo |
| Sinal de que está rastreando | notificação fixa | seta azul na barra de status |
| Caminho nos ajustes | Configurações > Apps > ... > Permissões | Ajustes > Gestou Entregador > Localização |

Ao mexer nesses textos, lembre que o motoboy lê isso na rua, com pressa.

## Comandos

```bash
npm run sync                  # cap sync android (só Android)
npx cap sync ios              # iOS: copia www/ e roda pod install

cd android && ./gradlew assembleDebug     # APK de teste
cd android && ./gradlew bundleRelease     # AAB (assina só com as envs do keystore)

xcodebuild -workspace ios/App/App.xcworkspace -scheme App \
  -destination 'generic/platform=iOS Simulator' build
```

Sempre rode o `cap sync` da plataforma depois de mexer em `www/` — senão o build usa a
cópia antiga.

## Release

Android é automático: suba `version` no `package.json`, commite, e crie a tag.

```bash
git tag v1.0.X && git push && git push --tags
```

O workflow `.github/workflows/build.yml` builda e publica no Releases o
`GestouEntregador.apk` (debug, pra instalar direto no aparelho) e — quando os secrets
`ANDROID_KEYSTORE_B64`/`ANDROID_KEYSTORE_PASSWORD` existem — o `GestouEntregador.aab`
assinado pra Google Play. A assinatura de release é lida de env vars em
`android/app/build.gradle`; sem elas o `bundleRelease` sai sem assinar.

iOS não tem CI: Archive e upload são manuais pelo Xcode
(`ios/App/App.xcworkspace` → "Any iOS Device" → Product > Archive).

O app é **só de iPhone** (`TARGETED_DEVICE_FAMILY = "1"`) e declara
`ITSAppUsesNonExemptEncryption = false` (só HTTPS, criptografia isenta) pra não
travar cada envio na pergunta de export compliance.

**A App Review não passa da primeira tela sem um código.** O `pdv360-api` já tem um
código de demonstração fixo pra isso (envs `COURIER_DEMO_CODE` e `COURIER_DEMO_TENANT`,
ramo de demo no `POST /courier-app/claim`). O passo a passo e o texto pronto das notas
estão em `docs/app-review-notes.md`. O código não entra no repositório — este repo é
público.

### Versão mora em 4 lugares

Todos estão em **1.0.5**. Ao subir de versão, atualize os quatro juntos — nada valida
isso, e o pill da tela já ficou uma versão atrás uma vez:

| Onde | Formato |
| --- | --- |
| `package.json` → `version` | `1.0.5` |
| `android/app/build.gradle` → `versionCode` / `versionName` | `5` / `1.0.5` |
| `www/index.html` → pill `#verPill` (linha ~73) | `v1.0.5` |
| `ios/.../project.pbxproj` → `MARKETING_VERSION` / `CURRENT_PROJECT_VERSION` | `1.0.5` / `5` (2 ocorrências cada: Debug e Release) |

### O appId difere entre plataformas (proposital)

Decisão do dono do projeto: fica assim, não unifique.

- Android publicado na Play: **`online.gestou.entregador`** (em `android/app/build.gradle`).
  Trocar isso quebra a atualização do app já publicado.
- iOS: **`com.gestou.entregador`** (`PRODUCT_BUNDLE_IDENTIFIER`), team `8XB22H74Q6`.
- `capacitor.config.json` na raiz usa `com.gestou.entregador`. `cap sync` copia esse
  arquivo pra dentro das plataformas, mas **não** reescreve o `applicationId` do Gradle
  nem o bundle id do Xcode — por isso cada plataforma mantém o seu.
