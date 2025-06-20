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

Ao criar sua instância de Vue (`Vue.createApp`), você passa a ela um objeto com as configurações dessa instância. A propriedade `data` faz parte dessa configuração (este nome não pode ser alterado). Ela contém uma função anônima que retorna um objeto. Este objeto contém as **variáveis reativas** do projeto (variáveis cujos valores, ao serem alterados, automaticamente fazem um **re-render da aplicação**, e alteram este conteúdo no HTML).

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

Duas diretivas (`v-bind` e `v-on`) são tão comumente utilizadas que receberam **formas abreviadas**. Você pode optar por usar tanto a forma completa quanto abreviada (mas tente manter a consistência no seu código).

| Diretiva | Para que serve |
| --- | --- |
| `v-bind:nomeDoAtributo` (ou a forma abreviada `:nomeDoAtributo`) | liga uma variável do objeto `data` da instância do Vue ao valor do `nomeDoAtributo` |
| `v-on:nomeDoEvento="metodoOuExpressaoJS"` (ou a forma abreviada `@nomeDoEvento="metodoOuExpressaoJS"`) | adiciona um evento HTML ao elemento, e o método ou expressão JS a ser executado quando o evento é disparado. No caso de um método, ele será invocado mesmo se você não usar parênteses (mas vai ser necessário se precisar enviar parâmetros) |
| `v-model:dataProperty` | é um *two-way binding*, possibilitando ao mesmo tempo obter e alterar o valor do `dataProperty`, ou seja, de uma das variáveis reativas. É uma combinação de `v-bind:value` e `v-on:input` (Vue 2), funcionando em elementos HTML e componentes que possuam uma prop `value` e um evento `input` |
| `v-if="condicao"` | é uma renderização condicional. O elemento (e seus subelementos) serão **adicionados ao DOM** somente se a `condicao` for verdadeira. Quando a condição é falsa, um **comentário** é adicionado ao DOM (`<!--v-if-->`).  |
| `v-else-if="outraCondicao"` e `v-else` | **logo após** um `v-if`, possibilitam encadear condições e escolher qual elemento (e subelementos) serão adicionados ao DOM. |
| `v-show="condicao"` | é uma alternativa ao `v-if`, porém, no caso de a condição ser **falsa**, ao invés de não adicionar o elemento, ele só o **esconde** por meio da propriedade CSS **`display: none`**. Em questão de performance, adicionar/remover elemento ao DOM é mais custoso, então usar o `v-show` pode ser vantajoso em elementos cuja visibilidade é frequentemente alterada (um accordion, por exemplo) |
| `v-for="item in lista"` | Suponha que `lista` é um array (ou qualquer outro enumerable), o `v-for` possibilita percorrer cada item desse array e armazená-lo em `item` (`item` e `lista` são nomes arbitrários). O **elemento** em que `v-for` foi definido será **repetido no HTML** e poderá **acessar o conteúdo** de `item`. Use **`:key`** (atenção ao v-bind) para auxiliar o Vue no gerenciamento da lista. |
| `v-for="(item, index) in lista"` | variação do `v-for`, em que podemos acessar o **índice do item** por meio de `index` (`index` é outro nome arbitrário) |
| `v-for="(valor, chave) in objeto"` | outra variação do `v-for`, iterando um objeto, em cada iteração acessando uma **chave e seu valor** (atenção que eles vêm **invertido nos parênteses**). Se você quiser só o valor, pode usar `v-for="valor in objeto"`. Também é possível pegar o **índice** do item como **terceiro parâmetro**. |
| `v-for="n in numero` | nessa variação, o elemento será repetido **`n` vezes**, de acordo com o valor de `numero`. |
|  |  |
|  |  |

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

Quando um event listener é adicionado a um atributo HTML (seja via JS puro ou pela diretiva v-on), por **padrão** o evento disparado é passado como **argumento ao método** que será executado. Esse argumento é um objeto contendo os dados do evento disparado. 

Com isso, ao criar um método em sua instância Vue para ser executado após algum evento, você pode capturar esse objeto `event` (ou qualquer nome que você queira dar) como sendo o primeiro parâmetro do método.

No caso do seu método ter **outros parâmetros**, aí você explicitamente precisa enviar o objeto `event` ao método. Neste caso, você utiliza a **variável `$event`** disponibilizada pelo Vue, que te dá acesso ao evento disparado. Essa é uma **palavra reservada** no Vue, então o nome não pode ser mudada.

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

