# Gestou Entregador

App do motoboy do **gestou. cardápio**: recebe as entregas da loja, navega até o cliente
e envia a posição GPS em tempo real pro mapa do painel (aba **Entregas**) — inclusive
com a tela desligada (serviço de localização em segundo plano).

## Como funciona

1. A loja cadastra o entregador no painel (Configurações → Entregadores).
2. Na aba **Entregas**, clica **Parear celular** → gera um código de 6 letras (15 min).
3. O motoboy abre o app, digita o código e pronto — token permanente no aparelho.
4. **Iniciar turno** liga o GPS em background (notificação fixa do Android); a loja vê
   a posição ao vivo. As entregas atribuídas a ele aparecem com endereço, rota
   (Google Maps), telefone do cliente e o que cobrar (ou "já pago").
5. Botões **Saí pra entrega** e **Entreguei** movem o pedido no kanban da loja
   (com os mesmos efeitos: cashback, Open Delivery, linha do tempo).

## Stack

Capacitor 7 (Android) + `@capacitor-community/background-geolocation`.
UI em `www/index.html` (sem bundler). API: `pdv360-api` rotas `/courier-app/*`
(auth por header `X-Courier-Token`).

## Release (gera o APK sozinho)

```
# mudanças + suba "version" no package.json
git add -A && git commit -m "..."
git tag v1.0.X && git push && git push --tags
```

→ o GitHub Actions builda e publica o **GestouEntregador.apk** no Releases (~5 min).
No celular: baixar o APK da release e instalar (permitir "fontes desconhecidas").

> O APK do Releases é **debug** (pra teste direto no aparelho). A publicação na
> Google Play usará build assinado (keystore + AAB) — etapa própria.
