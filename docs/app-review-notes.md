# Notas para a revisão das lojas (App Store / Google Play)

O app abre pedindo um **código de pareamento**. Sem ele o revisor não passa da
primeira tela e o app é reprovado como incompleto (na Apple, Guideline 2.1).

Não existe "criar conta": quem cadastra o entregador é a loja, pelo painel. Por isso
o `pdv360-api` tem um código de demonstração **fixo, que nunca expira** — o ramo de
demo em `app/api/courier_app.py`, no `POST /courier-app/claim`.

## Como funciona

Duas variáveis de ambiente no Render (serviço `pdv360-api`):

| Env | O que é |
| --- | --- |
| `COURIER_DEMO_CODE` | o código que o revisor digita |
| `COURIER_DEMO_TENANT` | slug **ou** subdomínio da loja de demonstração (fourburger) |

Com o código certo, o `claim` ignora a tabela `courier_pairings` (e portanto a
expiração de 15 min), acha a loja pelo slug/subdomínio, cria ou reusa um entregador
de revisão e devolve um token permanente.

> ⚠️ **O código nunca entra neste repositório — ele é público.** Vive só no Render e
> no campo de notas de cada loja.

## Antes de cada envio

1. Conferir no Render que as duas envs estão setadas e que `COURIER_DEMO_TENANT`
   aponta pra uma loja de **teste** (fourburger) — **nunca pra um cliente real**.
   O revisor não só lê: a lista traz nome, endereço e telefone dos clientes, e os
   botões "Saí pra entrega"/"Entreguei" movem os pedidos de verdade no kanban, com
   cashback e evento Open Delivery junto. Apontar a demo pra uma loja em operação
   entrega dado pessoal de terceiros a um revisor externo e mexe no expediente dela.
2. Garantir que existe **pelo menos um pedido em aberto** na fourburger atribuído ao
   entregador de revisão. O `GET /courier-app/orders` filtra por
   `courier_id` + status `CONFIRMED`, `READY` ou `DISPATCHED` — sem isso o revisor
   pareia e vê "Nenhuma entrega atribuída a você agora.", que parece app quebrado.
3. Colar o texto abaixo no campo de notas, trocando o `<CÓDIGO>`.

## Texto para o App Review Notes (Apple) / instruções de acesso (Google)

```
This app is used by delivery drivers of stores that use our restaurant
management platform (gestou). Drivers do not sign up: the store registers the
driver and generates a pairing code for the driver's phone.

To access the app, use this demo pairing code:

    PAIRING CODE: <CÓDIGO>

Steps:
1. Open the app.
2. Type the pairing code above and tap "Conectar".
3. You are now connected to a demo store with a sample delivery assigned.
4. Tap "Iniciar turno" to start the shift. The app will ask for location
   permission — please choose "Allow While Using App", and "Change to Always"
   if iOS asks again.

Why the app needs background location:
While a driver is on shift, the store must see the driver's position on a live
map to know when the order will arrive, and to tell the customer where the
driver is. The driver keeps the phone in a pocket or on a motorcycle mount, so
the position must keep updating with the screen locked. Tracking only happens
between "Iniciar turno" (start shift) and "Encerrar turno" (end shift), which
the driver controls; it never runs outside a shift.
```
