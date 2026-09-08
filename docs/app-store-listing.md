# Ficha da App Store — Gestou Entregador

Textos da página do app no App Store Connect (idioma: Português do Brasil).
O **texto promocional** pode ser trocado a qualquer momento, sem nova revisão; a
**descrição** e as **palavras-chave** só mudam junto com uma nova versão.

## Texto promocional (máx. 170)

```
As entregas da sua loja no bolso do motoboy: endereço, rota, telefone do cliente e quanto cobrar — com a loja acompanhando a posição no mapa em tempo real.
```

## Descrição (máx. 4.000)

```
O Gestou Entregador é o app do motoboy das lojas que usam a plataforma gestou. Ele recebe as entregas direto do balcão e mostra tudo o que o entregador precisa pra rodar: endereço completo com ponto de referência, rota que abre no mapa com o destino já carregado, telefone do cliente e quanto cobrar na porta — ou o aviso de que o pedido já foi pago pelo app.

COMO FUNCIONA

• A loja cadastra o entregador no painel e gera um código de pareamento.
• O entregador digita o código uma única vez e o celular fica conectado à loja.
• Ao iniciar o turno, a loja passa a ver a posição do entregador no mapa de entregas.
• As entregas atribuídas a ele aparecem na tela, com os botões "Saí pra entrega" e "Entreguei", que atualizam o pedido na loja na hora.

LOCALIZAÇÃO EM SEGUNDO PLANO

Com o turno ativo, o app continua enviando a posição mesmo com o aplicativo minimizado ou a tela bloqueada. É isso que permite à loja acompanhar a entrega em tempo real e dizer ao cliente onde o pedido está.

A coleta acontece somente entre "Iniciar turno" e "Encerrar turno" — quem controla é o próprio entregador, e fora do turno nada é enviado. O uso contínuo do GPS em segundo plano pode reduzir a duração da bateria.

PRECISA DE UMA LOJA

Este app não tem cadastro aberto: o acesso é dado pela loja em que o entregador trabalha. Se você é entregador, peça o código de pareamento na loja. Se você tem um restaurante e quer usar, conheça a plataforma em gestou.online.
```

## Palavras-chave (máx. 100)

```
motoboy,entrega,delivery,rota,gps,rastreio,mapa,pedidos,restaurante,cardapio,logistica,turno
```

92 caracteres. Sem espaço depois das vírgulas (espaço conta e não ajuda em nada) e sem
repetir "gestou" nem "entregador" — o nome do app já é indexado por conta própria.

## URLs e demais campos

| Campo | Valor |
| --- | --- |
| URL de suporte | `https://gestou.online/ajuda` |
| URL de marketing | `https://gestou.online` |
| Versão | `1.0.5` (tem que bater com o `CFBundleShortVersionString` do build) |
| Copyright | `2026 4K Enterprise Marketing Direto LTDA` |
| Arquivo de cobertura do app de roteamento | deixar vazio |
| Clipe de app / App para iMessage | não se aplica |

Política de privacidade: `https://cardapio.gestou.online/privacidade` (já cobre este app
nominalmente, incluindo GPS em segundo plano).

## Rótulos de App Privacy

- **Localização Precisa** → Funcionalidade do App → vinculada à identidade → **não** usada
  para rastreamento.
- **Identificadores → ID do Dispositivo** (o token de pareamento) → mesmos termos.
- Nada em publicidade. O app não precisa do prompt de ATT.

## Cuidados ao escrever

- **Nunca citar outra plataforma móvel** (Android, Google Play) na descrição — é motivo
  de rejeição.
- Não prometer recurso que o app não tem. A descrição acima só cita o que existe em
  `www/index.html`.
- A menção explícita ao segundo plano e ao controle pelo turno é proposital: é o que a
  App Review procura num app que rastreia com a tela bloqueada.
