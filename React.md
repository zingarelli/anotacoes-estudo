# Anotações sobre React

A primeira seção são anotações que eu fiz ao ler o livro ["React: Up & Running"](https://www.amazon.com.br/React-Up-Running-Stoyan-Stefanov/dp/1492051462/).

As seções seguintes são baseadas nos diversos cursos que eu faço na [Alura](https://www.alura.com.br), incluindo temas mais específicos como [roteamento](#rotas-e-o-react-router), [gerenciamento de estados](#gerenciando-estados-com-a-context-api), [testes](#testes-com-jest-e-react-testing-library) e [estilizações](#estilização-com-sass-e-scss). 

# Livro - React: Up & Running

**Dados do livro:**

React: Up & Running, 2nd edition, by Stoyan Stefanov (O’Reilly). Copyright 2022
Stoyan Stefanov, 978-1-492-05146-6.

**Material suplementar:**

https://github.com/stoyan/reactbook2

## O que é React

É uma **biblioteca** para construção de **UIs** (User Interfaces - interfaces de usuário), ou simplesmente "telas". Você cria **componentes**, que são partes de uma UI, e reutiliza estes componentes **quando** e **onde** você precisar. Desse modo, você escreve seu código somente uma vez e só se preocupa em **gerenciar o estado da sua aplicação**: toda vez que acontecer uma mudança no estado da aplicação, a UI é reconstruída para *reagir* (react, em inglês) a essa mudança. 

- Você não precisa se preocupar com a manipulação dos elementos do DOM - o React faz isso por você (mas você ainda pode manipular o DOM se desejar).

## Usando a biblioteca React

Aqui vou demonstrar os primórdios de como utilizar o React, com o intuito de compreender os conceitos iniciais de como a biblioteca foi construída. Na prática, **não é assim que se desenvolve em React**: a maneira mais moderna é por meio de JSX, explicado em uma [Seção posterior](#jsx). 

Para utilizar o React, a maneira mais básica é baixar a biblioteca para uma pasta, onde você desenvolverá sua aplicação. Também será necessário um "add-on" chamado **"ReactDOM"**. Você pode nomear as pastas e os arquivos como desejar. 

Uma maneira de baixar os arquivos é por meio de um servidor conhecido, como o `unpkg.com`. Você tanto pode baixar os arquivos como incluir diretamente a URL no `src` de seu script. 

- React: https://unpkg.com/react@17/umd/react.development.js

- ReactDOM: https://unpkg.com/react-dom@17.0.2/umd/react-dom.development.js

Estes links irão baixar a versão 17 do React.

Suponha que você tenha salvo os arquivos em uma pasta `react`. O próximo passo é adicioná-los ao arquivo HTML da sua aplicação (no final da tag de fechamento de `body`).


```html
<script src="react/react.js"></script>
<script src="react/react-dom.js"></script>
```

Um jeito simples de testar o React é adicionando o código a seguir em outra tag `script`:

```html
<body>
    <div id="app"></div>

    <script src="react/react.js"></script>
    <script src="react/react-dom.js"></script>
    <script>
        ReactDOM.render(
            // cria um elemento h1
            React.createElement('h1', null, 'Hello world'),
            // informa ao React onde a "magia" deve acontecer
            document.getElementById('app')
        )
    </script>
</body>
```

## Objetos React e ReactDOM

Um objeto `React` é o que você usa no código para acessar outras tecnologias do React, aqui chamadas de "APIs".

O objeto `ReactDOM` possui métodos para renderizar a aplicação no navegador. Entenda "renderizar" no sentido de mostrar o conteúdo HTML na página.

### Método `React.createElement()`

Este é o método que cria um componente React.

O primeiro parâmetro é o elemento a ser criado (a tag HTML). O segundo parâmetro é um objeto com as propriedades que esse elemento terá (os atributos DOM). O terceiro parâmetro (e os seguintes) corresponde ao filho deste elemento. Você pode ter quantos filhos desejar, utilizando o método `React.createElement()` novamente de forma aninhada.

O exemplo mais abaixo utiliza o método para criar o seguinte código:

```html
<h1 id='myHeading'>
    <span>
        Olá 
        <em>
            maravilhoso
        </em>
    </span>
     mundo!
</h1>
```

```js
// utilizando o método React.createElement
React.createElement(
    'h1', 
    {id: 'myHeading'}, 
    // filhos de h1: span e ' mundo!'
    React.createElement(
        'span', 
        null, // sem atributos DOM
        // filhos de span: 'Olá ' e em
        'Olá ',
        React.createElement(
            'em', 
            null, 
            // filho de em: 'maravilhoso'
            'maravilhoso'
        ),
    ),
    ' mundo!'
),
```

Internamente, este método cria um objeto JavaScript representando o elemento e suas propriedades (que chamamos em React de "props"). Seria algo mais ou menos assim:

```js
{
    type: 'h1',
    props: {
        id: 'myHeading',
        children: {
            type: 'span',
            props: {
                // e por aí vai...
            }
        }
    }
}
```

### JSX

Como pôde ser visto no exemplo anterior, aninhar elementos usando JavaScript e o método `React.createElement` cria um código "feio" e que vai ficando cada vez mais difícil de ler à medida que novos elementos vão sendo adicionados. É para resolver isso que entra a **sintaxe JSX**, que é XML dentro de um JavaScript. Com o JSX, é possível criar um código mais limpo e fácil de ler, e que na prática é semelhante a escrever HTML dentro do JS.

Mesmo exemplo anterior, escrito com JSX:

```js
// usando JSX para criar os elementos
ReactDOM.render(
    <h1 id="myHeading">
        <span>
            Olá  
            <em>
                maravilhoso
            </em>
        </span> 
         mundo!
    </h1>,
    document.getElementById('app')
);
```

O acrônimo JSX pode significar **J**ava**S**cript**X**ML
ou **J**avaScript **S**yntax e**X**tension. A [W3Schools](https://www.w3schools.com/react/react_jsx.asp) adota JavaScript XML.

O JSX é uma **tecnologia separada** do React. Escrever o código com JSX é a **forma mais moderna** de criar componentes React.

O JSX segue as regras de XML, ou seja, você **precisa fechar todas as tags HTML**, bem como as outras que você irá criar quando começar a fazer seus próprios componentes. Por exemplo, `<br />` e não `<br>`, `<img />` e não `<img>`, etc.

É necessário **transpilar** o JSX para um código JS válido, que os navegadores possam entender. Transpilar é o processo de reescrever um código de uma sintaxe para outra sintaxe. Uma maneira de transpilar JSX para JS é utilizando a biblioteca Babel. Você pode [baixá-la](https://unpkg.com/babel-standalone/babel.min.js) para a mesma pasta em que se encontra a biblioteca React e a reactDOM, e depois incluí-la em seu código.

- será necessário adicionar `type="text/babel"` no script que contém o JSX, para que o navegador ignore este script. Assim, somente o Babel irá acessar este script e fazer o parsing do código JSX.

```html
<script src="react/babel.js"></script>
<script type="text/babel">seu JSX vem aqui</script>
```

Você também pode **passar valores e expressões em JS** dentro do JSX. Para isso, você deve englobar a expressão/valor dentro de chaves (`{}`). É possível aninhar outras `{}` se você tiver outro conteúdo JSX dentro da expressão.

> **A partir de agora, os exemplos irão utilizar o JSX ao invés do método `React.createElement`.**

## Criando seus próprios componentes

Você pode definir um componente customizado de duas maneiras: **function components** (a maneira mais moderna atualmente) e **class components** (criando uma classe que estende de `React.Component`).

É válido entender a sintaxe de class components para poder trabalhar com aplicações mais antigas (legadas) ou até mesmo entender algum tutorial disponível na internet. Desenvolvedores acostumados com linguagens orientada a objetos provavelmente se sentirão mais à vontade com class components.

### Function components

Uma maneira de criar uma function component é atribuir uma função anônima (ou arrow function) para uma variável. Esta variável será seu componente. O retorno da função é a UI que você quer que o componente renderize.

- a convenção é que o nome do componente **comece com letra maiúscula**;

- para adicionar o componente ao seu HTML, você **usa uma tag** com o nome do componente.

Exemplo como uma função anônima:

```js
// definindo o componente
const MeuComponente = function() {
    return <span>Meu primeiro componente!</span>;
};

// usando o componente (lembre-se de fechar a tag!)
ReactDOM.render(
    <MeuComponente />,
    document.getElementById('app')
);
```

*Eu acredito que o React sabe que isso é um componente (e não simplemente uma função anônima) ao olhar o que ela está retornando (neste caso, um JSX).*

Exemplo utilizando arrow function:

```js
const MeuComponente = () => {
    return <span>Meu primeiro componente!</span>;
};
```

Você também pode criar como se fosse uma função nomeada:

```js
function MeuComponente() {
  return <span>Meu primeiro componente!</span>;
}
```

### Class components

Você cria class components estendendo de `React.Component` e implementando a função `render()`.

```js
// definindo o componente
class MeuComponente extends React.Component {
    render() {
        return <span>Meu primeiro componente!</span>;
    }
}

// o uso é do mesmo jeito que em function components
ReactDOM.render(
    <MeuComponente />,
    document.getElementById('app')
);
```

**Nota**: se você quiser **quebrar linhas** no retorno, é necessário envolver o conteúdo **em parênteses** logo após a cláusula `return`. O `return` **não** pode ficar sozinho na linha, senão o JS irá interpretar que seu retorno é `undefined` (ele entende que é `return;`). Segue um exemplo correto:

```js
class MeuComponente extends React.Component {
    render() {
        return (
            <span>
                Meu primeiro componente em uma frase muito
                longa e com quebra de linha. Preciso envolver 
                o return entre parênteses para funcionar.
            </span>
        );
    }
}
```

## Props

É possível **passar valores** para seu componente utilizando propriedades, ou simplesmemente "props". Essas propriedades são acessadas via o objeto `props`, que é somente leitura.

Em **class components**, você acessa por meio do **`this.props`**. Para utilizar um valor de prop no JSX você a coloca dentro de `{}`:

```js
class MeuComponenteComProps extends React.Component {
    render() {
        return <span>Meu nome é <em>{this.props.name}</em></span>;
    }
}
```

Em **function components**, o `this` geralmente se refere ao objeto global, então não temos um `this.props` (lembre-se que o function component é criado como uma função). As props nesse caso são acessadas no **primeiro parâmetro** da função. Por convenção, usamos o nome "props" para esse parâmetro:

```js
const MeuComponenteComProps = function(props) {
    return <span>Meu nome é <em>{props.name}</em></span>;
}
```

Você pode utilizar o [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) para acessar individualmente as propriedades dentro de `props`. Assim, não precisa ficar digitando `props.algumacoisa` toda vez.

```js
const NomeEProfissao = function({name, job}) {
    return <span>Meu nome é <em>{name}</em>, e sou {job}</span>;
}
```

Você pode passar valores default usando a propriedade `defaultProps`, que já vem disponível em componentes. Dentro dela, você define a propriedade e seu valor default:

```js
// fora do bloco em que o componente foi definido
NomeEProfissao.defaultProps = {
    job: 'estudante',
};
```

Você **passa propriedades** como se fossem atributos em uma tag HTML. Isso serve tanto para componentes de classe quanto de função:

```html
<!-- o componente irá utilizar o valor default para a propriedade job -->
<NomeEProfissao name="Matheus" />
```

**Nota:** em versões mais atuais do JavaScript (ES6), você pode passar valores default aos parâmetros da função **diretamente na definição da função**. Segue o mesmo exemplo com parâmetros default na função:

```js
const NomeEProfissao = function({name, job = 'estudante'}) {
    return <span>Meu nome é <em>{name}</em>, e sou {job}</span>;
}
```

### Tipando props

Você pode utilizar o TypeScript para tipar seu projeto React (isso é visto com detalhes no meu [arquivo sobre TypeScript](/TypeScript.md) disponível neste repositório de anotações). Mas também existem bibliotecas que adicionam tipos às props. Uma delas é a `prop types`.

Faça o download do arquivo para a pasta `/react`: https://unpkg.com/prop-types/prop-types.js

Adicione à biblioteca como um novo script:

```html
<script src="react/prop-types.js"></script>
```

Quando uma prop é tipada, atribuir um valor de um tipo diferente a ela gera um erro no **console** (um warning, na verdade, a aplicação continuará rodando caso esse valor não quebre alguma parte do seu código).

```js
// fora do bloco em que o componente foi definido
NomeEProfissao.propTypes = {
    name: PropTypes.string,
    job: PropTypes.string,
};

// valor aceito
// const job = 'arquiteta';

// tipo inválido, irá gerar um erro
const job = 0;

ReactDOM.render(
    <NomeEProfissao name="Olivia" job={job} />,
    document.getElementById('app')
);
```

Você pode verificar os outros tipos existentes em PropTypes digitando `PropTypes` no console.

## Gerenciando estados em Class Components

*Aqui é explicado como usar estados em componentes de **classe**. Há uma Seção específica para [componentes de função](#estados-e-hooks-em-function-components), em que utilizamos hooks.*

O React possui um conceito de "estado" (`state`), que é um **objeto** representando algum dado que o componente usa para se renderizar. Quando o `state` **é alterado**, o React *reage* fazendo uma nova renderização do componente, isto é, **seu conteúdo no DOM é reconstruído** (na prática, o método `render()` está sendo chamado novamente). 

O `state` é opcional. Um componente sem `state` é chamado de *stateless*. Já um componente com `state` é chamado de *stateful*. 

Você pode **acessar** o estado usando `this.state` (lembre-se que estamos tratando de class components). Agora para **modificá-lo**, é utilizado o `this.setState()`. A verdade é que é possível modificar o estado atribuindo um valor diretamente a `this.state`, mas isso **não é recomendado**. Então, use o `this.setState()` (a única exceção é ao *inicializar* o estado). 

Para tornar o componente  stateful, você inicializa o estado no construtor do componente. Aí então, você pode causar uma mudança no estado por meio de algum evento.

Exemplo de um componente `<ContadorDeCaracteres>`, em que contamos os caracteres dentro de um elemento `textarea` e exibimos a contagem em um elemento `h3`. O estado é o texto dentro da `textarea`: cada vez que esse texto muda, o componente é renderizado novamente (constumamos falar "re-renderizado") com a quantidade de caracteres atualizada:

```js
class ContadorDeCaracteres extends React.Component {
    // tornando o componente stateful
    constructor() {
        super();
        this.state = {}; // state inicializado com um objeto vazio
    }

    // função que atualiza o estado com o texto digitado 
    onTextChange(event) {
        this.setState({
            text: event.target.value,
        });
    }
    
    // será chamado toda vez que houver mudança no estado
    render() {
        // verificamos se o texto já está no estado 
        // (no caso de re-renderização), senão, utilizamos
        // o valor que veio via props
        const text = 'text' in this.state ? this.state.text : this.props.text;
        
        return (
            <div>
                <textarea 
                    value={text} 
                    onChange={event => this.onTextChange(event)}
                />            
                <h3>{text.length}</h3>
            </div>
        );
    }
}

ContadorDeCaracteres.defaultProps = {
    text: 'Conte enquanto eu digito',
};      

ReactDOM.render(
    <ContadorDeCaracteres text="react" />,
    document.getElementById('app')
);
```

O React possui sua **própria sintaxe** para eventos (para performance e compatibilidade entre navegadores). Então, ao invés de usar o `onchange` do HTML (todas as letras minúsculas), usamos o `onChange` do React (camelCase). O mesmo é feito para os outros eventos (`onClick`, `onBlur`, etc.).

### Métodos do ciclo de vida do componente

Os componentes possuem diversos métodos que representam diferentes fases do seu ciclo de vida (isto é, do momento em que o componente é instanciado até o momento em que é destruído). 

Há três fases principais:

- Mount: quando o componente é "montado", ou seja, adicionado ao DOM;
- Update: toda vez que uma mudança ocorre;
- Unmount: quando o componente é "desmontado", ou seja, removido do DOM.

Há outras fases intermediárias e respectivos métodos que as representam. Destas, duas são mais comumente utilizadas:

- `componentDidMount()`: o componente acaba de ser montado. Você pode aproveitar esta fase para pegar dados do servidor, acessar alguma propriedade do DOM, etc.;

- `componentDidUpdate(previousProps, previousState, snapshot)`: o componente acaba de ser atualizado. Neste estágio é possível acessar os valores anteriores que estavam nas props e no state (`previousProps, previousState`) e comparar com os valores atualizados (`this.props` e `this.state`).

O ciclo de vida completo é: 

- `constructor(props)`,
- `componentWillMount`,

- `componentDidMount`,

- (caso as props mudem) 
`componentWillReceiveProps(nextProps)`,
- (caso as props ou estado mudem) 
`shouldComponentUpdate(nextProps, nextState)`,

- `componentWillUpdate(nextProps, nextState)`,

- `componentDidUpdate(prevProps, prevState)`,

- `componentWillUnmount`.


*São muitos detalhes, então não vou entrar a fundo no ciclo de vida.* 

#### Como funciona em function components

Em function components, os métodos são substituídos, em certa medida, por hooks. Hooks são vistos em uma [Seção própria](#estados-e-hooks-em-function-components). Somente a título de exemplo, seguem dois hooks e seu método correspondente em class components:

- `useLayoutEffect`: corresponde ao `componentWillMount`;

- `useEffect`: mescla os métodos `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`;

### `render()` e o Virtual DOM

Toda vez que um estado muda em um componente React, seu método `render()` é chamado e a UI é atualizada. 

Na primeira vez que o `render()` é chamado, uma representação do DOM é criada em uma estrutura de árvore, denominada *virtual DOM tree* (árvore DOM virtual). 

Nas próximas vezes em que o `render()` é chamado, o que realmente acontece é um **diff** entre o virtual DOM anterior e o atual (por exemplo, um texto que foi alterado), e então o React otimiza as mudanças que precisam ser feitas para realmente **atualizar o DOM do navegador somente onde é necessário**. Isso otimiza a performance, e é por isso que a página inteira **não** precisa ser recarregada. 

Essa etapa é chamada de "*reconciliation*" (reconciliação).

### Padrões de Binding para o `this`

*Essa parte é complicada... Utilizei os links abaixo para me ajudar a entender melhor.*

- https://www.youtube.com/watch?v=kVWpBtRjkCk

- https://www.freecodecamp.org/news/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56/

Em class components, quando chamamos uma função do componente, pode ser que o contexto do `this` seja perdido. Por exemplo, ao manipular eventos:

```js
// função definida no componente
handleClick() {
    this.setState({message: 'Atualizei'});
}

// função chamada pelo elemento button
render() {
    //...
    return (
        <button onClick={this.handleClick}>Clique aqui</button>
    )
} 
```

O código acima irá gerar um erro, porque o `this` no bloco de código de `handleClick()` está undefined (é o comportamento inerente do JS). É necessário fazer o **bind do `this` do elemento para a função que ele está chamando**, de modo a não perder o contexto.

Ao fazer o binding, a função também terá acesso ao `event`, podendo acessar propriedades como `event.target.value`.

#### Padrão #1: bind do `this` no método `render()`

```html
<button onClick={this.handleClick.bind(this)}>Clique aqui</button>
```

Embora fácil de ler e de dar manutenção, esta abordagem implica em chamar `bind()` **toda vez** que o componente **renderizar**, o que pode impactar na performance em aplicações muito grandes.

#### Padrão #2: usar uma arrow function no método `render()`

```html
<button onClick={() => this.handleClick()}>Clique aqui</button>
```

A arrow function mantém o `this` do contexto do elemento. Do mesmo modo que no Padrão #1, esta abordagem pode impactar na performance por estar no método `render()`.

#### Padrão #3: bind das funções que manipulam evento no  `constructor()` do class component

```js
constructor(props) {
    super(props)
    // ....
    this.handleClick = this.handleClick.bind(this);
}

// ... dentro do método render()
    <button onClick={this.handleClick}>Clique aqui</button>

```

Esta é a abordagem recomendada em questão de performance, já que o bind está acontecendo no constructor, ou seja, será aplicado somente uma vez (o constructor não é chamado toda vez que o estado do componente atualiza, somente o render que é chamado). Toda vez que `handleClick` é chamado, ele já tem a referência ao `this` do contexto do componente.

#### Padrão #4: usar uma arrow function como propriedade da classe

```js
clickHandler = () => {this.setState({message: 'Atualizei'})}
```

Aqui, `clickHandler` é uma propriedade do class component, para a qual é atribuída uma função. Usando a arrow function, o contexto do `this` é o contexto do componente. Esta é também uma abordagem recomendada em questão de performance, mas utiliza conceitos novos da referência do EcmaScript, então pode não funcionar em projetos antigos. 

## Estados e hooks em Function Components

Os chamados **hooks** foram adicionados ao React na versão 
**v16.8**, permitindo a criação de function components stateful, ou seja, adicionando a possibilidade de gerenciar estados.

Um hook é uma **função** que permite utilizar várias funcionalidades do React, não apenas estados. É fácil identificar um hook pelo seu nome: por convenção, **todos levam o prefixo `use`**.

Além dos hooks disponíveis na biblioteca, como `useState` ou `useEffect`, você também pode criar seu próprio hook customizado, lembrando de manter a convenção de usar o prefixo `use`. 

### `useState()`

Talvez seja o hook mais comumente utilizado. A função **recebe um valor** e **retorna um array de 2 elementos**: uma **variável de estado** e uma **função para alterar** esta variável (um updater, ou setter). 

```js
const [nome, setNome] = React.useState('Felipe');

// é o mesmo que
// const arrayDeEstado = React.useState('Felipe');
// const nome = arrayDeEstado[0];
// const setNome = arrayDeEstado[1];
```

- a variável de estado pode ser de qualquer tipo;

- o valor inicial da variável de estado pode ser vazio;

- É uma convenção nomear a função updater com o **prefixo `set`**.

Quando é necessário atualizar o estado, você utiliza a função updater:

```js
setNome('Felipe Souza');
```

Como visto em seções anteriores, atualizações de estado causam uma nova renderização do componente. Variáveis de estado podem ser passadas via props para outros componentes. Isso faz com que atualizações na variável causem a renderização de cada componente em que elas são utilizadas, o que permite uma grande flexibilidade em **atualizar diferentes partes da UI dependentes daquele estado**. 

Você pode criar quantos hooks de estado desejar, criando assim diversas variáveis de estado e suas funções updater. Você também pode criar um objeto de estado que guarda diferentes variáveis de estado como propriedades (mas neste caso gera um acoplamento, o que nem sempre é desejável quando as variáveis não guardam uma relação entre si).

Não é necessário se preocupar com o binding do `this` nas funções de callback. Elas têm acesso ao contexto dentro do function component em que são usadas.


### `useEffect()`

Esse hook substitui os métodos do ciclo de vida `componentDidMount()` e `componentWillUnmount()`. Ele pode ser usado como um "efeito colateral" de um componente. A principal tarefa de um componente é renderizar algo, então podemos considerar como "efeitos colaterais" quaisquer outras tarefas que o componente precise realizar além da renderização (como buscar dados em uma API ou configurar events listeners).

O `useEffect()` recebe **dois argumentos**, com o segundo sendo opcional: uma função callback, que será chamada pelo React no momento apropriado, e um array de dependências. Essas dependências são variáveis que serão "observadas": sempre que houver uma **alteração nessas variáveis**, a função **callback será invocada**.

- se um **array vazio** for passado como dependência, a função callback é chamada **apenas uma vez**, quando o componente renderizar (similar ao `componentDidMount()`);

- se **nenhum array** for passado, a função callback é invocada após a primeira renderização e **em cada re-renderização**;

Segue um código de exemplo para os três casos:

```js
useEffect( () => {
    // faça alguma coisa TODA VEZ que a variável "data" for alterada
} , [data]);

useEffect(() => {
    // faça alguma coisa após a PRIMEIRA renderização
}, []);

useEffect(() => {
    // faça alguma coisa após a PRIMEIRA e EM CADA renderização
} , );

// não passar o segundo argumento faz o mesmo que o exemplo anterior
// useEffect(() => {
//     // faça alguma coisa após a PRIMEIRA e EM CADA renderização
// });
```

A função callback também pode **retornar outra função** (opcional). Ela pode ser útil como uma função de "limpeza" quando o componente é removido (similar ao `componentWillUnmount()`).

- se o array de dependências estiver **vazio**, a função de limpeza é chamada quando o componente é **removido do DOM**;

- se houver um **valor** no array de dependências, a função de limpeza será chamada quando o componente é **removido** ou quando o **valor da dependência mudar**;

- se **não houve**r um array de dependências, a limpeza ocorrerá quando o componente for **removido** do DOM ou **re-renderizado** (devido a uma alteração em uma variável de estado, por exemplo).

```js
useEffect(() => {
    // faça alguma coisa após a primeira renderização
    return () => {
        // faça alguma coisa quando o componente é removido do DOM
    };
}, []);
```

Da mesma forma que em `useState`, você pode ter múltiplos `useEffect` para tratar de casos distintos de efeitos colaterais que quer adicionar ao componente.

### `useReducer()`

*Esse é um pouco mais complicado e não tenho certeza se é muito utilizado. Você pode ler mais sobre este hook [neste artigo](https://blog.logrocket.com/react-usereducer-hook-ultimate-guide/) (em inglês).*

É uma função alternativa para o `useState`, em que você **concentra todas as mudanças de estado em um único local**: a função "reducer".

```js
const [data, dispatch] = useReducer(funcaoReducer, dadosIniciais);
```

O useReducer retorna um estado (atribuído a `data`), que já pode vir com um valor inicial (informado em `dadosIniciais`). Ela também retorna uma função `dispatch`.

O primeiro argumento de `useReducer` é a função **reducer**. Ela, por sua vez, recebe dois argumentos - um estado antigo e uma ação - e **retorna um novo estado**. A ação representa algo que aconteceu na aplicação e que requer uma alteração (como uma interação do usuário, por exemplo). As ações são despachadas (por meio do `dispatch`) e, em seguida, a função `reducer` é chamada pelo React. Dessa forma, todas as alterações em um estado são centralizadas no `reducer`, em vez de várias partes do componente lidarem com as alterações de estado (caso elas queiram que o estado mude, devem enviar a ação apropriada por meio do `dispatch`). A função reducer **não precisa ser implementada dentro do componente**.

```js
function funcaoReducer(estadoAntigo, acao) {
    const novoEstado = {}; // ou qualquer outro tipo do seu estado
    // verifique a acao recebida e modifique novoEstado
    return novoEstado;
}
```

O React vai verificar se o novo estado retornado por um reducer é diferente do estado anterior. Caso seja diferente, o componente será renderizado novamente e o novo estado se torna o estado atual. A **comparação usa `Object.is`** para determinar se ambos os estados são iguais. Portanto, **evite** atribuir `estadoAntigo` a `novoEstado` (`novoEstado = estadoAntigo`), pois ambos apontarão para o mesmo *objeto* na memória e a renderização não ocorrerá (uma alteração em qualquer uma dessas duas variáveis alterará ambas). Em vez disso, faça uma **cópia dos dados** para novoEstado.

A **ação é um objeto** que contém uma propriedade `type` e uma propriedade opcional `payload` (que também pode ser outro objeto com informações adicionais e não precisa ser chamado de "payload" - qualquer nome é aceitável, desde que você use o mesmo nome ao usar as propriedades da ação no reducer). Ela é **passada como argumento** para a função `dispatch()`.

Na função `reducer`, você pode ter cláusulas `if/else` ou um `switch` para determinar o que fazer com cada `type` de ação, e pode usar as informações de `payload` para um refinamento do que fazer em cada ação.

O exemplo abaixo mostra como usar `dispatch()` passando um objeto de ação como argumento. Este objeto é o segundo parâmetro da função `reducer`. O `reducer` poderia verificar que essa ação é um `incrementCount` e atualizar uma variável de estado contadora com o valor `incrementValue` recebido do `payload`.

```html
<button onClick={() => dispatch({
    type: 'incrementCount', 
    payload: {
        incrementValue: 1
    }})}
>
    Increment
</button>
```

A função `dispatch()` pode ser passada a outros componentes via props. Assim, outros componentes podem solicitar uma alteração no estado. Dessa forma, o `useReducer` pode ser uma alternativa para o Redux, que é uma biblioteca para gerenciamento de estados. O Redux é um assunto a parte e eu ainda não aprendi muito sobre ele. Outras alternativas para gerenciamento de estado são  a [ContextAPI](#gerenciando-estados-com-a-context-api) e o [Recoil](#gerenciando-estados-com-recoil), que possuem seções específica aqui neste arquivo..

### `useRef()`

"Ref" significa "referência" e é uma maneira de acessar elementos do DOM (ou elementos React) usando o React, ao invés dos famosos `document.getElementById` e derivados. Por exemplo, você pode definir uma ref para um elemento de formulário de modo a poder acessar o formulário (e percorrer seus elementos de entrada).

Refs são anexadas aos elementos por meio do atributo `ref`. Um **objeto ref** pode ser criado usando `React.createRef()` ou usando o hook `useRef()`.

Esse objeto possui uma propriedade `.current` que pode retornar:

- o elemento DOM subjacente quando `ref` é usado em um elemento HTML;

- a instância montada de um componente quando `ref` é usado em um componente de classe personalizado. Você **não** pode usar o atributo `ref` em componentes *funcionais* porque eles não têm instâncias.

O exemplo a seguir coloca uma referência a um elemento de input, podendo manipulá-lo na definição do componente `InputComFoco`. Assim consigo adicionar foco ao input quando o botão é clicado.

```js
function InputComFoco() {
  const inputEl = useRef(null); // ou useRef();
  // inputEl.current por enquanto é null
  
  // quando essa função é chamada após o componente 
  // ter sido renderizado, `current` agora aponta 
  // para o elemento de input
  const onButtonClick = () => {    
    inputEl.current.focus();
  };

  return (
    <>
      <!-- ligando inputEl e esse elemento de input -->   
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Foco no input</button>
    </>
  );
}
```

#### `forwardRef()`

Um componente pode passar uma ref para seus componentes filhos. O componente filho pode então passar essa ref para um de seus elementos filhos do tipo Node (ele pode **encaminhar a ref**). Dessa forma, o **componente pai pode ter acesso aos elementos Node de um de seus componentes filhos**.

A ref não está disponível em props, portanto, para passar uma ref para um componente e fazer com que esse componente encaminhe essa ref, ele precisa ser definido usando `React.forwardRef()`.

No exemplo a seguir, o componente `BotaoDiferente` renderiza um botão, que recebe um `ref`. Este componente será filho de `AlgumComponente`. Dessa forma, `AlgumComponente` consegue acessar o botão de `BotaoDiferente`.

```js
// BotaoDiferente.js
const BotaoDiferente = React.forwardRef((props, ref) => (
  <button ref={ref} className="BotaoDiferente">
    {props.children}
  </button>
));

// AlgumComponente.js
function AlgumComponente() {
    const myRef = React.createRef(); // ou o hook useRef

    // quando AlgumComponente() for montado, myRef tem acesso ao botão de BotaoDiferente
    useEffect( () => {
        console.log(myRef.current); 
        myRef.current.addEventListener('click', () => alert('o botao foi clicado'));
    }, []);

    // passando a referência para o componente filho
    <BotaoDiferente ref={myRef}>Clique aqui!</BotaoDiferente>;
}
```

## Arrays e a prop `key`

Ao trabalhar com arrays e usá-los para criar uma lista de elementos (um `<td>` ou um `<li>`, por exemplo), o React requer que cada filho em uma lista tenha um identificador único. Isso é interno ao React, para que ele saiba quais elementos foram alterados e precisam ser atualizados no DOM.

Para fazer isso, você adiciona uma propriedade `key` a cada elemento. Essa propriedade não será exibida no HTML. Você pode usar o índice do elemento do array como valor da chave, embora isso **nem sempre seja recomendável**, pois elementos podem ser removidos ou reordenados, fazendo com que esse índice seja mutável:

```js
for(index in this.props.headers) {
    <th key={index}>{this.props.headers[index]}</th>)
}
```

## Uso da função `map()`

A função `map()` é muito útil quando se trabalha com arrays em React. 

Você pode chamar `map()` em um array para executar uma função callback em cada item do array; `map` irá **retornar um novo array** com os resultados da função callback. A função callback pode receber o elemento ou o elemento e seu índice como parâmetros.

```js
// considere "headers" como sendo um array de strings
// map retorna um array de elementos th, usando o 
// conteúdo e o índice de cada elemento em headers. 
this.props.headers.map((header, index) => <th key={index}>{header}</th>)
```

**Observação**: se você precisar quebrar a linha do retorno da sua arrow function, lembre-se de envolver o conteúdo retornado em **parênteses**:


```js
{this.props.headers.map((header, index) => (
    <th key={index}>
        {header}
    </th>
))}
```

## Create-React-App (CRA)

> **Nota:** Em 2023 o CRA foi descontinuado e seu uso já não é recomendado. Alguns substitutos a ele são o [Vite](https://vitejs.dev/guide/) e o [Next.js](#nextjs) (há uma Seção sobre o Next neste arquivo de anotações). Eu mantive essa Seção do CRA como referência e também por abordar assuntos que ainda fazer parte de um projeto React, como o `package.json` e a pasta `node_modules`. 

O CRA é um conjunto de scripts Node.js e suas dependências que torna fácil **configurar** um projeto React e criar um **processo de build** para fazer o deploy.

- Você pode encontrar instruções para instalar o Node em https://nodejs.org

A maneira mais fácil de usar o CRA é usando o utilitário npx (**N**ode **P**ackage e**X**ecute) que vem com o Node.js. Abra um terminal, vá para o diretório onde deseja criar o projeto e execute o seguinte comando:

```js
npx create-react-app nomeDoSeuProjeto
```

Isso levará algum tempo e criará um diretório chamado "nomeDoSeuProjeto" com todo o conteúdo necessário para iniciar seu projeto React.

### Arquivo `package.json`

Este arquivo é encontrado no diretório raiz do projeto e contém várias configurações sobre o aplicativo. Por exemplo, ele lista todas as dependências necessárias para rodar o projeto (a biblioteca React e também todas as outras bibliotecas necessárias). O CRA irá criar este arquivo e algumas dependências necessárias para começar o seu projeto.

Quando você precisa instalar o projeto (usando `npm install` ou `npm i` - são sinônimos), essas dependências serão baixadas para uma pasta chamada `node_modules`.

Exemplo de um `package.json` com algumas dependências.

```json
{
    "dependencies": {
        "@testing-library/jest-dom": "^5.16.5",
        "@testing-library/react": "^13.4.0",
        "@testing-library/user-event": "^13.5.0",
        "react": "^18.2.0",
        "react-dom": "^18.2.0",
        "react-scripts": "5.0.1",
        "web-vitals": "^2.1.4"
    },
}
```

### Pasta `node_modules`

Essa pasta é onde todas as dependências listadas no `package.json` são instaladas. Essa pasta também está localizada no diretório raiz do aplicativo. Ao invocar uma função importada de uma biblioteca externa, o React irá procurá-la nessa pasta.

Você pode importar dependências dessa pasta para seus arquivos JS:

```js
import React from 'react';
import ReactDOM from 'react-dom';
```

Esta pasta geralmente é gigante e possui muitos arquivos e subpastas, por isso, é recomendado **não incluí-la em seu repositório remoto, caso o projeto esteja versionado (no GitHub por exemplo)**. Como as dependências estão listadas no `package.json`, você pode instalá-las manualmente ao baixar o projeto, usando `npm i` (execute esse comando no diretório raiz do seu projeto).

### Named imports

Você pode importar funcionalidades específicas do React para um arquivo, em vez da biblioteca React inteira, usando o que chamamos de "named imports", ou sintaxe de *importação de nome*:

```js
import { useState, useEffect } from 'react';
```

No exemplo acima, estamos importando as funções `useState` e `useEffect` da biblioteca React. Agora, sempre que você precisar usar `useState`, basta chamar `useState()` (e não `React.useState()`).

A sintaxe de importação de nome funciona com qualquer outra biblioteca que exporte funções ou variáveis.

### `npm start`

Esse comando iniciará seu aplicativo React e você poderá vê-lo na URL: http://localhost:3000/

Seu aplicativo será recarregado automaticamente sempre que você salvar seus arquivos (é o chamado *hot reload*).

### `npm run build`

Este é o comando para construir e criar um package da aplicação. O build é armazenado na pasta `/build`, que você pode copiar para fazer o deploy da aplicação.

### Organização de arquivos

O CRA gera vários arquivos e pastas, trazendo uma estrutura inicial de projeto. Você pode, no entanto, modificá-la e criar sua própria estrutura de pastas e arquivos.

A **página index do HTML** é encontrada em `/public/index.html` e contém uma `<div id="root"></div>` onde a mágica do React acontecerá. **Estilos gerais**, aplicáveis ​​a toda a página, podem ser encontrados em `/src/index.css`.

O React se liga à aplicação no arquivo **`/src/index.js`**.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

O **componente principal** da aplicação é o arquivo `/src/App.js`. É ele que está sendo renderizado no `index.js`.

Uma organização melhor do seu código JavaScript é usar *módulos* e a convenção "uma preocupação, um módulo". O modelo geral para um módulo é:

- fazer o import/require de dependências;

- prover uma API na forma de uma função/classe/objeto (seu componente, por exemplo);

- exportar a API (para que possa ser importada por outros arquivos/módulos JS).

    - `export default <nome_da_API>`

Exemplo:

```js
// --- fazer o import/require de dependências
import logo from './../images/whinepad-logo.svg';

// qualquer outro código/funções necessárias

// --- prover uma API
const Logo = () => {
    return <img src={logo} width="300" alt="Whinepad logo" />;
};

// qualquer outro código/funções necessárias

// --- exportar a API
export default Logo;
```

Outra convenção é ter um arquivo CSS por componente React, contendo os estilos relacionados a esse componente. Ele pode ser chamado de `NomeDoComponente.css`. Esse arquivo é importado pelo componente (`import './NomeDoComponente.css'`).

- existem várias outras formas de estilizar e organizar os arquivos CSS, como o [CSS Modules](https://github.com/css-modules/css-modules), [SASS](https://sass-lang.com/guide), etc.

## Fim do livro

O livro aborda ainda mais alguns conceitos e prossegue criando uma aplicação de avaliação de vinhos (ou qualquer outra coisa, fazendo um trocadilho entre wine e whine). 

Apesar de o React ter passado por modificações em suas novas versões, muitas coisas ainda se mantêm e o core não mudou. O livro continua como um ótimo material introdutório à tecnologia, principalmente por explicar como ela funciona por detrás, ao invés de partir direto para function components. **Recomendo [adquirir uma cópia do livro](https://www.amazon.com.br/React-Up-Running-Stoyan-Stefanov/dp/1492051462/)** e usá-lo como referência (não estou ganhando nada com isso). 

Finalizo minhas anotações sobre ele por aqui (11/01/2023).

--- 

# Rotas e o React Router 

O [React Router](https://reactrouter.com/en/main) é uma biblioteca para gerenciar rotas dentro de uma SPA (Single Page Application). **O conceito de SPA é dar ao usuário uma experiência de navegação dentro de uma única página**, de modo que ao clicar em algum link a página não é totalmente recarregada: **somente os elementos e dados necessários são atualizados no DOM**. 

Entenda "rotas" como sendo aquele texto que vem após a URL da aplicação. Por exemplo: `http://meusite.com/rota-para-outra-pagina`. Pode ser vista como um sinônimo para "página".

Comando para instalar a biblioteca (o `@6` indica que será instalada a versão 6):

    npm install react-router-dom@6

- Quando seu projeto trabalha com rotas (no caso de ter criado um projeto usando o CRA), uma convenção é renomear o App.js (ou o arquivo que usar o `<BrowserRouter />`) para routes.js, e o nome da função para AppRouter ou AppRoutes;

## Componentes do React Router

> A biblioteca está em constante evolução, com novos componentes sendo adicionados e outros removidos ou renomeados, então vale a pena verificar a documentação de cada versão no [site oficial](https://reactrouter.com/en/main).

Estes são os componentes mais usado na versão 6:

- `<BrowserRouter />`: é o componente principal, necessário para para fazer o gerenciamento das rotas. Ele irá acessar o mesmo que `window.location.pathname` (tudo que vem depois do domínio na URL). É dentro dele que são colocados os outros componentes da biblioteca;

- `<Routes />`: age como um container para as rotas disponíveis, agregando um conjunto de `<Route />`;

- `<Route />`: é o componente que contém informações sobre uma rota:

    - ele possui algumas props, como caminho para a rota (prop `path`) e a página que vai ser renderizada (prop `element`);

    - exemplo de rota para página 404: `<Route path="*" element={/* seu JSX para página não encontrada */} />`.

- `<Link />`: substitui a tag `<a>`, já que esta tag faz um refresh do navegador para ir para outra página. O componente, por outro lado, altera a rota (com a prop `to`) e impede o refresh, mantendo o conceito de SPA.

Componentes dentro de `<BrowserRouter />`, porém fora de `<Routes />` serão **renderizados em todas as rotas**. Isso serve para fixar componentes comuns a diferentes páginas, como header, navegação, footer, etc.

Rotas aninhadas: são rotas `<Route />` **filhas** de outra `<Route />`. Esse aninhamento serve para que a rota mãe seja um template padrão a ser compartilhado com as rotas filhas. Quando algumas páginas (mas não todas) compartilham de layout parecido, você pode utilizar este recurso para evitar repetição de componentes, colocando dentro da rota mãe os componentes em comum para as rotas filhas. O conteúdo de cada rota filha é renderizado na rota mãe por meio do uso do componente `<Outlet />`. 

No exemplo a seguir, temos uma rota para o caminho raiz (`path="/"`), que pode ser entendido como o index da aplicação. O index renderiza um componente `<DefaultTemplate />`. Sub-rotas irão renderizar este componente, mais o componte que está em seu `element`. Então, no caso da rota para `sobre-mim`, será renderizado `<DefaultTemplate />` e dentro dele será renderizado o `<AboutMe />`.

```js
// routes.js
<Route path="/" element={<DefaultTemplate />}>
    <Route path="/" element={<Home />} />
    <Route path="sobre-mim" element={<AboutMe />} />
</Route>

// DefaultTemplate.js
export default function DefaultTemplate() {
    return (
        {
            // aqui você insere os elementos que irão renderizar template.
            // em <Outlet /> será renderizado o conteúdo dos filhos deste componente.
        }
        <Outlet />
    );
}
```

> Qual a diferença no uso do `<Outlet />` e o uso da prop `children`? 
> - Ambas irão renderizar elementos filhos, no entanto, `children` é renderizado quando o componente pai envolve o componente filho. Já `<Outlet />` não precisa envolver os filhos, desde que eles sejam o elemento das rotas aninhadas. Além disso, `<Outlet />` é específico para rotas e não tem efeito fora delas. Em outras palavras, `<Outlet />` renderiza **rotas-filho** e `children`, **elementos-filho**.

O `path` da rota mãe pode ser qualquer rota, não necessariamente `/`. As rotas filhas serão descendentes do caminho que você der à rota mãe caso você coloque um *caminho relativo* (sem o `/` no início) no `path` delas. Caso o caminho de uma rota filha seja o mesmo da mãe, você pode substituir `path="/"` por `index` (sem valor, é um booleano true por padrão) na rota filha.

- Dica: é possível também adicionar `<Routes />` e `<Route />` não somente no arquivo `routes.js`, como também na **definição** de um componente. Isso é útil quando você quer aproveitar dos benefícios da biblioteca react router dom para um componente específico, criando rotas específicas dentro dele. (*eu acho que deixa o código mais confuso e desorganizado...*)

## Hooks do React Router

- `useLocation()`: retorna um objeto com informações sobre a URL. É semelhante ao que recebemos usando `window.location`, porém, retorna mais informações como as query params e state;

    - state é um objeto que pode ser usado para trafegar informações entre o `useLocation` e o `useNavigate`

- `useNavigate()`: retorna uma função que possibilita a navegação por rotas e pelo histórico de navegação. Útil, por exemplo, em eventos de onClick de botão Voltar. A função aceita uma string com a rota de onde se quer navegar, ou também números (para o caso de navegar pelo histórico);
    
    - o segundo parâmetro da função retornada pelo `useNavigate` é opcional e nela você pode enviar o state, que pode ser utilizando pelo `useLocation`; desse modo, você navega para uma página e pode passar alguma informação para esta página;

        ```js
        navigate(`prato/${prato.id}`, { state: {...prato} });

        // em algum outro arquivo
        console.log(useLocation())
        ```

    - em versões anteriores à do react router 6, o hook utilizado era o `useHistory()`;

    - existe também o componente `<Navigate />`. O funcionamento é semelhante ao do hook? Existem diferenças ou preferência no uso de um ou outro? -> Resposta monitor no fórum Alura:  Apesar de ambos terem a mesma finalidade, que é navegar entre as rotas, eles têm diferenças importantes. O **hook** `useNavigate` é uma função que retorna um objeto contendo um método navigate. Esse método pode ser utilizado para navegar para uma rota específica. Ele é utilizado **dentro de um componente funcional**. Já o componente `<Navigate />` é um componente que **pode ser utilizado dentro de um componente funcional ou de classe** para navegar para uma rota específica. Ele é muito útil quando você precisa navegar para uma rota em resposta a um evento, como um clique em um botão. A principal diferença entre os dois é que o hook useNavigate é mais flexível e permite que você navegue para uma rota em qualquer lugar do seu componente. Já o componente `<Navigate />` é mais específico e deve ser utilizado dentro do corpo do componente. Em resumo, se você precisa navegar para uma rota em resposta a um evento, use o componente `<Navigate />`. Se você precisa navegar para uma rota em qualquer lugar do seu componente, use o hook `useNavigate`.

- `useParams()`: retorna um conjunto de pares "chave: valor" com os parâmetros enviados via URL. Por exemplo, na URL `http://meusite.com/posts/1`, o parâmetro da rota `posts` é `1`). Os parâmetros podem ser enviados ao `path` das rotas utilizando um seletor com a nomenclatura `:nomeDoParametro` após a rota. O hook irá retornar o par `nomeDoParametro: valor`. Para acessar o valor, basta usar a notação de ponto (`.nomeDoParametro`) ou fazer o destructuring do objeto de retorno. Exemplo de código:

    ```js
    // --- dentro do JSX que contém as rotas
    <Route path="posts/:id" element={<Post />} />

    // --- em algum outro código que utiliza o useParams
    const { id } = useParams();
    ```

    - Para passar mais parâmetros, você deve separá-los por `/` no path. Exemplo de código:

        ```js
        // --- dentro do JSX que contém as rotas
        <Route path="/posts/:sectionId/:postId" element={<Post />} />

        // --- em algum outro código que utiliza o useParams
        const { sectionId, postId } = useParams();
        ```

- `useSearchParams()`: trabalha com os parâmetros enviados pela URL do modo "convencional", que é utilizando **query params** (também chamadas de "query strings"), em que os parâmetros são enviados após `?` e concatenados por `&` na URL (exemplo `http://meusite.com/posts?sectionId=1&postId=7`). O retorno desse hook é um objeto do tipo URLSearchParams. Exemplo de código:

```js
// --- neste caso path não possui seletores
<Route path="posts" element={<Post />} />

// --- em algum outro código que utiliza o useSearchParams
// observe o uso de colchetes ao invés de chaves (semelhante a useState)
const [searchParams, setSearchParams] = useSearchParams();

// uso do get para obter o valor dos parâmetros
console.log(searchParams.get('sectionId'));
console.log(searchParams.get('postId'));

```

Query params são mais comuns para realizar buscas e filtros de resultados, por exemplo. Para renderizar conteúdos como posts diferentes, costuma-se utilizar mais os parâmetros de rota.

---

# Gerenciando estados com a Context API

A Context API faz parte da biblioteca padrão do React (ou seja, o import de suas funcionalidades é direto de `import { ... } from 'react'`).

A Context API provê variáveis (e funções) que podem ser utilizadas por diversos componentes sem a necessidade de passá-las via props. Isso previne o prop drilling.

## Prop drilling

Imagine a situação em que você possui vários componentes aninhados:

```xml
<ComponenteA>
    <ComponenteB>
        <ComponenteC>
            <ComponenteD />
            <ComponenteE />
        </ComponenteC>
    </ComponenteB>
</ComponenteA>
```

Aí imagine que `<ComponenteD />` precisa de uma informação que se encontra no `<ComponenteA />`. Você pode passar essa informação via props. No entanto, para fazer isso, você precisa **passar a prop para cada subcomponente** até chegar no `<ComponenteD />`, ou seja, passar para o `<ComponenteB>`, que passa para o `<ComponenteC>` que então passa para o `<ComponenteD>`. Isso é o **prop drilling**.

A grosso modo, com a Context API as informações de "uso geral" ficam como que em "variáveis globais", que os componentes e subcomponentes podem acessar. Já não preciso ficar passando essas informações via props.

A gente fala "gerenciamento de estados" porque em muitos casos o que queremos compartilhar entre os componentes são variáveis de estados, para permitir que eles se atualizem conforme algum estado seja alterado.

## Como usar

- `createContext()`: atribuído a uma constante, por exemplo, faz com que essa constante se torne um contexto. Essa constante é nomeada com a inicial em maiúsculo, pois também irá se tornar dois componentes: `<NomeDoComponente.Provider />` e `<NomeDoComponente.Consumer />`. Como o nome após o `.` indica, esses componentes têm a função de prover contexto ou consumir contexto. É possível passar um valor default como argumento para `createContext()`; esse valor será utilizado caso não seja passado nada na prop `value` do `NomeDoComponente.Provider`.

- `<NomeDoComponente.Provider />`: provê o contexto. Os valores que o contexto provê (por exemplo, estados e funções) são passados via prop `value`. Subcomponentes desse `Provider` poderão solicitar acesso ao contexto;

- `<NomeDoComponente.Consumer />`: indica que seus subcomponentes podem consumir os valores de contexto. Essa é a forma **antiga** de uso de contexto, da época em que existiam somente class components. Dentro desse componente vai uma arrow function, cujos parâmetros são os dados de contexto, e cujo retorno é o conteúdo que irá consumir esse dados (componentes e subcomponentes)). A forma mais **moderna** é por meio do **hook** `useContext()`;

- `useContext()` recebe como parâmetro um objeto de contexto. Esse objeto é a constante que você define com o `createContext()`. O retorno desse hook é um objeto com todos os dados de contexto disponíveis (pela prop `values`).

Dadas essas definições, segue um exemplo utilizando estes componentes e o hook: 

```js
// --- em um arquivo context.js, por exemplo
// exportando o contexto
export const UserContext = createContext();

// --- em um arquivo que irá prover os dados de contexto 
// supondo que UserContext foi importado
<UserContext.Provider value={{ dado1, dado2 }}>
    {/* aqui são incluídos os componentes (e seus subcomponentes) que terão acesso ao contexto */}
</UserContext.Provider>

// --- em um arquivo que irá consumir o contexto
// supondo que UserContext foi importado

// [MODO ANTIGO]
<UserContext.Consumer>
    { ({ dado1, dado2 }) => (
        {/* 
            JSX vai dentro dessa arrow function.
            Componentes dentro do contexto têm acesso aos dados de contexto, sem necessidade de recebê-los via props
        */}
    )}        
</UserContext.Consumer>

// [MODO NOVO]
const { dado1, dado2 } = useContext(UserContext);
```

## Debug

Para facilitar o debug de contexto, pode-se usar a extensão  [React Context DevTool](https://chrome.google.com/webstore/detail/react-context-devtool/oddhnidmicpefilikhgeagedibnefkcf) no Chrome. Por meio da extensão é possível monitorar os contextos e suas mudanças de valor.

- Pode ser necessário também ter a extensão [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/) instalada para que o React Context DevTool funcione corretamente.

A constante que recebe o `createContext()` também irá possuir uma propriedade chamada `displayName`. Pode ser atribuído a essa propriedade uma string qualquer. O conteúdo dessa string servirá para identificar esse contexto na aba "React Context" que irá aparecer no Dev Tools do navegador após a instalação da extensão. 

- o uso do `displayName` é opcional. Caso não seja dado um nome, o contexto ainda irá aparecer no Dev Tools, mas com um nome qualquer, por exemplo: "debugId_5". Quando se tem vários contextos, o uso do `displayName` facilita a identificação de cada um.

---

# Gerenciando estados com Recoil

> Os conceitos de Recoil foram dados em um curso de React com TypeScript, então alguns códigos de exemplo estarão com tipagem.

O Recoil é outra biblioteca que possibilita o gerenciamento de estados em uma aplicação React. 

Instalação:

    npm i recoil

## Context API vs Recoil

Durante o curso que ensinou Recoil, resolvi **utilizar a chatGPT (que aceitou ser chamada de "Chat" e tratada pelos pronomes ela/dela)** para me ajudar nas minhas dúvidas. Isso foi feito a partir de 13/04/23. Segue a tradução de uma conversa em inglês feita com ela:

**Pergunta**: qual é o mais relevante com relação ao gerenciamento de estados em React: Context API ou Recoil?

**Chat**: Tanto a Context API quanto o Recoil são opções viáveis para o gerenciamento de estados em React, e ambos têm suas vantagens e desvantagens.

A **Context API** é uma ferramenta que permite o compartilhamento de dados entre componentes em uma hierarquia de componentes, sem precisar passar manualmente as propriedades por vários níveis de componentes. A Context API é **incluída no React**, portanto, é fácil de usar e bem documentada. No entanto, em casos de aplicações com **grande volume de dados**, ela pode ter um **desempenho um pouco inferior** se comparada com outras ferramentas de gerenciamento de estados.

O **Recoil** é uma biblioteca de gerenciamento de estados **desenvolvida pelo Facebook**, que visa facilitar o gerenciamento de estados em aplicações React. Ele possui recursos avançados de gerenciamento de estado, como suporte a átomos (unidade fundamental de estado no Recoil), seletores (funções que permitem combinar, transformar e selecionar dados dos átomos) e suporte para "async selectors" (seletores assíncronos). O Recoil pode ser uma ótima opção para **aplicações maiores e complexas**, onde o gerenciamento de estados é mais crítico.

Em resumo, ambas as opções são úteis e podem ser usadas em diferentes cenários. A Context API é uma ótima opção para projetos menores e mais simples, enquanto o Recoil é uma biblioteca mais avançada que pode ser melhor para projetos maiores e complexos.

**P**: Por favor, poderia informar quando cada uma delas foi lançada?

**C**: Claro, aqui está a data de lançamento de cada uma das ferramentas:

Context API: foi lançada pela primeira vez no React v16.3.0 em 29 de março de 2018.

Recoil: foi lançada pela primeira vez em maio de 2020, com sua primeira versão estável sendo lançada em outubro de 2020. É uma biblioteca relativamente nova em comparação com a Context API, mas já ganhou bastante popularidade no ecossistema React.

**P**: por favor, pode me passar o link para a documentação de cada uma?

**C**: claro, aqui estão os links para a documentação da Context API e do Recoil:

Context API: https://pt-br.reactjs.org/docs/context.html

Recoil: https://recoiljs.org/docs/introduction/getting-started

**P**: o link da context API é antigo e foi atualizado. Segue o link atual: https://react.dev/learn/passing-data-deeply-with-context

**C**: Peço desculpas pela informação desatualizada. Obrigado por informar. Realmente, a documentação da Context API foi atualizada e pode ser acessada no link que você informou: https://react.dev/learn/passing-data-deeply-with-context.

O link que eu informei anteriormente, embora ainda válido e funcional, redireciona para a nova documentação no link que você informou. Mais uma vez, obrigado por atualizar a informação.

## Componente `<RecoilRoot />`

Para um componente `A` ter acesso às funcionalidades do Recoil, é necessário que ele **descenda do `<RecoilRoot />`**. Subcomponentes de `A` também terão acesso ao Recoil. 

```js
import { RecoilRoot } from 'recoil';

export default function App() {
  return (
    <RecoilRoot>
        {/* JSX */}
    </RecoilRoot>
  );
}
```

## Atom

Conceito introduzido pelo Recoil, em que um estado é composto por vários átomos. O átomo então **representa um pedaço de estado** a ser gerenciado pelo Recoil.

### Criação de um átomo

Uma abordagem é criar no `src` do projeto uma pasta `state` e um arquivo `atom.ts`, em que definimos e exportamos todos os átomos.

Para criar um átomo, utiliza-se a **função `atom()`** e um objeto com **duas propriedades**:

- `key`: chave única que identifica esse átomo. É usado internamente pelo Recoil para gerenciamento dos átomos. Parece ser uma convenção utilizar o nome da variável instanciada como sendo a key;

- `default`: valor inicial para o átomo.

```js
import { atom } from "recoil";

// o átomo será um array de um tipo IEvento, vazio por padrão
export const listaDeEventosState = atom<IEvento[]>({
    key: 'listaDeEventosState',
    default: []
});
```

### Lendo um átomo com useRecoilValue

O Recoil disponibiliza três hooks para manipular os átomos: um somente para ler o valor, outro somente para alterar o valor, e um terceiro que faz os dois: lê e altera o valor.

Para **ler o valor** contido em um átomo, pode ser utilizado o hook `useRecoilValue`, passando como parâmetro o átomo desejado. Esse hook possui um **comportamento parecido com o `useState`**, em que, caso haja uma mudança no valor do átomo, o Recoil irá avisar ao componente para se renderizar novamente.

```js
import { useRecoilValue } from 'recoil';
import { listaDeEventosState } from '../../state/atom';

const ListaDeEventos<T> = () => {
  // eventos irá acessar o átomo listaDeEventosState
  const eventos = useRecoilValue(listaDeEventosState);
  return (
    {/* JSX */}
  )
}

export default ListaDeEventos;
```

### Alterando um átomo com useSetRecoilState

Para fazer **alteração no valor** de um átomo, é utilizado outro hook, o `useSetRecoilState`, passando como parâmetro o átomo desejado. Esse hook **retorna uma função setter** para o átomo informado. Como está sendo feita somente alteração de valor, esse hook **não causa uma nova renderização** do componente. No jargão do Recoil, é dito que o componente não está subscrito ao átomo.

```js
import { useSetRecoilState } from 'recoil';

// componente do tipo React.FC (function component)
// é uma abordagem antiga de tipagem que já não se usa atualmente
const Formulario: React.FC = () => {
  // criando a função setter, que irá manipular arrays do tipo IEvento
  const setListaDeEventos = useSetRecoilState<IEvento[]>(listaDeEventosState);

  // ...

  const submeterForm = (event: React.FormEvent<HTMLFormElement>) => {
    //usando a função setter
    setListaDeEventos(listaAntiga => [...listaAntiga, evento]);
    // ...
  }

  return (
    <form className={style.Formulario} onSubmit={submeterForm}>
    {/* ... */}
    </form>
  )
}

export default Formulario
```

### Lendo e alterando um átomo com o `useRecoilState`

Quando o componente precisa **ler e alterar** o estado em um átomo, é utilizado o hook `useRecoilState`, cujo retorno é semelhante ao `useState`.

```js
const [tamanhoDaFonte, setTamanhoDaFonte] = useRecoilState<number>(tamanhoDaFonteState)
```

## Encapsulando o Recoil

Uma boa prática para organizar as responsabilidades do código é encapsular o uso do Recoil em um **hook customizado**, exportando esse hook para ser utilizado pelos componentes. Desse modo, **os componentes não precisam saber do Recoil nem como alguma funcionalidade é feita** - somente chamam essa funcionalidade. 

Isso também garante facilidade de manutenção do código: caso alguma regra de negócio seja alterada (ou até mesmo se decida que não será mais utilizado o Recoil), basta fazer a alteração no hook e isso será replicado para todos os componentes que fazem uso do hook. 

Uma sugestão é agrupar estes hooks customizados dentro de uma pasta em `state/hooks`, usando a convenção de colocar o prefixo `use` no nome do hook (exemplo: `useAdicionarNaLista`).

## Seletores

Seletores são uma forma de se aproveitar de um átomo para entregar um **estado derivado de um estado original**. Por exemplo, temos um átomo representando o estado de uma lista de eventos. Podemos criar um seletor que irá mostrar essa lista de eventos filtrada por data. **O seletor será atualizado toda vez que o átomo for atualizado**. Componentes podem então obter o valor diretamente do seletor, ao invés do átomo, usando a mesma função `useRecoilValue`.

Para criar um seletor, utiliza-se a função `selector`, que recebe como argumento um objeto com duas propriedades (há outras propriedades opcionais):

- `key`: chave única que identifica esse seletor;

- `get`: uma função que define como será o estado derivado. Essa função possui um objeto como argumento, que contém um método também chamado `get`. Esse método `get` pode ser utilizado para obter o valor de átomos e outros seletores (substitui o `useRecoilValue`).

Uma sugestão é criar os seletores em um arquivo `state/seletores/index.ts` e exportá-los desse arquivo.

```js
// state/seletores/index.ts
import { selector } from "recoil";
import { filtroEventos, listaDeEventosState } from "../atom";

export const eventosFiltradosState = selector({
    key: 'eventosFiltradosState',
    get: ({ get }) => {
        // meu seletor irá trabalhar com os valores dos átomos filtroEventos e listaDeEventosState
        const filtro = get(filtroEventos);
        const todosEventos = get(listaDeEventosState);

        // aplicando o filtro
        const eventos = todosEventos.filter(evento => {
            if (filtro.data) {
                return (filtro.data.toISOString().slice(0, 10) === evento.inicio.toISOString().slice(0, 10));
            }
            else return true;
        })

        // eventos será então um derivado de listaDeEventosState, trazendo somente os eventos de uma determinada data
        return eventos;
    }
})

// usando o seletor em algum componente/arquivo
import { useRecoilValue } from "recoil";
import { eventosFiltradosState } from "../seletores";

const eventos = useRecoilValue(eventosFiltradosState);
```

### Seletores para eventos assíncronos

Seletores também podem ser usados para lidar com **operações assíncronas**, de modo a obter dados que vêm de uma fonte externa (uma API, um arquivo, etc), que geralmente retornam Promises. Nesse caso, eles não necessariamente vão derivar um estado ou manipular átomos. Sua função get somente irá retornar algum dado.

O retorno de um seletor assíncrono pode ser usado para atribuir um valor default para um átomo, como pode ser visto no exemplo abaixo. 

```js
// --- algum arquivo com os seletores
// este seletor irá obter dados da API e retorná-los no padrão utilizado pela aplicação
export const seletorAsync = selector({
    key: 'seletorAsync',
    get: async () => {
        // obtém os dados da API
        const response = await fetch('http://localhost:8080/eventos');
        const dados: IEvento[] = await response.json();
        return dados.map(evento => ({
            ...evento,
            inicio: new Date(evento.inicio),
            fim: new Date(evento.fim),
        }));
    }
})

// --- atom.ts
import { seletorAsync } from "./seletores";

// o valor inicial do átomo será o que for retornado pelo seletorAsync
export const listaDeEventosState = atom<IEvento[]>({
    key: 'listaDeEventosState',
    default: seletorAsync
});
```

Quando algum componente do Recoil **depende de um resultado assíncrono**, é necessário envolvê-lo em um componente React chamado `<Suspense />`, que irá renderizar alguma coisa enquanto a requisição é processada. Essa "alguma coisa" é inserida na prop `fallback` desse componente, e pode ser uma string ou outro componente (você pode utilizá-la para exibir uma mensagem de loading, por exemplo).

No exemplo abaixo, `<ListaDeEventos>` é um componente que renderiza os eventos recebidos via API. O texto "Carregando eventos..." será exibido na tela enquanto se espera o término da chamada assíncrona que traz os eventos da API. Ou seja, o conteúdo da `div` só irá aparecer na tela quando a Promise for resolvida.

```js
<Suspense fallback='Carregando eventos...'>
    <div className={style.App}>
        <ListaDeEventos />
    </div>
</Suspense>
```

A abordagem utilizando seletor assíncrono e o componente `<Suspense />` é uma das soluções possíveis com o Recoil. No entanto, abordagens mais "tradicionais" utilizando o `useEffect` também são válidas.

- Resposta do instrutor no fórum Alura: "A principal diferença entre as duas abordagens é que o **seletor assíncrono e o componente Suspense** são específicos do Recoil e foram criados para lidar com o **gerenciamento de estado de forma mais eficiente e organizada**, enquanto o `useEffect` é um hook padrão do React que pode ser utilizado em qualquer situação que envolva efeitos colaterais. Ambas as abordagens são válidas e podem ser utilizadas de acordo com a necessidade do projeto e preferência do desenvolvedor. O importante é entender as diferenças e vantagens de cada uma delas para escolher a melhor opção em cada situação."

## Debug de estados

O Recoil oferece o que eles chamam de "Dev Tools", que são alguns hooks e métodos para fazer o debug de estados, possibilitando observar e logar mudanças de estados. Essa API ainda se encontra em desenvolvimento e pode ser [vista na documentação oficial](https://recoiljs.org/docs/guides/dev-tools).

Exemplo de componente que loga toda vez que um átomo é modificado (se aparecer um erro em `snapshot.getNodes_UNSTABLE`, altere o `tsconfig.json` e adicione a seguinte configuração em `compilerOptions`: `"downlevelIteration": true`):

```js
import { useEffect } from "react";
import { useRecoilSnapshot } from "recoil";

export default function DebugObserver() {
    const snapshot = useRecoilSnapshot();
    useEffect(() => {
        console.debug('The following atoms were modified:');
        for (const node of snapshot.getNodes_UNSTABLE({ isModified: true })) {
            console.debug(node.key, snapshot.getLoadable(node));
        }
    }, [snapshot]);
    return null;
}
```

---

# Requisições a APIs utilizando o Axios

O Axios é uma biblioteca utilizada para fazer requisições a uma URL ou API. Semelhante à Fetch API.

[Link com a documentação do Axios](https://github.com/axios/axios).

Comando para instalação:

    npm i axios

A seguir, um pequeno resumo sobre requisições utilizando o Axios. Além delas, há também a `request`, cujos argumentos são um pouco diferentes, mas que serve quando queremos utilizar uma mesma requisição para diferentes finalidades (get, put, post, etc). Um exemplo dela se encontra ao final da [Seção sobre envio de conteúdo binário](#envio-de-conteúdo-binário-usando-formdata).

## Requisição GET

Retorna uma **promise** com a resposta. A resposta é um objeto e o conteúdo da resposta fica na propriedade `data`. Diferente da Fetch API, aqui **não** é preciso converter a resposta para JSON antes de utilizá-la.

```js
import axios from 'axios';

axios.get('http://localhost:8000/api/v1/restaurantes/')
    .then(response => {
        console.log(response.data)
    })
    .catch(error => {
        console.log(error)
    })
```

## Requisições POST/PUT

Além do endpoint (a URL de acesso ao recurso da API), passamos no segundo argumento os dados a serem salvos. É necessário verificar na API o tipo e formato esperado desses dados.

No exemplo abaixo, a API espera receber um objeto com a propriedade `nome`, que é do tipo string:

```js
nomeRestaurante = 'Alura Deli';

axios.post(
    'http://localhost:8000/api/v2/restaurantes/',
    {
        nome: nomeRestaurante
    }
)
    .then(() => console.log('Restaurante cadastrado'))
    .catch((err) => console.log(err))
}
```

## Requisições DELETE

É enviada na URL a id do que deseja ser deletado. É necessário verificar na documentação da API se é assim que ela trabalha com este verbo.

```js
axios.delete(`http://localhost:8000/api/v2/restaurantes/${restaurante.id}/`)
    .then(response => {
        console.log('Restaurante excluído');
    })
    .catch(err => console.log(err))
```

## Enviando parâmetros 

Caso a API aceite parâmetros em suas requisições, este parâmetros podem ser enviados de duas formas. Uma forma é fazer o envio dos parâmetros pela URL, via query string:

```js
axios.get<IPaginacao<IRestaurante>>('http://localhost:8000/api/v1/restaurantes/?ordering=nome&search=neo');
```

A outra forma é usando a propriedade `params`. Eu prefiro dessa forma:

```js
axios.get<IPaginacao<IRestaurante>>('http://localhost:8000/api/v1/restaurantes/', {
    params: {
        ordering: 'nome',
        search: 'neo'
    }
});
```

É necessário verificar na documentação de cada API quais parâmetros são aceitos e seus tipos.

## Criação de instância `axios` para reúso

No caso de trabalharmos com a mesma URL em diferentes partes do código, ao invés de ficar repetindo a URL, podemos criar uma instância do Axios com essa URL base. Assim, quando chamamos esta instância, passamos somente o restante do caminho para completar a URL.

```js
// em um arquivo .ts separado
import axios from "axios";

const http = axios.create({
    // essa será a URL ao se utilizar a instância http criada
    baseURL: 'http://localhost:8000/api/v2/'
});

export default http;

// em algum outro arquivo

// usando a instância do axios, somente adiciono o restante do caminho necessário
http.get<IRestaurante[]>('restaurantes/')

http.delete(`restaurantes/${restaurante.id}/`)
```

## Envio de conteúdo binário usando `FormData`

Quando queremos enviar ao backend algum **conteúdo binário** (o upload de uma imagem, por exemplo), não é possível o envio de um JSON, que só trafega texto. 

Neste caso, uma solução (existem outras) é criar uma instância da classe `FormData`. Essa é uma classe nativa de JS para envio de dados de formulário, num padrão de chave/valor, enviando o conteúdo com o encoding correto.

Para adicionar dados à instância de `FormData`, é utilizado o método `append`, sendo o primeiro argumento o nome do que está sendo enviado, e o segundo argumento, o conteúdo.

```js
const formData = new FormData();
// conteúdo de string
formData.append('nome', nomePrato);
formData.append('descricao', descricao);
formData.append('tag', tag);
formData.append('restaurante', restaurante);

// conteúdo binário
if(imagem) {
    formData.append('imagem', imagem);
}
```

Ao enviar esses dados ao back-end, é necessário informar no **cabeçalho da requisição** o tipo de conteúdo que está sendo enviado. Essa parte deve ser verificada com cada API. Um exemplo utilizando o método `request` do Axios:

```js
http.request({
    url: 'pratos/',
    method: 'POST',
    // informando que o conteúdo não contém somente texto
    headers: {
        'Content-Type': 'multipart/form-data'
    },
    data: formData
})
```

Mais sobre FormData [neste artigo da MDN](https://developer.mozilla.org/en-US/docs/Web/API/FormData).

--- 

# Convenções e dicas diversas

## Props que são objetos

Um componente pode receber como prop um objeto que contém várias propriedades. Uma abordagem simples é **passar o próprio objeto como prop** e depois desestruturar suas propriedades no corpo do componente. 

```js
{menu.map(item => (
    // passando o objeto item na prop homônima
    <Item key={item.id} item={item} />
))}
```

Isso também pode ser simplificado desestruturando o objeto na hora de passar como prop, assim o componente já recebe cada uma dessas propriedades do objeto como uma prop:

```jsx
{menu.map(item => (
    // desestruturando o objeto item na prop homônima
    <Item key={item.id} {...item} />
))}
```

Passar um objeto para uma prop, no entanto, **não é uma boa abordagem** quando você trabalha com **dados recebidos de uma API**, em que você não tem controle sobre tudo que a API irá retornar. Passando o objeto inteiro, há o risco de você incluir no componente diversas **propriedades desnecessárias para a renderização**, comprometendo a **performance**. Nesses casos, o bom é passar ao componente **cada propriedade de forma individual**:

```js
{menu.map(item => (
    // passando somente as props necessárias (suponha que item tenha várias outras propriedades)
    <Item key={item.id} title={item.title} price={item.price} description={item.description} />
))}
```

## Import de arquivos estáticos

Colocar o caminho para uma imagem na propriedade `src` de um `img`, por exemplo, **não irá funcionar** na hora de fazer o build de uma aplicação React. Isso ocorre por que o webpack (o software por trás da geração do build no CRA) não consegue entender que a string passada em `src` é de um arquivo estático, nem entender o caminho passado nessa string. Por conta disso, a solução do CRA é que se faça o **import dos arquivos estáticos** e utilizar o objeto deste import para atribuir a `src`.

- Nem mesmo rodando localmente irá funcionar colocar o caminho do arquivo estático na propriedade `src`, já que, quando a aplicação roda localmente, é feito um build dela em memória.

```jsx
// import
import logo from '../assets/logo.svg';

// uso
<img src={logo} alt="texto alternativo" />
```

**Importante:** no caso de **arquivos CSS**, o caminho para a imagem em uma propriedade CSS **pode sim ser o caminho relativo ou absoluto**, sem a necessidade de fazer algum import. O CRA nesse caso consegue entender onde encontrar a imagem na hora de gerar o build.

- uma convenção é criar uma pasta `assets` dentro de `src`, para armanezar as imagens (pode até criar subpastas para cada página criada ou da maneira que preferir para organizar as imagens).

### E quando tenho várias imagens?

Imagine a situação em que você possui muitas imagens. Fazer o import individual utilizando a solução acima já não se mostra viável, ainda mais conforme a aplicação cresce. Neste caso, o melhor seria referenciar o caminho para cada imagem dinamicamente, dentro de um loop, por exemplo. Como fazer então o import de maneira dinâmica?

A solução dada pelo CRA é **salvar essas imagens dentro da pasta `public`** do projeto. Todos os arquivos dentro dela são visíveis e **acessáveis a partir da raiz** do projeto. Por exemplo, pode-se criar uma pasta `assets` dentro de `public` e colocar as imagens nesta nova pasta. Para utilizá-las como caminho da propriedade `src` de uma tag `img`, você utiliza `/assets/nome_da_imagem.extensao`.

Para deixar o projeto ainda mais organizado, você pode criar subpastas para organizar as imagens por tema ou página, por exemplo.

## SVG como componente

É possível transformar um SVG em um componente React utilizando o pacote SVGR, disponível por padrão em um projeto criado com o CRA:

```js
import { ReactComponent as NomeParaOComponente } from 'caminho_do_arquivo_SVG';
```

## Absolute imports

Ao criar um arquivo `jsconfig.json` com o código abaixo, é possível deixar a pasta `src` como sendo a raiz para os imports de arquivos, facilitando o import entre componentes e outras subpastas de `src`:

```json
{
    "compilerOptions": {
        "baseUrl": "src"
    },
    "include": ["src"]
}
```

É possível adicionar outros caminhos nessa configuração, criando diferentes absolute imports a partir de diferentes diretórios. Isso facilita a leitura dos imports (imagine um projeto grande, a quantidade de `../` que poderia existir com imports de caminhos relativos). 

No exemplo abaixo, será possível importar arquivos das pastas `src/components`, `src/pages` e `src/assets` a partir de absolute imports (você começa o import usando `components/...`, ou `pages/...`, ou `assets/...` para ir direto aos arquivos destas pastas):

```json
{
    "compilerOptions": {
        "baseUrl": "src",
        "paths": {
            "components": ["components"],
            "pages": ["pages"],
            "assets": ["assets"]
        }
    },
    "exclude": ["node_modules"],
    "include": ["src"]
}
```

## Biblioteca React Markdown

Oferece um componente `<ReactMarkDown /> ` em que você pode englobar um texto escrito em linguagem Markdown. Este componente irá fazer a conversão do texto em Markdown para um equivalente em HTML. 

É necessário instalar a biblioteca

    npm install react-markdown

## Alternativa ao reset.css

O CRA oferece um reset.css próprio ao adicionar o seguinte comando no seu arquivo CSS mais no topo: `@import-normalize;`.

*Isso, no entanto, parece não funcionar... Pelo menos comigo não deu certo.*

Uma alternativa é instalar o normalize.css manualmente

    npm i normalize.css

E depois importá-lo antes de qualquer outro CSS

```jsx
import 'normalize.css';
```

## Pacote classnames

Quando um elemento HTML possui mais de uma classe (atributo `className`) e levando em conta o uso de CSS modules, é comum ser necessário utilizar template strings para concatenar as strings e variáveis JS. Isso pode gerar um código confuso e difícil de ler. Por meio do pacote classnames, é possível adicionar várias classes de uma maneira mais fácil.

Necessário instalar o pacote:

    npm i classnames

As classes são adicionadas utilizando a função classNames(), que aceita como argumento um objeto, cujas propriedades terão como chave as classes em si (informadas dentro de **colchetes**, pois é um valor que será computado, não determinado em tempo de execução), e como valor um booleano indicando se serão ou não incluídas. Como é um booleano, é possível criar condicionais para decidir quando a classe deve ou não ser incluída.

Exemplo com template string concatenando as classes (supondo estar utilizando CSS Modules):

```html
<button className={`${styles.filtros__filtro} ${active === option.id ? styles['filtros__filtro--ativo'] : ''} `}>
    {option.label}
</button>
```

Exemplo com o pacote classNames:

```js
import classNames from 'classnames';

// filtros__filtro será sempre aplicado, enquanto filtros__filtro--ativo será aplicado somente quando a condição for verdadeira
className={classNames({
    [styles.filtros__filtro]: true,
    [styles['filtros__filtro--ativo']]: active === option.id
})}
```

Caso os estilos sejam sempre `true`, você pode passá-los diretamente, sem a necessidade de um objeto:

```js
// supondo que ambos os estilos serão sempre verdadeiros
className={classNames(
    styles.filtros__filtro,
    styles['filtros__filtro--ativo']
)}
```

---

# React Developer Tools

O React Developer Tools é uma extensão para os navegadores que adiciona duas novas opções (abas) no Developer Tools do navegador, específicas para análise de uma aplicação feita em React:

## Components

Nesta aba é possível ver todos os componentes que foram montados na tela e sua hierarquia. Ao clicar em um componente, você pode ver mais detalhes, como as props que o component possui, hooks utilizados, etc. Esses detalhes vão sendo atualizados em tempo real conforme alguma interação é feita na página ou algo causa uma mudança de estado. 

Clicando na engrenagem que aparece quando esta aba está aberta, é possível habilitar a seguinte opção: "Highlight updates when components render.". Isso irá fazer com que uma borda verde apareça brevemente ao redor dos componentes quando eles sofrerem re-renderização por conta de alguma mudança de estado. Isso é útil para analisar componentes que podem ser memoizados ou passar por alguma mudança para melhorar sua performance.

## Profiler

Esta aba coleta informações sobre tempo envolvido na renderização dos componentes, seja quando são montados, seja quando sofrem alguma atualização. Esse tempo auxilia na identificação de gargalos de performance. 

É necessário clicar no ícone de bolinha, inicialmente na cor azul, para começar a monitorar a aplicação. Posteriormente, clica novamente na bolinha, agora na cor vermelha, para parar o monitoramente. Com isso, o Profiler irá listar os componentes e o tempo envolvido em sua montagem.

O Profiler também exibe uma "linha do tempo" de mudanças na aplicação. Novas linhas do tempo são adicionadas a cada interação ou atualização feita pelo React na aplicação, e você pode inspecionar cada uma delas para ver quais componentes foram renderizados ou re-renderizados e o tempo gasto na montagem.

---

# Memoização

Por padrão, quando um componente re-renderiza, **todos os seus subcomponentes** também sofrem a re-renderização. Isso pode impactar a performance no caso de aplicações grandes.

## `memo()`

O React dispobiliza uma função para memoizar (ou memorizar) certas porções de código, para fins de performance: `memo()`. Essa função, quando aplicada a um **function component**, irá verificar o estado dos props do componente antes do render e comparar com seu estado atual. Caso **não tenha havido mudança**, o componente **não** é re-renderizado. 

Isso é útil, principalmente, no caso de componentes que renderizam listas, sendo que os itens dessa lista são outros componentes. Em uma mudança de estado nesse componente de lista (por exemplo, aplicação de um filtro ou ordenação), causa re-renderização da lista e seus componentes-filhos. Se os componentes-filhos forem memoizados, a re-renderização não irá acontecer caso eles não tenham sido alterados. Com isso, somente os itens que foram alterados serão de fato re-renderizados.

```js
import { memo } from 'react';
function Item() {
    // ...
}
// chama a função memo na hora de exportar o componente
export default memo(Item);
```

A função `memo()` funciona quando **utilizada no export do componente** e aceita um segundo parâmetro opcional, em que você pode passar uma função comparadora que informa se o componente deve ou não ser re-renderizado. Essa função comparadora recebe dois argumentos: `prevProps` e `nextProps`. Quando não é passado um segundo parâmetro, é feito por padrão uma comparação "shallow".

Caso uma das props do componente memoizado seja uma **função**, a comparação shallow irá falhar pois a função é uma **referência**. Uma forma de contornar esse problema é utilizar o **`useCallback()`** na definição da função que será enviada por prop. Veja mais na [Seção sobre o `useCallback()`](#usecallback).

O `memo()` deve ser utilizado com **parcimônia**, pois essa chamada faz com que o React "faça um esforço" a mais para memorizar a função e fazer as comparações para decidir se um componente deve ou não ser re-renderizado. Esse "esforço", se aplicado a todo componente criado, pode não compensar com o ganho de performance proporcionado.  

**Dica:** aplicar a otimização de performance **depois** que um problema de performance for detectado (por meio de alguma ferramenta de profiling ou report de um usuário reclamando de alguma lentidão). Senão, você pode estar resolvendo um problema que não tem.

## `useMemo()`

A função `memo()` memoiza um componente em sua **definição**, e **não** uma instância. 

O hook `useMemo()` é utilizado quando queremos **memoizar o resultado** de uma função ou uma instância de um componente. Ele recebe dois parâmetros: o primeiro é a função a ser memoizada e o segundo é um **array de dependências**. A função memoizada somente será reavaliada quando alguma das dependências do array muda.

Se o array de dependências for vazio, a função é computada somente uma vez, durante o render inicial.

O exemplo abaixo mostra o uso tanto da função `memo()` para memoizar o componente `Searchbar`, quanto do hook `useMemo()` para memoizar uma instância de um subcomponente de `Searchbar`:

```js
import React, { memo, useMemo } from 'react';
function Searchbar() {
    // ícone será renderizado somente uma vez, quando o Searchbar é montado
    const searchIcon = useMemo(() => <CgSearch size={20} color='#4C4D5E' />, []);    
    return (
        <div>
            <input placeholder='Buscar' />
            {searchIcon}
        </div>
    );
}
export default memo(Searchbar);
```

Assim como em `memo()`, o hook `useMemo()` deve ser utilizado com **parcimônia**, analisando se seu uso será benéfico à performance da aplicação.

## `useCallback()`

Este hook é o utilizado para memoizar a **definição de uma função**. É útil quando **montar** a função é custoso. Por exemplo, toda vez que o componente é re-renderizado, uma função não memoizada é montada novamente, podendo impactar a perfomance. Ao memoizar a função, estamos criando um "cache" dela em memória.

A definição da função é passada como primeiro argumento, e o segundo argumento é um array de dependências, contendo as entradas a serem monitoradas e que podem causar a mudança na função memoizada (passando um array vazio, a função não irá mudar). Quando algumas dessas entradas são modificadas, a função deve ser construída novamente.

```js
import { useCallback } from 'react';

function App() {
    const calculaHipotenusa = useCallback((ladoA, ladoB) => {
        return Math.sqrt(Math.pow(ladoA, 2) + Math.pow(ladoB, 2))
    }, []);

    <Calculadora calculo={calculaHipotenusa} />
}
```

Além de memoizar a função, caso o componente `<Calculadora />` também seja memoizado (função `memo`), como agora a prop recebe uma função memoizada, não haverá re-render desnecessário, mesmo a prop sendo uma função.

Resumindo: 

- `memo`: memoiza um **function component**, útil para evitar re-render desnecessário de um componente; 

- `useMemo`: guarda referência ao **resultado** de uma função; útil quando a **execução** da função é custosa;

- `useCallback`: guarda referência à **definição** de uma função, útil quando **montar** esta função é custoso.

---

# Lazy import

Por padrão, todos os imports contidos em um componente são feitos quando o componente é montado. Isso é chamado de *import síncrono*. Pensando em termos de build, isso significa que haverá um arquivo único com o código que englobe tudo que a aplicação irá utilizar. 

No exemplo de um componente de rotas, que importa várias páginas que serão gerenciadas pelo roteador, isso significa que todos esses imports serão armazenados no código do componente de rotas e serão carregados quando o componente é montado, *mesmo se essa páginas ainda não estão sendo exibidas na tela*. Isso pode causar problemas de perfomance. 

Para resolver esse problema, podemos utilizar o chamado import dinâmico (*dynamic import*), em que dizemos ao React quando algo deve ser importado. Outro conceito envolvido nesse processo é o chamado *code-splitting*, em que durante o build o código é "quebrado" em arquivos menores, para possibilitar esse carregamento sob demanda, dinâmico. 

No React, esse import dinâmico é chamado de **import "preguiçoso", ou *lazy import***, em que os imports são feitos somente quando necessário. Voltando ao exemplo de um componente de rotas, o import de certa página deveria ser feito somente quando aquela página é requisitada. 

Para efetuar lazy import de **componentes**, é utilizada a função `lazy()`. Essa função irá **substituir o import** feito no começo de um componente. A função `lazy()` recebe como parâmetro um callback que irá retornar uma **Promise** do componente a ser importado (no caso, uma função `import()`). 

Exemplo: 

```js
// --> import síncrono
// import Cardapio from 'pages/Cardapio';

// --> substituindo por um lazy import
const Cardapio = lazy(() => import('pages/Cardapio'));
```

O import feito pela função `lazy()` acontece somente uma vez, quando é requisitado. Enquanto o componente estiver montado, o import não é feito novamente. 

Por retornar uma Promise, é necessário **utilizar o componente `<Suspense />`** para dizer ao React o que mostrar enquanto o import não terminou de ser efetuado. Esse componente é explicado no final dessa [Seção do Recoil](#seletores-para-eventos-assíncronos). 

Os componentes filhos de `<Suspense />` serão desmontados e montados cada vez que a operação assíncrona finaliza. Então tente colocar dentro de `<Suspense />` somente os componentes que precisam aguardar o lazy import. Você pode inclusive **criar múltiplos `<Suspense />`** para gerar diferentes fallbacks.

---

# Análise estática

É uma análise feita no código **antes de sua execução** (e também pode acontecer antes ou durante a compilação) e serve para verificar a *qualidade do código* em termos de estilo, formatação e erros de digitação. Por exemplo: verificar identação, verificar se uma variável foi utilizada com um nome errado, avisar quando está usando um componente sem importá-lo, verificar uma regra de estilo adotado pelo time, etc.

Ferramentas que auxiliam na análise estática: 

## Prettier

Auxilia em deixar o código mais "bonito" e seguindo uma formatação padrão, por meio de configurações em um arquivo `.prettierrc`. 

Necessário instalar. Comando para instalar o pacote somente em ambiente de dev: 

    npm install -D prettier

No exemplo abaixo, o Prettier irá formatar o código e colocar todas as aspas como sendo aspas simples e toda a identação como tendo um espaço de tamanho 2.

```json
{
    "singleQuote": true,
    "tabWidth": 2
}
```

O Prettier pode aplicar essas formatações cada vez que o arquivo for salvo. Para isso, é necessário adicionar os seguintes comandos no `settings.json` do VS Code (Ctrl+Shift+P -> Preferences: Open User Settings (JSON)):

```json
    "editor.formatOnSave": true,
    "prettier.requireConfig": true
```

É possível também adicionar um script no `package.json` para aplicar a formatação em todos os arquivos quando o comando é acionado.

Exemplo de script no `package.json` que formata todos os arquivos `js` e `jsx` dentro de subdiretórios da pasta `src`: 

    "format": "prettier --write \"src/**/*{.js,.jsx}\""

Existe também uma **extensão do VS Code** que aplica o Prettier (acho mais fácil).

## ESLint

Pode ser utilizado em conjunto com o Prettier. Ele auxilia a manter um padrão de código e identificar erros de escrita, com o lint avisando quando algo está errado (quando esqueceu de fazer um import, por exemplo), sublinhando no código onde há um erro a ser verificado. 

**Dica**: para que o VS Code mostre em tempo real os erros/warnings gerados pelo ESLint, é necessário instalar a extensão [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint). Sem a extensão, os erros/warnings são vistos somente ao rodar um comando correspondente na linha de comando.

### Instalação guiada

No curso de React/Typescript sobre React Router, o instrutor Luiz Fernando mostrou uma maneira de instalar o ESLint de modo guiado, em que na linha de comando vão sendo feitas perguntas para que você configure como e o que o ESLint deve instalar. Ao final, as dependências necessárias são instaladas e é gerado um arquivo `.eslintrc`.

    npm init @eslint/config

Caso você não queira usar a extensão ESLint para o VS Code, o comando para o ESLint **fazer a verificação do código** a partir da pasta `/src` é:

    npx eslint ./src

O ESLint irá mostrar na linha de comando os erros/warnings que encontrou, separados por arquivo em que foram encontrados. Você pode ir em cada arquivo e corrigir os erros manualmente, ou solicitar ao ESLint para tentar corrigi-los (quando é erro de falta de `;`, alguma identação errada, etc). Para **corrigir automaticamente**, o comando é

    npx eslint ./src --fix

Apos o fix automático, provavelmente o seguinte erro irá aparecer em vários arquivos

    'React' must be in scope when using JSX  react/react-in-jsx-scope

Esse erro é devido a uma regra do ESLint, que está obrigando que o import do react deva ser declarado no topo de todo código com a extensão `.jsx`. Essa regra é mais voltada para projetos antigos, sendo que nos atuais essa importação não é necessária em todos os arquivos. Para desativar essa regra, basta adicionar a seguinte linha nas "rules" do arquivo `.eslintrc`:

    “react/react-in-jsx-scope”: “off”


### Instalação manual

*Não gostei. Não funcionou como mostrado na aula do curso de testes. Não sei o motivo.*

No curso de testes do Neilton, ele mostrou a instalação manual, já incluindo alguns plugins e um arquivo de configurações pré-configurado.

Comando para instalar em ambiente de dev, incluindo configurações para atuar com o Prettier: 

    npm install -D eslint eslint-config-prettier

O próximo comando instala algumas análises de regras próprias do React:

    npm install -D eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks

Também necessário criar um arquivo de configurações, `.eslintrc`. Segue um exemplo de configuração padrão para trabalhar com a versão mais atual do EcmaScript, módulos javascript e JSX. Não entraram em detalhes sobre cada opção, então recomendo [ler a documentação](https://eslint.org/docs/latest/use/configure/configuration-files).

```json
{
    "extends": [
        "eslint:recommended",
        "plugin:import/errors",
        "plugin:react/recommended",
        "plugin:jsx-a11y/recommended",
        "plugin:react-hooks/recommended",
        "prettier"
    ],
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "react/prop-types": "off",
        "react/jsx-uses-react": "off",
        "react/react-in-jsx-scope": "off",
        "react/self-closing-comp": "warn",
        "no-unused-vars": "off",
        "import/no-unresolved": "off"
    },
    "plugins": [
        "react",
        "import",
        "jsx-a11y"
    ],
    "env": {
        "es6": true,
        "browser": true,
        "jest": true,
        "node": true
    },
    "settings": {
        "react": {
            "version": "detect"
        }
    }
}
```

Também pode configurar o VS Code para aplicar as correções ao salvar o arquivo (*não funcionou comigo, e por algum motivo desconhecido zuou o autocomplete do VS Code...*). No arquivo `settings.json`: 

```js
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
},
```

Exemplo de script para adicionar ao `package.json` para automatizar a análise de ESLint nos arquivos dentro de subdiretórios da pasta `src` (*esse funcionou*):

```json
"lint": "eslint \"src/**/*{.js,.jsx}\" --quiet",
```

Também possui uma extensão no VS Code.

---

# Testes com Jest e React Testing Library 

*Essa seção possui muitos códigos de exemplo, o quais foram copiados e adaptados dos projetos que eu desenvolvi durante os cursos. Eles podem não ser interessantes ou difíceis de entender para quem não conhece o projeto... Mantive eles aqui com a intenção de ao menos a pessoa saber como o código deve ser escrito.*

Duas bibliotecas famosas para testes de projetos React são o **Jest** e a **React Testing Library (RTL)**. O CRA já traz ambas, com algumas configurações e um script para rodar testes (ele irá rodar em watch mode):

    npm run test

[Artigo sobre a RTL](https://www.robinwieruch.de/react-testing-library/).

A RTL tem como objetivo **colocar o dev no lugar do usuário**, para que o dev crie testes baseados na experiência que o usuário deveria ter com a aplicação, ao invés de testar os detalhes de implementações. Ela trabalha em conjunto com o Jest, cada uma com sua própria resposabilidade. 

## Jest

O [Jest](https://jestjs.io) é o *test runner*, responsável por **rodar os testes** (quando chamado pelo `npm run test`) e fornece as funções de **`describe()`** (que é um *test suit*), **`it()`** e **`test()`** (que são os testes em si), além de assertivas usando **`expect()`**;

O Jest possui uma lib chamada "JS DOM", que **simula** um navegador web em memória, como se a aplicação web estivesse rodando de verdade para um usuário (só que você não vê nada visualmente), fornecendo recursos para interceptar eventos.

## RTL

Já a [RTL](https://testing-library.com/docs/react-testing-library/intro/) é uma **biblioteca para testes de componentes React**. Ela irá oferecer funções adicionais, que podem ser aplicadas aos testes, e assertivas, como renderizar um componente (função **`render()`**), fazer queries em elementos da tela (métodos da propriedade **`screen`**), passar props e mocks de funções, bem como disparar cliques e outras ações do usuário. Outra biblioteca famosa é a Enzyme, do Airbnb.

A RTL faz parte de um "universo" maior da ["Testing Library"](https://testing-library.com/docs/). A Testing Library também possui versões voltadas para diferentes bibliotecas e frameworks, como Vue, Angular, Svelte, etc.

## Trabalhando com testes

Para criar um teste de unidade para um componente, você pode criar um arquivo de teste com a seguinte nomenclatura: `NomeDoComponente.spec.js` ou `NomeDoComponente.test.js` (eu prefiro `.test.js`). É por meio dessa nomenclatura que o Jest irá procurar os arquivos de teste (isso se encontra na configuração do Jest, mas você também pode customizar, tanto isso quanto outras coisas).

A função para criar um teste é **`test()`** ou **`it()`**. Eles são sinônimos e fazem a mesma coisa. Pensando no inglês, `it()` deixa a leitura do código mais natural: `it('should do something', () => {...})`). 

A função recebe como **primeiro argumento uma string** com um nome/descrição do teste, e como **segundo argumento uma função callback** (uma arrow function) com o teste a ser de fato aplicado.

- Algumas pessoas recomendam não dizer "teste *unitário*" e sim teste *de unidade*. No entanto, em diferentes vídeos e cursos ambos são mencionados e utilizados de maneira intercambial.

- **Padrão AAA (Arrange, Act, Assert)**: é um padrão de organização de testes, em que primeiro "arrumamos" o cenário de teste (fazemos queries, definimos o que será utilizado no teste), depois agimos (executamos alguma ação do usuário, definimos valores, etc) para então fazermos assertivas/afirmações (que é aquilo que esperamos que aconteça no teste).

### Exemplo de teste simples usando `expect()`

```js
test("Meu primeiro teste", () => {
    const numero = 10;
    expect(numero).toBe(10);
})
```

O **`expect()`** é a função que chamamos para fazer uma verificação em um elemento (o elemento é passado como argumento). Ela possui diferentes métodos, chamados "matchers", utilizados para fazer as assertivas, cada um avaliando alguma característica diferente (se elementos são iguais, se estão no documento, se uma função foi chamada, etc).

Uma boa prática é que **cada teste falhe por um único motivo**. Por isso, é bom evitar criar vários `expect` dentro de um único teste - eles devem estar relacionados com aquilo que se está tentando testar no momento. Se necessário, crie outro teste para outro `expect`.

### Exemplos com `render()` e `screen`

A função **`render()`** é utilizada para renderizar um componente dentro do teste. É necessário fazer a importação do componente. É possível também passar props mockadas ao componente (ver mais adiante).

- **Atenção:** nas versões recentes da RTL, o componente renderizado é **desmontado quando o teste finaliza**. Desse modo, a cada teste (a cada `it()` ou `test()`), você deve chamar novamente o `render()`. O Jest oferece funções como  [**`beforeEach()`**](https://jestjs.io/docs/api#beforeeachfn-timeout) que podem auxiliar em replicar comandos antes de cada teste.

O objeto **`screen`** provê diversos métodos para fazer queries de elementos e valores que são renderizados na tela:

- `getByText` e `getByRole` são as funções **comumente** utilizadas para selecionar elementos;

    - se você chamar `getByRole('')` (com um argumento vazio), o teste irá retornar um erro com os roles disponíveis; essa é uma forma de verificar quais roles estão disponíveis naquilo que foi renderizado, para auxiliar na seleção do elemento desejado;

    - você pode atribuir uma role a um elemento (uma tag HTML, por exemplo), adicionando a propriedade `role` a ele, com o nome da role desejada (*isso provavelmente atrapalha a semântica do código*);
    
        - `<div role='button'>Estou agindo como um botão</div>`

    - fazer a busca por role é uma boa prática para também garantir acessibilidade da aplicação;
    
    - [Lista da W3](https://www.w3.org/TR/wai-aria-1.1/#role_definitions) com as roles disponíveis e seu uso.

- é possível fazer consulta utilizando um data attribute próprio para teste (`data-testid`). A consulta é feita com a função `screen.getByTestId()`; isso, no entanto, deve ser **usado como último recurso**, quando você não consegue selecionar o elemento de outra maneira;

- use os `getBy...` para selecionar **elementos**, `queryBy...` para fazer assertivas com elementos que **não** devem estar presentes no documento, e `findBy...` para **eventos assíncronos** (quando um elemento estará presente posteriormente);

- [Documentação das queries](https://testing-library.com/docs/queries/about), seu uso e retornos dos getBy, queryBy e FindBy;

- `screen` também fornece um método `debug()` que printa na tela o conteúdo HTML renderizado pelos testes. É útil quando você está em dúvida com o conteúdo HTML que está sendo renderizado;

Tanto `screen` quanto `render()` precisam ser importadas. 

```js
import {render, screen} from '@testing-library/react';
import Menu from '.'; //importação de componente <Menu />

test("Deve renderizar um link para a página inicial", () => {
    render(<Menu />);

    // getByText retorna um node que contenha o texto 
    // (exato e completo), ou uma regex. Joga um ERRO 
    // se o texto não é encontrado, o que ENCERRA a 
    // execução do código.
    // É o recomendado de utilizar, pois o erro auxilia
    // o dev a verificar por que o teste falhou
    const linkPaginaInicial = screen.getByText('Inicial');
    expect(linkPaginaInicial).toBeInTheDocument();
})

test('Não deve renderizar link para extrato', () => {
    render(<Menu />);

    // Já o queryByText retorna NULL se o texto não é 
    // encontrado. O teste CONTINUA a execução.
    // É recomendado utilizar quando se quer fazer uma 
    // asserção a um conteúdo que NÃO deve estar presente
    const linkExtrato = screen.queryByText('Extrato')
    expect(linkExtrato).not.toBeInTheDocument();
})

test('Deve renderizar uma lista de 4 links', () => {
    render(<Menu />);

    // opção do ...AllBy...() retorna um array de elementos
    const listaDeLinks = screen.getAllByRole('link');
    expect(listaDeLinks).toHaveLength(4);
})

test('Deve renderizar uma lista de links com a classe link', () => {
    render(<Menu />);
    const listaDeLinks = screen.getAllByRole('link');
    listaDeLinks.forEach(link => expect(link).toHaveClass('link'));
    
    // teste visual (testa se a estrutura da página não mudou)
    // se não existir, cria um snapshot (pasta __snapshots__) 
    // e compara a estrutura atual com este snapshot.
    // Se quiser atualizar o snapshot, pode atualizá-lo via 
    // console que está rodando o script de testes
    // (opções u e i)
    expect(listaDeLinks).toMatchSnapshot()
})
```

### Exemplo com `describe()` e `userEvent`

Quando estou aplicando diversos testes parecidos (por exemplo, testando diversos aspectos de um mesmo componente), posso agregá-los em uma suíte de testes com o método **`describe()`**. O primeiro parâmetro é uma descrição geral dos testes e o segundo é uma callback com todos os testes (semelhante aos parâmetros de `test()`). A descrição do `describe()` inicia uma frase (por exemplo: "Deve renderizar um botão"), que é completada com a descrição do `test()` (por exemplo "com o texto 'Avançar'").

É possível também fazer testes simulando ações do usuário (digitar, clicar, etc) por meio do **`userEvent`**.

```js
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Formulario from ".";

describe('Deve renderizar um campo de input', () => {
    test('no documento', () => {
        render(<Formulario />);
        const input = screen.getByPlaceholderText('Digite um valor');
        expect(input).toBeInTheDocument();
    })

    test('do tipo "number"', () => {
        render(<Formulario />);
        const input = screen.getByPlaceholderText('Digite um valor');
        expect(input).toHaveAttribute('type', 'number');
    })

    test('o usuário deve conseguir digitar um valor', () => {
        render(<Formulario />);
        const input = screen.getByPlaceholderText('Digite um valor');

        // simula que a pessoa digitou 50 no campo
        userEvent.type(input, '50');

        expect(input).toHaveValue(50);
    })
})
```

- Em testes que envolvem **interações do usuário**, como no caso do evento de clique, pode haver uma demora na atualização da página após a interação. Dessa forma, **o melhor é utilizar testes assíncronos** na hora de executar alguma **query** (ou seja, utilizar o `findBy...` ao invés de um `getBy...`), garantindo que a página foi atualizada corretamente antes de prosseguir com o teste.

### Exemplo de interação com o `fireEvent`

O `fireEvent` é outra opção da Testing Library para simular eventos do usuário.

```js
import { fireEvent, render, screen } from '@testing-library/react';
import Formulario from "./Formulario";

test('o campo de input deve estar em branco e com foco a cada participante adicionado', () => {
    render(<Formulario />);

    const input = screen.getByPlaceholderText('Insira os nomes dos participantes');
    const botao = screen.getByRole('button');

    // -- simulando as interações com FireEvent
    
    // inserção de valor em um input
    fireEvent.change(input, {
        target: {
            value: 'Matheus'
        }
    });

    // clique do botão
    fireEvent.click(botao);

    expect(input).toHaveFocus();
    expect(input).toHaveValue('');
})
```

### `fireEvent` vs `user-event`

Segundo a Chat:

    Embora ambas as bibliotecas possam ser usadas para testar eventos de usuário em aplicações React, UserEvent é geralmente preferível por sua simulação mais realista de eventos de usuário. No entanto, é importante observar que UserEvent requer uma instância do navegador para ser executado, o que pode ser um problema em ambientes que não possuem um navegador disponível. Já o FireEvent é independente de plataforma, mas pode ser menos realista na simulação de eventos de usuário.

Na [documentação da Testing Library](https://testing-library.com/docs/user-event/intro/) eles recomendam o uso de user-event, pois ele se aproxima mais da simulação de um evento de usuário (a fireEvent simula eventos do DOM), apesar de que nem todas as interações foram implementadas na user-event - nestes casos, recomenda-se usar a fireEvent.

### Exemplo passando props e usando o `rerender()`

É possível também **passar props para o componente** na hora do teste. Neste caso, você pode criar uma variável com valores fakes para sua props dentro do teste e enviar essa variável como props no método `render()`.

Também é possível efetuar uma **re-renderização do componente**, inclusive passando outros valores de props. O método `render()` retorna uma **propriedade `rerender`** que pode ser utilizada para essa finalidade.

```js
import { render, screen } from "@testing-library/react";
import Transacoes from ".";
import estilos from "../Extrato.module.css";

test('Deve renderizar o componente com os valores de suas props', () => {
    // props fake
    const deposito = {
        transacao: 'Depósito',
        valor: 100
    }
    const transferencia = {
        transacao: 'Transferência',
        valor: 85
    }

    // primeiro render, passando deposito na prop
    const { rerender } = render(<Transacoes transacao={deposito} estilos={estilos} />)
    let tipoTransacao = screen.getByTestId('tipoTransacao');
    let valorTransacao = screen.getByTestId('valorTransacao');
    expect(tipoTransacao).toHaveTextContent('Depósito');
    expect(valorTransacao).toHaveTextContent('R$ 100');

    // segundo render, passando tranferencia na prop
    rerender(<Transacoes transacao={transferencia} estilos={estilos} />)
    tipoTransacao = screen.getByTestId('tipoTransacao');
    valorTransacao = screen.getByTestId('valorTransacao');
    expect(tipoTransacao).toHaveTextContent('Transferência');
    expect(valorTransacao).toHaveTextContent('- R$ 85');
})
```

### Exemplo utilizando `jest.fn()`

Quando a **prop** a ser utilizada é uma **função** (um comportamento), utilizamos um **mock da função** por meio do **`jest.fn()`**. Lembrando que o interesse é testar comportamento, e não a implementação da função.

```js
test('Deve chamar um evento de onSubmit ao clicar em realizar transação', () => {
    // suponha que o componente <Formulario> possui um <button> que recebe o conteúdo da prop realizarTransacao em seu evento de clique
    const funcaoMockada = jest.fn();
    render(<Formulario realizarTransacao={funcaoMockada} />);
    const botao = screen.getByRole('button');
    userEvent.click(botao);

    // toBeCalled e toHaveBeenCalled são a mesma coisa 
    // toBeCalled é um alias
    expect(funcaoMockada).toHaveBeenCalledTimes(1);
})
```

### Exemplo utilizando `jest.useFakeTimers()`

O Jest auxilia a rodar os timers existentes em uma aplicação (setTimeout, por exemplo). Ao invés de criar um timer dentro do teste, podemos pedir ao Jest para executar os timers necessários durante uma ação a ser testada. Com isso, não precisamos nem saber qual é o tempo de cada timer.

Pedimos ao Jest para mockar os timers com a função `jest.useFakeTimers()`. Depois, para rodá-los, utilizamos a função `jest.runAllTimers()`.

```js
test('a mensagem de erro deve sumir após um tempo determinado', () => {
    jest.useFakeTimers();

    // ...

    act(() => {
        jest.runAllTimers();
    });
    
    // ...
});
```

### Testando hooks

Os **hooks** de React funcionam somente **dentro do corpo de function components**. Para evitar ter que definir um componente no teste, é possível utilizar a **função `renderHook()`** para poder utilizar hooks dentro de um teste.

A função **aceita um callback**, em que você pode definir os hooks (ou chamar uma função que já cria algum hook customizado) e o retorno esperado. A função **retorna um objeto `result`, cuja propriedade `current`** irá possui os retornos do callback. **Não** pode alterar o nome do objeto (**deve** ser `result`).

```js
import { renderHook } from "@testing-library/react";
import { useState, useEffect } from "react";

test('Testando um hook', () => {
    const { result } = renderHook(() => {
        const [nome, setNome] = useState('');

        useEffect(() => {
            setNome('Matheus')
        }, []);

        return nome;
    })

    expect(result.current).toBe('Matheus');
})
```

#### Hooks assíncronos

No caso de hooks que contenham efeitos assíncronos, o resultado após o efeito assíncrono pode ser verificado utilizando o método `waitFor()`.

Ver mais sobre nos links:

- https://testing-library.com/docs/dom-testing-library/api-async/#waitfor
- https://stackoverflow.com/questions/72308116/what-is-the-alternative-the-function-waitfornextupdate-testing-library-react-h

Exemplo:

```js
import { renderHook, waitFor } from "@testing-library/react"
import useSaldo from "./useSaldo"
import { buscaSaldo } from "../services/saldo"

jest.mock('../services/saldo');

describe('hooks/useSaldo', () => {
    // o callback é definido como assíncrono
    test('retorna o saldo e uma função para atualizá-lo', async () => {
        // sobrescrevendo a função buscaSaldo para
        // somente retornar um valor
        buscaSaldo.mockImplementation(() => 2000);
        
        // useSaldo retorna um array [saldo, setSaldo] com 
        // valor inicial = 0. Dentro dele há um useEffect
        // com uma chamada assíncrona à api para atualizar
        // o saldo quando o componente é montado
        const { result } = renderHook(() => useSaldo());

        // componente acabou de ser montado, retorna 
        // saldo zero
        expect(result.current[0]).toBe(0);

        // espera pelo término do retorno assíncrono
        // do useEffect para testar a assertiva
        await waitFor(() => expect(result.current[0]).toBe(2000));
    })
})
```

#### Uso do `act()`

Quando em seu teste ocorrer alguma operação **assíncrona** que cause alguma **atualização de estado**, é necessário envolver o código que causa essa alteração dentro da função `act()`. Isso garante que o teste irá aguardar que todas as atualizações e re-renderizações tenham sido feitas antes de prosseguir com o teste.

Pode ser que o teste funcione mesmo sem o uso de act devido a algumas atualizações ocorrerem em frações ínfimas de segundo, mas o ideal é garantir que o teste esteja simulando uma situação real.

```js
act(() => {
    // função assíncrona ou operação que causa atualização assíncrona 
});
```

### "Dublando" módulos e arquivos

É possível pedir ao Jest para mockar módulos ou arquivos que são importados para seus testes. Ou seja, quando no teste for feito o import de algum módulo (o import de um hook customizado, por exemplo), o jest irá substituir esse import por uma versão mockada. É utilizado o jargão de se estar **"dublando"** o módulo/arquivo, no sentido de ser um **dublê**.

Nessa versão dublada, é possível também informar o que o jest deve **retornar** quando alguma função do módulo for invocada no teste.

A dublagem é feita com o método `jest.mock()`, que recebe dois parâmetros: 

- o caminho para o módulo que será dublado (o caminho do import);

- uma função que retorna um objeto com os exports que serão dublados;

No exemplo a seguir, é definido um mock para o hook customizado `useListaParticipantes`. Posteriormente, mockamos o que ele deve retornar:

```js
import { render, screen } from "@testing-library/react";
import { RecoilRoot } from "recoil";
import ListaParticipantes from ".";
import { useListaParticipantes } from "../../state/hooks/useListaParticipantes";

jest.mock('../../state/hooks/useListaParticipantes', () => {
    return {
        useListaParticipantes: jest.fn()
    }
})

describe('Uma lista de participantes vazia', () => {
    beforeEach(() => {
        // mockando o retorno do hook, que já está dublado
        (useListaParticipantes as jest.Mock).mockReturnValue([])
    })
    test('deve ser renderizada sem elementos', () => {        
        render(
            <RecoilRoot>
                <ListaParticipantes />
            </RecoilRoot>
        );

        const itens = screen.queryAllByRole('listitem');
        expect(itens).toHaveLength(0);
    })
})
```

### Testando rotas

Algumas considerações e exemplos para testes de rotas com o React Router.

#### Usando o parâmetro `wrapper` da função `render()`

Caso o componente que você quer testar fizer o uso de algum hook que envolva um **contexto de rotas/navegação**, como o `useLocation` por exemplo, você pode adicionar o **parâmetro `wrapper` à função `render()`**, para poder informar ao Jest que o componente é filho de componente de roteamento (por exemplo, o `<BrowserRouter />`), responsável por dar esse contexto de rotas.

```js
import { screen, render } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import App from './paginas/Principal/App';

describe('Rotas', () => {
    test('deve renderizar a rota principal', () => {
        render(<App />, { wrapper: BrowserRouter });
        const usuario = screen.getByText('Olá, Joana :)!');
        expect(usuario).toBeInTheDocument();
    })
})
```

- O `wrapper` também é útil quando o componente depende de um contexto, por exemplo.

- Ao invés de usar o `wrapper`, **também é possível** colocar no próprio `render()` os **componentes aninhados** (eu acho mais simples dessa forma). Seguindo o exemplo anterior:

```js
render(
    <BrowserRouter>
        <App />
    </BrowserRouter>
);
```

#### `<MemoryRouter />`

O componente `<MemoryRouter />` permite controlar a pilha de histórico de navegação sem precisar depender de uma fonte externa, como o histórico do browser, por exemplo. Caso você queira aplicar testes em diferentes rotas, o uso do `<MemoryRouter />` é recomendável.

O componente recebe via props `initialEntries` um array com as rotas que se deseja acessar (o padrão é `['/']`). Esse array **simula o histórico de navegação**, em que a última entrada do array corresponde à URL atual a ser renderizada.

Quando o array possui somente um elemento, é como **se estivéssemos digitando a URL** a ser acessada para que a página associada à rota seja renderizada.

```js
test('deve renderizar a rota Cartões', () => {
    const rota = '/cartoes';
    render(
        <MemoryRouter initialEntries={[rota]}>
            <Routes>
                <Route path='/' element={<App />} >
                    <Route path='cartoes' element={<Cartoes />} />
                </Route>
            </Routes>
        </MemoryRouter>
    )

    const titulo = screen.getByText('Meus cartões');
    expect(titulo).toHaveTextContent('Meus cartões');
})
```

No exemplo acima, se fosse um array com mais de um elemento, `initialEntries={['/', '/cartoes', '/investimentos']}`, essa prop estaria simulando um histórico que passou pelas rotas home, depois cartoes e por fim investimentos.

#### Dublando comportamento do `useNavigate()`

Como visto na seção ["Dublando módulos e arquivos"](#dublando-módulos-e-arquivos), é possível dublar hooks, evitando ter de implementar toda a lógica para fazer um teste. 

Segue um exemplo de teste dublando o `useNavigate()` para testar se uma rota é chamada após o clique de um botão:

```js
// nomeando uma função para ter acesso a ela nas assertivas dos testes
// ATENÇÃO: é necessário usar o prefixo "mock" na variável para não dar erro de escopo
const mockNavegarPara = jest.fn();

jest.mock('react-router-dom', () => {
    return {        
        useNavigate: () => mockNavegarPara
    }
})

describe('Quando há participantes suficientes', () => {
    beforeEach(() => {
        (useListaParticipantes as jest.Mock).mockReturnValue(['Eeny', 'Meeny', 'Miny']);
    });
    
    test('vai para a rota sorteios após clicar no botão', () => {
        render(
            <RecoilRoot>
                <Rodape />
            </RecoilRoot>
        );

        fireEvent.click(screen.getByRole('button'));

        // o componente chamará o useNavigate para a rota /sorteio
        expect(mockNavegarPara).toHaveBeenCalledTimes(1);
        expect(mockNavegarPara).toHaveBeenCalledWith('/sorteio');
    })
})
```

### Testes "visuais" com snapshot

O método `toMatchSnapshot()` é útil para testar se a estrutura de uma **página renderizada** não mudou. A primeira vez que ele é chamado, um snapshot da página é criado e salvo na pasta ``__snapshots__``. É como se fosse uma "fotografia" do que é renderizado no navegador (mas não é um arquivo de imagem, e sim a estrutura HTML). Nas próximas vezes que o teste rodar, ele irá comparar o snapshot do teste com o que está armazenado, e irá falhar caso sejam diferentes.

Esse teste é útil para verificar se nenhuma atualização feita em algum componente está causando uma mudança indesejada na página renderizada. Quando falha, acaba também sendo útil no code review para que quem esteja revisando saiba, pelo diff do snapshot, o que foi de fato mudado após algum novo commit.

É possível atualizar o snapshot armazenado, usando o console que está rodando os testes (opções u e i).

## Gerando relatório de cobertura de testes

Você pode utilizar o comando a seguir:

    npm run test -- --coverage --watchAll=false

Este comando irá rodar todos os seus testes e gerar um relatório mostrando quanto do seu código foi coberto pelos testes criados. Na **linha de comando, será gerada uma tabela**, chamada de relatório de cobertura de testes. Ela possuirá as seguintes colunas:

- File: arquivo testado;
- Stmts: instruções do código que foram testadas;
- Branch: blocos de código if-else (dentre outros que geram ramificações) testados;
- Funcs: funções que foram executadas durante os testes;
- Lines: linhas que passaram pelos testes;
- Uncovered Lines: linhas não atingidas pelos testes;

É gerado também um **relatório em HTML** que traz essas informações de maneira mais dinâmica e interativa, inclusive mostrando no código as linhas que não foram testadas (ficam com um destaque vermelho). O relatório pode ser acessado na pasta `coverage\lcov-report`.

### Estabelecendo máximos e mínimos aceitos de cobertura

É possível adicionar configurações no `package.json` para informar o mínimo e máximo aceitável de cobertura que se deseja que seja testado.

```json
"jest": {
    "coverageThreshold": {
        "global": {
            "branchs": 80,
            "lines": 80,
            "functions": 80,
            "statements": -10
        }
    }
}
```

Valores positivos informam o mínimo aceitável de cobertura. Valores negativos informam o máximo de não-cobertura aceitável (*isso está um pouco confuso e eu não tenho certeza de que a explicação foi correta...*). 

Esses limites podem ser utilizados, por exemplo, na automatização de um Pull Request (PR) no GitHub por meio do GitHub Actions. Você pode adicionar a cobertura de testes em uma action e, caso algum mínimo/máximo esteja fora do estabelecido, a action irá falhar e não irá permitir que a PR seja enviada.

## Testes de Integração

São testes que irão analisar o comportamento de unidades independentes (dois componentes, por exemplo) e verificar se a interação entre eles ocorre da maneira esperada. Em testes de unidade, componentes podem se comportar da maneira esperada, mas pode acontecer algum erro quando se comunicam com outro componente (por exemplo, quando compartilham de uma mesma variável de estado ou quando dependem de algum retorno de outro componente).

Testes de integração podem ser vistos ao aplicar testes em uma página, que pode conter diversos componentes que se comunicam entre si. Por exemplo, em uma página de e-commerce, um componente de produto, ao ser clicado, atualiza o componente de carrinho. Podemos verificar um cenário assim por meio de um teste de integração.

## Configurando a Vercel para rodar testes

É possível configurar um projeto hospedado na Vercel para que os testes sejam rodados antes do build e aborte o deploy de uma nova versão caso algum teste falhe. Para isso, é necessário abrir o projeto na Vercel, ir até a aba "Settings" e, na seção "Build & Development Settings" clicar para fazer o override do "build command".

O arquivo `package.json` do projeto possui os scripts para rodar os comandos de build, start, test, etc. Então é só copiar os scripts que você deseja e colar no campo na Vercel. Para que os comandos sejam executados em sequência, você coloca `&&` entre cada script. Por exemplo:

    react-scripts test && react-scripts build

No caso de testes de snapshot, quando eles falham e é devido a alguma atualização que sabemos que deveria acontecer, precisamos atualizar manualmente o snapshot e enviá-lo ao repositório para que o teste passe na Vercel.

---

# Sobre a MUI (Material UI)

É uma biblioteca React famosa, que possui diversos **componentes de UI prontos (e customizáveis)** para serem utilizados. 

Necessário instalar. A instalação padrão recomendada pelo MUI é o comando a seguir: 

    npm install @mui/material @emotion/react @emotion/styled

- A Emotion é outra biblioteca que permite escrever CSS com o JS. É instalada junto com a MUI por ser o mecanismo de estilização padrão da MUI.

- [Link da MUI](https://mui.com).

- [Link da Emotion](https://emotion.sh/docs/introduction).

É necessário também instalar ou adicionar ao head do `index.html` a fonte e família de ícones utilizados pela MUI:

```html
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
/>

<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/icon?family=Material+Icons"
/>
```

---

# Estilização com SASS e SCSS 

SASS significa **S**yntactically **A**wesome **S**tyle **S**heets.

[Neste link](https://sass-lang.com/guide) (em inglês) é possível ler um guia inicial do projeto do Sass e navegar por sua documentação.

## CSS com superpoderes

O Sass é uma extensão para o CSS, escrita em Ruby e criada em 2006. Criador: Hampton Catlin. Principal mantenedora atual: Natalie Weizenbaum.

A extensão pode ser entendida como uma camada acima do CSS. Ela ajuda a "dar superpoderes" ao CSS e possui uma linguagem própria, que adiciona **novos recursos ao CSS**. O código escrito em Sass é pré-processado e então traduzido para um arquivo CSS, que pode ser então utilizado no HTML (**não deve** ser usado o arquivo SASS/SCSS no HTML - o navegador não entende esses arquivos).

Um pré-processador como o Sass, ajuda a gerenciar e dar manutenção em folhas de estilo de CSS, quando ficam muito longas e complexas.

Por meio da extensão Sass é possível **criar variáveis, classes aninhadas, mixins (funções), partial de arquivos (quebrar o estilo em diversos arquivos) e realizar importação de arquivos, bem como operações matemáticas**.

## Instalação

É necessário instalar o Sass para ser utilizado. No VS Code, isso pode ser feito por meio da extensão ["Live Sass Compiler"](https://marketplace.visualstudio.com/items?itemName=glenn2223.live-sass), que possibita compilar arquivos SASS/SCSS em tempo real. Para que isso aconteça, após ter a extensão instalada, é necessário ativá-la em "watch mode": aperte Ctrl+Shift+P procure a opção "Live Sass: Watch Sass". Também é possível ativar esse modo na barra azul, que fica na parte inferior do VS Code.

Outras opções de instalação podem ser vistas na [página oficial do projeto](https://sass-lang.com/install) (em inglês).

- Para projetos React criados por meio do CRA, é possível instalar o SASS em modo de desenvolvedor por meio do seguinte comando:

        nmp i -D sass

## SCSS e SASS

Arquivos Sass possuem a extensão `.sass` e suportam todos os recursos do SCSS (**S**assy **C**ascading **S**tyle **S**heets, cuja extensão é `.scss`). O Sass irá gerar um CSS tanto de arquivos com a extensão `.sass` quanto `.scss`. 

O **SASS é um pré-processador**, enquanto o **SCSS é uma sintaxe**, um superset do CSS. O SASS suporta a sintaxe SCSS, mas também possui uma **sintaxe própria, que não utiliza chaves nem ponto e vírgula**. A sintaxe é baseada em espaços e quebras de linha.

Exemplos:

**SCSS**

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

**SASS**

```scss
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

No curso, foi mais utilizada a extensão `.scss` mesmo. Ao **utilizar a extensão `.sass`**, o VS Code parou de fazer o syntax-highlight e o autocomplete; neste caso, é necessário **instalar a extensão do VS Code [Syler.sass-indented](https://marketplace.visualstudio.com/items?itemName=Syler.sass-indented)**.

## Aninhamento

Com o Sass é possível criar **classes aninhadas**, isto é, adicionar **uma classe dentro da outra**, e outra subclasse dentro dessa classe e assim por diante. Isso auxilia na hora de escrever o código, facilitando em ver a hierarquia das classes e subclasses:

```scss
.container {
    height: 100vh;
    display: flex;
    // subclasse aninhada de .container
    .esquerdo {
        width: 50%;
        background-color: #e29dab;
        // subclasse aninhada de .esquerdo
        .texto {
          font-size: 32px;
        }
    }
    // outra subclasse aninhada de .container
    .direito {
        width: 50%;
        background-color: #99ef99;
    }
}
```

## Seletor `&` (*Parent Selector*)

Serve para **aninhar pseudo-classes** ao estilizar um elemento. Por exemplo, para dar um estilo no hover de um link:

```scss
a {
  text-decoration: none;
  text-transform: uppercase;
  font-weight: 700;
  &:hover {
      border-bottom: 2px solid #e3e3e3;
  }
}
```

O seletor `&` **também serve** para aninhar classes com nomes semelhantes (no estilo classe e subclasses, como na **metodologia BEM**, por exemplo): 

```scss
// CSS original
.component {
  width: 100%;
  padding: 20px 0;
}

.component__logo {
  max-width: 100px;
}

// versão SCSS com seletor &
.component {
  width: 100%;
  padding: 20px 0;

  &__logo {
    max-width: 100px;
  }
}
```

## Variáveis

São criadas utilizando o prefixo `$`. Você pode usar hífen ou underline no nome da variável (camelCase também deve ser aceito, não encontrei uma convenção). Para utilizá-las, basta chamar o nome da variável (não precisa digitar `var()`, como é feito no CSS):

```scss
$cor-primaria: #e29dab;
$cor-secundaria: #99ef99;

body {
  background-color: $cor-primaria;
  color: $cor-secundaria;
}
```

As variáveis também podem possuir **escopo de bloco**. Caso uma variável com **mesmo nome** seja definida dentro de um bloco (por exemplo, dentro de um elemento aninhado em outro), ela irá assumir o valor do **bloco interno**. Além disso, uma variável definida somente dentro de um bloco não será acessível fora dele.

- Uma convenção é criar um arquivo de variáveis `_variaveis` ou `_variables` e definir neste arquivo todas as variáveis utilizadas, facilitando o acesso e a busca por uma variável. O underline `_` possibilita, em alguns pré-processadores, que este arquivo seja ignorado durante o build da aplicação. 

## Mixins

Mixins são funções, que podem ser aplicadas ao código para replicar um conjunto de estilos, ajudando na manutenção e reúso. Elas devem ser definidas antes de serem invocadas.

Para criar uma função, usa-se a notação `@mixin`. Para invocá-la, utiliza-se `@include` (se a função receber parâmetros, você coloca um parênteses na hora de utilizar a função; se não houver parâmetros, não é necessário colocar parênteses):

```scss
// função sem parâmetros
@mixin reset {
    margin: 0;
    padding: 0;
}

// função com parâmetros
@mixin background-e-texto($cor-background, $cor-texto) {
    width: 50%;
    background-color: $cor-background;
    .texto {
        font-size: 128px;
        margin-top: 40%;
        text-align: center;
        color: $cor-texto;
    }
}

body {
  // uso de parênteses é opcional se não houver parâmetros
  @include reset;
}

.container {
    height: 100vh;
    display: flex;
    .esquerdo {
        @include background-e-texto($cor-primaria, $cor-secundaria);
    }
    .direito {
        @include background-e-texto($cor-secundaria, $cor-primaria);
    }
}
```

## Comentários

Comentários de **uma linha** (`//`) **não aparecem** no CSS gerado. Já comentários de **múltiplas linhas** (`/* */`) **aparecem**. 

Arquivos `.sass` possuem indentação também no comentário de uma linha, tornando-a multilinhas:

```sass
// Este comentário não será incluído no CSS
   Este comentário também não será incluído, devido à indentação.
```

## Partials

Podemos quebrar os estilos em diversos arquivos, cada um com uma responsabilidade específica (um para estilizar fontes, outro para definir variáveis, outro para uma seção específica do HTML, etc). Isso facilita em identificar o que cada arquivo faz, auxiliando na manutenção. 

Cada arquivo neste caso é denominado *"partial"* e a convenção de nomenclatura é iniciar o arquivo com underline (`_nomeDaPartial.scss`). Outra convenção é criar uma pasta "abstract" para salvar as partials responsáveis por estilos gerais da página, e uma pasta "components" para as partials que irão estilizar seções específicas da página.

Partials podem ser **importadas** por outros arquivos e outras partials, utilizando o `@import` e o caminho para o arquivo (sem incluir o underline nem a extensão `.scss` ou `.sass`). 

```scss
@import './abstract/base';
```

- para arquivos `.sass`, o VS Code inclui o nome com o underline e a extensão `.sass`, mas também funcionou ao removê-los...

Futuramente, a regra `@import` cairá em **desuso** e será substituída pela `@use`.

O uso de underline faz com que o SASS **não gere** um arquivo CSS. O conteúdo desse código será colocado no arquivo CSS que o importa. Isso contribui com a questão de performance.

## Operações matemáticas

O SASS aceita operações matemáticas para trabalhar com valores numéricos. Por exemplo:

```scss
$tamanho-heading: 3rem;

h1 {
  font-size: $tamanho-heading;
}

h2 {
  font-size: $tamanho-heading/2;
}
```

Operações assim, no entanto, estão programadas para serem **removidas em futuras atualizações**, sendo **recomendado** o uso de `calc()` ou `math.div()`, por exemplo.

## Condicionais

O SASS/SCSS também aceita condicionais if/else para controlar blocos que serão ou não aplicados. Exemplo:

```scss
// os ... após o $cores indica que pode haver mais de uma variável sendo recebida como argumento
@mixin bg-cores($lado, $cores...)
    @if $lado == left
        background: linear-gradient(to left, $cores)
    @else 
        background: linear-gradient($cores)
```

---

# Estilização com styled-components

O [styled-components](https://styled-components.com) é uma biblioteca para estilizar componentes React, com uma abordagem CSS-in-JS, em que você escreve o CSS dentro de um código JS/TS. Ela pode ser útil para encapsular estilos e comportamento a um componente de UI reutilizável: quando algo no estilo precisa ser mudado, é fácil efetuar a mudança localmente no componente, que é então aplicada em todos os lugares em que o componente é reutilizado.

A instalação da biblioteca em sua versão mais atual pode ser feita por meio do comando:

    npm install styled-components

No caso de projeto em **TypeScript**, também é necessário instalar o pacote para a tipagem de styled-components:

    npm install --save-dev @types/styled-components

## Exemplo de uso

Você pode utilizar o styled-components no mesmo arquivo em que o componente é definido, ou então criar um arquivo (a convenção é geralmente nomeá-lo de `styled.js` ou `styled.ts`) e nele fazer o export de seus componentes estilizados. 

Para criar um componente estilizado, você importa a função `styled` da biblioteca. A partir dela, é possível criar um componente (elementos do HTML, por exemplo), seguida de uma template string em que você inclui o CSS.

É possível incluir no CSS conceitos parecidos do SASS, como o parent selector (`&`) e aninhamento. Também é possível passar variáveis e expressões JS com a sintaxe `${}`. Além disso, todas as outras funcionalidade CSS estão acessíveis, como uso de seletores para adicionar estilos a um elemento/classe/id, e media queries.

Exemplo de uso em um arquivo único:

```js
import styled from "styled-components";

// definindo um componente estilizado
const StyledButton = styled.button`   
    font-size: 20px;
    cursor: pointer;
    &:hover {
        background-color: #B87900;
    }
`;

export const AbButton = () => {
    return (
        <StyledButton>Clique aqui</StyledButton>
    )
}
```

## Função `createGlobalStyle`

Quando você quer aplicar estilos globais ao projeto ou até mesmo um css reset, você pode utilizar a função `createGlobalStyle` e com ela adicionar os estilos a serem aplicados no projeto inteiro.

Utilizamos o `createGlobalStyle` para exportar um componente com os estilos globais, e então importá-lo para o componente mais ao topo da aplicação (o `App.js`, por exemplo).

```js
// arquivo GlobalStyle.js
import { createGlobalStyle } from 'styled-components';

export const Reset = createGlobalStyle`
    * {
        font-family: "Montserrat", sans-serif;
        margin: 0;
        padding: 0;
        text-decoration: none;
    }
`;

// arquivo App.js
import { Reset } from "./components/GlobalStyle";

export default function App() {
  return (
    <>
      <Reset />
      <SeusOutrosComponentesVemDepoisDoGlobalStyle />
    </>
  );
}
```

## Variáveis

Para criar variáveis que guardam algum valor e podem ser reutilizadas em seus estilos, você pode criar um arquivo nele exportar variáveis JS e o valor que devem assumir. Elas podem ser então importadas em seus styled components e usadas dentro de `${}`.

```js
// arquivo variaveis.js
export const corPrimaria = "#41d3be";

// arquivo Titulo.js
import styled from 'styled-components';
import { corPrimaria } from '../variaveis';

const Titulo = styled.h1`
  color: ${corPrimaria};
`;
```

- lembrando que o CSS também possui sua própria solução para variáveis, em que as definimos com o prefixo `--` e as utilizamos com a função `var()`.

## Props

Os styled components também podem receber props. Para acessá-las, você utiliza uma arrow function.

```js
const BtnCabecalho = styled.a`
  background-color: ${(props) => props.primary ? 'blue' : 'white'};
`;

// quando a prop não é informada, o valor será undefined
const Cabecalho = () => {
  return (
    <BtnCabecalho primary href="https://google.com">
        Ajuda
    </BtnCabecalho>
    <BtnCabecalho href="https://google.com">
        Sair
    </BtnCabecalho>
  );
};
```

- o nome `props` dentro da arrow function é somente uma convenção. Você pode utilizar o nome que quiser;

- você também pode passar valores às props.

Exemplo mais avançado, utilizando TypeScript e interface:

```js
import styled from "styled-components";

export interface TituloProps {
    cor?: string;
    tamanhoFonte?: string;
    alinhamento?: 'center' | 'left' | 'right';
}

export const Titulo = styled.h2<TituloProps>`
    padding: 20px 0;
    background-color: #FFF;
    color: ${props => props.cor || '#000'};
    font-size: ${props => props.tamanhoFonte || '18px'};
    text-align: ${props => props.alinhamento || 'center'};;
    margin: 0;
`;

<Titulo 
    cor='#EB9B00'
    tamanhoFonte='36px'
    alinhamento='center'
>
    Últimos lançamentos!
</Titulo>
```

## Herança

Podemos herdar estilos de outro styled component, utilizando a mesma função `styled`, passando como parâmetro o styled component a ser herdado:

```js
export const Icone = styled.img`
    height: 25px;
    width: 25px;
`;

// herdando estilos de Icone e adicionando mais estilos
export const IconeMargin = styled(Icone)`
    margin-top: 2px;
`;

// posso também sobrescrever estilos herdados 
export const IconeMaior = styled(Icone)`
    width: 80px;
`;
```

Veja mais sobre herança na [documentação da biblioteca](https://styled-components.com/docs/basics#extending-styles).

É possível até adicionar estilos a componentes de outras bibliotecas. A [documentação](https://styled-components.com/docs/basics#styling-any-component) também dá detalhes sobre como isso pode ser feito.

## Componente `ThemeProvider`

O styled-components oferece o componente `ThemeProvider`, que permite a aplicação de temas de maneira fácil. Por detrás dos panos, esse componente usa a Context API.

O `ThemeProvider` possui uma prop `theme`, em que você passa um objeto com suas definições para o tema. Subcomponentes de `ThemeProvider` podem acessar esse `theme` e utilizá-lo para estilizar seus elementos. 

```js
// arquivo variaveis.js
export const fundoClaro = "#f1f1f1";
export const textoFundoClaro = "grey";

// arquivo temas.js
export const TemaClaro = {
    body: fundoClaro,
    text: textoFundoClaro
};

// arquivo App.js
import { ThemeProvider } from "styled-components";
import { TemaClaro } from "./Components/UI/temas";
import Container from "./Components/Container";

function App() {
  return (
    <ThemeProvider theme={TemaClaro}>      
      <Container />
    </ThemeProvider>
  );
}

// arquivo Container.jsx
import styled from 'styled-components';

// acessando a prop theme e seus valores
const StyledContainer = styled.div`
  background-color: ${({ theme }) => theme.body};
  color: ${({ theme }) => theme.text};
  padding: 0px 15vw;
`;
```

A [documentação](https://styled-components.com/docs/advanced#theming) possui mais conteúdo a respeito de temas.

---

# Criação de uma biblioteca de componentes com o TSDX

Para criar um pacote TypeScript/React em que possamos disponibizar nossos próprios componentes (uma biblioteca de componentes, parte de um *design system*), podemos utilizar o **CLI [TSDX](https://tsdx.io)**. Basta rodar o comando abaixo para fazer o bootstrap de um projeto com a estrutura básica necessária.

    npx tsdx create nome_da_sua_lib

Durante a instalação, você pode selecionar um template para o projeto. No curso, utilizamos o template `react-with-storybook`. Saiba mais sobre o Storybook na [seção React Storybook](#react-storybook).

**Dica:** Coloque um prefixo no nome de seus componentes, para evitar colisão com componentes de outras bibliotecas. Uma sugestão é colocar as iniciais do projeto. Por exemplo, no caso do projeto da Alura Books, os componentes começam com "Ab": `AbButton`.

## Testando localmente

O projeto criado pelo TSDX também possibilita ver uma aplicação rodando localmente, em que você pode adicionar seus componentes e ver como eles são renderizados e se comportam. 

Para isso, primeiro faça um build na raiz do projeto:

    npm run build

Na pasta `example`, altere o arquivo `package.json` para que a dependência do parcel seja exatamente a versão "1.12.3" (sem o `^` antes da versão):

    "parcel": "1.12.3",

Na linha de comando, ainda na pasta `example`, faça a instalação das dependências

    npm i

e aí então você pode subir a aplicação

    npm start

A aplicação irá rodar no endereço: http://localhost:1234

Você pode adicionar seus componentes no arquivo `example/index.tsx`.

## Publicando no NPM

Necessário [criar uma conta no NPM](https://www.npmjs.com/signup).

Com a conta criada, abra um terminal, navegue até a pasta do projeto e faça login no NPM com o comando `npm login`.

Para publicar, rode o comando a seguir:

    npm publish --access=public

Pronto! O pacote estará disponível no NPM e você poderá acessá-lo/gerenciá-lo no seu perfil no NPM. 

- o nome do pacote será de acordo com o que está no `package.json` de seu projeto (propriedade `name`); caso o pacote não seja criado devido a conflito de nome (pacote que já existe com esse nome no NPM), basta alterar essa propriedade no `packgage.json` para algum nome diferente.

Para utilizar o pacote publicado, é o comando de sempre:

    npm i nome_do_seu_pacote.

### Publicando novas versões

Toda vez que o código do seu pacote evoluir, para incluir novas funcionalidades ou corrigir bugs, por exemplo, o comando para publicar esta evolução é o mesmo: `npm publish --access=public`.

No entanto, é necessário que você **altere a versão do seu pacote**, caso contrário o NPM irá reportar um erro de conflito de nomes ao rodar o comando. Essa alteração é feita no `package.json`, na propriedade `version`.

A numeração de versão pode seguir a convenção chamada ["versionamento semântico"](https://semver.org/lang/pt-BR/) (major.minor.patch). Exemplo:

    "version": "0.1.0"

- primeiro dígito (major): quando incrementado, indica que o código possui mudanças que não são compatíveis com versões anteriores. Costuma começar em 1; se for zero, significa que ainda não teve uma primeira release;

- segundo dígito (minor): quando incrementado, indica que o código possui mudanças, mas se mantém compatível com versões anteriores;

- terceiro dígito (patch): quando incrementado, indica que o código possui somente pequenas mudanças para correção de bugs, também se mantém compatível com versões anteriores.

## Automatizando a integração GitHub x NPM

Existe um pacote chamado `auto`, que possibilita automatizar a publicação de novas versões da biblioteca de componentes no NPM, a partir de push de arquivos no GitHub, por meio da configuração de uma Action no GitHub.

Instalação: 

    npm install auto

Script a ser adicionado no `package.json`, que será chamado pela Action do GitHub, para o `auto` publicar o conteúdo da branch main:

    "release": "auto shipit --base-branch=main"

A integração com o GitHub é por meio de uma GitHub Action, configurada com um arquivo (chamado de "ação") com a extensão `.yml`. Quando utilizado o CLI TSDX, ele já cria o diretório `.github\workflows` com duas ações de exemplos. Estes arquivos podem ser apagados e um novo pode ser criado com o nome `push.yaml` e o seguinte conteúdo:

```yaml
# Name of our action
name: Release

# The event that will trigger the action
on:
  push:
    branches: [main]

# what the action will do
jobs:
  release:
    # The operating system it will run on
    runs-on: ubuntu-latest
    # This check needs to be in place to prevent a publish loop with auto and github actions
    if: "!contains(github.event.head_commit.message, 'ci skip') && !contains(github.event.head_commit.message, 'skip ci')"
    # The list of steps that the action will go through
    steps:
      - uses: actions/checkout@v2
      - name: Prepare repository
        run: git fetch --unshallow --tags
      - name: Use Node.js 16.x
        uses: actions/setup-node@v1
        with:
          node-version: 16.x
      - name: Cache node modules
        uses: actions/cache@v1
        with:
          path: node_modules
          key: yarn-deps-${{ hashFiles('yarn.lock') }}
          restore-keys: |
            yarn-deps-${{ hashFiles('yarn.lock') }}
      - name: Create Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          #�� npm token, see https://storybook.js.org/tutorials/design-systems-for-developers/react/en/distribute/ to obtain it
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: |
          yarn install --frozen-lockfile
          yarn build
          yarn release
```

Na propriedade env é necessário passar o GITHUB_TOKEN e o NPM_TOKEN. O GITHUB_TOKEN será acessado diretamente pelo GitHub quando este arquivo estiver no repositório. Já para o NPM_TOKEN é necessário seguir dois passos:

- gerar o token no site do NPM:

    - clicar no seu avatar -> "Access Tokens", botão "Generate New Token" -> "Classic Token"

    - dar um nome para o token

    - selecionar o tipo "Publish"
    
- salvar esse token no repositório da biblioteca no GitHub 

    - aba "Settings", menu "Secrets and variables" -> "Actions", botão "New repository secret"

A publicação envolve alterar o package.json para incrementar a versão da biblioteca. Por isso, é necessário também dar permissão para o que o GITHUB_TOKEN possa escrever no repositório:

- Aba "Settings", menu "Actions -> General"

- Em "Workflow permissions", selecione "Read and write permissions"

---

# React Storybook

*Nota: os códigos e explicações valem para a versão 6.5. A partir da versão 7, algumas funções, módulos, etc. foram descontinuados e outros entraram no lugar.*

[Documentação da versão 6.5](https://storybook.js.org/docs/6.5/react/get-started/install/)

O React Storybook é uma ferramenta que auxilia na criação de um catálogo de componentes. Ele provê uma interface visual em que é possível visualizar e testar os componentes de modo isolado, sem interferência de outros componentes. Como forma de documentação, é possível também visualizar quais props o componente possui e até modificar seus valores, verificando dinamicamente as alterações no componente. Na tela, também há opções para ver o código, alterar a viewport (para ver como fica em mobile e tablet), etc.

Por meio de um arquivo com a extensão `.stories.tsx` (ou `.stories.jsx` ou `.js`/`.ts`), é possível informar como o componente deve ser exibido no Storybook, podendo passar diferentes props (por convenção é chamado de `args`) e dados mockados para renderizar o componente de modos diferentes. A isso é dado o nome de *"story"*.

Para subir o Storybook, tendo feita a instalação com o TSDX e selecionado o template react-with-storybook, basta entrar na pasta do projeto criado e rodar o seguinte comando:

    npm run storybook

O Storybook irá rodar no endereço: http://localhost:6006/ 

## Exemplos de arquivo `.stories`

Exemplo (em TypeScript) para um componente de botão (`AbButton`) com duas stories: Primario e Secundario. Isso faz com que o componente renderize com estilos de cor diferentes, baseado na prop `btnStyle`.

```jsx
import React from "react";
import { ComponentMeta, ComponentStory } from "@storybook/react"
import { AbButton, AbButtonProps } from "../src"

// objeto para mostrar o componente no painel lateral do Storybook
export default {
    title: 'Components/AbButton',
    component: AbButton
} as ComponentMeta<typeof AbButton>

//--- criação de stories

// modelo para renderizar o componente, passando props (args)
const Template: ComponentStory<typeof AbButton> = (args) => <AbButton {...args} />

// exportando a story de nome "Primario", baseado no modelo Template
export const Primario = Template.bind({});

// alterando os argumentos
Primario.args = {
    text: 'Primary button'
} as AbButtonProps;

// exportando uma segunda story, com outros valores
export const Secundario = Template.bind({});
Secundario.args = {
    text: 'Secondary button',
    btnStyle: 'secondary'
} as AbButtonProps;
```

É possível também passar children como argumento. Nesse caso, ele entra como mais uma propriedade de `args`. O exemplo abaixo é para um componente `AbCard`. Repare que no Template não houve mudança - você pode passar tudo como `{...args}` ao invés de englobar `{args.children}` dentro de uma tag `AbCard`:

```jsx
const Template: ComponentStory<typeof AbCard> = (args) => <AbCard {...args} />;

export const Card = Template.bind({});
Card.args = {
    children: 'Your content goes here'
} as AbCardProps
```

## Erros e solução para subir o Storybook

O curso foi feito na versão 6.5. Quando rodamos o comando do TSDX, ele automaticamente coloca as versões mais novas das dependências no arquivo `package.json`. Acontece que na versão nova do storybook (versão 7) o comando `npm run storybook` (ou `yarn storybook`) gera um **erro e o comando não é reconhecido**. 

A solução que encontrei foi fazer um **downgrade das dependências**, alterando o arquivo `package.json`:

```json
"devDependencies": {
    "@babel/core": "^7.18.10",
    "@size-limit/preset-small-lib": "^8.0.0",
    "@storybook/addon-essentials": "^6.5.10",
    "@storybook/addon-info": "^5.3.21",
    "@storybook/addon-links": "^6.5.10",
    "@storybook/addons": "^6.5.10",
    "@storybook/react": "^6.5.10",
    "@types/react": "^18.0.17",
    "@types/react-dom": "^18.0.6",
    "babel-loader": "^8.2.5",
    "husky": "^8.0.1",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-is": "^18.2.0",
    "size-limit": "^8.0.0",
    "tsdx": "^0.14.1",
    "tslib": "^2.4.0",
    "typescript": "^4.7.4"
}
```

Feita essa alteração, apaguei a pasta node_modules e fiz uma nova instalação do projeto com o `npm i`. Feito isso, consegui subir o storybook utilizando o `npm run storybook`.

## Deploy na Vercel

[Este link](https://vercel.com/guides/storybook-with-vercel) dá detalhes para fazer o deploy do Storybook na Vercel.

Caso encontre problemas no deploy, adicione esta entrada no `package.json` para utilizar a versão 16 do Node.

```json
"engines": {
    "node": "16.x"
},
```

---

# Next.js

O [Next.js](https://nextjs.org) é um **framework construído em cima do React**. Ou seja, ele dá poderes adicionais ao React para construção de aplicações Web, proporcionando muita **otimização** da aplicação e **ferramentas para facilitar o building/deploy** do produto final.

- Atualmente (2023), o Next.js é um dos frameworks [recomendados pelo React](https://react.dev/learn/start-a-new-react-project) para criação de um novo projeto React. **O CRA já não aparece na recomendação e tende a ser preterido pelos desenvolvedores**.

O framework possui sua **própria solução para roteamento**, mas também é possível utilizar outras bibliotecas, como a React Router. 

Em questões de otimização, é possível fazer uma renderização híbrida dos componentes, com aqueles que exigem interatividade sendo renderizados no cliente (no navegador, como estamos acostumado com aplicações React), e outros **componentes sendo renderizados pelo servidor**, que entrega o HTML pronto ao cliente - isso ajuda também na indexação da página pelos buscadores (questão de SEO). Há também otimização no uso de imagens e fontes.

- A documentação do Next.js usa os nomes **"Client Components"** e **"Server Components"** para diferenciar os componentes renderizados pelo cliente (CSR - Client Side Rendering) e os renderizados pelo servidor (SSR - Server Side Rendering);

- Por padrão, os componentes dentro da pasta `app` são Server Components. Para indicar que um componente deve ser renderizado pelo cliente, é utilizada a diretiva `use client` **no topo do código** no arquivo do componente.

    - Client Components são utilizados quando há necessidade de interatividade por meio de eventos ou quando se usa hooks.

## Instalação

Para as versões atuais do Next.js (versão 13), recomenda-se o Node versão 16.8 ou superiores.

É possível fazer uma [instalação manual](https://nextjs.org/docs/getting-started/installation#manual-installation) do framework, mas também há a opção automatizada, por meio de um processo interativo utilizando o npx: 

    npx create-next-app@latest

## Pasta `public`

Quando criada, esta pasta será entendida pelo Next.js como sendo o local em que serão armazenados os **arquivos estáticos** (imagens, fontes, arquivo robots.txt, etc). Esse arquivos ficarão públicos, ou seja, acessíveis diretamente da raiz do projeto (da base URL, sem necessidade de incluir `public`).

Por exemplo, caso você queira utilizar uma imagem que está em `public/images/logo.svg` como sendo o `src` de uma tag `img`, você passa o caminho relativo dessa forma: `/images/logo.svg`. 

Da mesma forma, caso queira acessar o arquivo pelo navegador, basta informar: `http://www.nomedoseusite.com.br/images/logo.svg`.

## App Router

A partir da versão 13, o Next.js introduziu o conceito de App Router (anteriormente, isso era feito pelo [Pages Router](https://nextjs.org/docs/pages/building-your-application/routing)). 

O **roteamento acontece dentro da pasta `app`**, considerada a raiz da aplicação (a homepage). Novas rotas são definidas por meio da **criação de subpastas** dentro de `app`. A página que será exibida por cada rota é incluída em um arquivo **`page.js`** (ou .jsx ou .tsx).

- é possível criar **rotas aninhadas**, por meio da criação de outras subpastas dentro das subpastas, mantendo a mesma estrutura.

O App Router suporta criação de layouts, templates, solução para o erro 404, etc, por meio do uso de **"special files"**, que seriam arquivos que utilizam uma nomenclatura específica: `page.js`, `layout.js`, `template.js`, `not-found.js`, etc.

- você também pode adicionar outras pastas e arquivos dentro de `app` não relacionados a roteamento. Elas **não** serão acessíveis ou consideradas como uma rota da aplicação, a não ser que esteja presente um `page.js` ou `route.js` nesta pasta;

- alternativamente, nada te impede de criar outra pasta fora de `app` para colocar os demais arquivos de seu projeto. Por exemplo, ter a pasta `app` para lidar somente com rotas e páginas, e outra pasta irmã `components` para criar seus componentes. O dev e sua equipe são livres para decidir como o projeto será estruturado.