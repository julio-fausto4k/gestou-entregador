# Resposta ao Guideline 2.1 — Information Needed (conta nova)

Na primeira submissão (set/2026) a Apple pediu o questionário padrão de conta com
histórico curto de revisão. Não era defeito do app: nenhum item citava crash, acesso
bloqueado ou política de privacidade.

Duas coisas importantes:

- A resposta vai **nos dois lugares**: no Resolution Center *e* colada no campo **Notas**
  da seção de informações do revisor, como o próprio texto deles pede.
- O risco escondido é a **Guideline 3.2**: se a resposta sugerir que o app é ferramenta
  interna de uma empresa, eles exigem distribuição privada pelo Apple Business Manager.
  Por isso o item 2 abre deixando claro que o gestou é plataforma comercial com muitos
  restaurantes clientes independentes, e que qualquer entregador de qualquer um deles
  usa o app.

O vídeo (item 1) tem que ser gravado em iPhone físico, começando pelo lançamento do app.

## Texto (trocar `<CÓDIGO>`, `<MODELO>` e `<VERSÃO iOS>`)

```
Thank you for reviewing our app. Please find the requested information below.

1. SCREEN RECORDING

A screen recording is attached. It was captured on a physical <MODELO> running
iOS <VERSÃO iOS>, and begins with launching the app. It shows the typical user
flow: entering the pairing code, the delivery list loading from a demo store,
starting a shift (including the iOS location permission prompt), opening the
route in Maps, and marking a delivery as "out for delivery" and then "delivered".

Regarding the specific flows mentioned in your request:

- Account registration / login / account deletion: the app does NOT offer account
  registration or account creation. Delivery drivers never sign up inside the app.
  A restaurant that subscribes to our platform registers its own driver in its web
  dashboard and generates a pairing code for that driver's phone. Because the app
  does not support account creation, there is no account deletion flow. The driver
  can, however, disconnect at any time using "Desconectar deste celular"
  (Disconnect this phone) at the bottom of the main screen, which permanently
  revokes that device's token.
- User-generated content: the app has none. Drivers cannot post, comment, upload
  or share anything. There is no messaging, no profile and no public content, so
  content reporting and blocking mechanisms do not apply.
- Paid content or features: there is none. The app is free and contains no in-app
  purchases, subscriptions or paywalled features.

2. PURPOSE AND TARGET AUDIENCE

Gestou Entregador is a free, public app for delivery drivers ("motoboys") who
deliver food orders for restaurants in Brazil.

Important clarification regarding Guideline 3.2: this is NOT an internal app for
one specific company. It is the driver-side app of "gestou", a commercial SaaS
platform for restaurant order management. Our platform is used by many independent
restaurants, each one a separate paying customer, and new restaurants subscribe
continuously. Any delivery driver working for any of those restaurants can
download this app from the App Store and connect to the restaurant that hired
them. The audience is open-ended and not a fixed set of employees of a single
organization, which is why we distribute it publicly on the App Store rather than
as a custom app.

The problem it solves: today, small restaurants send delivery addresses to their
drivers by phone, paper or messaging apps. Once the driver leaves, the restaurant
has no idea where the order is, and cannot answer the customer who calls asking
when the food will arrive.

The value it provides:
- The driver gets each delivery on the phone, with full address and reference
  point, one tap to open the route in Maps, the customer's phone number, and how
  much to collect at the door (or a clear notice that the order was already paid
  online).
- The restaurant sees the driver's position on a live map while the driver is on
  shift, so it can tell the customer where the order is.
- The driver updates the order status ("out for delivery", "delivered") directly
  from the app, which keeps the restaurant's order board accurate without phone
  calls.

3. SETUP AND ACCESS INSTRUCTIONS

The app does not use a username/password login. Access is granted by the
restaurant through a PAIRING CODE typed on the first screen. We provide a
permanent demo code below. Because App Store Connect requires both fields, we put
the same code in the username and password fields of the App Review Information
section.

    PAIRING CODE: <CÓDIGO>

Steps:
1. Open the app.
2. Type the code above in the code field and tap "Conectar" (Connect).
3. The app connects to a demo restaurant ("Four Burger") with sample deliveries
   already assigned to a demo driver.
4. Tap "Iniciar turno" (Start shift). iOS will ask for location permission —
   please choose "Allow While Using App", and "Change to Always" if iOS asks
   again. The shift indicator turns green.
5. On any delivery you can tap "Rota" (opens Maps with the destination loaded),
   "Ligar" (call the customer), "Saí pra entrega" (out for delivery) and
   "Entreguei" (delivered).
6. Tap "Encerrar turno" (End shift) to stop location updates.

No sample files are required.

4. EXTERNAL SERVICES USED

- Our own backend API (gestou / pdv360-api), hosted on Render. It provides all
  app data: pairing, deliveries, order status updates and driver position. The app
  authenticates to it with a per-device token issued at pairing time.
- Apple Maps / Google Maps, opened through a standard universal link when the
  driver taps "Rota". We do not embed a maps SDK; we hand the destination to the
  device's maps app.
- Capacitor (open-source framework) with the open-source plugin
  @capacitor-community/background-geolocation, used for location updates while the
  driver is on shift.

We do not use payment processors inside the app (payments are handled by the
restaurant, outside this app), and we use no AI services, no advertising SDKs and
no third-party analytics.

5. REGIONAL DIFFERENCES

There are none. The app behaves identically everywhere. Its interface is in
Brazilian Portuguese and its availability is currently limited to Brazil, because
the restaurants using our platform operate in Brazil.

6. REGULATED INDUSTRY / PROTECTED THIRD-PARTY MATERIAL

The app does not operate in a regulated industry and contains no protected
third-party material. The information shown in the app (orders, addresses,
customer phone numbers) belongs to the restaurant that hired the driver, and the
app only reaches it after that restaurant explicitly authorizes the device through
the pairing code it generates. Location data handling is described in our privacy
policy: https://cardapio.gestou.online/privacidade
```
