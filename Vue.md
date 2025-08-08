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

São formas de adicionar instruções extras para os elementos HTML, de modo a "injetar" o Vue por meio de **"atributos"** especiais com o sufixo `v-`.

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

- O Vue disponibiliza um **atributo especial `ref="nomeParaAReferencia"`** que pode ser adicionado aos elementos HTML. Isso possibilita acessar o elemento por meio da instância do Vue. Para acessá-lo, você usa a **propriedade `$refs`** (por exemplo, `this.$refs.nomeParaAReferencia`, te dará **acesso ao objeto DOM do elemento** ao qual essa ref foi associada).

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

> Em Vue 2, um template só pode ter 1 elemento principal que engloba os outros elementos (você não pode ter elementos irmãos). Já em Vue 3 é possível ter elementos irmãos, filhos diretos de um template. Isso é chamado de "fragment".


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

### Attribute Fallthrough

Quando você adiciona atributos ou event listeners para um componente customizado (um componente que você criou), estes atributos e eventos são **atribuídos ao elemento principal** do componente (o filho direto de `<template>`). Isso é denominado *"attribute fallthrough"*.

Note que em Vue 3 podemos ter mais de um elemento principal abaixo de template (elementos irmãos). Nesse caso, o attribute fallthrough não é aplicado, pois Vue não tem como saber para qual elemente encaminhar os atributos e eventos. Você pode, no entanto, manualmente indicar para qual elemento os atributos devem ir, adicionando `v-bind="$attrs"` a este elemento.

## Usando `$emit`

Da mesma forma que um clique de um botão emite um evento para avisar sobre a ação de clique, os componentes em Vue podem emitir eventos customizados para avisar que uma determinada ação foi executada. Com isso, um **componente filho pode se comunicar com um componente pai** por meio de eventos customizados, possibilitando ao componente pai tomar alguma ação quando o filho emite o evento. Além disso, junto com o evento podemos passar algum dado para o componente pai.

No componente **filho**, emitimos eventos com o método fornecido pelo Vue: `this.$emit(nome-do-evento, dados)`.

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

Você pode adicionar a propriedade (opcional) `emits` ao componente como forma de documentar que tipo de emits seu componente possui e até mesmo fazer algumas validações. Isso serve mais como ajuda na hora do desenvolvimento, concentrando em um só local os emits que podem ser usados.

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

## Prop drilling e provide/inject

Prop drilling é o que ocorre quando temos componentes aninhados em vários níveis e precisamos passar uma prop de um componente que está em um nível mais alto para outro componente que está níveis abaixo. Essa prop precisa ser passada para cada componente nos níveis intermediários, que eventualmente nem precisam dessa prop, mas a recebem para poder repassá-la para o próximo componente um nível abaixo, até chegar ao componente de destino.

Para evitar o prop drilling, o Vue disponibiliza as opções (propriedades do objeto de configuração do componente) `provide` e `inject`, que implementam o Padrão de **Injeção de Dependência**.

Usamos o `provide` no componente que irá **fornecer** um dado (e que está em algum nível acima), e o `inject` no componente que irá **consumir** esse dado (e que está em algum nível abaixo). O dado pode ser uma string, array, objeto, etc, até mesmo uma variável reativa ou uma função. Com isso, não importa o quão profundamente aninhado esteja um componente, injetamos nele os dados que ele precisa consumir, sem que estes dados precisem trafegar por todos os componentes intermediários. 

A opção `provide` é um **método** que retorna um **objeto** cujas propriedades são os dados a serem disponibilizados. Já a opção `inject` é um **array**, em que cada elemento é o nome de alguma das propriedades presentes em `provide`.

