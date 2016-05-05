# Steam Trade Offers for Node.js

[![NPM Version](https://img.shields.io/npm/v/steam-tradeoffers.svg)](https://www.npmjs.com/package/steam-tradeoffers "steam-tradeoffers on NPM")
[![NPM Downloads](https://img.shields.io/npm/dm/steam-tradeoffers.svg)](https://www.npmjs.com/package/steam-tradeoffers "steam-tradeoffers on NPM")
[![PayPal Donate Button](https://img.shields.io/badge/donate-paypal-orange.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=DDE8H72QKJHRJ&item_name=node%2dsteam%2dtradeoffers&currency_code=USD "Donate to this project via PayPal")
[![Steam Items Donate Button](https://img.shields.io/badge/donate-steam%20items-yellowgreen.svg)](https://steamcommunity.com/tradeoffer/new/?partner=89647317&token=vmYJwN-y "Donate Steam Items")

`steam-tradeoffers` é uma biblioteca para Node.js escrita em JavaScript. Ela permite vc automatizar as trocas da Steam usando trade offers. Ela foi desenvolvida com [node-steam](https://github.com/seishun/node-steam) em mente, mas não depende diretamente dele. Alguns dos metodos da biblioteca são compilados para Steam Web API.

__Por favor leia o [FAQ](https://github.com/Alex7Kom/node-steam-tradeoffers/wiki/FAQ)__ primeiro caso vc tenha alguma duvida.

Se sua questao nao esta respondida ali, por favor pergunte-a em [https://github.com/steam-forward/node-steam-forum](https://github.com/steam-forward/node-steam-forum), __please do not open an issue here__. Issues are only for bugs and feature requests. <- Bla bla

# Instalação

```
npm install steam-tradeoffers
```

# Uso
Instancie um objeto SteamTradeOffers...

```js
var SteamTradeOffers = require('steam-tradeoffers');
var offers = new SteamTradeOffers();
```

...depois setup session and WebAPI key:

```js
offers.setup({
  sessionID: sessionID,
  webCookie: cookies,
  APIKey: webAPIKey
});
```

Vc pode obter informações de seção com [node-steam](https://github.com/seishun/node-steam) e seu plugin [steam-weblogon](https://github.com/Alex7Kom/node-steam-weblogon).

# Demo

Um [demo bot](https://steamcommunity.com/profiles/76561198049913045/) existe e tambem serve como bot de doação.

[Source code](https://gist.github.com/Alex7Kom/d764ef2925060c4e7c27) (baseado no exemplo `storehouse.js`).

# Exemplos

Vc vai precisar instalar [node-steam](https://github.com/seishun/node-steam), [steam-weblogon](https://github.com/Alex7Kom/node-steam-weblogon), e [steam-web-api-key](https://github.com/Alex7Kom/node-steam-web-api-key) para poder rodar os exemplos.

O arquivo `storehouse.js` contem um exemplo de manusear ofertas de troca recebidas.

O `offerbot.js` é um exemplo de fazer uma oferta de troca.

Na primeira tentativa ambos exemplos irao dar 'crash'. Confira seu email para o codigo Steam Guard e edite um arquivo do exemplo para adiciona-lo, entao rode de novo.

Por favor leia o [FAQ](https://github.com/Alex7Kom/node-steam-tradeoffers/wiki/FAQ) antes de criar um topico de reclamação dos exemp.

# Metodos

Parametro `options` de todos metodos é apenas um objeto. Todas respostas(callbacks) contendo `Error` como primeiro argumento ou `null` se não ocorrerem erros.

## setup(options)

Como observado acima, esse metodo é usado para instalar a web session. Se vc quer operar com ofertas de troca logo apos a inicializacao, faça isso após chamar esse metodo.

Opçoes:

* `sessionID` é uma web session ID valida.
* `webCookie` é uma ordem de cookies.
* `APIKey` é uma Web API key para a conta que vc usa para trocar. API key de outra conta nao funcionará.
* `timeout` (optional) é um numero de milisegundos para aguardar os servers Steam responderem. Padrão é `30000`. Mais informações sobre timeouts podem ser encontradas em [request docs](https://github.com/request/request#timeouts).

`sessionID` e `webCookie` podem ser adquiridas usando [node-steam](https://github.com/seishun/node-steam) com o     [node-steam-weblogon plugin](https://github.com/Alex7Kom/node-steam-weblogon). `APIKey` pode ser obtida usando o [node-steam-web-api-key](https://github.com/Alex7Kom/node-steam-web-api-key).

## loadMyInventory(options, callback)

Carrega seu inventário para o app dado e contexto. Por ex, use 440 e 2 para TF2(Team Fortress2) e 570 e 2 para Dota2. Se obtiver sucesso o segundo argumento para `callback` será uma ordem de objetos de item e o terceiro argumento vai conter objetos de inventário, nao misturado com descrições mas ainda relacionado a multiplas paginas de inventario.

Opçoes:

* `appId` é a Steam AppID
* `contextId` é o inventory context Id
* `language` (optional) linguagem da descrição dos itens
* `tradableOnly` (optional) é um booleano que vai marcar como `true` para retonar apenas itens trocáveis

## loadPartnerInventory(options, callback)

Carrega o inventario do seu parceiro de troca para o app dado e contexto.

Opções:

* `partnerSteamId` é a SteamID do parceiro de troca.  Vc precisa especificar apenas `partnerAccountId` ou `partnerSteamId`.
* `partnerAccountId` é a Steam Account ID do parceiro de troca. Vc precisa especificar apenas`partnerAccountId` ou `partnerSteamId`.
* `appId` é Steam AppID
* `contextId` é o inventory context Id
* `tradeOfferId` (optional) is needed to load private inventory of the trade partner for received trade offer (IGNORAR E PERGUNTAR)
* `language` (optional) é a linguagem da descrição dos itens

## makeOffer(options[, callback])

Faz uma oferta de troca ao parceiro.

Opções:

* `partnerAccountId` ou `partnerSteamId`, vc só precisa usar 1 desses.
* `accessToken` (optional) é um token da Trade URL publica do parceiro.
* `itemsFromMe` são os itens q vc vai perder na troca.
* `itemsFromThem` são os itens q vc vai receber na troca.
* `counteredTradeOffer` (optional) é a ID de uma oferta de troca que vc está contrariando.
* `message` (optional) é uma mensagem inclusa na oferta.

`itemsFromMe` e `itemsFromThem` ambos sao uma ordem de item objects que vao parecer com isso:

```json
{
    "appid": 440,
    "contextid": 2,
    "amount": 1,
    "assetid": "1627590398"
}
```

Se obtiver sucesso o segundo parametro para `callback` sera um objeto com `tradeofferid` da nova oferta de troca criada.

## getOffers(options, callback)
## getOffer(options, callback)

O primeiro metodo carrega uma lista de of. de trocas, a segunda apenas carrega uma unica of. de troca.

Opções:

* Veja [Steam Web API/IEconService](https://developer.valvesoftware.com/wiki/Steam_Web_API/IEconService).

O segundo argumento para `callback` sera um objeto que retorna Steam Web API. A unica coisa para notar é que o compilado adiciona a propriedade `steamid_other` com a SteamID do parceiro de troca para cada objeto `Cecon_TradeOffer` em trocas recebidas.

## declineOffer(options[, callback])
## acceptOffer(options[, callback])
## cancelOffer(options[, callback])

`declineOffer` ou `acceptOffer` que foi enviada pra vc. `cancelOffer` que vc enviou.

Opções:

* `tradeOfferId` é uma trade offer Id

O segundo argumento para `callback` sera um objeto com resposta da Steam, mas nao aguarde nada significativo nisso.

## getOfferUrl(callback)

O segundo argumento para `callback` sera a trade offer URL.

## getOfferToken(callback)

O segundo argumento para `callback` sera o token de oferta do bot, extraido da sua trade offer URL.

## getItems(options, callback)

Opções:

* `tradeId` é a ID da troca completa que vc deseja pegar os itens, disponivel como uma propriedade `tradeid` em ofertas de `getOffers` ou `getOffer`

O segundo argumento para `callback` sera uma ordem de itens adquiridos numa troca completa.

## getHoldDuration(options, callback)

Obtenha uma duração de retenção para vc mesmo e seu parceiro de troca antes da troca.

Opçoes:

* `partnerAccountId` ou `partnerSteamId`, vc só precisa de 1 deles.
* `accessToken` é um token da Trade URL publica do parceiro de troca (requerida se ele nao estiver na sua lista de amigos).

O segundo argumento para `callback` sera um objeto dessa maneira:

```json
{
    "my": 0,
    "their": 0
}
```

## getTradeHoldDuration(options, callback)


Obtenha uma duração de retenção para a of. de troca ativa existente.

Opções:

* `tradeOfferId` é uma trade offer Id

A saída é a mesma de `getHoldDuration`.

# Licensa (Bla bla bla)

The MIT License (MIT)

Copyright (c) 2013-2016 Alexey Komarov <alex7kom@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