O Vue também disponibiliza modificadores baseados na [tecla pressionada](https://vuejs.org/guide/essentials/event-handling#key-modifiers) e também nos [botões do mouse](https://vuejs.org/guide/essentials/event-handling#mouse-button-modifiers).

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

## Computed properties

São como métodos que **retornam um valor**, mas só são executados se uma das propriedades reativas das quais ele é dependente for mudada. É **similar ao `useMemo` do React**, com as variáveis listadas no array de dependências, otimizando a performance. Quando há um re-render da página, as computed properties serão re-executadas somente se uma das suas dependências for alterada.

- as dependências serão as variáveis reativas utilizadas no corpo do método (`this.nomeDaVar1`, `this.nomeDaVar2`, ...). Até mesmo **outras computed properties** podem entrar como dependência, caso sejam utilizadas no corpo do método.

As computed properties vão em outra propriedade da instância do Vue: `computed`, que recebe um objeto, em que cada propriedade é um método que vai retornar o valor da computed property. Quando usada no HTML, você **não precisa executar o método**, somente usar o nome dado à computed property.

```js
  computed: {
    // computed properties sempre retornam um valor
    nomeCompleto() {
      if (this.nome === '') return '';
      return this.nome + ' da Silva'; 
    }
  },
```

```html
<p>O nome completo será: {{ nomeCompleto }}</p>
```

## Watcher

**Similar a computed properties**, um watcher é uma função que será executada quando o valor de alguma de suas dependências for alterado. No entanto, um watcher **não precisa retornar um valor**. Pode ser algum pedaço de código que você quer que seja executado como um "efeito colateral" baseado na mudança em alguma variável. Por conta disso, é **semelhante a um useEffect do React**.

Watchers são interessantes, por exemplo, para quando você precisa fazer algum fetch na sua API baseado em alguma mudança que ocorreu em uma variável reativa.

Os watchers ficam na propriedade `watch` da instância do Vue, composta por um objeto cujas propriedades são métodos, cujo **nome é o mesmo nome da variável ou computed property que você quer observar**. Se você tem uma variável `nome` dentro da propriedade `data`, por exemplo, você pode criar um watcher `nome(novoValor)` para ela. O **primeiro parâmetro** do watcher é o **novo valor** da propriedade observada. O **valor antigo** da propriedade pode ser obtido do **segundo parâmetro**. Ambos os parâmetros são opcionais. 

Você pode usar watchers tanto para propriedades de `data` quanto para computed properties.

```js
data() {
  return {
    qtdeItensNoCarrinho: 0,
  };
},
watch: {
  qtdeItensNoCarrinho(novoValor) {
    atualizaItensNoCarrinho(novoValor);
  }
}
```

## Classes dinâmicas

Quando usamos o `v-bind` no atributo `class` de um elemento, podemos passar um **objeto** a ele e setar dinamicamente quando uma classe deve ser ou não adicionada. Cada **propriedade** desse objeto representa uma **classe**, e seus valores são **booleanos**: quando verdadeiro, a classe é adicionada.

Podemos também ter `class` e `v-bind:class` em um mesmo elemento, mantendo em `class` as classes que nunca mudam e em `v-bind:class` as que serão adicionadas de maneira dinâmica.

```html
<div 
  class="demo" 
  :class="{ active: boxASelected }" 
  @click="selectBox('A')"
></div>
<!-- ^ demo é uma classe "fixa" e active é adicionada dinamicamente, 
 dependendo do valor da variável boxASelected -->
```

Se quiser manter o HTML mais limpo, você pode criar uma variável com o valor de uma classe, ou uma computed property que retorna um objeto com as classes, e usá-las em `:class`.

## Condicionais e loops

As diretivas de condicionais e loops foram explicadas na [Seção de Diretivas](#diretivas). Segue aqui um código de exemplo de uso delas:

```html
<h2 v-if="isAdmin">Painel Administrativo</h2>
<h2 v-else-if="isGerente">Painel Gerencial</h2>
<nav v-else>
  <ul>
    <li 
      v-for="item in listaDeMenus" 
      :key="item.id"
    >
      <a :href="item.link">{{ item.text }}</a>
    </li>
  </ul>
</nav>
```