```vue
<!-- App.vue -->
<script>
data() {
  return {
    posts: [
      {id:1, titulo: 'Primeiro post', conteúdo: 'Lorem ...'},
      {id:2, titulo: 'Segundo post', conteúdo: 'Ipsum ...'},
    ],
  };
},
provide() {
  return {
    posts: this.posts, // posso fornecer uma variável
    abrePost: this.selecionaPost // também posso fornecer métodos
  }
},
methods: {
  selecionaPost(id) {
    //...
  }
}
</script>

<!-- ListaPosts.vue -->
<template>
  <ul>
    <item-post
      v-for="post in posts"
      :key="post.id"
      :id="post.id"
      :titulo-post="post.titulo"
      :conteudo="post.conteudo"
    ></item-post>
  </ul>
</template>

<script>
export default {
  inject: ['posts'] // uso somente o que preciso consumir
};
</script>

<!-- ItemPost.vue -->
<template>
  <li>
    <h3>{{ titulo-post }}</h3>
    <p>{{ conteudo }}</p>
    <button @click="abrePost(id)">Leia mais</button>
  </li>
</template>

<script>
export default {
  props: ['id', 'tituloPost', 'conteudo'],
  inject: ['abrePost'] // posso usar o método sem a necessidade de um emit
};
</script>
```

### Quando usar provide/inject ou props e emits

Não há uma regra e você não estará errado se optar usar um ou outro. 

Uma sugestão é optar primeiro pelo uso de props e emits para comunicação entre componentes aninhados. Use o provide/inject quando um dado precisa ser enviado níveis abaixo, sem necessariamente ser consumido nos níveis intermediários. Se o aninhamento não for tão profundo, você pode optar continuar a comunicação via props.

Nada te impede também de usar os dois, aproveitando o provide/inject para os dados que valem a pena serem injetados ao invés de passados via props ou emits.

## Componentes globais e locais

Quando você adiciona um componente a sua instância Vue (a instância Vue é a criada com o método `createApp`), ele se torna disponível **globalmente** em sua aplicação, ou seja, qualquer outro componente pode utilizá-lo. 
 
O problema com componentes globais é que todos eles precisam ser carregados pelo Vue quando a aplicação é iniciada, incluindo aqueles que talvez venham a ser utilizados por um componente ou tela específica. Conforme a aplicação cresce, isso pode ser um gargalo de performance.

```js
// main.js

// instância Vue
const app = createApp(App);

// componentes globais
app.component('header', Header);
app.component('lista-post', ListaPost);
app.component('post', Post);
app.component('footer', Footer);
```

Um componente pode **importar outros componentes** e usá-los internamente. Componentes importados assim são **locais**, pois ficam disponíveis somente pelo componente que o importou. Importante observar que componentes **filhos NÃO têm acesso ao componente importado** pelo pai - cada componente deve importar localmente os componentes que precisar usar.

Para registrar um componente localmente, utilizamos a **propriedade `components`** no objeto de configuração do componente que está fazendo a importação. O `components` recebe um objeto, com cada propriedade sendo o nome customizado para o componente importado, e o valor sendo o componente em si. Aqui o nome pode ser em PascalCase, inclusive quando ele é usado no template do componente que está fazendo a importação.

```vue
<!-- ListaPost.vue -->
<script>
import ListaPost from './components/ListaPost.vue';

export default {
  components: {
      ListaPost // o mesmo que ListaPost: ListaPost
  },
  // ...
}
</script>

<template>
<!-- exemplo de uso com PascalCase e autofechamento -->
<ListaPost />

<!-- assim também funcionaria (necessário ter a tag de fechamento neste caso) -->
<lista-post></lista-post>
</template>
```

Sugestão: opte por registrar componentes globalmente caso eles sejam utilizados por vários outros componentes. Caso contrário, é mais performático registrá-los localmente, conforme a necessidade.

## Estilos locais

Em arquivos .vue, é possível aplicar estilos locais (CSS), adicionando o **atributo `scoped`** em `<style>`. Com isso, o CSS criado será aplicado somente aos elementos, classes, etc. contidos no `<template>` deste componente. Importante notar que os estilos **NÃO serão aplicados aos filhos** do componente.

