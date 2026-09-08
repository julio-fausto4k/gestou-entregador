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

## Campos de "Informações para a equipe de revisão"

O app **não tem usuário e senha** — a credencial é o código de pareamento. Mas o
App Store Connect exige os dois campos quando "Início de sessão obrigatório" está
marcado, e desmarcar seria mentira: o revisor precisa de credencial pra entrar.

Solução: **deixe marcado** e ponha o código nos dois campos. As notas explicam.

| Campo | Valor |
| --- | --- |
| Início de sessão obrigatório | marcado |
| Nome do usuário | o código de pareamento |
| Senha | o mesmo código |
| Informações de contato | pessoa real que responda a Apple, com telefone e e-mail |
| Anexo | não precisa |

## Texto para o campo Notas

Em inglês: a App Review é internacional e o revisor pode não ler português.
Troque `<CÓDIGO>` pelo código real.

```
IMPORTANT: this app does not use a username/password login. Access is granted by
the store the driver works for, through a PAIRING CODE typed on the first screen.
The code below is a permanent demo code. We put it in both the username and the
password fields because App Store Connect requires both.

PAIRING CODE: <CÓDIGO>

Steps:
1. Open the app.
2. Type the code above in the code field and tap "Conectar" (Connect).
3. You are now connected to a demo store ("Four Burger") with sample deliveries
   assigned to a demo driver.
4. Tap "Iniciar turno" (Start shift). iOS will ask for location permission —
   please choose "Allow While Using App", and "Change to Always" if iOS asks again.
5. On any delivery you can tap "Rota" (opens Maps with the destination loaded),
   "Ligar" (call the customer), "Saí pra entrega" (out for delivery) and
   "Entreguei" (delivered).

Why the app needs background location:
While a driver is on shift, the store must see the driver's position on a live map
to know when the order will arrive and to tell the customer where the driver is.
The driver keeps the phone in a pocket or on a motorcycle mount, so the position
must keep updating with the screen locked. Tracking happens only between
"Iniciar turno" and "Encerrar turno", which the driver controls; nothing is sent
outside a shift. This is disclosed in our privacy policy:
https://cardapio.gestou.online/privacidade

The app interface is in Portuguese (Brazil) because it is used by delivery drivers
in Brazil.
```

## Lançamento da versão

Na primeira publicação, escolha **"Lançar esta versão manualmente"**. A aprovação da
Apple costuma sair em horário comercial da Califórnia — de madrugada aqui. Com
lançamento automático o app entra no ar sozinho nesse momento, sem ninguém por perto
pra conferir. No manual, você aperta o botão quando quiser.
