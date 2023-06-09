# Anotações sobre JavaScript

## Instrutoras e Instrutores

### DIO

[Stephany Nusch](https://github.com/stebsnusch)

[Diana Martine](https://github.com/DianaMartine)

### Alura

[Vanessa Me Tonini](https://www.linkedin.com/in/vanessametonini/)

[Pedro Marins](https://www.linkedin.com/in/pedromarins/)

[André Bessa](https://br.linkedin.com/in/andre-bessa-silva-xti)

[Juliana Amoasei](https://github.com/JulianaAmoasei)

[João Manoel Lima](https://github.com/joaom98)

[Beatriz Moura](https://www.linkedin.com/in/beatrizmouradev/)

[Nayanne Batista](https://www.linkedin.com/in/nayannebatista/)

## Introdução

O JavaScript (JS) é **baseado no ECMAScript** (ES), que por sua vez é uma linguagem de programação baseada em scripts padronizada pela Ecma International na especificação ECMA-262.

- JavaScript, ActionScript, JScript seguem os padrões da ECMAScript, e adicionalmente têm seus próprios recursos.

Não tem *nada a ver com Java*. O nome foi uma jogada de *marketing* para se aproveitar da popularidade crescente que Java estava adquirindo na época em que o JavaScript foi lançado (o nome inicial era Mocha, depois mudado para LiveScript e então JavaScript). 

Pode ser utilizada também em dispositivos móveis, wearables (smartwatch, por exemplo), IoT (Alexa e Google Home, por exemplo), games, APIs.

Um jeito de **rodar arquivos JS puro no terminal é instalar o Node.js**.

Ao importar o arquivo JS para um HTML, é necessário ter **ambas** as tags, de abertura e fechamento, mesmo não havendo nada entre elas:

```js
<script src="main.js"></script>
```

Local da tag script: o *ideal* é colocá-la **antes da tag de fechamento do body**, pois assim garante que todo o conteúdo já foi renderizado (no caso de seu script trabalhar com algum conteúdo que precisa ser renderizado antes).

- Quote da Diane Martine (Avanade): "Primeiro precisamos ter a estrutura do prédio, para depois incluir a parte funcional";

- Caso seu script não dependa da renderização do HTML ou você precise que o **JS faça alguma coisa *antes* de a página renderizar**, pode ser colocado **dentro da tag head**.

Convenção de nome: utilizar o nome main.js para seu script principal.

Uso do ponto e vírgula: é opcional, mas é **altamente recomendável** utilizá-lo, para evitar dores de cabeça.

Frameworks e bibliotecas: oferecem facilidades para programar em JS, economizando tempo e facilitando algumas atividades. O React é considerado uma *biblioteca*.

## Funções para manipular o DOM

`document.querySelector('texto_para_buscar')`: busca no HTML o texto informado (chamado de "seletor"). Pode ser **uma tag, uma classe (usando o .), um id (usando o #)**. Retorna o **PRIMEIRO** elemento que der match com o seletor;

- no seletor, posso incluir **atributos** de uma tag, utilizando colchetes

```js
document.querySelector('input[type=tel]');
```

``document.querySelectorAll('texto_para_buscar')``: irá retornar **todos** os elementos que derem match com o seletor passado. O retorno é um `NodeList` (uma lista, um array).

- Os `querySelector` são *mais poderosos* do que os `getElementById` e `getElementsByTagName`, então **use** querySelector e querySelectorAll.

`<elemento>.addEventListener('nome do evento', função)`: adiciona o evento ao `<elemento>`, com a função passada.

- Só o nome do evento, não precisa começar com "on" (por exemplo: somente `click` ao invés de `onclick`)

- pode ser uma função *definida em outra parte do código* (sem o parênteses) ou uma função anônima/arrow function (veja mais na [Seção sobre Funções](#funções)).

### Eventos

Começam com a palavra `on`+`nomeDoEvento` e permite executar uma função

```js
button.onclick = () => { 
    // seu código... 
}
```

Se atribuir uma função definida em **outra parte do código**, ela **NÃO** deve ser atribuída ao evento com **parênteses**, senão será invocada imediatamente. Caso a função **necessite de parâmetros**, precisa **criar uma função anônima ou arrow function** para chamar essa outra função com os parâmetros.

- Uma explicação aprofundada sobre funções, funções anônimas e arrow function é vista na [Seção sobre Funções](#funções).

Os eventos possuem um **parâmetro padrão** que geralmente chamamos de **`event`, ou `e`**, ou qualquer nome que você der. Esse `event` é um **objeto**, com propriedades e métodos que podem ser acessados e utilizados.

- se der um `console.log` no evento, é possível ver o que tem dentro dele no console do navegador.

- `e.target.parentNode`: acessa o **pai do elemento** que causou o evento.

Lista de eventos: https://www.w3schools.com/jsref/dom_obj_event.asp

Dentro do evento, temos a propriedade `target`, que retorna o elemento em que o evento ocorreu (por exemplo, no clique de um botão, o `event.target` retorna o `<button>`).

## Data attributes

Às vezes, por decisões de design, classes e ids de elementos HTML podem ser alterados. Por isso, **não é recomendável** que o código JavaScript **dependa do nome de uma classe ou id**. Uma alternativa para isso é **usar os data attributes**, conceito incluído no HTML5. 

Os data attributes são atributos personalizados que podem ser dados aos elementos do HTML. Esses atributos começam com o prefixo `data-` e podem possuir ou não um valor associado

```html
<input id="cadastro" data-ativo data-tipo="cadastra-usuario" >
```

Elementos com data attribute podem ser selecionados utilizando o `querySelector` e `querySelectorAll`, passando o nome do data-atribute **entre colchetes**.

```js
const cadastro = document.querySelector("[data-tipo]");
console.log(cadastro); 
// retorna <input id="cadastro" ...>
```

Valores de data attributes podem ser **accessados e modificados** por meio do **objeto `dataset`**, presente no elemento que possui data attributes. 

- Para **acessar** um atributo específico do dataset, basta informar o nome colocado após o prefixo `data-`.

```js
console.log(cadastro.dataset.tipo); 
// retorna 'cadastra-usuario'
```

- também pode ser acessado dentro de um evento

`e.target.dataset.tipo`

O retorno do dataset é uma *string*. O valor pode ser **modificado**, bastando *atribuir* um novo valor.

`cadastro.dataset.tipo = 'remove-usuario'`

## Variáveis e escopo

`var nome`: escopo **global e local** (local quando declarada dentro de uma função, por exemplo);

`let nome`: escopo **local de BLOCO** (não será vista pelo código após o bloco (bloco é qualquer coisa entre chaves `{}`)); 

- por convenção, `let` é usada somente dentro de blocos; 

- boa prática: dê preferência ao `let` ao invés de `var`, por questões de segurança, já que `var` tem um contexto global e pode ser manipulada via console. O `let` também pode ser manipulado via console, dependendo de onde for declarado. No entanto, devido a seu escopo local de **bloco**, oferece mais proteção do que var;

- dentro de uma função, `var` é vista por *todos* na função, *inclusive* se for declarada em *blocos internos* (dentro de um if, por exemplo). Já `let`, se declarada dentro de um bloco `if` dentro da função, será vista *somente pelo bloco* `if`, e não pela função inteira.

`const nome`: boa prática é declarar constantes em SNAKE_UPPPER_CASE.

- diferente das variáveis, `const` **precisa ser inicializada com algum valor**, senão irá subir uma exceção no console;

- também possui **escopo de BLOCO**.

Boa prática para `var`, `let` e `const`: faça a declaração *no topo do bloco de código*, para auxiliar o "hoisting" do interpretador a elevá-las até o topo desde o início.

- no caso de `let` e `const`, o interpretador faz o hoisting, mas não inicializa estas variáveis; 

- no caso de var, ela é inicializada como undefined se nenhum valor tiver sido atribuído a ela.

### Hoisting

Hoisting é uma etapa em que o interpretador **coloca na memória** as declarações de funções, variáveis ou classes, *antes de executar o código*. É como se ele colocasse no topo do arquivo essas declarações. 

Funciona bem para funções (é por isso que você pode usá-las antes de declará-las), mas pode causar erros com variáveis e classes (o hoisting eleva somente as **DECLARAÇÕES** e **não as inicializações**; em casos específicos elas são inicializadas com seu valor padrão, em outros casos não são nem inicializadas).

Diferença para `var`: ela tem um escopo mais abrangente, então se possível é melhor considerar utilizar o `let` e o `const`. Diferente de `let` e `const`, durante o hoisting a `var` é elevada e *inicializada como undefined*, podendo, por essa causa, *ser utilizada antes mesmo de ser declarada no código*.

## Comparação

`==` : irá comparar os valores. O interpretador irá tentar descobrir o "valor primitivo" entre as duas comparações, por isso às vezes o retorno pode trazer alguma surpresa ("0" == 0 é true, por exemplo);

`===` : comparação idêntica: tanto o **valor** quanto o **tipo** têm que ser iguais;

Comparadores lógicos: `&&,` `||` e `!`

## Arrays

Em JavaScript, dentro de um mesmo array pode haver elementos de diversos tipos, inclusive objetos e outros arrays.

- para adicionar itens: `push(novo_item)` (adiciona ao **final**) e `unshift(novo_item)` (adiciona ao **início**);

- remoção: `pop()` (**último** item), `shift()` (**primeiro** item);

- `splice(indice, quantos, item_1...item_n)`: remove a partir do `indice`, `quantos` itens você informar (`quantos` é opcional). Se informar outros itens (também opcional), splice irá *ADICIONAR* os itens a partir do `indice` e, caso `quantos` for informado, irá antes remover os itens a partir de `indice` para depois adicionar os novos itens. O método retorna um **novo array**;

- `slice(indice, quantos)`: faz um recorte; cria um novo array com `quantos` itens a partir de `indice` do array em que o método foi invocado. Necessário tomar cuidado quando o array possui objetos, pois o `slice()` faz uma ["shallow copy"](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) do array.

- `foreach(function(element, index, arr), thisValue)`: método que executa uma função; `function` e `element` são obrigatórios, os outros são opcionais. A função pode ser definida dentro ou fora do método (se fora do método, somente faz a chamada da função, sem argumentos);

    - também pode ser mais direto com uma arrow function: `forEach((element, index, arr) => {...})`, `index` e `arr` são opcionais

- reticências (`...`) antes de um array (`...arr`) pode indicar um **spread** ou um **rest**:
    - Spread: acontece quando, por exemplo, você passa um array como **argumento a uma função**; neste caso, o array será *desmembrado* e seus elementos serão *tratados um por um*
    - Rest: é o oposto do spread, quando usado na **DEFINIÇÃO de uma função**, *agrupa* em um array os elementos que vêm depois das reticências; deve ser o **último parâmetro** na definição da função.

```js
function foo(argA, argB, ...args); 
//tudo que vier a partir do 3º parâmetro será agrupado em um array args dentro da função
```

## Condicionais e loops

JavaScript possui o if ternário: `[condicao] ? [instrução se true] : [instrução se false];`

Laço de repetição: **três tipos de for**:

```js
var valores = [0, 1, 2, 'três'];

// i como índice; é o for clássico
for(let i = 0; i < valores.length; i++){
    console.log(valores[i]); //0, 1, 2, 'três'
}

// i como ÍNDICE, percorrendo array e objeto
for(i in valores){
    console.log(i); //índices 0, 1, 2, 3
}

// i como VALOR, somente para array e outras estruturas iteráveis (como strings)
for(i of valores){
    console.log(i); //valores 0, 1, 2, 'três'
}
```

## Template Strings

Usada com duas crases (\`\`), podendo **juntar** strings com expressões e variáveis (chamadas com `${nome_variável}`), sem necessidade de usar alguma função para concatenar.

```js
console.log(`O resultado de ${num1} + ${num2} é ${resultado}`);
```

- se pular linha, a quebra de linha **também** será incluída na saída.

## Funções

Esqueleto de uma função:

```js
function nome(param_a, param_b, ...) {
	...
	return algum_retorno;
}
```

- parâmetros e retorno são opcionais

    - **parâmetro** é o nome dado quando você **DECLARA** a função; **argumento** é o nome dado quando você **INVOCA** a função;
        
        - "A função recebe como parâmetro..."
        
        - "Você passa à função como argumento...";

    - parâmetros podem ter valor padrão (estilo Python); isso foi implementado a partir do ES2015 (aka ES6);

    - o objeto `arguments` pode ser acessado dentro da função e traz, dentro de um array, todos os argumentos que a função recebeu;

Quando a função é **atribuída a uma variável** (chamado de "função de expressão"), o nome da função é opcional, e torna-se o que se chama no JavaScript de "função anônima":

```js
var soma = function() { 
    return 1 + 2; 
}; 
//para chamar a função, use soma();
```
### IIFE

Uma função pode ser **autoinvocável** (IIFE: Immediately Invoked Function Expression): declare-a (anônima ou não) dentro de parênteses, seguido de `(parâmetros_opcionais)`;

```js
(function() {
    console.log('Autoinvocável');
})();
```

### Callback

Nome dado quando passamos uma **função como argumento para outra função** (ouviremos bastante sobre isso quando tratamos sobre JS assíncrono); é passado *somente o nome da função, sem os ()*, senão ela será executada;

### Funções e o `this`

Funções `call()` e `apply()`: quando uma função usa o `this`, você usa o `call`/`apply` para passar o `this` para a função; no `call`, outros argumentos são passados separado por vírgulas, e no `apply` você passa os outros argumentos dentro de um array 

- Sim... é complicado de entender.... E tem também o `bind()`, que clona a função e você pode alterar o `this` dentro dela como argumento...

Vamos tentar novamente:

- `call()` invoca uma função; o primeiro parâmetro é o `this` que será utilizado como contexto (por exemplo, um objeto); o segundo parâmetro em diante são os argumentos a serem passados para função invocada.

- `apply()` também invoca uma função, semelhante a `call()`, com o primeiro parâmetro sendo o `this` e o segundo sendo um **array** com os argumentos da função invocada.

-  com o `bind()` você cria uma função `x` baseada em outra função `y`, chamando o `bind()` em `y` para anexar o contexto que será utilizado quando a função for executada (`bind()` recebe como parâmetro o `this`); é um objeto emprestar o método de outro objeto. 

- São formas de usar o `this` como referência a um objeto dentro do escopo de funções. O `bind()` não executa imediatamente, enquanto `call()` e `apply()` sim.

### Arrow function 

São anônimas e também podem ser atribuídas a uma variável
   
```js
var soma = () => { 
    return 1 + 2; 
};
```

- tem muito mais detalhes; ver a documentação: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#syntax

- arrow function **não possui** o objeto `arguments`;

- `call()`, `apply()` e `bind()` **não irão** funcionar;

- **não possui** this -> ela **herda automaticamente o contexto** de onde foi criada; 

- arrow function e funções anônimas não são "hoisted", ou seja, são interpretadas no momento da sua execução, não podendo ser utilizadas antes de serem declaradas;

- é **melhor usar const** ao atribuir uma arrow function a uma variável, já que o retorno dessa função é um valor constante;

## Objetos 

Declarados entre `{};` possuem **propriedades** e **métodos**.

```js
let objeto = {
    texto: 'texto', // não precisa usar var nem let, e cada propriedade é separada por vírgula
    numero: 1, // é como se fosse um conjunto de propriedades chave : valor
    valido: true,
    itens: ['item 1'],
    objetoInterno: {
        textoInterno: 'texto interno'
    },
    ola: function(){return 'Olá, sou um objeto!'}
};
```

- propriedades são acessadas pelo ponto (`objeto.numero`) ou colchetes (`objeto['numero']`);

- métodos também são acessados pelo ponto + parênteses, para a função ser executada: `objeto.ola()`;

- posso selecionar mais de uma propriedade de uma vez: `var { texto, valido } = objeto`;

- posso adicionar novas propriedades: `objeto.novaPropriedade = 'sou uma nova propriedade'`;

- posso deletar propriedades com o comando `delete`; ao tentar acessá-las, irá retornar "undefined": `delete objeto.novaPropriedade`

- posso também inicializar um objeto vazio: `let novoObj = {}`;

- `Object.values(objeto)`: retorna um array com todos os **valores**;

- `Object.keys(objeto)`: retorna um array todas as **chaves** (nome das propriedades);

- `Object.entries(objeto)`): retorna um array de arrays bidimensionais, sendo que cada cada elemento é uma [chave, valor]

### JSON

Formato `chave: valor`. Muito utilizado para comunicação entre back-end e front-end (APIs). Notação parecida com objetos, porém, mais restritiva com relação ao formato (o nome das chaves é entre aspas, não pode ter comentários, não pode ter uma vírgula no final da última propriedade (trailing comma), etc).

`JSON.parse(dadosJSON)`: converte JSON para um objeto JavaScript. Comumente usado quando **recebemos** um JSON da API;

`JSON.stringify(objetoJS)`: converte um objeto JavaScript para o formato JSON. Comumente usado quando **enviamos** dados para uma API.

### Prototype

`Object.prototype`: é o objeto no *topo* da cadeia de objetos do JS. No JS, **todos** os objetos herdam **propriedades e métodos de um prototype**. Se eu chamo um método de um `obj`, será primeiro procurado se o `obj` possui este método; se não possuir, vai subindo na cadeia procurando o método nos pais e assim por diante.

`objeto.__proto__`: propriedade que lista os métodos de protótipo do objeto instanciado, herdados do "objeto oculto", que seria o pai desse objeto. Funciona melhor no console do navegador (o Node não mostra todas as propriedades).

- a propriedade `__proto__` serve mais para exemplificar e está sendo descontinuada;

- dados primitivos (numeros, textos, etc): não herdam necessariamente (já que são primitivos), mas possuem um objeto pai que os engloba.

```js
[1, 2, 3].__proto__; // mostra todas as propriedades de Array
```

### Funções construtoras

É possível **criar objetos a partir de um modelo**, por meio de funções construtoras. Esta era a maneira que o JS oferecia para se trabalhar antes de possibilitar o uso de classes na linguagem, que é a maneira moderna de criar objeto 

- A criação antes era feita por meio de protótipos e cadeia de protótipos. Internamente, continua sendo assim - a maneira moderna é somente um "syntax sugar", ou seja, uma sintaxe mais fácil de ler por um humano. 

Hoje em dia, trabalha-se mais com classes, mas entender protótipos é ideal para entender as particularidades do JS e também no caso de cair em um projeto legado.

```js
// função construtora
function User(nome, email) { // por convenção, o nome da função começa com maiúscula
    this.nome = nome; // uso do this para referenciar o contexto do objeto quando criado
    this.email = email;

    this.exibirInfos = function() {
        return `Nome: ${this.nome}; e-mail: ${this.email}`;
    }
}

// criando um novo objeto a partir desse modelo User
const novoUsuario = new User('Matheus', 'meu@email.br');
console.log(novoUsuario.exibirInfos());
```

Objetos criados por meio de funções construtoras têm esta função como sendo seu protótipo.

Existem outras formas de criar objetos a partir de um modelo, como o uso de `Object.create()`, sem a necessidade de uma função construtora.

## Classes 

Surgiram a partir do ES6 (ou ES2015 - é a sexta versão do EcmaScript, lançada em 2015, daí os dois nomes). 

Não existem nativamente no JS, mas podem ser criadas como forma de facilitar a escrita/entendimento (o tal do *syntax sugar*). Por trás dos panos, o que há são objetos, que herdam métodos e propriedades de protótipos.

É possível declarar e inicializar propriedades dentro do constructor, sem a necessidade de declará-las fora. As propriedades declaradas e inicializadas no constructor ficam visíveis para o restante da classe.

- boa prática: propriedades que terão **getters e setters** costumam ter o nome **começando com underline**, para diferenciar do nome no get e set (que são palavras-chave: `get saldo = return this._saldo;`)

### Herança

```js
class NovaClasse { //sem atribuição
    constructor(/*...*/) {
        // ...
    } 
    // getters, setters, métodos
}

class ClasseFilha extends NovaClasse {
    constructor(){ // pode ter atributos adicionais, específicos dessa classe
        super(/*...*/) // chamada ao construtor da classe mãe, com os atributos que ela precisa
    }
    // ...
}

let a = new NovaClasse();
let b = new ClasseFilha();
```

### Composição

Na herança, o relacionamento é do tipo "É um". Na composição, o relacionamento é do tipo "Tem um". 

No JS, **não** há herança múltipla. Neste caso, você pode utilizar de composição, em que uma classe pode conter outra ou outras classes -> você pode criar uma propriedade dentro dessa classe, sendo que essa propriedade é instância de uma outra classe (você dá um `new` dessa outra classe e atribui à propriedade).

- Isso é uma suposição minha. Nenhum instrutor chegou a dizer que composição é uma alternativa para herança múltipla. 

Composição pode ser usada quando você quer usar o comportamento de outra classe, e não necessariamente **ser**  uma instância por completo daquela classe.

### Propriedades e métodos estáticos

Propriedades e métodos estáticos são declarados com a palavra reservada `static` antes do nome da variável:

```js
class Quadrado {
    static tipo = 'quadrado';
    static calculaPerimetro(lado) { 
        return lado * 4 
    }
}
```

Essa propriedades/métodos pertencem **à classe**, e não a uma instância da classe. Assim, quando uma instância é criada, estas propriedades/métodos não precisam ser recriados, pois já existem na classe.

O acesso pode ser feito de duas maneiras: pelo nome da Classe ou pela propriedade `constructor`. O acesso diretamente da instância da classe **não** irá funcionar.

```js
console.log(Quadrado.tipo); 

const meuQuadrado = new Quadrado();
console.log(meuQuadrado.constructor.tipo); 
console.log(meuQuadrado.constructor.calculaPerimetro(4));

// irá retornar undefined, pois a propriedade é da Classe
console.log(meuQuadrado.tipo); 

// irá dar erro, pois o método é da Classe
console.log(meuQuadrado.calculaPerimetro(4));

```

### Propriedades privadas 

As propriedades de uma classe são públicas por padrão. Para torná-las privadas, adicione `#` como prefixo ao nome da propriedade:

```js
class Conta {
    #saldo = 1000; // propriedade privada
    nome = 'Fulano'; // propriedade pública

    // se estivesse dentro de uma função
    // this.#saldo
}
```

Propriedades privadas *devem* ser declaradas *fora* do construtor, ou seja, na definição da classe.

Propriedades privadas **não podem** ser acessadas nem modificadas **fora da classe** em que foram declaradas. Nem mesmo instâncias da classe possuem acesso a essas propriedades. Subclasses **também não** têm acesso nem podem modificar essas propriedades.

Para fazer o acesso ou modificação, é necessário criar getters (com a palavra reservada `get` antes do nome da propriedade que irá retornar o valor da propriedade privada) e setters (mesma lógica, mas com a palavra reservada `set`)

```js
// adicionado ao código da class Conta
get saldo() { return this.#saldo }

set saldo(valor) { this.#saldo += valor }
```

Os getters são criados como funções, mas são acessados como propriedades (sem o `()`). Os setters também não precisam de `()` e o valor é passado como atribuição (uso do `=`). Os nomes para os getters e setters podem ser qualquer coisa, mas a convenção é ser o nome do atributo que eles acessam/modificam.

```js
const minhaConta = new Conta();
console.log(minhaConta.saldo); // e não saldo(), nem minhaConta.#saldo
minhaConta.saldo = 450;
console.log(minhaConta.saldo); // 1450
```

### Métodos privados

Métodos também podem ser privados, bastando adicionar `#` antes da declaração do método. Para serem executados, devem ser chamados com a `#`. (`this.#metodoPrivado()`).

### Polimorfismos

No JS pode haver **somente sobrescrita** de métodos (overriding), ou seja, os métodos nas subclasses podem ter um comportamento diferente da superclasse, mas a **assinatura do método não pode mudar** (não pode conter novos argumentos ou remover argumentos). Ou seja, a sobrecarga (overload) não é possível em JS.
 
## Erros

- `throw`: usado para **criar seu próprio erro**; pode substituir um return na função, jogando um erro que pode ser capturado e tratado.

- `try...catch`: captura e manipulação de um erro; coloca o código dentro do try e captura/manipula o erro com o catch;

- `finally`: bloco de código que **será executado**, independente de existir ou não um erro pego no try...catch; é opcional.

```js
try {
    //bloco a ser executado
}
catch(e) {
    //se um erro "e" for jogado, será manipulado aqui
}
finally {
    //será executado, mesmo se acontecer um erro no bloco do try
}
```

- `Error`: é um objeto que pode ser criado para personalizar o erro com uma mensagem, o nome do arquivo e a linha onde o erro ocorreu; os três parâmetros são opcionais
    - também pode dar um nome ao erro com a propriedade `name`;

## Sincronicidade e assincronicidade

JavaScript por padrão é síncrono (é uma única thread), processando e executando o código em sequência, linha a linha.

### Call Stack 
Local em que ocorre o processamento síncrono de cada linha do JS que precisa ser executada.

Enquanto toda a operação/processamento requerido por aquela linha não for finalizado, ela fica "empilhada" dentro da call stack. 

Se for uma função, por exemplo, ela irá entrar na Call Stack e cada comando dentro dela será empilhado, processado e, após finalizado, sairá da stack, prosseguindo para o próximo comando até finalizar a função.

### Task Queue 

É onde ocorre o **"processamento assíncrono"**. Comandos assíncronos são colocados na fila da Task Queue para serem processados após algum evento que informe que ela deve começar (por exemplo, um tempo de espera, uma resposta da API, uma ação de clique do usuário). 

Quando a task deve ser de fato processada, ela sai da Task Queue e aguarda sua vez de ser empilhada na Call Stack. 

Enquanto a tarefa está na Task Queue, as **outras linhas** de código após ela **vão sendo executadas** (e caso haja outra operação assíncrona, ela será enviada para a Task Queue e o restante do código continuará a ser executado, e assim por diante).

### Event Loop 

É quem monitora e executa as operação do JS a serem feitas, tanto aquelas da Call Stack como aquelas da Task Queue que estiverem prontas para serem executadas. É o gerenciador de eventos.

### Callback

Já comentado anteriormente, são funções enviadas como parâmetro para outras funções. Por exemplo, `setTimeout()` e eventos (click, change, etc) podem receber uma função como parâmetros; elas serão chamadas de callback. 

Callbacks podem ser usados para tarefas assíncronas como essas dos exemplos anteriores. 

### Promise

É um objeto de processamento assíncrono; representa a conclusão de uma operação assíncrona; uma **promessa de que aquele código será executado**, podendo retornar que foi resolvido ou rejeitado. 

- possui uma função anônima (ou arrow function) com dois parâmetros: `resolve` e `reject`, os quais também são funções.

- a **resposta** de uma Promise é um **objeto** do tipo response

    - para **acessar** um response que foi **resolvido**, utiliza-se o **método `then()`**

    - caso a response seja **rejeitada**, ela é acessada pelo **método `catch()`** (o catch também irá pegar qualquer erro que for jogado). 
    
    - é possível também utilizar o método `finally()` ao final, que será executado independente se a response foi resolvida ou rejeitada;

```js
retorno_da_promise
    .then('faz alguma coisa, pode ser um callback')
    .catch('faz outra coisa, pode ser um callback')
    .finally('faz mais uma coisa, pode ser um callback')
```

Mais detalhes sobre isso abaixo

- propriedade `result`: é o resultado da Promise, que pode ser undefined, um valor, um objeto Error;

- propriedade `state`: estado da Promise: pending (está executando, `result` undefined), fulfilled (executou com sucesso, `result` é um valor) e rejected (não executou como esperado, `result` é um erro);

    - `rejected` informa que houve um erro, mas o código continua sendo executado e o erro pode ser tratado posteriormente (diferente do `throw`, que encerra a execução do código);

- ambas as propriedades `result` e `state` **não podem ser acessadas**. Para tratar uma promise, é utilizado o método `then(callback_sucesso, callback_erro);`. Ambos os parâmetros são opcionais. O **`then` retorna outra promise**, então pode ser encadeado; também é possível utilizar .`then() .catch` para tratar sucesso e rejeição separadamente.

- mais detalhes: https://medium.com/trainingcenter/entendendo-promises-de-uma-vez-por-todas-32442ec725c2

### `async` e `await`

Promises também podem ser tratadas em funções assíncronas, **sem necessidade de then/catch/finally**. Nesse caso, é colocada a palavra `async` antes da declaração da função, e a palavra `await` antes da chamada de uma função que irá retornar uma promise.

- introduzidos na EcmaScript 2017 (ES8);

- ao digitar a palavra-chave `async` antes de uma função faz com que a **função RETORNE uma promise**, que pode então ser tratada;

- ao digitar a palavra-chave `await` antes de uma função/variável faz com esta **ESPERE por uma promise ser finalizada**; desse modo, o código para e aguarda a função terminar e devolver uma promise para ser resolvida/rejeitada, antes de continuar a execução. A palavra `await` **só pode** ser utilizada em uma função declarada como `async`.

- tratamento de **exceção** pode ser feito utilizando `try/catch`

### `Promise.all`

Quando temos um **conjunto de promises**, podemos utilizar o método `all()` para **aguardar todas serem resolvidas** e então tratadas.

```js
Promise.all(<conjunto_de_promises>).then(<faz alguma coisa>)
```

```js
/* https://medium.com/trainingcenter/entendendo-promises-de-uma-vez-por-todas-32442ec725c2 */
// Criando uma promise
const p = new Promise((resolve, reject) => {
    try {
        resolve(funcaoX())
    } catch (e) {
        reject(e)
    }
})
                      
// Executando uma promise
p
    .then((parametros) => /* sucesso, resolve a promise */)
    .catch((erro) => /* erro, tratar promise rejeitada */)

// Tratando erros e sucessos no then
p
    .then(resposta => { /* tratar resposta */ }, erro => { /* tratar erro */ })

```

## API (Application Programming Interface)

É um intermediário entre o front-end e o back-end (ou entre o cliente e o servidor); acessada por meio de URLs; também pode ser utilizada por outras APIs.

- muito comum que o resultado de uma API seja um JSON;

- também comum enviar dados para a API no formato JSON;

- `fetch(url, options)`: função para consumir/usar uma API. Retorna uma *promise*.

- boa prática: criar uma constante para a url: BASE_URL

- padrões de API Web: RPC, Soap e REST (o mais utilizado atualmente; verbos GET e POST no caso do Front End).

## D.O.M.

Document Object Model

Document representa a página HTML e por meio dele é possível acessar os diferentes elementos do HTML.

B.O.M.: Browser Object Model: Os navegadores possuem um objeto window, que representa a janela do navegador. O Document (do DOM) é filho de window. Outros filhos: history, location, screen, navigator;

## Local Storage

É um **objeto do navegador** (`localStorage`), que permite que sites **armazenem dados** (strings) **localmente no navegador** do usuário. 

As informações são armazenadas no formato JSON {chave: valor}

- os dados **PERMANECEM** salvos mesmo se a janela do navegador é **fechada**

    - há outro objeto, `sessionStorage`, que também armazena dados, mas que são **apagados ao final da sessão** (quando fecha o navegador, por exemplo)

Exemplo para salvar novas informações:

```js
localStorage.setItem('chave', 'valor');
```

Exemplos para acessar as informações:

```js
localStorage.chave;
localStorage['chave'];
localStorage.getItem('chave');
```

Exemplo para apagar uma informação específica:

```js
localStorage.removeItem('chave');
```

Exemplo para apagar tudo:

```js
localStorage.clear();
```

Diferença entre Local Storage e Cookie:

- **Cookies** devem ser utilizados quando é necessário armazenar dados considerados **sensíveis** (de acordo com a LGPD);

- Cookies parecem ser mais utilizados para interação entre o navegador e o servidor (podem ser enviados no header de requisições);

- já no caso de Local Storage, sua utilização é para **leitura/escrita** de dados **no browser**, **sem acesso pelo servidor**;

- https://blog.shahednasser.com/localstorage-vs-cookies-whats-the-difference