```vue
<style scoped>
/* Vue irá criar um CSS com algo parecido como header[data-v-9a9f6144] */
header {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #14005e;
}

/* h1[data-v-9a9f6144] */
header h1 {
  color: white;
  margin: 0;
}
</style>

<template>
<!-- Vue irá renderizar algo como <header data-v-9a9f6144> -->
<header>
  <!-- <h1 data-v-9a9f6144> -->
  <h1>Principal</h1>
</header>
</template>
```

## Uso de `<slots>`

Semelhante a `children` no React, podemos passar conteúdo a um componente filho, para que este renderize tal conteúdo dentro de seu template. Para isso, adicionamos ao componente filho um elemento próprio do Vue chamado `<slot>`. 

Tenha em mente que usamos `props` para passar dados a um componente, e `<slot>` para passar HTML (o `<template>` de outro componente).

Um componente pode ter **mais de um slot**, possibilitando uma grande customização do template. Para diferenciar cada slot, usamos o atributo `name`, criando assim "named slots". É possível deixar um dos slots sem `name`, e ele se tornará o slot padrão naquele componente. 

No componente pai, indicamos os slots por meio de um elemento `<template>` com a diretiva `v-slot`: `<template v-slot:nome-do-slot>`. Note que `nome-do-slot` vem **depois dos dois pontos (`:`)** e **não está entre aspas** - é um argumento da diretiva v-slot. O slot padrão não precisa de `template` e tudo que estiver fora do template com v-slot será renderizado no slot padrão, mas se quiser adicionar um template para deixar mais claro, pode utilizar `<template v-slot:default>`.

- A diretiva tem uma forma abreviada: `#`. Então poderia ser `<template #nome-do-slot>` ou `<template #default>`

```vue
<!-- Post.vue -->
<!-- Componente que possui slots -->
<template>
  <article>
    <header>
      <!-- named slot -->
      <slot name="header"></slot>
    </header>
    <!-- slot padrão -->
    <slot></slot>
  </article>
</template>

<!-- ---- -->

<!-- ListaPost.vue -->
<!-- Componente pai que irá preencher os slots -->
<template>
<post>
  <!-- conteúdo renderizado no slot de Post nomeado como "header" -->
  <template v-slot:header>
    <h2>Título</h2>
  </template>
  
  <!-- O conteúdo abaixo é renderizado no slot padrão -->
  <!-- Se quisesse deixar mais claro, poderia envolvê-lo em -->
  <!-- <template v-slot:default> -->
  <p>Lorem ipsum...</p>
</post>
</template>
```

Importante salientar que o conteúdo inserido em um slot **não têm acesso às props do componente pai**, e os **estilos scoped do componente pai não são aplicados a ele**. Isso se deve ao fato de esse conteúdo ser "enxergado" e renderizado pelo componente filho.

### Conteúdo padrão

Os slots não são obrigatórios, ou seja, o componente pai não precisa passar conteúdo para o slot de um componente filho. Nesse caso, o elemento em que o slot se encontra será renderizado como vazio pelo componente filho. 

É possível, no entanto, deixar um valor padrão para o caso de o slot não receber conteúdo (um fallback). No componente que tem slots, basta adicionar algum conteúdo dentro de `<slot></slot>` - ele será renderizado somente se o slot não receber conteúdo do componente pai.

```vue
<template>
<header>
  <slot name="header">
    <!-- esse heading é renderizado caso o componente 
     pai não passe nada ao slot header -->
    <h2>Título padrão</h2>
  </slot>
</header>
</template>
```

É também possível evitar que um slot seja renderizado caso esteja vazio, verificando seu conteúdo por meio da propriedade `$slots`. Essa propriedade é um objeto, cujos atributos são os nomes dos slots (ou `default` para o slot sem nome). Se um slot `abc` é vazio, o conteúdo dele em `$slots.abc` é `undefined`, então podemos renderizar condicionalmente com `v-if`, por exemplo.

```vue
<template>
<!-- se o componente pai não passar conteúdo para o 
 slot header, nem renderiza o elemento header -->
<header v-if="$slots.header">
  <slot name="header"></slot>
</header>
</template>
```

