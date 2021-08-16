> Artigo escrito e publicado em: https://warcontent.com/cross-origin-resource-sharing/

É comum quando você inicia seus estudos com algum framework moderno, em especial voltado a frontend, esbarrar no “problema” de **CORS (Cross-origin Resource Sharing)**…

Sendo parecido com isso: *Origin http://localhost is not allowed by Access-Control-Allow-Origin*

Porém o CORS não é exatamente um “erro”, mas sim um **mecanismo de segurança** que ajuda a proteger sua Aplicação Web de ataques mal-intencionados.

Então, apesar de ser muito chato, é bom você saber como ele funciona e assim adicionar mais essa arma ao seu **Arsenal de Programador**…

Afinal ter sua aplicação invadida é muito pior! E com certeza gera mais dores de cabeça…

Então **continue lendo esse artigo** para descobrir o que é CORS e como resolver seus principais erros.

## O que é CORS (Cross-Origin Resource Sharing)?

Quando realizamos uma requisição a um servidor, os navegadores utilizam uma **política de segurança** chamada [same-origin](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) (ou mesma origem), que só autoriza a troca de informações se os envolvidos tiverem origens iguais…

Em resumo, a política same-origin exige que ambas as partes tenham a mesma origem para se comunicarem.

### Mas o que é uma origem?

Uma origem é composta por 3 elementos: **protocolo, domínio e porta**.

Por exemplo, a origem `http://exemplo.com:80` possui como protocolo o `http`, o domínio `exemplo.com` e a porta (oculta) `80`.

< imagem esquema de origem >

E caso algum desses 3 itens mude, já será uma origem diferente, e portanto a política same-origin inválida a comunicação. Isso explica o porquê de 2 Aplicações Web rodando em seu computador, possuírem origens diferentes.

Afinal, apesar de estarem na mesma máquina, são servidas em portas diferentes, como por exemplo:

* Interface frontend disponível na origem: `http://localhost:4200`;
* Serviço backend disponível na origem: `http://localhost:8080`.

Mas isso não costuma ser um problema fora do ambiente de desenvolvimento, pois é comum toda aplicação estar hospedada em um único servidor, logo **a origem é a mesma**.

Porém caso um servidor A precise se comunicar com um servidor B, ele viola a política de same-origin e o servidor B acaba não respondendo:

< imagem esquema de servidores se comunicando > 

Nesse ponto você já deve ter percebido que a política de same-origen é muito restritiva, afinal a várias situações onde diferentes servidores precisam se comunicar. Como por exemplo:

* Quando você cria uma API de serviço usada por outras pessoas (como a API do Google Maps);
* Ou quando sua aplicação Javascript fica em um servidor diferente do seu backend.

Nesses casos é necessário usar outra política de segurança **mais flexível**, como o padrão CORS (cross-origin resource sharing ou compartilhamento de recursos de origem cruzada).

Pois a política cross-origin permite que você especifique quem (ou seja, qual origem) pode acessar os recursos do seu servidor…

## Como funciona o CORS (Cross-Origin Resource Sharing)?

< imagem de segurança de festa >

Imagine o CORS como um **segurança de festa mal-humorado…** Assim que você solicita entrar no local, ele verifica se seu nome está na lista de convidados. E caso não esteja, você não vai ter acesso de jeito nenhum!

Da mesma forma, se você tentar acessar um servidor B de origem diferente, e sua origem não tem autorização, então seu acesso será negado…

Mas como adquirir essa autorização?

O padrão CORS gerencia solicitações de origens diferentes ao adicionar [cabeçalhos HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Headers) especiais nas **respostas do servidor…**

E apesar de existir [uma lista de cabeçalhos](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers) para você utilizar, o principal deles, que determina quais origens podem acessar um recurso no servidor, é o `Access-Control-Allow-Origin`.

Por exemplo, para permitir o acesso de qualquer origem, você pode definir esse cabeçalho com asterisco (`*`):

```
Access-Control-Allow-Origin: *
```

Ou pode ser reduzido a uma **única origem específica**:

```
Access-Control-Allow-Origin: https://OrigemServidorA.com
```

> Cuidado! O padrão não aceita uma lista de origens, ou seja apenas uma única origem pode ser definida.

## Como implementar o CORS (Cross-Origin Resource Sharing)?

A implementação desses cabeçalhos de configuração vai depender da linguagem ou framework do seu backend.

