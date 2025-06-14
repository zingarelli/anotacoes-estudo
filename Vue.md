# Vue

Anotações baseadas no curso [Vue - The Complete Guide (incl. Router & Composition API)](https://www.udemy.com/course/vuejs-2-the-complete-guide/) que comprei na Udemy.

O curso está atualizado para a **versão 3** do Vue.

Para não ficar muito extenso, nem sempre coloco códigos completos nos exemplos. Geralmente coloco somente partes do código necessários para entender o conceito.

## Conceitos iniciais

- Vue (/viu/) é um framework JavaScript (JS). Oferece um conjunto de ferramentas e regras para desenvolvimento em JS;

- Reativo: dinamicamente atualiza a tela baseado nas interações da pessoa usuária;

- Possibilita a criação de SPAs (Single Page Applications);

- Flexível o suficiente para "injetarmos" o Vue e utilizá-lo somente em algumas porções da nossa página - na criação de um widget, por exemplo;

- Segundo o [State of JS 2024](https://2024.stateofjs.com/en-US/libraries/), Vue aparece em 2º lugar das linguagens de Front-end (React vem em 1º e Angular em 3º, mas bem próximo de Vue).

## Instalação

O jeito mais **simples** é adicionar o pacote do Vue no `index.html`, seja no final do body ou no head, mas antes do script em que você vai trabalhar com o Vue.

```html
<body>
  <div id="app">
    <!-- seu conteúdo -->
  </div>
  <script src="https://unpkg.com/vue@3.4.9/dist/vue.global.js"></script> 
  <!-- ^ pacote do vue -->
  <script src="app.js"></script>
  <!-- ^ seu script para trabalhar com Vue (vem depois do script do pacote) -->
</body>
```

## Propriedade `data` e interpolação

Ao criar sua instância de Vue (`Vue.createApp`), você passa a ela um objeto com as configurações dessa instância. A propriedade `data` faz parte dessa configuração (este nome não pode ser alterado). Ela contém uma função anônima que retorna um objeto. Este objeto contém as **variáveis reativas** do projeto (variáveis cujos valores, ao serem alterados, automaticamente fazem um re-render da aplicação, e alteram este conteúdo no HTML).

Para passar o conteúdo de alguma variável de `data` para um **elemento HTML**, utilizamos a **interpolação**. Essa é a forma do Vue "injetar" o conteúdo no HTML. Fazemos isso passando o nome da variável entre chaves duplas (`{{ nomeDaVariável }}`).

```html
<p>{{ tarefa }}</p>
<!-- ^ interpolação -->
```

```js
const app = Vue.createApp({
  data() { // sintaxe ES6
    return {
      tarefa: 'Aprender Vue.'
    };
  }
});
``` 

## Propriedade `methods`

Outra propriedade da configuração da instância do Vue. É composto por um objeto, em que cada propriedade é um método (uma função) que pode ser utilizada por sua aplicação.

Os métodos podem ser invocados dentro de elementos HTML usando a **interpolação** vista na seção anterior.

- qualquer expressão JS simples pode ser passada via interpolação.

## Diretivas

São formas de adicionar instruções extras para os elementos HTML, de modo a "injetar" o Vue por meio de **atributos** com o sufixo `v-`.

| Diretiva | Para que serve |
| --- | --- |
| `v-bind:nomeDoAtributo` | liga uma variável do objeto `data` da instância do Vue ao valor do `nomeDoAtributo` |
| `v-on:nomeDoEvento="metodoOuExpressaoJS"` | adiciona um evento HTML ao elemento, e o método ou expressão JS a ser executado quando o evento é disparado. No caso de um método, ele será invocado mesmo se você não usar parênteses (mas vai ser necessário se precisar enviar parâmetros) |

### Shortcuts

## Sobre o `this` em Vue

A instância do Vue pode ser entendida como um objeto global, cujas propriedades são as propriedades reativas definidas em `data`, bem como as propriedades em `methods`, dentre outras que iremos ver. 

Por conta disso, quando você quer referenciar algum dado ou método da instância do Vue **dentro dela mesma** (por exemplo, um método acessar uma variável definida em `data`), você usa o `this.nomeDaVariavel` ou `this.nomeDoMetodo`.

É por conta disso que os métodos não podem ser arrow functions em `methods`. O `this` em uma arrow function faz referência ao contexto em que esta função é chamada, então o `this` não apontará para a instância do Vue nesse caso. Por isso, use funções anônimas.

```js
data() {
  return {
    textoA: 'Vue é incrível',
    textoB: 'É fácil aprender Vue'
  };
},
methods: {
  escrever() {
    const randomNumber = Math.random();
    if (randomNumber < .5) {
      return this.textoA;
    } else {
      return this.textoB;
    }
  },
  naoFunciono: () => {
    return this.textoA; // <- este this NÃO é da instância Vue
  }
}
```

## O objeto `event`

Quando um event listener é adicionado a um atributo HTML (seja via JS puro ou pela diretiva v-on), o evento disparado é passado, por **padrão**, como **argumento ao método** que é executado. Esse argumento é um objeto contendo os dados do evento disparado. 

Com isso, ao criar um método em sua instância Vue para ser executado após algum evento, você pode capturar esse objeto `event` (ou qualquer nome que você queira dar) como sendo o primeiro parâmetro do método.

No caso do seu método ter **outros parâmetros**, aí você explicitamente precisa enviar o objeto `event` ao método. Neste caso, você utiliza a variável `$event` disponibilizada pelo Vue, que te dá acesso ao evento disparado. Essa é uma palavra reservada no Vue, então o nome não pode ser mudado.

```html
<input type="text" v-on:input="setNome">
<input type="text" v-on:input="setNomeCompleto($event, 'da Silva')">
```

```js
setNome(e) { this.name = e.target.value },
setNomeCompleto(ev, lastName) { 
  this.name = ev.target.value + ' ' + lastName;
}
```

## Modificadores de eventos e teclas

O Vue possibilita modificar a ação padrão de alguns eventos, ao incluir modificadores **após o nome do evento** que você está manipulando. Por exemplo, se houver um evento de submit e você quer impedir o comportamento padrão do navegador de fazer um reload da página, você pode usar `v-on:submit.prevent="executaSeuMetodo"`.

Lista de modificadores e mais detalhes: https://vuejs.org/guide/essentials/event-handling#event-modifiers

O Vue também disponibiliza modificadores baseados ao [pressionar algumas teclas](https://vuejs.org/guide/essentials/event-handling#key-modifiers) e também [botões do mouse](https://vuejs.org/guide/essentials/event-handling#mouse-button-modifiers).

Você pode concatenar modificadores, bem como criar mais de um listener, com modificadores diferentes, fazendo ações diferentes.

```html
<button v-on:click.right.prevent="subtract(5)">Diminuir 5</button>
<!-- ^ concatenando dois modificadores, executando subtract somente quando 
 o botão direito do mouse é clicado, e também prevenindo a ação padrão de 
 acontecer, que seria abrir o menu de contexto -->
<input type="text" v-on:input="setName" v-on:keyup.enter="confirmInput">
<!-- ^ setName será invocado a cada tecla pressionada, mas confirmInput só 
 será invocado quando a tecla ENTER for pressionada -->
```