### Props em slots

Um slot pode ter uma ou mais props (atributos passados a ele no **componente filho**). Essas props podem ser utilizadas para **passar dados ao componente pai**. Chamamos esses slots de "scoped slots".

Para o componente pai acessar essas props, adicionamos um nome a ser recebido pela diretiva `v-slot`. Por exemplo: `<template v-slot:nome-do-slot="propsDoSlot">`. Essa `propsDoSlot` é um objeto, e acessamos as props como atributos desse objeto.

De maneira abreviada: `<template #nome-do-slot="propsDoSlot">`. Para o slot padrão: `<template v-slot="propsDoSlot">` ou `<template #default="propsDoSlot">`.

Se o componente possui somente um slot, o componente pai pode passar `v-slot="propsDoSlot"` (ou `#default="propsDoSlot"`) diretamente para o componente filho, sem necessidade de envolver em um `<template>`.

```vue
<!-- Post.vue -->
<template>
  <article v-if="$slots.default">
    <!-- assuma que os valores para titulo e autor vêm da 
     propriedade data desse componente -->
    <slot :titulo="post.titulo" :autor="auth.nome"></slot>
  </article>
</template>

<!-- ListaPosts.vue -->
<template>
  <post #default="dados">
    <h2>{{ dados.titulo }}</h2>
    <h3>{{ dados.autor }}</h3>
    <p>Lorem ipsum...</p>
  </post>
</template>
```

## Componente `<component>`

Podemos renderizar componentes de maneira dinâmica usando o componente fornecido pelo Vue chamado `<component>`. Ela aceita uma prop `is`, cujo valor é o nome do componente a ser renderizado (sua tag, tanto kebab-case quanto PascalCase).

Com isso, é possível, por exemplo, ter uma variável reativa cujo valor é uma string, e nela passar o nome do componente que deve ser renderizado. Assim, por meio de algum evento, podemos alterar o valor dessa variável, por sua vez alterando o componente a ser renderizado.

```vue
<template>
  <div>
    <button @click="setComponenteAtivo('novo-post')">+ Novo Post</button>
    <button @click="setComponenteAtivo('gerencia-posts')">Tela administrativa</button>
    <component :is="componenteAtivo"></component>
  </div>
</template>

<script>

// imports omitidos

export default {
  components: {
    NovoPost,
    GerenciaPosts
  },
  data() {
    return {
      componenteAtivo: 'gerencia-post' // ou 'GerenciaPost'
    };
  },
  methods: {
    setComponenteAtivo(cmp) {
      this.componenteAtivo = cmp
    }
  }
};
</script>
```

Por ser dinâmico, quando o componente em `<component>` é modificado, ele é de fato destruído ("unmounted"). Com isso, suas variáveis no objeto de data, por exemplo, são perdidas. Esse pode não ser um comportamento desejado: imagine que o componente era um formulário com vários campos de input, cujos valores estavam associados a variáveis do objeto data; ao ser destruído, esses valores seriam perdidos.

Se você não quiser que o Vue destrua o componente, mantendo seu estado em cache, adicione o componente fornecido pelo Vue chamado `<keep-alive>`:

```js
<keep-alive>
  <component :is="componenteAtivo"></component>
</keep-alive>
```

## Componente `<teleport>`

> Este recurso está disponível somente a partir do Vue 3.

O Vue oferece o componente `<teleport>` que possibilita que você mova porções do seu template para algum outro lugar da estrutura do DOM.

Imagine que seu componente renderiza uma modal. Semanticamente, seria interessante que essa modal fizesse parte do `body` do HTML. Você pode usar o `<teleport>` para pedir ao Vue que faça isso. 

Para informar para qual local o conteúdo deve ser renderizado, você usa a prop `to`, cujo valor pode ser o nome de um elemento, ou até um id ou classe (ou seja, um seletor CSS).