Por exemplo, se você estiver usando o [ExpressJS](https://expressjs.com/) (framework de NodeJS), pode simplesmente aplicar o método `setHeader()`:

```
app.get('/', (request, response) => {
  response.setHeader('Access-Control-Allow-Origin', 'https://example.com');
  response.send('Testando configuração do CORS...');
});
```

Porém é muito inconveniente você adicionar as configurações de cross-origin para cada rota de sua aplicação. Pois, além de ser muito chato, dificulta a manutenção.

Por isso você pode definir os cabeçalhos necessários com [um middleware de CORS](https://expressjs.com/en/resources/middleware/cors.html):

```
var express = require('express');
var cors = require('cors');
var app = express();

app.use(cors());

app.get('/', (request, response) => {
  response.send('Testando configuração do CORS...');
});
```

Assim as configurações serão aplicadas a todas as rotas automaticamente!

## Outras opções de configuração para você arrasar na balada

< imagem de robo DJ na balada > 

O padrão CORS é necessário porque permite que o servidor especifique não apenas **quem** pode realizar solicitações, mas também **como** elas podem ser feitas.

Afinal a maioria dos servidores permitem solicitações `GET`, o que significa que qualquer origem externa pode ler seus dados e informações.

No entanto, [métodos HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Methods) que causam modificações no servidor, como `PATCH`, `PUT` ou `DELETE`, podem ser negados para evitar atividades maliciosas.

Para isso utilize o cabeçalho `Access-Control-Allow-Methods` da seguinte maneira:

```
Access-Control-Allow-Methods: GET
```

Por fim, as principais configurações disponíveis são:

* `Access-Control-Allow-Origin:` Especifica qual origem (máximo 1) tem permissão para fazer a solicitação, ou `*` se uma solicitação pode ser feita por qualquer origem;
* `Access-Control-Allow-Methods:` Indica uma lista (separada por vírgula) dos métodos HTTP permitidos;
* `Access-Control-Allow-Headers:` Indica uma lista (também separada por vírgula) dos cabeçalhos personalizados permitidos;
* `Access-Control-Max-Age:` Informa quanto tempo uma autorização pode permanece válida (armazenada em cache).

> E caso você queira ver a lista completa de cada cabeçalho do padrão CORS, de uma olhada [na documentação](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#cors).

## Boas práticas e cuidados ao utilizar CORS

Por último, as configurações de cross-origin exigem **boas práticas** para manter a segurança, afinal vulnerabilidades surgem a partir de configurações incorretas.

Então veja a seguir **4 dicas** para você configurar seu “segurança de festa”:

### 1 – Libere apenas o necessário

Em `Access-Control-Allow-Origin`, evite utilizar `*`, que autoriza o acesso ao seu servidor para qualquer origem. A menos que faça sentido no seu caso como, por exemplo, uma API aberta integrada a vários sites de terceiros.

Aliás também evite liberar para qualquer método HTTP ou cabeçalho…

Pois o objetivo é manter sua conexão segura e, se você liberar para qualquer um, esse objetivo não será atingido.

Então apenas autorize as origens, métodos e cabeçalhos necessários para suas requisições.

### 2 – Evite as Gabiarras

Existem algumas gabiarras para você evitar configurar o CORS e resolver apenas no lado do cliente…

Mas como foi dito, ele existe para a nossa segurança, então o ideal é configurá-lo com o minimo de abertura possível (ou seja, apenas o necessário).

### 3 – Não esqueça a segurança no lado do servidor

O CORS não substitui a proteção de dados confidencias no servidor, afinal um invasor pode falsificar uma solicitação de qualquer origem confiável.

Portanto seu servidor deve continuar a aplicar técnicas de proteção como autenticação ou criação de sessões.

### 4 – Otimize a performance com cache

Lembre-se de especificar um tempo para o navegador deixar armazenado em cache sua permissão de acesso, através do `Access-Control-Max-Age`.

Se não, toda requisição vai precisar de uma nova autorização, o que pode impactar negativamente na performance de sua aplicação.

## Parabéns por ter chegado até aqui!

Afinal esse não foi um artigo fácil…

Quando me deparei com problemas relacionados a CORS (Cross-Origin Resource Sharing) pela primeira vez, foi muito difícil entender o que estava errado e encontrar soluções.

Mas ficou claro que é só colocar `*` em `Access-Control-Allow-Origin` e todos os seus problemas serão resolvidos como mágica… **Brincadeira!**

Configure com carinho suas politicas de segurança e elas irão evitar muita dor de cabeça no futuro.

E caso você tenha gostado desse artigo, ou tem alguma dúvida, **seu comentário e opinião** são sempre muito bem vindos.



