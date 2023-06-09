# Anotações sobre o protocolo HTTP

## Instrutores Alura: 

- [Fábio Pimentel](https://www.linkedin.com/in/f%C3%A1bio-pimentel-25751b10/) - curso ["HTTP: Entendendo a web por baixo dos panos"](https://cursos.alura.com.br/course/http-fundamentos)

- [Geovane Fedrecheski](https://cursos.alura.com.br/user/fedrecheski) - [mesmo curso, atualizado 2023](https://cursos.alura.com.br/course/http-entendendo-web-por-baixo-dos-panos).

## Protocolo HTTP (**H**yper**t**ext **T**ransfer **P**rotocol)

É o protocolo de comunicação utilizado em um modelo cliente-servidor, sendo o cliente o navegador, por exemplo, e o servidor um BD, a nuvem, um servidor físico, etc. Ele é utilizado para transferir dados na web.

- o protocolo são as regras de comunicação a serem obedecidas para que se possa enviar requisições e receber respostas;

- essas regras são definidas em documentos chamados "RFC" (Request for Comments);

    - [RFC atualizada para o HTTP](https://www.rfc-editor.org/rfc/rfc9110)

- o protocolo define a forma como os dados irão trafegar na rede.

O HTTP é **independente** de linguagem de programação/plataforma.

Existem diversos outros protocolos estabelecidos para diversas outras atividades e modelos: P2P, SMTP, FTP, IRC, etc.

## HTTPS

Uma requisição HTTP trafega **texto puro**, o que pode ser perigoso quando enviamos informações sensíveis como senhas e dados de cartão.

A versão HTTPS adiciona uma camada de segurança ao protocolo.

    HTTP + SSL/TLS

SSL: Secure Socket Layer

TLS: Transport Layer Security. É a versão mais nova dessa camada de segurança.

Uso de Certificado Digital para criptografia dos dados:

- servidor disponibiliza em seu certificado digital uma **chave pública** para que os dados a serem **enviados sejam criptografados**, podendo trafegar de modo embaralhado. Ainda é um texto, porém, difícil de entender ou decifrar;

- servidor possui uma **chave privada**, que é usada para **remover a criptografia** dos dados; somente o servidor (em teoria) conseguirá saber o que de fato está sendo enviado; os intermediários no caminho só passam a informação, sem entender o que ela é.

### Criptografia simétrica e assimétrica

A chave pública funciona como um "embaralhador" dos dados. A chave privada seria como uma senha; é a única que sabe como desembaralhar esses dados. Esse tipo de criptografia é chamado de **criptografia assimétrica**, e é **mais lenta**.

Quando a **mesma chave** é utilizada para criptografar e descriptografar os dados, temos um caso de **criptografia simétrica**.

Na prática, **o HTTPS utiliza os dois tipos de criptografia**:

- 1º: é utilizada a criptografia assimétrica, em que o cliente gera na hora uma chave simétrica única e a envia ao servidor, por meio da criptografia assimétrica;

- 2º: o servidor usa sua chave privada para descobrir qual é essa chave simétrica. Agora, ambos tem conhecimento dessa chave simétrica e começam a utilizá-la em suas comunicações.

### Como é garantido que os certificados sejam seguros? 

A garantia é feita através de uma **assinatura digital**. Uma  **autoridade certificadora** (CA - Certificate Authority) assina digitalmente o certificado. Existem serviços gratuitos e pagos para emissão de assinatura.

Uma CA é um órgão que garante ao navegador e ao usuário que a identidade de um servidor (por exemplo o servidor da Alura) é realmente válida.

Auditorias anuais como a WebTrust servem para conferir legitimidade e segurança às Autoridades Certificadoras, para que sejam adicionadas pelos navegadores e sistemas operacionais como entidades seguras para emissão de certificados.

## Endereços

`http://www.alura.com.br` é um endereço web.

- `http`: protocolo;

- `www.alura.com.br`: domínio;

- `br`: raiz do domínio;

- `com`: especifica que é um domínio comercial.

DNS - Domain Name System: é o responsável pelo mapeamento entre um endereço IP e o nome de um domínio. É mais fácil guardarmos um nome de domínio do que seu endereço IP (do mesmo jeito que é mais fácil guardarmos o nome e número de uma casa ao invés de suas coordenadas GPS).

Uma forma de verificar o IP de um site por meio do `cmd` no Windows é utilizando o comando `nslookup` (imagino que o `ping` também ajude):

```cmd
> nslookup alura.com.br

Servidor:  b5d58402.virtua.com.br
Address:  181.213.132.2

Não é resposta autoritativa:
Nome:    alura.com.br
Addresses:  2606:4700:20::681a:483
          2606:4700:20::681a:583
          2606:4700:20::ac43:48e8
          104.26.4.131
          172.67.72.232
          104.26.5.131
```

### Portas

São as formas de **acesso aos serviços do servidor**. É onde sua requisição vai "bater" para ser atendido pelo servidor. O servidor abre portas para poder ser acessado. Invasores procuram por portas abertas para explorar falhas de segurança em um servidor.

Aparecem no final do endereço, no padrão `:numero_da_porta`. Não é obrigatório passar a porta no endereço, pois algumas já são padrão (porta 80 para HTTP e porta 443 para HTTPS, por exemplo).

`https://www.alura.com.br:80` é o mesmo que `https://www.alura.com.br` 

### Recursos

Aquilo que é digitado após o domínio (e a porta), é chamado de caminho para um recurso. Por exemplo: `https://cursos.alura.com.br/course/http-fundamentos/task/25393`. Tudo que vem após `.br` é o caminho para um recurso (`course/http-fundamentos/task/25393`).

Outro exemplo: `https://cursos.alura.com.br/`. A barra no final da URL indica que o recurso que queremos accesar está na **raiz** do domínio.

O recurso é **aquilo que queremos acessar** dentro do domínio informado.

![imagem mostrando as partes de uma URL](https://s3.amazonaws.com/caelum-online-public/http/http-url.png)

### URL e URI

- URI (Uniform Resource Identifier);
- URL (Uniform Resource Locator).

Embora **geralmente tratadas como sinônimos**, tecnicamente possuem diferenças. 

Uma URL é uma URI, mas o contrário não é verdade. Uma **URI identifica um recurso**, mas não informa sobre o protocolo a ser utilizado. Já a **URL** é uma URI que **informa qual o protocolo** a ser utilizado (http, por exemplo) **e por onde** irá trafegar (www, por exemplo). A URL dá um endereço para **localizar** o recurso, enquanto a URI apenas dá um identificador a esse recurso.

## Requisições e respostas

No **protocolo HTTP**, a comunicação **sempre** começa pelo lado do **cliente**, que envia uma requisição e espera por uma resposta do servidor.

As requisições HTTP são *stateless*, ou seja, elas não guardam informações. Uma requisição não conhece nada de requisições anteriores a ela. Como fazer então para que requisições que, por exemplo, necessitam de um usuário logado, não precisem executar a ação de login a cada nova requisição?

Problemas assim são resolvidos por meio do conceito de **"sessão"**. Após uma ação de login, por exemplo, o servidor gera um código único (por exemplo, um token) e o envia para o cliente. Este código pode ser então utilizado em requisições seguintes para que o servidor reconheça o cliente, sem necessariamente devolver uma resposta solicitando novamente o seu login. A sessão encerra no momento em que o cliente faz o logout, ou após um tempo definido, ou qualquer outra regra estabelecida pelo servidor. 

Informações como essa de sessão são armazenadas nos navegadores em **cookies** (pares de chave: valor). Cookies também são utilizados para o armazenamento de diversas outras informações, inclusive atividades, preferências e interações do usuário em diferentes páginas do mesmo domínio.

- Na comunicação cliente-servidor, os cookies são enviados pelo cabeçalho da requisição (`Set-Cookie` do lado do servidor, e `Cookie` do lado do cliente);

- [Artigo da Alura sobre cookies](https://www.alura.com.br/artigos/o-que-sao-cookies-como-funcionam).

### Developer tools

A "DevTools" ou "Developer Tools" ou "Ferramentas do Desenvolvedor" presentes nos navegadores possui uma aba chamada de "Network", em que é possível depurar as requisições que são feitas ao servidor. O atalho de acesso ao devtools nos navegadores geralmente é a tecla `F12`.

Ao digitar uma URL e apertar ENTER, a aba Network é preenchida com todas as requisições e respostas feitas para se mostrar a página solicitada. É possível verificar o método de requisição feito, o código da resposta, o que foi enviado no corpo da requisição, etc.

### Status code

O protocolo HTTP estabelece códigos padrão de resposta do servidor ("status code"). Existe o famoso 404, quando o recurso não existe no servidor. Outros códigos comuns: 200 (requisição bem sucedida, resposta processada OK), 301 (recurso movido permanentemente, deve ser acessado por meio de outra URL), 500 (erro interno dentro do servidor).

[Lista de status code com fotos de doguinhos 🐶](https://httpstatusdogs.com)

[Mesma lista, para amantes de gatos 🐱](https://http.cat/)

[Tabela de status code da W3Schools](https://www.w3schools.com/tags/ref_httpmessages.asp)

Esses códigos servem como uma boa prática, e não uma obrigação do protocolo. Cabe ao desenvolvedor utilizá-los da maneira correta na resposta gerada pelo servidor.

## Métodos HTTP

Os métodos mais comuns na vida de um dev Web são GET e POST. 

Quando se trabalha com desenvolvimento de **serviços** Web, comunicação entre aplicações Web, também são comuns o uso de **PUT e DELETE**.

Métodos também são chamados de **Verbos**.

### GET

Serve para receber dados do servidor. É possível enviar parâmetros na URL para especificar o que se está solicitando (*query string* ou *query params*). Esses parâmetros são textos puros e ficam expostos.

### POST

Serve para enviar dados ao servidor. Os dados geralmente vão no corpo da requisição, o que possibilita protegê-los por meio de criptografia, por exemplo.

### DELETE

Serve para remoção de um recurso.

### PUT

Serve para atualização/substituição de dados de um recurso.

## Serviços REST

No modelo cliente-servidor "tradicional", em que o cliente é um navegador, ele envia uma requisição e recebe uma resposta. Quando a requisição é bem sucedida, a resposta costuma ser um HTML ou elementos para renderizar a página (um CSS, um JS, uma imagem, etc.). Esse tipo de resposta não é suficiente ou não consegue ser interpretada quando o cliente **não é um navegador**. Imagine, por exemplo, um aplicativo de celular: uma resposta em HTML precisaria ser parseada para obter os dados que interessam ao aplicativo.

Para facilitar a comunicação entre aplicativos e serviços web, ou até a comunicação entre dois aplicativos web, utilizam-se outros formatos na resposta, como por exemplo, o XML ou o JSON, que facilitam a obtenção dos dados, que são recebidos em um "modelo mais hierárquico".

É possível passar no GET um cabeçalho chamado `accept` para especificar o formato de resposta que se espera receber do servidor.

### REST (**Re**presentational **S**tate **T**ransfer)

Seria um modelo que **une uma mesma URI + um método**, determinando a ação a ser feita no servidor. Pela URI estou acessando um recurso e pelo método o servidor sabe o que está sendo esperado que seja feito no recurso, dando uma resposta apropriada.

É a mesma forma que o HTTP foi proposto, porém voltado para serviços Web (web services, serviços que se comunicam via rede). É um **modelo arquitetural para especificar como se comunicar com serviços Web**: como acessar seus recursos, que ações (métodos) cada recurso aceita, como os resultados podem ser devolvidos.

- quando se trata de **Web Services**, o costume é falar **URI** ao invés de URL, pois o endereço está servindo como identificador único para acesso a determinado recurso (lembrando que uma URL é uma URI, mas o inverso não é verdade);

- Exemplo: `http://alurafood.com/api/restaurante` é uma URI que acessa um recurso que pode listar os restaurantes (quando o método é GET) ou pode adicionar um novo restaurante (quando o método é PUT).

[Artigo Alura sobre REST e boas práticas.](https://www.alura.com.br/artigos/rest-principios-e-boas-praticas)

Quando uma aplicação segue os princípios REST, ela é chamada de RESTful.

### E o SOAP (**S**imple **O**bject **A**ccess **P**rotocol)?

[Artigo sobre REST e SOAP feito pela RedHat](https://www.redhat.com/pt-br/topics/integration/whats-the-difference-between-soap-rest)

SOAP é um **protocolo** mantido pela W3C (não confundir com protocolo de comunicação cliente-servidor, que no caso é o HTTP). REST não é um protocolo, é somente um modelo. Ambos servem para comunicação e transfência de dados. 

Enquanto o REST possui princípios que devem ser implementados pelos desenvolvedores, o SOAP é um protocolo padrão, que já estabelece como algumas coisas devem ser feitas, incluindo questões de segurança e obrigação de a resposta SOAP ser um XML.

REST é mais leve e flexível, surgiu depois do SOAP e foi abraçado para serviços web. SOAP é mais robusto e pesado, ainda visto em soluções empresariais que precisam de mais confiabilidade e segurança.

## Postman

O Postman é um software que permite fazer requisições a uma API, servidor, etc, para testar a comunicação com elas.

Download: https://www.postman.com/downloads/

Na interface do Postman, você pode digitar uma URL e o método HTTP que quer utilizar. A resposta a essa requisição é exibida e você pode analisá-la.

## HTTP/2

É a versão atualizada do protocolo HTTP, levando em conta um mundo altamente conectado, em que mais dados trafegam na rede e por diferentes meios (IoT - Internet of Things).

A **ativação** da versão 2 é algo feita a nível de **servidor**. Os principais navegadores já suportam essa versão. A nível de desenvolvimento front-end, não muda nada.

Você pode usar o devtools para saber qual versão está sendo utilizada. Para isso, na aba "Network", é preciso adicionar a coluna "Protocol" (botão direito em alguma coluna -> selecione "Protocol").

Algumas diferenças:

- o conteúdo do corpo de uma resposta agora é comprimido com **GZIP** por padrão, para diminuir o tráfego de dados;

- o cabeçalho tanto da requisição quanto da resposta não é mais texto puro, e sim **dado binário**, comprimidos com **HPAC** (técnica de compressão feita exclusivamente para headers HTTP), também por questões de performance;

- Requisições e respostas são criptografados  utilizando TLS, para segurança. Apesar de não ser obrigatório por padrão, os principais navegadores fazem essa exigência para utilização da versão 2;

- Os cabeçalhos já enviados em uma requisição passada **não precisam** ser enviados novamente, desde que sejam semelhantes (header stateful);

    - caso haja uma mudança no conteúdo de algum cabeçalho, você só envia aquilo que mudou;

    - o HPAC tem uma forma de guardar este estado, portanto a requisição continua stateless.

- Ao receber um requisição válida, o servidor envia a resposta e também já se antecipa e envia outras respostas com conteúdo que poderá ser requisitado pelo cliente, baseado no que ele requisitou em primeiro lugar, conceito definido como **Server Push**;

    - exemplo: o cliente requisita uma página HTML; o servidor responde com a página e verifica que nessa página há necessidade de outros arquivos do servidor (CSS, JS, imagem, por exemplo), também enviando estes outros arquivos ao cliente em respostas adicionais.

- O processamento de requisições e respostas são feitos em paralelo (multiplexing), ou seja, o cliente não fica esperando uma requisição ser resolvida para então enviar outra, ele vai enviando; da mesma forma, o servidor não aguarda o processamento de uma resposta para enviar outra, ele envia conforme elas estejam prontas.

    - para o HTTP/1.1, os navegadores resolvem isso parcialmente abrindo mais de uma conexão TCP, enviando requisições diferentes para cada conexão. O processamento dentro de cada conexão, entretanto, continua sequencial.

## HTTP/3

É a versão mais recente do protocolo, ainda sendo adotada pelos servidores e linguagens de programação back-end.

Um das principais mudanças é na camada de transporte: ao invés de utilizar o protocolo TCP, será utilizada uma versão melhorada do protocolo UDP, chamada "QUIC", que resolve algumas limitações do UDP e trafega os dados de modo mais rápido que o TCP. Além disso, o QUIC já vem com o TLS embutido, estabelecendo a conexão segura desde o começo.

O Google é uma das empresas que já utiliza o HTTP/3.