```vue
<template>
  <input type="text" ref="nome">
  <button @click="addNome">Adicionar</button>
  <!-- o conteúdo abaixo será renderizado no final do body -->
  <teleport to="body">
    <minha-modal v-if="inputVazio">
      <p>Por favor, digite seu nome</p>
    </minha-modal>
  </teleport>
</template>
```

## Detalhes sobre formulários

- para obter o valor de um input, podemos utilizar o `v-model` ou o `ref`. No entanto, é necessário um cuidado extra com o `ref`: ele sempre retorna uma **string** com o valor. Já `v-model` tenta converter o valor de acordo com o **tipo do input**: se for `number`, devolve um número.

  - com input do tipo `date`, no entanto, ambos devolvem uma string com a data (yyyy-mm-dd)

  - `v-model` também tem um modificador para forçar a conversão do valor para número: `v-model.number`. Além disso, também há um modificador para eliminar espaços em branco do começo e fim do input: `v-model.trim`

- Podemos usar o `v-model` em campos de `select`. Com isso, fazemos o two-way binding do **`value` selecionado** no dropdown. Se desejar, ao criar uma variável em `data` para armazenar esse valor, podemos inicializá-la com o value de um dos `option` para que seja o valor padrão do select.

- Em grupos de checkboxes ou radio buttons, adicionamos o mesmo `v-model` para cada um dos inputs que compõem o grupo. O comportamento deles varia:

  - no caso de somente **um** checkbox, o valor de `v-model` será um **booleano** indicando se o campo está checked ou não;
  - no caso de um **grupo de checkboxes** (checkbox com mesmo atributo `name`), `v-model` deve ser inicializado como um **array**. Além disso, cada checkbox precisa ter um **atributo `value`** com um **valor diferente** em cada um. Esse value será adicionado ao array caso o checkbox em questão seja marcado como checked.
  - para um grupo de radio buttons, como somente um no grupo é selecionado, `v-model` pode ser inicializado como **string**. É necessário, no entanto, atribuir um valor ao **atributo `value`**, para que o v-model utilize este valor para indicar qual radio button foi selecionado.

## Usando `v-model` em componentes customizados

Quando usado em elementos HTML de input, o `v-model` uma combinação de `v-bind:value` e `v-on:input`. Mas ele também pode ser utilizado em componentes customizados, que você mesmo cria.

Quando usado em um componente customizado, `v-model` vai disponibilizar uma **prop `modelValue`** e emitir um **evento `update:modelValue`** ao componente. Por meio da prop temos o valor atribuído a `v-model` e pelo evento podemos atualizar esse valor.

```vue
<!-- Avaliacao.vue. -->
<!-- Quando clicamos em um botão, modelValue é atualizado 
com o novo valor. Esse valor também é usado para ativar
um CSS no botão clicado. -->
<template>
  <ul>
    <li :class="{ active: modelValue === 'amei' }">
      <button type="button" @click="activate('amei')">Amei</button>
    </li>
    <li :class="{ active: modelValue === 'legal' }">
      <button type="button" @click="activate('legal')">Legalzinho</button>
    </li>
    <li :class="{ active: modelValue === 'odiei' }">
      <button type="button" @click="activate('odiei')">Odiei</button>
    </li>
  </ul>
</template>

<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  methods: {
    activate(option) {
      this.$emit('update:modelValue', option)
    }
  }
}
</script>

<!-- ----- -->

<!-- App.vue -->
<!-- Agora podemos passar v-model a Avaliacao -->
<template>
  <form @submit.prevent="handleSubmit">
    <div class="form-control">
      <Avaliacao v-model="nota" />
    </div>    
    <div>
      <button>Salvar</button>
    </div>
  </form>
</template>

<script>
import Avaliacao from './Avaliacao.vue';

export default {
  components: {
    Avaliacao
  },
  data() {
    return {      
      nota: null
    }
  },
  methods: {
    handleSubmit() {
      console.log('Nota: ' + this.nota)
      this.nota = null;
    }
  }
}
</script>
```




Continuar a partir de 153.