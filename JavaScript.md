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


### Livro

Complementos a essas anotações foram feitas utilizando como referência o livro ["JavaScript: The Definitive Guide"](https://www.oreilly.com/library/view/javascript-the-definitive/9781491952016/), Seventh Edition, escrito por  David Flanagan, editora O'Reilly.

## Introdução

O JavaScript (JS) é **baseado no ECMAScript** (ES), que por sua vez é uma linguagem de programação baseada em scripts padronizada pela Ecma International na especificação ECMA-262.

- JavaScript, ActionScript, JScript seguem os padrões da ECMAScript, e adicionalmente têm seus próprios recursos.

Não tem *nada a ver com Java*. O nome foi uma jogada de *marketing* para se aproveitar da popularidade crescente que Java estava adquirindo na época em que o JavaScript foi lançado (o nome inicial era Mocha, depois mudado para LiveScript e então JavaScript). 

Pode ser utilizada também em dispositivos móveis, wearables (smartwatch, por exemplo), IoT (Alexa e Google Home, por exemplo), games, APIs.

A **versão ES6** foi um marco importante na linguagem, introduzindo novos escopos de variáveis, arrow functions, classes, módulos, etc. Essa versão também é conhecida como **ES2015**, que é o ano de lançamento. 

> A partir do ES6, as versões começaram a ser nomeadas pelo seu ano de lançamento (que é anual). Então temos ES2016, ES2017 e por aí vai.

Um jeito de **rodar arquivos JS puro no terminal é instalar o Node.js**.

Ao importar o arquivo JS para um HTML, é necessário ter **ambas** as tags, de abertura e fechamento, mesmo não havendo nada entre elas:

```js
<script src="main.js"></script>
```

Local da tag script: o *ideal* é colocá-la **antes da tag de fechamento do body**, pois assim garante que todo o conteúdo já foi renderizado (no caso de seu script trabalhar com algum conteúdo que precisa ser renderizado antes).

- Quote da Diane Martine (Avanade): "Primeiro precisamos ter a estrutura do prédio, para depois incluir a parte funcional";

- Caso seu script não dependa da renderização do HTML ou você precise que o **JS faça alguma coisa *antes* de a página renderizar**, pode ser colocado **dentro da tag head**.

Convenção de nome: utilizar o nome main.js para seu script principal.

Frameworks e bibliotecas: oferecem facilidades para programar em JS, economizando tempo e facilitando algumas atividades. O React é considerado uma *biblioteca*.

## Pontos de atenção da sintaxe

Uso do ponto e vírgula: é opcional, mas é **altamente recomendável** utilizá-lo, para evitar dores de cabeça. Exemplo:

```js
let y = x + f
(a+b).toString()
```

Por conta do uso do parênteses no início da segunda linha, o JS irá interpretar esse código como sendo a execução de uma função `f`:

```js
let y = x + f(a+b).toString()
```

---

Quebra de linha após `return`: o JS irá interpretar como sendo o fim da sentença.

```js
return
true;
```

Será interpretado como

```js
return;
true;
```

Se precisar quebrar a linha no return, **use parênteses**. Isso é muito comum quando se trabalha com React.

O mesmo vale para as palavras reservadas `throw`, `yield`, `break` e `continue`.

## Tipos e valores

Tipos são divididos em duas categorias: primitivos e objetos.

| tipo | mutabilidade | valores | 
| - | - | - |
| primitivo | imutável | números, string, boolean, null, undefined, symbol |
| objeto | mutável | qualquer valor que não seja um dos primitivos |

Variações especiais de objetos (possuem uma sintaxe especial ou operações visando melhor performance): array, Set, Map, *typed array*, RegExp, Date, Error, funções e classes.

Imutabilidade: significa que o valor não muda. O número 2 vai ser sempre 2, o valor false será sempre false.

- Observe que **string** é tipo primitivo, portanto, **imutável** em JavaScript. Métodos como `toUpperCase` retornam uma nova string ao invés de modificar a original.

> **Não declaramos tipos** para as variáveis/constantes (no jargão de dev: "não tipamos"). A conversão é feita pelo JavaScript de acordo com o valor atribuído.

### Texto

Para declarar textos (strings), podemos utilizar aspas simples, duplas ou crase (backtick). O backtick foi adicionado pelo ES6 e permite interpolação de texto com expressões JS.

> Quando usado o backtick, denominamos esse valor de *template literal*

Caracteres de escape são adicionados por meio da barra invertida (`\`). Por exemplo, para incluir uma quebra de linha no texto, podemos utilizar o `\n`; para adicionar aspas simples em um texto declarado com aspas simples, utilizamos `\'`, etc.

### Booleano

Qualquer valor em JS pode ser convertido para booleano, o que pode ser chamado de valores *truthy* ou *falsy*. 

**Seis valores** são considerados ***falsy***:

- `undefined`;

- `null`;

- `0`;

- `-0`;

- `NaN`;

- `""` (string vazia).

Isso ajuda a simplificar expressões condicionais, por exemplo.

### null e undefined

São valores primitivos que representam **ausência de valor**. 

O `undefined` é o "valor" retornado nos seguintes casos:

- variáveis não inicializadas;

- propriedade que não existe em um objeto;

- elemento que não existe em um array;

- retorno de função que não retorna nada;

- valor de parâmetros de uma função quando o argumento não é passado.

### Symbol

É um tipo introduzido pelo ES6 para poder atribuir nomes a propriedades de objetos sem utilizar strings, de modo a garantir que uma propriedade não seja acidentamente sobrescrita. 

Um valor de símbolo é gerado pela função `Symbol()`, que aceita uma string como parâmetro opcional. O valor retornado é garantido que seja único, mesmo se você passar o mesmo parâmetro em uma segunda chamada. Ou seja, **a função `Symbol()` nunca retorna o mesmo valor duas vezes**.

No exemplo a seguir, criamos duas novas propriedades utilizando a função `Symbol('novaProp')`. Observe que elas não se sobrescrevem, mesmo tendo sido geradas da mesma forma:

```js
let obj1 = {};
const propA = Symbol('novaProp');
const propB = Symbol('novaProp');
obj1[propA] = 162;
obj1[propB] = null;

obj1[propA]; // 162
obj1[propB]; // null

propA == propB; // false
propA === propB; // false
propA == propA; // true
propA === propA; // true

// mas o valor convertido para string é igual
propA.toString(); // 'Symbol(novaProp)'
propB.toString(); // 'Symbol(novaProp)'
propA.toString() === propB.toString(); // true

// o valor de símbolo não é uma string
obj1['Symbol(novaProp)']; // undefined
propA === 'Symbol(novaProp)'; // false
propA.toString() === 'Symbol(novaProp)'; // true
```

Já a função `Symbol.for()` é o oposto. Ela garante que irá retornar o **mesmo valor** de símbolo quando chamada com a mesma string.

```js
const symA = Symbol.for('somosIguais');
const symB = Symbol.for('somosIguais');
symA === symB; // true
```

### Operadores de incremento/decremento

Os operadores para incrementar (`++`) ou diminuir (`--`) um valor tem um comportamento diferente dependendo da posição relativa ao operando. 

- Se vier do **lado esquerdo** do operando (`++i`, por exemplo), o valor é incrementado e **retorna o valor incrementado**;

- Se vier do **lado direito** do operando (`i++`), o valor é incrementado, mas **retorna o valor *não* incrementado**.

As mesmas regras se aplicam para decremento.

Exemplo:

```js
let i, j;
i = 1;
j = i++; // i=2, mas j=1
j = ++i; // i=3 e j=3 também
```

## global object

O JS tem um objeto global que provê uma série de propriedades disponíveis para qualquer programa JS.

No Node, esse objeto é acessado usando `global`. 

No navegador, é acessado usando `window`.

O **ES2020** define o `globalThis` como o nome padrão de acesso ao objeto global, em substituição ao `global` e ao `window`.

## Conversões úteis

Para fazermos conversões explícitas, podemos utilizar as funções (observe a inicial maiúscula) `Boolean()`, `Number()` e `String()`. 

Podemos converter qualquer valor para string (exceto `null` e `undefined`) usando o método `toString()`.

Para números, também temos as funções globais `parseInt()` e `parseFloat()`.

Atalhos para conversão para número ou string:

```js
+'2'; // a string será convertida para número
3 + ''; // o valor número 3 será convertido para string '3'
```

## Variáveis e escopo

**Anterior** ao ES6: declara usando `var`.

A partir do **ES6**: declara usando `let` e `const`. Ainda é possível usar `var` por questões de compatibilidade. O uso de **`const`** é para declaração de um **valor que não irá mudar**. Se não for o caso, use `let`.

Podem começar com letra, underline (`_`) ou dólar (`$`). **Não** podem começar com **número**.

```js
let variavel123; // ok
let 123variavel; // SyntaxError
```

> Embora a linguagem aceite qualquer caracter Unicode como identificador, a convenção é se ater a caracteres ASCII (ou seja, sem acentos ou letras de outros alfabetos)

`var`: escopo **global e local** (local quando declarada dentro de uma função, global nos outros casos); é uma maneira antiga de declarar uma variável. Quando global, se torna **propriedade do [global object](#global-object)**.

`let`: escopo **local de BLOCO** (não será vista pelo código após o bloco)

- bloco é qualquer coisa entre chaves `{}` ou em loops; 

- boa prática: dê preferência ao `let` ao invés de `var`, tanto para se adequar às versões modernas de JS quanto por questões de segurança, já que `var` é parte do global object e pode ser manipulada via console. O `let` também pode ser manipulado via console, dependendo de onde for declarado. No entanto, devido a seu escopo local de **bloco**, oferece mais proteção do que `var`;

- dentro de uma função, `var` é vista por *todos* na função, *inclusive* se for declarada em *blocos internos* (dentro de um if, por exemplo). Já `let`, se declarada dentro de um bloco `if` dentro da função, será vista *somente pelo bloco* `if`, e não pela função inteira.

`const`: uma prática comum é declarar constantes em SNAKE_UPPER_CASE, para diferenciá-las de variáveis.

- diferente das variáveis, `const` **precisa ser inicializada com algum valor**, senão irá subir uma exceção de `TypeError`;

- também possui **escopo local de BLOCO**.

> Quando `let` ou `const` são declaradas **antes** de qualquer bloco de código, seu escopo é **global** (mas não são propriedades do global object). No Node, o escopo global é o arquivo em que a variável foi definida. No navegador, o escopo é o **documento HTML**, o que significa que **outros scripts têm acesso a ela**, caso sejam executados após o script que definiu a variável. 

Boa prática para `var`, `let` e `const`: faça a declaração *no topo do bloco de código*, para auxiliar o "hoisting" do interpretador a elevá-las até o topo desde o início.

- no caso de `let` e `const`, o interpretador faz o hoisting, mas as variáveis não são acessíveis enquanto não forem declaradas; tentar utilizá-las antes da declaração, causará um erro;

- no caso de `var`, ela é inicializada como `undefined` e **pode ser acessada antes da declaração**.

### Hoisting

Hoisting é uma etapa em que o interpretador **coloca na memória** as declarações de funções, variáveis ou classes, *antes de executar o código*. É como se ele colocasse no topo do arquivo essas declarações. 

Funciona bem para funções (é por isso que você pode usá-las antes de declará-las), mas pode causar erros com variáveis e classes (o hoisting eleva somente as **DECLARAÇÕES** e **não as inicializações**; em casos específicos elas são inicializadas com seu valor padrão, em outros casos não são nem inicializadas).

### Destructuring assignment

O ES6 possibilita inicializar uma ou mais variáveis baseada em valores vindos de um **array ou objeto**. Isso é chamado de `destructuring assignment`, pois é como se você estivesse abrindo/desestruturando aquele array ou objeto e colocando seus valores nas variáveis. Exemplos:

```js
// -- Arrays
let [x, y] = [1, 7]; // x=1 e y=7
[x, y] = [1, 7, 11]; // x=1, y=7 e o valor 11 é ignorado
[x, y] = [1]; // x=1 e y=undefined
[x, , y] = [1, 7, 11]; // x=1 e y=11; adicionamos uma vírgula para ignorar o valor 7

// use ... para indicar que o resto dos valores 
// devem ir para a próxima variável
[x, ...y] = [1, 7, 11, 3, 14]; // x=1 e y=[7, 11, 3, 14],

// se tivéssemos uma função que retorna um array 
// com dois números, poderíamos também utilizar 
// o destructuring assigment
let [x1, y1] = getCoordinates(); 

// -- Objetos
let transparent = {
    r: 0.0, 
    g: 0.0, 
    b: 0.0, 
    a: 1.0
};
// use o mesmo nome das propriedades que deseja extrair
let {r, b} = transparent; // r=0.0 e b=0.0

// pode também atribuir um nome diferente
{r: red, b: blue} = transparent; // red=0.0 e blue=0.0,
```

## Comparação

O operador `==` irá comparar os *valores*. O interpretador irá tentar descobrir o "valor primitivo" entre os dois valores/variáveis que estão sendo comparados, por isso às vezes o retorno pode trazer alguma surpresa (`"0" == 0` resulta em `true`, por exemplo, pois é feita uma conversão de string para number e então a comparação).

Já o operador `===` efetua uma comparação idêntica (*strictly equality operator*): tanto o **valor** quanto o **tipo** têm que ser iguais. Esse é o operador recomendado para casos de comparação de valores.

Comparadores lógicos: `&&,` `||` e `!`.

Pontos de atenção:

- `Infinity` será sempre maior do que qualquer número e `-Infinity` será sempre menor do que qualquer outro número;

- Comparação de strings é `case-sensitive` (`A` é diferente de `a`) e a comparação é feita pela ordem númerica do valor Unicode de 16-bits de cada caracter. Caracteres minúsculos, neste caso, têm um valor **maior** que os maiúsculos (`'a' > 'Z'` retorna `true`). 

### Comparação em objetos

Objetos não são comparados por valor, mas sim **por referência**. Os objetos são uma referência a uma posição da memória e, por conta disso, quando comparamos dois objetos que possuem as **mesmas propriedades e os mesmos valores** nestas propriedades, eles serão **diferentes**, pois estão referenciando diferentes regiões de memória.

```js
let objA = { x: 10 };
let objB = { x: 10 };
objA == objB; // false
objA === objB; // false

// mas os VALORES nas propriedades são iguais, 
// pois são primitivos
objA.x == objB.x; // true
objA.x === objB.x; // true
```

Desse modo, para comparar se dois objetos são iguais, devemos manualmente comparar se suas propriedades são iguais e se os valores delas são iguais. 

> É por isso que, ao declararmos uma variável e atribuir a ela um objeto que está em outra variável, **não** estamos copiando aquele objeto, e sim **atribuindo a referência** ao mesmo objeto. Por conta disso, qualquer modificação em uma variável irá ser refletida na outra.

```js
let a = { x: 10 };
let b = a; // estamos copiando a REFERÊNCIA
b === a; // true, pois referenciam o mesmo obj

a.x; // 10
b.x; // 10
b.x = 15;

b.x; // 15
a.x; // também 15, pois referenciam o mesmo obj
```

Essas mesmas observações se **aplicam para arrays** e seus elementos (lembrando que arrays são uma variação especial de objeto).

### Curto circuito

O **operador `&&`** pode ser utilizado para "avaliações de curto-circuito" (*short-circuit*) em expressões contendo valores truthy ou falsy (relembre sobre isso na [Seção sobre booleanos](#booleano)) 

- caso o valor da **esquerda** seja falsy, `&&` **retorna esse valor e não avalia o que vier depois**;

- Caso o valor da esquerda seja truthy, o que vier **à direita será avaliado e este valor será retornado pelo `&&`**.

Isso é interessante, por exemplo, quando você quer condicionar a execução de uma função baseado em uma expressão:

```js
// se a === b for falso, minhaFunc 
// NÃO será executada
(a === b) && minhaFunc(); 

// é o mesmo que if (a === b) minhaFunc(); 
```

> React utiliza isso para renderizar dinamicamente partes do componente baseado em algum estado.

---

A avaliação de curto-circuito também acontece para o **operador `||`**:

- se o valor à esquerda é **truthy**, **retorna este valor** e não avalia o restante;

- se o valor à esquerda é **falsy**, **avalia o valor à direita e retorna este valor**;

- caso seja uma expressão que contenha mais de um operador `||`, vai aplicando esta regra até chegar ao último operando.

Isso é útil, por exemplo, para definir um valor padrão a uma variável, caso um valor atribuído a ela possa ser falsy (chamado de *fallback value*, ou valor reserva). 

No exemplo abaixo, se a função `getWidth()` retornar um valor truthy, esse será o valor de `num`; caso seja falsy (`undefined`, por exemplo), `getHeight()` será executado e, se truthy, irá atribuir esse valor a `num`; se falsy, será atribuído o valor `50`, que é truthy.

```js
let num = getWidth() || getHeight() || 50;
```

---

Por fim, também temos o **operador `??`** para avaliações curto-circuito. Ele foi incluído pelo ES2020 e é chamado de *nullish coalescing operator*. Seu funcionamento é parecido com o do `||`, porém restrito a **valores "nullish", ou seja, `null` ou `undefined`**. Por exemplo, `0` e `""` (string vazia) são falsy, mas não nullish, então são considerados como válidos quando usados com `??`.

```js
let valido = null || 10 // valido é 10
valido = null ?? 10     // valido é 10
valido = 0 || 10;       // valido é 10
valido = 0 ?? 10;       // valido é 0!!
```


## Arrays

Em JavaScript, dentro de um mesmo array pode haver elementos de tipos **diferentes**, inclusive objetos e outros arrays.

- para acessar elementos: coloque o índice entre colchetes: `arr[2]` irá acessar o **terceiro** (o índice começa em zero) elemento do array `arr`.

    - caso o array **não exista**, o acesso ao elemento irá resultar em um `ReferenceError`; caso seja atribuído `null` ou `undefined` (por exemplo, quando recebe o resultado de uma função), o acesso irá resultar em um `TypeError`;

        - o ES2020 adicionou a possibilidade de acesso com `?.[]` (sim, com o ponto no meio) para evitar o erro quando a variável é `null` ou `undefined`. Será retornado `undefined`.

    - caso acesse um índice que não tem valor, o resultado será `undefined`.

- para adicionar itens: `push(novo_item)` (adiciona ao **final**) e `unshift(novo_item)` (adiciona ao **início**);

- remoção: `pop()` (**último** item), `shift()` (**primeiro** item);

- `splice(indice, quantos, item_1...item_n)`: remove a partir do `indice`, `quantos` itens você informar (`quantos` é opcional). Se informar outros itens (também opcional), splice irá *ADICIONAR* os itens a partir do `indice` e, caso `quantos` for informado, irá antes remover os itens a partir de `indice` para depois adicionar os novos itens. O método retorna um **novo array**;

- `slice(inicial, final)`: faz um recorte; cria um novo array com os itens do array em que o método foi invocado, começando a partir do índice `inicial` (inclusivo) até o índice `final` (exclusivo, ou seja, não será incluído no novo array). 
    - o segundo parâmetro é opcional; caso não seja utilizado, o recorte é feito até o último elemento do array (inclusivo, neste caso);
    - necessário tomar cuidado quando o array possui objetos, pois o `slice()` faz uma ["shallow copy"](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) do array.

- `foreach(function(element, index, arr), thisValue)`: método que executa uma função; `function` e `element` são obrigatórios, os outros são opcionais. A função pode ser definida dentro ou fora do método (se fora do método, somente faz a chamada da função, sem argumentos);

    - também pode ser mais direto com uma arrow function: `forEach((element, index, arr) => {...})`, `index` e `arr` são opcionais

- reticências (`...`) antes de um array (`...arr`) pode indicar um **spread** ou um **rest**:
    - Spread operator: acontece quando você quer "desempacotar" ou desmembrar os elementos de um array. Você pode usar para copiar os elementos de um array para outro array, sem precisar passar os valores um por um.
    - Rest parameter: é o oposto do spread, ajuntando elementos em um array. Pode ser usado na **definição de uma função**, quando você não sabe a quantidade de parâmetros que ela vai ter (*rest* vem de "resto"). Você aplica o rest parameter como o **último** parâmetro da função, *agrupando* em um array todos os argumentos que vierem a mais quando a função é invocaada. 

        ```js
        function foo(paramA, paramB, ...otherParams); 
        // tudo que vier a partir do 3º parâmetro será agrupado em um array aqui definido como otherParams, que pode ser usado dentro da função
        ```

## Condicionais e loops

JavaScript possui o **if ternário**: `[condicao] ? [instrução se true] : [instrução se false];`

Laço de repetição: **três tipos de for**: `for`, `for/in` e `for/of` (o `for/of` foi introduzido pelo ES6). Veja a diferença no exemplo:

```js
const valores = [0, 1, 2, 'três'];

// i como índice; é o for clássico
for(let i = 0; i < valores.length; i++){
    console.log(valores[i]); // 0, 1, 2, 'três'
}

// idx como nome de uma PROPRIEDADE de objeto
// - índices são propriedades de um array
// - array é "objeto especializado"
let idx;
for(idx in valores){ // poderia fazer o let idx aqui
    console.log(idx); // índices 0, 1, 2, 3
}

// val como VALOR, percorrendo objetos ITERÁVEIS,
// como array, strings, set, etc
let val;
for(val of valores){ // poderia fazer o let val aqui
    console.log(val); // valores 0, 1, 2, 'três'
}
```

Há também uma variante do `for/of`: a `for/await`, introduzida pelo ES2018 para iteradores assíncronos. Exemplo retirado do livro ["JavaScript: The Definitive Guide"](https://www.oreilly.com/library/view/javascript-the-definitive/9781491952016/):

```js
// Read chunks from an asynchronously iterable stream
// and print them out
async function printStream(stream) {
    for await (let chunk of stream) {
        console.log(chunk);
    }
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

    - parâmetros podem ter valor padrão (estilo Python); isso foi implementado a partir do ES6;

    - o objeto `arguments` pode ser acessado dentro da função e traz, dentro de um array, todos os argumentos que a função recebeu;

    - se a função não tiver um `return`, ao ser chamada ela irá retornar `undefined`.

Quando a função é **atribuída a uma variável** (chamado de "função de expressão"), o nome da função é opcional, e torna-se o que se chama no JavaScript de "função anônima":

```js
var soma = function() { 
    return 1 + 2; 
}; 
// para chamar a função, use soma();
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

- O W3Schools possui bons artigos explicando [call](https://www.w3schools.com/js/js_function_call.asp), [apply](https://www.w3schools.com/js/js_function_apply.asp) e [bind](https://www.w3schools.com/js/js_function_bind.asp), incluindo exemplos que podem ser testados online.

### Arrow function 

São anônimas e também podem ser atribuídas a uma variável
   
```js
var soma = () => { 
    return 1 + 2; 
};
```

- os argumentos e a "flecha" (`() =>`) precisam estar na **mesma linha**;

- tem muito mais detalhes; ver a documentação: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#syntax

- arrow function **não possui** o objeto `arguments`;

- `call()`, `apply()` e `bind()` **não irão** funcionar;

- **não possui** this -> ela **herda automaticamente o contexto** de onde foi criada; 

- arrow function e funções anônimas não são "hoisted", ou seja, são interpretadas no momento da sua execução, não podendo ser utilizadas antes de serem declaradas;

- é **melhor usar const** ao atribuir uma arrow function a uma variável, já que o retorno dessa função é um valor constante;

## Objetos 

Declarados entre `{}` possuem **propriedades** e **métodos**.

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

    - o acesso pelo `.` não funciona se a propriedade tiver espaços, pontuação, ou se for um número ou uma expressão. Nestes casos, opte pelo `[]`;

    - se a propriedade não existir, será retornado `undefined`;

    - você também pode acessar subpropriedadades. Exemplo: `objeto.objetoInterno.textoInterno`;

    - assim como em arrays, caso o objeto **não exista**, o acesso a uma propriedade resulta em um `ReferenceError`; caso seja atribuído `null` ou `undefined` ao objeto (por exemplo, quando recebe o resultado de uma função), o acesso irá resultar em um `TypeError`. Se quiser evitar isso, utilize o `?.` ou `?.[]` para acessar; neste caso, será retornado `undefined` se o objeto for `null` ou `undefined`. O mesmo vale para acesso a subpropriedades.

    - caso acesse um índice que não tem valor, o resultado será `undefined`.

- métodos também são acessados pelo ponto + parênteses, para a função ser executada: `objeto.ola()`;

- posso selecionar mais de uma propriedade de uma vez: `var { texto, valido } = objeto`;

- posso adicionar novas propriedades: `objeto.novaPropriedade = 'sou uma nova propriedade'`;

- posso deletar propriedades com o comando `delete`; ao tentar acessá-las após remoção, irá retornar `undefined`: `delete objeto.novaPropriedade`

- posso também inicializar um objeto vazio: `let novoObj = {}`;

- `Object.values(objeto)`: retorna um array com todos os **valores**;

- `Object.keys(objeto)`: retorna um array todas as **chaves** (nome das propriedades);

- `Object.entries(objeto)`): retorna um array de arrays bidimensionais, sendo que cada cada elemento é uma [chave, valor];

- O operador `in` pode ser usado para verificar se um objeto possui uma propriedade (`'numero' in objeto` retornaria `true`).

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

Surgiram a partir do ES6. 

Não existem nativamente no JS, mas podem ser criadas como forma de facilitar a escrita/entendimento (o tal do *syntax sugar*). Por trás dos panos, o que há são objetos, que herdam métodos e propriedades de protótipos.

É possível declarar e inicializar propriedades dentro do constructor, sem a necessidade de declará-las fora. As propriedades declaradas e inicializadas no constructor ficam visíveis para o restante da classe.

- Se o construtor da classe não recebe argumentos, você pode chamá-la sem o parênteses (`new SuaClasse` ou `new SuaClasse()`). Isso, no entanto, não é recomendado;

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

Exemplo de composição gerado pelo ChatGPT

```javascript
// Classe que será composta
class Motor {
  constructor() {
    this.velocidade = 0;
  }

  // Método da classe Motor
  acelerar(incr) {
    this.velocidade += incr;
  }
}

// Classe que contém uma instância de Motor (composição)
class Carro {
  constructor() {
    this.motor = new Motor(); 
    // Carro agora tem uma propriedade "motor", 
    // que é um objeto da classe Motor
  }
}

// Utilizando a composição para acessar métodos do objeto Motor
const meuCarro = new Carro();
meuCarro.motor.acelerar(50);
console.log(`Velocidade atual: ${meuCarro.motor.velocidade}`); // Saída: Velocidade atual: 50

```

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

Essas propriedades/métodos pertencem **à classe**, e não a uma instância da classe. Assim, quando uma instância é criada, estas propriedades/métodos **não** são acessíveis quando chamados a partir da instância. Por outro lado, você pode acessar estas propriedades/métodos diretamente da classe, sem a necessidade da criação de uma instância (um exemplo: `Date.now()`). 

O acesso a propriedade e métodos estáticos pode ser feito de duas maneiras: 

1. diretamente da classe, sem a necessidade da criação de uma instância. 

2. por uma instância da classe, utilizando a propriedade `constructor` (o `constructor` nesse caso permite o acesso à classe).

```js
// acesso pela classe
console.log(Quadrado.tipo); // quadrado

// acesso usando constructor
const meuQuadrado = new Quadrado();
console.log(meuQuadrado.constructor.tipo); // quadrado
console.log(meuQuadrado.constructor.calculaPerimetro(4)); // 16

// o acesso direto à propriedade estática da classe pela instância irá retornar undefined
console.log(meuQuadrado.tipo); 

// já o acesso ao método irá dar erro
console.log(meuQuadrado.calculaPerimetro(4));
```

**Observação**: como a instância não "conhece" as propriedades e métodos da classe, você pode definir propriedades/métodos de mesmo nome para a instância. Isso no entanto, pode dificultar a leitura/entendimento do código.

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

Vale uma observação: no DevTools do navegador ou no Node, é possível que você consiga acessar propriedades e métodos privados. Isso é um "relaxamento" da própria ferramenta para auxílio dos desenvolvedores.

### Polimorfismos

No JS pode haver **somente sobrescrita** de métodos (overriding), ou seja, os métodos nas subclasses podem ter um comportamento diferente da superclasse, mas a **assinatura do método não pode mudar** (não pode conter novos argumentos ou remover argumentos). Ou seja, a sobrecarga (overload) não é possível em JS.
 
## Erros

- `throw`: usado para **criar seu próprio erro**; pode substituir um return na função, jogando um erro que pode ser capturado e tratado. Se não for tratado na função, o erro "propaga" pela stack até encontrar um bloco que trata erros. Se nenhum for encontrado, o programa retorna um erro ao usuário. O erro dado pelo `throw` pode ser uma string, um número, ou uma instância da classe `Error`.

- `try...catch`: captura e manipulação de um erro; coloca o código dentro do try e captura/manipula o erro com o catch; O `catch` também pode jogar um erro com `throw`, para ser tratado por quem chamou a função, propagando o erro para cima.

- `finally`: bloco de código que **será executado**, independente de existir ou não um erro pego no try...catch, ou até se houver um return ou um break no try. É opcional. Pode ser usado, por exemplo, para fazer alguma "limpeza" no código.

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

Mais detalhes:

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
Promise.all(/* conjunto_de_promises */).then(/* faz alguma coisa */)
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

É um intermediário entre o Front End e o Back End  (ou entre o cliente e o servidor); acessada por meio de URLs; também pode ser utilizada por outras APIs.

- muito comum que o resultado de uma API seja um JSON;

- também comum enviar dados para a API no formato JSON;

- `fetch(url, options)`: função para consumir/usar uma API. Retorna uma *promise*.

- boa prática: criar uma constante para a url: BASE_URL

- padrões de API Web: RPC, Soap e REST. REST é o mais utilizado atualmente; verbos GET e POST no caso do Front End.

## DOM

Document Object Model.

Document representa a página HTML e por meio dele é possível acessar os diferentes elementos do HTML.

B.O.M.: Browser Object Model: Os navegadores possuem um objeto window, que representa a janela do navegador. O Document (do DOM) é filho de window. Outros filhos: history, location, screen, navigator;

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


# Continuar em

5.6
pag. 237