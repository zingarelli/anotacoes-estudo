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

Ao criar sua instância de Vue (`Vue.createApp`), você passa a ela um **objeto com as configurações** dessa instância. A propriedade `data` faz parte dessa configuração (este nome não pode ser alterado). Ela contém uma função anônima que retorna um objeto. Este objeto contém as **variáveis reativas** do projeto (variáveis cujos valores, ao serem alterados, automaticamente fazem um **re-render da aplicação**, e alteram este conteúdo no HTML).

Para passar o conteúdo de alguma variável de `data` para um **elemento HTML**, utilizamos a **interpolação**. Essa é a forma do Vue "injetar" o conteúdo no HTML. Fazemos isso passando o nome da variável entre chaves duplas (`{{ nomeDaVariável }}`).

```html
<section id='myVueApp'>
  <p>{{ tarefa }}</p>
  <!-- ^ interpolação -->
</section>
```

```js
const app = Vue.createApp({
  data() { // sintaxe ES6
    return {
      tarefa: 'Aprender Vue.'
    };
  }
});

app.mount('#myVueApp');
``` 

Você pode ter **mais de uma instância Vue** na sua aplicação (mais de um ``Vue.createApp`), e montar essa instância em outra parte do seu HTML, ou seja, referenciar outro bloco HTML em que essa instância Vue será utilizada. No entanto, **cada instância** possui **suas próprias propriedades**, e uma não pode acessar as propriedades da outra. Ou seja, são *standalone*.

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
| `v-for="(valor, chave) in objeto"` | outra variação do `v-for`, iterando um **objeto**, em cada iteração acessando uma **chave e seu valor** (atenção que eles vêm **invertido nos parênteses**). Se você quiser só o valor, pode usar `v-for="valor in objeto"`. Também é possível pegar o **índice** do item como **terceiro parâmetro**. |
| `v-for="n in numero"` | nessa variação, o elemento será repetido **`n` vezes**, de acordo com o valor de `numero`. |

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

## Vue por trás dos panos

- Vue usa o conceito de [proxy object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) do JS para lidar com as variáveis reativas. Por meio do proxy, o Vue é notificado quando essas propriedades são modificadas e recebem um novo valor. E por meio dessa notificação, Vue sabe que é hora de atualizar a UI para refletir esse novo valor. 

- Quando você usa o `instanciaVue.mount('#idDoElementoHTML')`, o elemento em que o Vue é montado é chamado de **template** dessa instância do Vue.

- Vue usa o template para criar um **"Virtual DOM"** dessa seção do HTML (uma cópia do DOM feita em JS, que **roda em memória** - mais rápido). Por meio do proxy, o Vue sabe quando alguma coisa mudou e atualiza esse Virtual DOM. Com isso, baseado nas diferenças entre o Virtual DOM **antigo e o novo** (que são em JS e rodam em memória, além de outras técnicas que aumentam a perfomance), o Vue consegue atualizar o HTML de maneira eficiente. 

- O Vue disponibiliza um **atributo `ref="nomeParaAReferencia"`** que pode ser adicionado aos elementos HTML. Isso possibilita acessar o elemento por meio da instância do Vue. Para acessá-lo, você usa a **propriedade `$refs`** (por exemplo, `this.$refs.nomeParaAReferencia`, te dará **acesso ao objeto DOM do elemento** ao qual essa ref foi associada).

## Ciclo de vida da Instância do Vue

O Vue possui pontos (*hooks*) em seu ciclo de vida que podem ser **acessados** por meio de propriedades do objeto usado no `createApp`. Cada um representa um estágio da instância do Vue em determinado momento e em cada um você pode rodar algum código necessário naquele estágio (para fazer alguma requisição a uma API, ou fazer alguma limpeza antes de encerrar a instância, por exemplo).

| hook | descrição |
|---|---|
| `beforeCreate()` | antes de a aplicação ter sido completamente inicializada |
| `created()` | logo após a aplicação ter sido inicializada, mas antes de estar ligada ao HTML (antes de vermos alguma coisa na tela). Neste ponto, o Vue conhece seus data properties e sua configuração. A partir daí, o Vue pode compilar o template e saber onde serão feitas as interpolações, métodos, etc. |
| `beforeMount()` | logo antes de o Vue mostrar alguma coisa na tela |
| `mounted()` | Vue já está ligado ao HTML e alguma coisa (o HTML) é mostrada na tela |
| `beforeUpdate()` | quando algum dado foi alterado, o beforeUpdate é executado antes de essa alteração ser aplicada |
| `updated()` | logo após a mudança ter sido processada e aplicada. Alterações na UI ocorrem nesse momento. |
| `beforeUnmount()` | antes de a instância Vue ser destruída. Ainda conseguimos ver alguma coisa na tela. |
| `unmounted()` | após a instância Vue ter sido removida. Já não vemos nada na tela associada a essa instância. |

### Convenções

Seu **componente principal** no projeto, aquele que será chamado para iniciar a aplicação, vai se chamar **`src/App.vue`**.

Os **outros componentes** serão colocados na pasta **`/src/components`**.

Nome dos arquivos: PascalCase (mais comum) ou kebab-case.

## Componentes

Um componente pode ser entendido como um pedaço **encapsulado e reusável** de código HTML, que possui **dados e lógica** atrelados a ele. Quando utilizado, um componente é **independente** de outro, ou seja, se você adicionar um componente duas vezes em seu código, cada um deles possui seus próprios dados e sua própria lógica, e **um não irá afetar o outro**. Eles podem, no entanto, se comunicar para compartilharem dados entre si, como será visto mais para frente.

O uso de componente auxilia em dividir uma grande aplicação em pequenos pedaços que interagem entre si.

Existe mais de uma maneira de criar componentes: por meio do método `component()` da instância do Vue, ou por meio de um SFC (Single File Component) - um arquivo com a extensão `.vue`.

### Método `component()`

Por meio da instância do Vue, chamamos o método `component()` e passamos dois argumentos: o **identificador** e o **objeto de configuração**.

- Identificador: uma string que será usada para adicionar o componente ao HTML. Sugestão: use **mais de uma palavra, separadas por `-`** (por exemplo: `meu-componente`), o chamado "kebab-case". Isso impede que você crie um nome que possa entrar em conflito com alguma tag do HTML.

- Objeto de configuração: mesmo objeto que usamos ao criar uma instância do Vue. Nele podemos criar `data`, `methods`, dentre outras propriedades, que pertencerão somente a este componente. Ou seja, um componente pode ser visto como uma instância do Vue, que pertence a outra instância Vue pai.

```js
const app = Vue.createApp({
  // código omitido
});

// criando um componente que pertence a app
app.component('contato-agenda', {
  // a propriedade template permite que esse componente renderize este HTML ao ser chamado
  template: `
    <li>
      <h2>{{contato.nome}}</h2>
      <button @click="toggleDetalhes">
        {{detalhesVisiveis ? 'Esconder' : 'Mostrar'}} Detalhes
      </button>
      <ul v-if="detalhesVisiveis">
        <li>
          <strong>Telefone:</strong>{{ contato.telefone }}
        </li>
        <li>
          <strong>Email:</strong> {{ contato.email }}
        </li>
      </ul>
    </li>
  `,
  data() {
    return {
      detalhesVisiveis: false,
      // hard-coded somente a fins de exemplo
      contato: {
        id: 'f-139',
        nome: 'Fulano de Tal',
        telefone: '99 99999-9999',
        email: 'fulano@email.br'
      }
    }
  },
  methods: {
    toggleDetalhes() {
      this.detalhesVisiveis = !this.detalhesVisiveis;
    }
  }
});
```

```html
<ul>
  <contato-agenda></contato-agenda>
  <contato-agenda></contato-agenda>
  <contato-agenda></contato-agenda>
  <contato-agenda></contato-agenda>
</ul>
```

### SFC (Single File Component)

É um recurso do Vue para **escrever componentes**. Por meio de um **único arquivo**, conseguimos **escrever HTML** para a UI, **JS** para a lógica (aquilo que escrevemos dentro do objeto de configuração) **e CSS** para estilização. Internamente, um projeto Vue (construído com o Vue CLI Ou Vite) faz um processo de build para criar os códigos necessários para rodar o app no navegador.

Diferente do método `component()`, aqui temos uma seção separada para o HTML na tag `<template>`, não precisando mais utilizar a propriedade `template` dentro do objeto de configuração.

Arquivos SFC terminam com a **extensão `.vue`**.

Além de centralizar a escrita do componente em um único arquivo, podemos também instalar **extensões** para facilitar o **syntax highlight** e o **autocomplete**. O próprio Vue disponibiliza uma [extensão oficial para VS Code](https://marketplace.visualstudio.com/items?itemName=Vue.volar).

Para utilizar SFC, é necessário ter alguma ferramenta de build, como o Vite, que irá compilar esses arquivos para poder abrir um projeto Vue no navegador.

```vue
<!-- ContatoAgenda.vue -->
<template>
  <li>
    <h2 class="nomeContato">{{contato.nome}}</h2>
    <button @click="toggleDetalhes">
      {{detalhesVisiveis ? 'Esconder' : 'Mostrar'}} Detalhes
    </button>
    <ul v-if="detalhesVisiveis">
      <li>
        <strong>Telefone:</strong>{{ contato.telefone }}
      </li>
      <li>
        <strong>Email:</strong> {{ contato.email }}
      </li>
    </ul>
  </li>
</template>

<style>
.nomeContato {
  font-size: 24px;
  font-weight: 800;
}
</style>

<script>
export default {
  data() {
    return {
      detalhesVisiveis: false,
      // hard-coded somente a fins de exemplo
      contato: {
        id: 'f-139',
        nome: 'Fulano de Tal',
        telefone: '99 99999-9999',
        email: 'fulano@email.br'
      }
    }
  },
  methods: {
    toggleDetalhes() {
      this.detalhesVisiveis = !this.detalhesVisiveis;
    }
  }
}
</script>
```

Usando um projeto Vue criado pelo Vite, por exemplo, temos um arquivo `main.js`, onde criamos nossa instância Vue e registramos novos componentes por meio do método `component`, dessa vez passando o SFC ao invés de um objeto de configuração. Por conta do Vite, podemos importar o Vue e também os métodos, deixando o código um pouco diferente: 

**Atenção:** esse é o código para **Vue 3**. Em Vue 2 não temos o createApp.

```js
// main.js
import { createApp } from 'vue';
import App from './App.vue';

// por ser um export default (componente anônimo),
// podemos dar o nome que quiser a ele. Por convenção,
// usamos o mesmo nome do arquivo .vue
import ContatoAgenda from './components/ContatoAgenda.vue';

// Cria a instância Vue e usa o componente App 
// como componente principal da aplicação
const app = createApp(App);

// Registra um novo componente à instância.
// Desse modo, App pode usá-lo.
app.component('contato-agenda', ContatoAgenda);

// associa a instância ao HTML
app.mount('#app');
```

Uma vez importado e registrado o componente a uma instância Vue, podemos utilizá-lo: 

```vue
<!-- App.vue -->
<template>
  <section>
    <h2>Agenda</h2>
    <ul>
      <contato-agenda></contato-agenda>
      <contato-agenda></contato-agenda>
    </ul>
  </section>
</template>

<!-- restante omitido -->
```

## Vue CLI e Vite

É uma ferramenta disponibilizada pelo Vue para criar um projeto com uma estrutura de pastas e arquivos pré-configurados, contendo também scripts para rodar localmente e fazer builds para produção. Por meio desse projeto também podemos utilizar os componentes criados com a extensão `.vue` ([SFC - Single File Component](#sfc-single-file-component)).

Até 06/2025, o [CLI está em **"manutenção"**](https://vuejs.org/guide/scaling-up/tooling.html#vue-cli), e o time do Vue recomenda o uso do [**Vite para criar projetos Vue**](https://vuejs.org/guide/scaling-up/tooling.html#vite).

Para criar um projeto Vue e sua versão mais atual, usando o Vite, rode o seguinte comando no terminal:

  npm create vue@latest

Depois basta seguir as instruções na tela para selecionar as opções que você deseja no projeto.

## Props

Uma das formas de passar dados aos componentes é por meio de props.

No componente, **`props` é outra propriedade da instância Vue**, que recebe um **array de strings**, cada string representando o nome de uma prop - um dado que o componente recebe e pode usar. As strings são em **camelCase**.

Ao **usar** o componente, você passa dados usando o nome das props definidas no componente, **como se fossem atributos do HTML**. Aqui, você usa os nomes em **kebab-case**.

As props também podem ser usadas **internamente** na seção `script` do componente, por meio do **`this.nomeDaProp`**. Também podemos usá-las na **interpolação** na seção `template`, simplesmente chamando pelo `nomeDaProp` (**não** preciso usar `props.nomeDaProp`).

```vue
<!-- ContatoAgenda.vue -->
<template>
  <li>
    <h2>{{ nome }}</h2>
    <p><strong>Telefone:</strong>{{ numeroTelefone }}</p>
    <p><strong>Email:</strong> {{ email }}</p>
  </li>
</template>

<script>
export default {
  props: [
    'nome',
    'numeroTelefone',
    'email'
  ],
  data() {
    return {
      detalhesVisiveis: false,
    }
  },
  // código omitido
}
</script>

<!--------->

<!-- App.vue -->
<template>
  <section>
    <h2>Agenda</h2>
    <ul>
      <contato-agenda
        nome='Fulano de Tal',
        numero-telefone='999999999',
        email='fulano@email.br'
      ></contato-agenda>
      <contato-agenda
        nome='Sicrano de Qual',
        numero-telefone='111111111',
        email='sicrano@email.br'
      ></contato-agenda>
    </ul>
  </section>
</template>
<!-- restante omitido -->
```

### Props como objetos

Ao invés de passar um array de strings para `props`, podemos passar um **objeto**. Por meio do objeto, podemos informar o tipo esperado para cada prop, se ela é ou não obrigatória, passar um valor default e até mesmo validar o valor recebido. Essas configurações são mais para **ajudar os desenvolvedores** na hora de utilizar um componente, e muitas vezes irão somente jogar um warning no dev-tools.

```js
props: {
  nome: String, // pode ser somente o tipo
  numeroTelefone: { // pode também ser um objeto com mais validações
    type: Number,
    required: true,
    validator: function(value) { // deve retornar true ou false
      return validaNumeroDeTelefone(value)
    }
  },
  email: {
    type: String,
    default: '' // valor usado se essa prop não for passada
  }
}
```

### Props não devem ser mutadas

Props são usadas por um componente **pai** para passar dados a um componente **filho**. Podemos entender isso como um "fluxo de dados unidirecional".

O componente filho é quem define as props que espera receber, mas **é um erro** alterar valor de uma prop ("mutar" a prop) no componente filho. Entenda a prop como uma variável **readonly**.

Se você precisa trabalhar com o valor de uma prop e internamente modificar esse valor no componente filho, uma alternativa é criar uma **variável interna** no componente filho e usar o valor da prop como valor inicial. Assim, você pode alterar o valor dessa variável como quiser dentro do componente filho.

Uma maneira de inverter o fluxo de dados, isto é, avisar ao componente pai que queremos alterar um valor recebido como prop, é por meio da **emissão de eventos customizados**.

## Usando `$emit`

Da mesma forma que um clique de um botão emite um evento para avisar sobre a ação de clique, os componentes em Vue podem emitir eventos customizados para avisar que uma determinada ação foi executada. Com isso, um **componente filho pode se comunicar com um componente pai** por meio de eventos customizados, possibilitando ao componente pai tomar alguma ação quando o filho emite o evento. Além disso, junto com o evento podemos passar algum dado para o componente pai.

No componente filho, emitimos eventos com o método fornecido pelo Vue: `this.$emit(nome-do-evento, dados)`.

- `nome-do-evento`: é como o evento será identificado ao usar o componente. O componente pai pode "ouvir" o evento com `v-on:nome-do-evento` (ou `@nome-do-evento`). A **convenção é usar kebab-case**.

- `dados`: o segundo argumento (e demais argumentos separados por vírgula) é **opcional** e nele você pode enviar dados para o componente pai. Quando o componente pai ouve o evento, ele pode chamar um método próprio dele e usar esse segundo argumento (e demais) como parâmetro de seu método.

Você também pode emitir eventos no template do componente filho. Neste caso, não precisa do `this`, somente `$emit(nome-do-evento, dados)`.

```vue
<!-- ContatoAgenda.vue -->
<template>
  <li>
    <h2>{{ nome }} {{ favorito ? '💛' : '' }}</h2>
    <p><strong>Telefone:</strong>{{ numeroTelefone }}</p>
    <p><strong>Email:</strong> {{ email }}</p>
    <button @click="mudaFavorito">{{ favorito ? 'Desfavoritar' : 'Favoritar' }}</button>
  </li>
</template>

<script>
export default {
  props: [
    'id',
    'nome',
    'numeroTelefone',
    'email',
    'favorito'
  ],
  methods: {
    mudaFavorito() {
      this.$emit('muda-favorito', this.id)
    }
  }
  // código omitido
}
</script>

<!--------->

<!-- App.vue -->
<template>
  <section>
    <h2>Agenda</h2>
    <ul>
      <contato-agenda
        v-for='amigo in listaAmigos'
        :nome='amigo.nome',
        :numero-telefone='amigo.telefone',
        :email='amigo.email'
        :id='amigo.id'
        @muda-favorito='alteraFavorito'
      ></contato-agenda>
    </ul>
  </section>
</template>
<script>
export default {
  data() {
    return {
      listaAmigos: [
        {
          id: 'fulano',
          nome: 'Fulano de Tal',
          telefone: '999999999',
          email: 'fulano@email.br',
          favorito: false
        },
        {
          id: 'sicrano',
          nome: 'Sicrano de Qual',
          telefone: '111111111',
          email: 'sicrano@email.br',
          favorito: true
        },
      ]
    }
  },
  methods: {
    alteraFavorito(id) {
      const amigoEncontrado = this.listaAmigos.find(a => a.id === id);
      if (amigoEncontrado) {
        amigoEncontrado.favorito = !amigoEncontrado.favorito;
      }
    }
  }
}
</script>
```

### Propriedade `emits`

Você pode adicionar a propriedade (opcional) `emits` ao componente como forma de documentar que tipo de emits seu componente possui e até mesmo fazer algumas validações. Isso serve mais como ajuda na hora do desenvolvimento, concentrando em um só local os emits que pode ser usados.

Essa propriedade pode ser um array de strings, com cada string representando o nome de um emit criado. Mas ela também pode ser um objeto, como cada emit sendo uma propriedade, e seu valor sendo uma função anônima e os parâmetros esperados, podendo passar dentro do corpo da função algum tipo de validação.

```js
// ContatoAgenda.vue

// emits: ['muda-favorito'] // pode ser um simples array ou...

// ... pode ser um objeto
emits: {
  'muda-favorito': function(id) {
    // Apenas uma validação interna do emit
    // O emit em si é chamado via this.$emit
    if (id) return true;
    else {
      console.warn('id é obrigatório!');
      return false;
    }
  }
}
```


Rever a partir de 105