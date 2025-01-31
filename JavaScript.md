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

> 👉 A W3Schools possui uma seção que aborda os **recursos (features) introduzidos em cada versão** do ECMAScript. É uma referência importante para saber o que há de disponível por versão, caso você trabalhe em projetos antigos. [Clique aqui para ver](https://www.w3schools.com/js/js_versions.asp).

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

- Observe que **string** é tipo primitivo, portanto, **imutável** em JavaScript. Métodos como `toUpperCase` retornam uma **nova string** ao invés de modificar a original.

> **Não declaramos tipos** para as variáveis/constantes (no jargão de dev: "não tipamos"). A conversão é feita pelo JavaScript de acordo com o valor atribuído.

### Texto

Para declarar textos (strings), podemos utilizar aspas simples, duplas ou crase (backtick). O backtick foi adicionado pelo ES6 e permite interpolação de texto com expressões JS.

> Quando usado o backtick, denominamos esse valor de [*template literal* ou *template string*](#template-strings).

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

No **Node**, esse objeto é acessado usando **`global`**. 

No **navegador**, é acessado usando **`window`**.

O **ES2020** define o **`globalThis`** como o nome **padrão** de acesso ao objeto global, em substituição ao `global` e ao `window`.

## Conversões úteis

Para fazermos conversões explícitas, podemos utilizar as funções (observe a inicial maiúscula) `Boolean()`, `Number()` e `String()`. 

Podemos converter qualquer valor para string (exceto `null` e `undefined`) usando o método `toString()`.

Para números, também temos as funções globais `parseInt()` e `parseFloat()`.

Atalhos para conversão para número ou string:

```js
+'2'; // a string será convertida para número
3 + ''; // o número 3 será convertido para string '3'
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

- no caso de `let` e `const`, o interpretador faz o hoisting, mas as variáveis **não** são acessíveis enquanto não forem declaradas; tentar utilizá-las antes da declaração, causará um erro;

- no caso de `var`, ela é inicializada como `undefined` e **pode ser acessada antes da declaração**.

### Hoisting

Hoisting é uma etapa em que o interpretador **coloca na memória** as declarações de funções e variáveis, *antes de executar o código*. É como se ele colocasse no topo do arquivo essas declarações. 

Funciona bem para funções (é por isso que você pode usá-las antes de declará-las), mas pode causar erros com variáveis e classes (o hoisting eleva somente as **DECLARAÇÕES** e **não as inicializações**; em casos específicos elas são inicializadas com seu valor padrão, em outros casos não são nem inicializadas).

### Destructuring assignment

O ES6 possibilita inicializar uma ou mais variáveis baseada em valores vindos de um **array ou objeto**. Isso é chamado de *destructuring assignment*, pois é como se você estivesse abrindo/desestruturando aquele array ou objeto e colocando seus valores nas variáveis. Exemplos:

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

// pode também atribuir um nome diferente usando :
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

Objetos não são comparados por valor, mas sim **por referência**. Os objetos são uma referência a uma posição da memória e, por conta disso, quando criamos dois objetos que possuem as **mesmas propriedades e os mesmos valores** nestas propriedades, ao compará-los eles serão **diferentes**, pois estão referenciando diferentes regiões de memória.

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

Essas mesmas observações se **aplicam para arrays** e seus elementos (lembrando que arrays são uma forma especializada de objeto, otimizado para o acesso a índices numéricos).

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

Usada com valores entre duas crases (\`\`), podendo **juntar** strings com expressões e variáveis (chamadas com `${nome_variável}`), sem necessidade de usar alguma função para concatenar.

```js
console.log(`O resultado de ${num1} + ${num2} é ${resultado}`);
```

- se pular linha, a quebra de linha **também** será incluída na saída.

## Funções

Seguem abaixo algumas definições gerais.

Funções são blocos de código que podem ser executados quantas vezes quiser. 

👉 **Executar** uma função, **invocar** uma função, **chamar** uma função, **rodar** uma função... são todos **sinônimos** e significa executar o bloco código definido na função.

Quando uma função é definida em um **objeto como propriedade**, ela ganha o nome de **método**. Quando uma função é definida para **inicializar um novo objeto**, ela é chamada de **construtora**.

> Leia mais sobre [Objetos](#objetos) e [Classes](#classes).

Quando passamos uma **função `A` como argumento para outra função `B`** (ouviremos bastante sobre isso quando tratamos sobre JS assíncrono), e **`A` é invocada** em algum momento durante a execução de `B`, podemos chamar `A` de **callback**. Se a função `A` é definida fora de `B`, passamos *somente o nome da função, sem os ()* (ex: `B(A, arg2, ...)`), para evitar que `A` seja imediatamente executada no momento da chamada de `B`. Se `A` precisa de parâmetros, podemos usar uma [arrow function](#arrow-function) como "wrapper" para evitar executá-la (ex: `B(() => A(argX, argY, ...), arg2, ...)`).

Lembre-se que durante o [hoisting](#hoisting), funções são "elevadas" para o topo do bloco de código em que se encontram, e por isso **podem ser invocadas antes de serem declaradas** (com **exceção** das expressões de função e arrow functions, explicadas mais pra frente).

**Curiosidade:** funções são tipos especializados de objetos, então elas **podem ter propriedades definidas dentro delas**. Isso possibilita armazenar valores dentro da função que se mantenham, mesmo após outra invocação (algo que não é possível com uma variável local). Da mesma forma, essas propriedades podem ser criadas, acessadas e manipuladas via código. Por exemplo, podemos criar uma propriedade de "cache" que armazena resultados de chamadas anteriores da função, e utilizar esse cache para retornar um resultado ao invés de computá-lo novamente - no entanto, como as propriedades podem ser acessadas e modificadas, há outra solução para encapsulamento de dados em funções: por meio das chadamas ["closures"](#closure).

### Declaração

Esqueleto de uma função:

```js
function nome(param_a, param_b, ...) {
	// código da função
	return algum_retorno;
}
```

Parâmetros e retorno são **opcionais**:

- **parâmetro** é o nome dado quando você **DECLARA** a função; **argumento** é o nome dado quando você **INVOCA** a função;
    
    - "A função recebe como parâmetro..."
    
    - "Você passa à função como argumento...";

- a partir do ES6, parâmetros podem ter valor padrão (estilo Python);

- o objeto `arguments` pode ser acessado dentro da função e traz, na forma de um array-like, todos os valores dos argumentos que a função recebeu (ou seja, você acessa os valores pela posição, e não pelo nome do argumento). Essa é uma abordagem antiga e, a partir do ES6, você pode ao invés disso optar pelo *rest parameters* (ver abaixo);

- os parâmetros se comportam como variáveis locais dentro do corpo da função;

- se a função não tiver um `return`, ao ser chamada ela irá retornar `undefined`.

Quando uma função é invocada com um número de argumentos **menor** do que o número de parâmetros esperados, os parâmetros  que não receberam valor serão `undefined`.

Quando uma função é invocada com um número de argumentos **maior** do que o número de parâmetros esperados, os parâmetros adicionais podem ser acessados pelo objeto `arguments` explicado acima. Se você não sabe quantos parâmetros sua função espera receber, pode usar o chamado ***"rest parameters"*** (ES6), isto é, definir um parâmetro **no final**, precedido de `...`: `function soma(val1, val2, ...outrosVal)`. Este parâmetro é **um array que contém os outros parâmetros recebidos**.

- cuidado para não confundir o [rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) (que "ajunta" os parâmetros em um array) com o [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) (que "desempacota" os valores de um array). O primeiro pode ser útil na definição de uma função, enquanto o segundo pode ser útil na invocação de uma função.

### Expressões de função e função anônima

Quando a função é **atribuída a uma variável** ou como **argumento de outra função**, a chamamos de "expressão de função" - *function expression*. O nome da função é opcional (quando omitido, também podemos chamar de "função anônima"):

```js
const soma = function() { // nome é opcional
    return 1 + 2; 
}; 
// para chamar a função, use soma();
```

Uma boa prática é declarar a variável com `const`, para evitar sobrescrevê-la acidentalmente com outro valor.

> O **hoisting não ocorre** em expressões de função, ou seja, não podem ser usadas antes de serem definidas.

### IIFE

Uma função pode ser **autoinvocável** (IIFE: Immediately Invoked Function Expression): declare-a (anônima ou não) dentro de parênteses, e logo após invoque-a utilizando `()`, incluindo os argumentos, se houver;

```js
(function() {
    console.log('Autoinvocável');
})();
```

### Funções e o `this`

No JS, o `this` é uma referência ao contexto de execução de um bloco código. Quando uma função é invocada, o valor do `this` é:

- o objeto global (modo não restrito - non-strict mode);

- undefined (modo restrito - strict mode).

No entanto, temos **exceções**:

- [**Arrow function**](#arrow-function): herda o **`this` do escopo** em que foi definida;

- **Métodos**: acessam o **`this` do objeto** em que foram definidas. Conceito de programação orientada a objetos.  

    - entretanto: funções aninhadas **não** herdam o `this` da sua função pai; então, se um método possui uma função aninhada, esta não terá acesso ao `this` do objeto, mas sim ao `this` do objeto global ou undefined (modos não restrito e restrito, respectivamente).

#### Métodos `bind()`, `call()` e `apply()`

Uma maneira de **passar um `this` específico** para uma função é por meio do métodos `bind()`, `call()` e `apply()`, vindos do prototype de `Function`.

Os métodos `call()` e `apply()` fazem uma **"invocação indireta"** de uma função: você usa o `call`/`apply` para invocar a função e passar o `this` para ela. A diferença entre os dois é na forma como você passa os argumentos para eles:

- `call()`: o primeiro parâmetro é o `this` que será utilizado como contexto (por exemplo, um objeto); os **parâmetros seguintes** são os argumentos a serem passados como parâmetros para a função invocada;

- `apply()`: o primeiro parâmetro é o `this` e o **segundo parâmetro é um array** com os argumentos a serem passados como parâmetro para a função invocada. A vantagem de usar um array é quando a função a ser invocada recebe uma quantidade indefinida de argumentos.

```js
// suponha que obj é um objeto.
// nomeDaFuncao poderá utilizar o this dentro 
// dela e isso fará referência às propriedades 
// de obj
nomeDaFuncao.call(obj, param1, param2, ..., paramN);
nomeDaFuncao.apply(obj, [params]);
```

Exemplo prático:

```js
function digaOla(frase) {
    // acesso a um objeto com a propriedade nome
    return `${frase}, ${this.nome}`;
}

const matheus = {
    nome: 'Matheus'
}

const alana = {
    nome: 'Alana'
}

digaOla.call(matheus, 'Bem-vindo'); // "Bem-vindo, Matheus"
digaOla.apply(alana, ['É um prazer te conhecer']); 
// "É um prazer te conhecer, Alana"

// posso invocar a função, mas como o this dela 
// desconhece a propriedade nome, retornará undefined
digaOla('Hello world'); // 'Hello world, undefined'
```

---

Com o `bind()` você **cria** uma função `x` baseada em outra função `y`, **vinculando a ela o contexto** que será utilizado quando for executada (uma tradução de bind é vincular). O primeiro parâmetro de `bind()` é o valor do `this`. Com isso, posso criar um método em um objeto e utilizar o `bind()` para que esse método **use outro objeto** na execução.

```js
function adicionaValor(y) { 
    return this.x + y; // precisamos vincular um this
}

const umObjeto = { 
    x: 1 
};

// agora a função está vinculada a umObjeto
const adicionaEmObjeto  = adicionaValor.bind(umObjeto); 
adicionaEmObjeto(2) // => 3

const outroObjeto = { 
    x: 10, 
    adicionaEmObjeto 
}; 
outroObjeto.adicionaEmObjeto(2);
// continua sendo 3, pois a função ainda
// está vinculada a umObjeto
```

O `bind()` também aceita **argumentos adicionais**, que serão **vinculados aos parâmetros da função** que utilizar o `bind()`. Com isso, você consegue definir parcialmente os parâmetros de uma função. Essa é uma técnica chamada de [currying](https://www.geeksforgeeks.org/what-is-currying-function-in-javascript/) (em homenagem a Haskell Curry, e não ao ingrediente de pratos indianos), em que você transforma uma função de vários parâmetros em outra com menos parâmetros.

```js
function exemplo(y, z) { 
    return this.x + y + z; 
}

// o segundo argumento de bind será vinculado 
// ao primeiro argumento recebido pela função 
// "exemplo" (ou seja, y = 2)
const exemploVinculado = exemplo.bind({ x: 1 }, 2);

exemploVinculado(3);
// => 6: this.x está vinculado a 1, y está 
// vinculado a 2, então só preciso passar
// o valor de z, que neste caso é 3
```
---

**De forma resumida**: os três são métodos que recebem o valor do `this` e podem utilizá-lo em sua execução. Com `call()` e `apply()`, você já **executa** a função, enquanto que com `bind()` você **cria** uma função.

- O W3Schools possui bons artigos explicando [call](https://www.w3schools.com/js/js_function_call.asp), [apply](https://www.w3schools.com/js/js_function_apply.asp) e [bind](https://www.w3schools.com/js/js_function_bind.asp), incluindo exemplos que podem ser testados online.

#### Method chaining

Quando você define um método e faz ele retornar o `this`, é possível criar o chamado "method chaining" ou cadeia de métodos, em que, de um mesmo objeto, um método pode invocar outro método e assim por diante (invocações em sequência, separadas por `.`). Esse processo é algo utilizado, por exemplo, pela biblioteca de visualização de dados [D3](https://d3js.org/).

Operações que trabalham com [Promises](#promise) também são um exemplo de method chaining:

```js
getDadosAssincronos()
    .then(fazAlgumaCoisa)
    .then(fazOutraCoisa)
    .catch(lidaComErros);

// a quebra de linha é somente para auxílio visual
// a chamada acima poderia ser assim:
getDadosAssincronos().then(fazAlgumaCoisa).then(fazOutraCoisa).catch(lidaComErros);
```

> O termo "method chaining" foi cunhado por Martin Fowler.

### Arrow function 

São funções anônimas e também podem ser atribuídas a uma variável. É uma **maneira compacta** de declarar uma função.
   
```js
// a arrow function pode ser criada assim...
const soma = (x, y) => { 
    return x + y; 
};

// ... ou de uma forma ainda mais compacta:
// - sem parênteses, caso seja somente um parâmetro
// - sem return ou {}, caso o retorno seja uma única
// expressão e na mesma linha
const quadrado = x => x * x; 

// se o retorno for um objeto, e seja retornado
// em uma única linha, OBRIGATÓRIO inserir o
// parênteses no retorno para evitar ambiguidade 
// com a abertura de um bloco de código.
const objeto = () => ({ mensagem: 'Olá mundo' });
```

- os parênteses e a "flecha" (`() =>`) precisam estar na **mesma linha**;

- tem muito mais detalhes; ver a documentação: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#syntax

- arrow function **não possui** o objeto `arguments`;

- **não possui** `this` próprio: ela **herda o valor do `this` do contexto** em que foi definida; 

    - por conta disso, `call()`, `apply()` e `bind()` **não irão** funcionar, o valor do `this` passado a elas será ignorado;

- não é "hoisted", ou seja, são interpretadas no momento da sua execução, não podendo ser utilizadas antes de serem declaradas;

- é **melhor usar const** ao atribuir uma arrow function a uma variável, já que o retorno dessa função é um valor constante;

- são muito **úteis** para definição de uma **callback** .

### Closure

🗒️ [Explicação MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

🗒️ [Explicação CodeInfinity](https://codefinity.com/blog/JavaScript-Closures)

Conceito importante no JS e em outras linguagens. Uma closure é a combinação de uma função e seu escopo léxico (o contexto em que a função foi definida). 

O **escopo léxico** se refere a como o interpretador do JS determina **onde uma variável pode ser acessada**, com base no local em que ela foi definida (o nome que foi dado a ela e o valor associado). Nesse caso, o escopo da variável é determinado no momento em que ela é **declarada**. 

Em outras palavras, **uma closure é a combinação de uma função e as variáveis que ela tem acesso** no contexto em que ela está cercada (a tradução para closure pode ser "fechamento", "cercamento"). No JS, o escopo das variáveis de uma closure é baseado em onde a função é **definida**, e não onde ela é executada - e isso faz muita diferença.

De maneira prática, isso explica porque funções aninhadas têm acesso às variáveis da função externa (a "função-pai"), e preservam esse acesso mesmo depois que a função externa tenha terminado. 

Suponha que uma função `A` possua algumas variáveis locais e **retorne** uma função interna `B`, sendo que `B` utiliza as  variáveis definidas em `A`. Quando você chama `let X = A()` (`X` agora é uma referência para `B`) e daí executa `X()`, a função `B` será executada e **ainda terá acesso** às variáveis definidas em `A`, mesmo que a função `A` já tenha terminado sua execução. Internamente, é como se `B` se "lembrasse" das variáveis de seu escopo externo, criando um ambiente fechado em que `B` ainda pode utilizar essas variáveis.

O exemplo abaixo mostra como o escopo da closure é baseado onde a função é definida e não onde é executada. Observe que, mesmo tendo uma variável "global" de mesmo nome (`scope`), quando a função `myScope` é executada, ela se "lembra" da variável local onde a função `funcaoInterna` foi definida. É importante notar isso, por ser algo contraintuitivo.

```js
let scope = 'escopo externo'; // "global"

function funcaoExterna() {
    let scope = 'escopo interno'; // local
    function funcaoInterna() { 
        return scope; // 'escopo interno' 
    }
    return funcaoInterna;
}

// estamos executando a função e atribuindo o
// retorno (que é outra função) a uma variável.
let myScope = funcaoExterna(); 

// funcaoInterna vai ser executada, mas ela se 
// lembra do escopo em que foi DEFINIDA (ou seja, 
// a variável scope definida internamente)
myScope(); // 'escopo interno'
```

Por que isso acontece? Quando uma função é executada e utiliza uma variável, esta variável **primeiro é procurada em seu escopo léxico local**. Caso não seja encontrada, ela é procurada um nível acima e assim por diante até encontrar a variável ou jogar um erro. É por isso que no exemplo acima o valor retornado por `myScope()` é o valor da variável `scope` definido em `funcaoExterna()`.

#### Em outras palavras...

Uma **função definida dentro de outra função** tem **acesso às variáveis da função externa** e pode manipulá-las. Isso é uma **closure**. 

Indo além, se essa função interna é **retornada pela externa**, a função interna mantém uma referência às variáveis da função externa, mesmo após a invocação dessa função externa. A função interna tem seu **próprio "ambiente" em que essas variáveis existem**.

> Lembre-se: quando uma função é invocada e termina sua execução, a grosso modo, ela deixa de existir. Ou seja, suas variáveis também deixam de existir. No entanto, devido à característica de uma closure, funções internas ainda "se lembram" do escopo da função externa, mesmo se a função externa deixou de existir.

O mesmo é verdadeiro caso a função externa tenha **mais de uma função interna** e retorne elas em um objeto, por exemplo - neste caso, **cada função** interna continua tendo acesso às variáveis, **compartilhando um mesmo escopo**.

- **cada invocação** da função irá criar um **novo escopo** para as funções internas retornadas, ou seja, o escopo **não** é compartilhado **entre invocações**. Veja o exemplo:

```js
function contador() {
    let i = 0;
    return {
        incrementa: () => { return i++; },
        reset: () => { i = 0; }
    }
}

// a e b são invocações independentes,
// portanto possuem escopos independentes
let a = contador(); 
let b = contador();
a.incrementa(); // 0
b.incrementa(); // 0

// o reset de b não influencia a
b.reset(); 
a.incrementa(); // 1

// mas o reset() e incrementa() de a 
// compartilham do mesmo escopo
a.reset(); 
a.incrementa(); // 0
```

Segundo o ChatGPT: closures funcionam inclusive em funções com mais de um nível de aninhamento (funções dentro de função dentro de outra função, etc). Cada nível se lembra das variáveis definidas tanto em seu pai direto quanto seus ancestrais.

#### Vantagens

Uma das vantagens de closures é possibilitar que funções tenham **"estados privados"**, isto é, variáveis que somente a função tem acesso. Conseguimos isso criando uma função que possui variáveis locais e retorna outra função que manipula essas variáveis. Assim, encapsulamos essas variáveis locais, impedindo seu acesso imediato, bem como uma manipulação maliciosa destes dados, possibilitando o acesso, por exemplo, por meio de getters e setters. Esse entendimento se aproxima de conceitos de OOP (Object-Oriented Programming, ou POO - Programação Orientada a Objetos).

Por manter estados privados dentro da função, podemos criar funções com características de cache, isto é, podemos armazenar informações dentro da função (resultados de chamadas anteriores, por exemplo) e utilizá-las para retornar dados sem necessariamente computá-los novamente, o que otimiza a performance. Isso é chamado de **memoização**.

#### Closures e o `this`

Lembre-se que somente as arrow functions herdam o valor  do `this` no escopo em que foram definidas. Então, se a sua closure necessitar do valor do `this`, use uma arrow function. 

Caso não utilize arrow function, lembre-se de chamar o `bind()` na função para atrelar o `this` a ela, ou crie alguma estratégia que compartilhe o `this` (por exemplo, criar uma variável na função externa e atribuir o `this` a essa variável). 

### Programação funcional

É um paradigma de programação baseado no uso das chamadas "funções puras" e um **estilo declarativo para resolução de problemas**. Ao invés de você detalhar passo a passo como resolver o problema, você descreve o que deseja que seja feito por meio do **uso de funções que atuem no problema**, podendo até mesmo passar outras funções como parâmetros ou retornar como saída uma nova função. É **diferente** de **programação imperativa**, em que você **descreve passo a passo** como resolver o problema, usando loops, condicionais e declarando variáveis, etc.

**Funções puras** são aquelas que sempre produzem o **mesmo resultado** para para as **mesmas entradas**, sem causar efeitos colaterais.

O paradigma é mais completo e aborda outros conceitos para que uma linguagem seja considerada específica para programação funcional. Mais detalhes nesse [artigo da Turing](https://www.turing.com/kb/introduction-to-functional-programming) em inglês.

#### JS e programação funcional 

O JS **não** é uma linguagem de programação funcional, mas **podemos usar técnicas de programação funcional** na linguagem, já que funções podem ser manipuladas como um objeto e serem passadas como argumento de outras funções. Um exemplo é quando usamos `map` ao invés de declarar e definir um loop tradicional: estamos usando uma função e dentro dela declaramos o que queremos fazer, por meio de outra função (callback).

> O JS é uma linguagem **multiparadigma**, pois neles podemos adotar diferentes estilos de programação, como funcional, imperativo e orientado a objetos.

**Higher-Order Functions** (funções de ordem superior): são aquelas que recebem uma ou mais funções como argumento (além de valores primitivos) e retornam outra função. Essa é uma das técnicas de programação funcional que conseguimos efetuar em JS.

- mais do que isso, por retornar uma função, podemos dizer que funções de ordem superior também criam uma **closure**.

## Arrays

São formas especializadas de objetos (veja mais sobre objetos na [próxima Seção](#objetos)), em que as propriedades são índices numéricos. Isso possibilita implementar uma maneira otimizada de acesso aos elementos por meio do índice. Além disso, a classe Array disponibiliza vários métodos úteis e poderosos para se manipular arrays.

> Por serem objetos, você pode adicionar propriedades com um nome qualquer ao array. No entanto, somente aquelas propriedades cujo nome seja um **valor inteiro não negativo (>=0)** serão consideradas índices do array e **contarão para o tamanho** do array.

```js
const mistura = [10, 20, 30];
mistura[3] = 40;
mistura['quatro'] = 50; // NÃO será índice
mistura; // => [10, 20, 30, 40, quatro: 50]
mistura.length; // => 4. Somente índices contam.

// ... PORÉM, os abaixo serão considerados índices:
mistura['4'] = 50; // converte para o inteiro 4
mistura[5.0000] = 60; // converte para o inteiro 5
mistura; // => [10, 20, 30, 40, 50, 60, quatro: 50]
mistura.length; // => 6
```

Em JS, dentro de um mesmo array pode haver elementos de **tipos diferentes**, inclusive objetos e outros arrays.

- O ES6, no entanto, introduz classes de arrays denominadas "typed arrays", que possuem tamanho fixo e aceitam somente um tipo númerico para seus elementos.

Arrays podem ser **esparsos**, isto é, pode haver índices sem elemento (vazios), definido por vírgulas sem valor entre elas. O índice não existe (o operador `in` retorna falso para esses índices), mas o acesso retorna `undefined`. 

```js
// Observe que se houver somente UMA vírgula no final, 
// NÃO adiciona um elemento vazio no final - precisaria
// ter duas vírgulas para isso. Ou seja, 'palavra'
// é o último elemento desse array.
const sparse = [1,,, false,, 'palavra',];

sparse.length; // => 6 (elementos vazios também contam)
sparse[1]; // => undefined
1 in sparse; // => false, índice vazio não é uma propriedade
```

### Acesso e gerenciamento

Para **acessar** elementos: coloque o índice entre colchetes: `arr[2]` irá acessar o **terceiro** elemento do array `arr`, pois o índice começa em zero.

- caso o array **não exista**, o acesso ao elemento irá resultar em um `ReferenceError`; caso seja atribuído `null` ou `undefined` (por exemplo, quando recebe o resultado de uma função), o acesso irá resultar em um `TypeError`;

    - o **ES2020** adicionou a possibilidade de **acesso com `?.[]`** (sim, com o ponto no meio) para evitar o erro quando a variável é `null` ou `undefined`. Será retornado `undefined`.

- caso acesse um **índice** que não tem valor, o resultado será `undefined`.

Para saber o **tamanho** do array, use a propriedade `length`.

- por ser uma propriedade, `length` pode ser manualmente alterado. No entanto, se for colocado um **valor menor** do que o tamanho atual, os elementos após o tamanho atribuído a `length` serão **removidos** e não podem ser recuperados (o array é "truncado"). Se for atribuído um **valor maior**, uma **área esparsa** (índices vazios) será colocada ao final do array até o tamanho atribuído. Isso, no entanto, não é uma prática comum.

Opções para **adicionar** elementos: 

- atribua um valor a um índice que ainda não existe (atenção: pode tornar o array esparso se os índices anteriores também não existirem);

- `push(novo_elemento)`: adiciona ao **final**;

- `unshift(novo_elemento)`: adiciona ao **início** e reorganiza os índices.

    - Atenção: se você passar mais de um elemento para o unshift (`arr.unshift(val1, val2, ..., valN)`), eles serão inseridos **em ordem** (`[val1, val2, ... valN, ...]`), diferente do que aconteceria se fossem passados individualmente.

Para **remoção**: 

- `pop()`: remove **último** elemento;

- `shift()`: remove **primeiro** elemento e reorganiza os índices;

- além de removerem, esses métodos também retornam o elemento que foi removido.

Para **modificar**: simplesmente acesse o elemento e atribua um novo valor a ele.

Para **deletar**: use o operador `delete` (exemplo: `delete arr[2]`). Diferente da remoção, o delete torna o índice vazio, ou seja, o `length` continua **igual**, mas o array se torna **esparso**.

### Alguns métodos úteis

- `slice(inicial, final)`: faz um recorte; cria um **novo array** com os itens do array em que o método foi invocado, começando a partir do índice `inicial` (inclusivo) até o índice `final` (**exclusivo**, ou seja, não será incluído no novo array). 

    - o segundo parâmetro é opcional; caso não seja utilizado, o recorte é feito até o último elemento do array (inclusivo, neste caso);

    - um valor negativo para os argumentos faz com que o recorte comece a partir do **final** do array;

    - necessário tomar cuidado quando o array possui objetos, pois o `slice()` faz uma ["shallow copy"](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) do array.


- `splice(indice, quantos, item_1...item_n)`: remove a partir do `indice`, `quantos` itens você informar (`quantos` é opcional). O terceiro argumento em diante, também opcional, indica elementos a serem **adicionados** a partir do `indice`, após a remoção. O método **modifica o array** e **também** retorna um **novo array**.

    - observe que `quantos` é diferente do `final` visto em `slice()`: `quantos` especifica o tamanho, a quantidade de elementos a serem removidos.

- `includes(elemento)`: introduzido pelo ES2016, verifica se um elemento se encontra no array e retorna true/false. Se o que você precisa é saber o índice da primeira ocorrência de um elemento, use o método `indexOf(elemento)`;

- reticências (`...`) antes de um array (`...arr`) podem indicar um **spread** ou um **rest** operator:

    - Spread operator: acontece quando você quer "desempacotar" ou desmembrar os elementos de um array. Você pode usar para copiar os elementos de um array para outro array, sem precisar passar os valores um por um. Essa é uma shallow copy e modificações feitas nos valores **não** mudam o array original no qual foi feito o spread.

        ```js
        const original = [1, 2, 3];
        const expanded = [0, ...original, 4];

        expanded[0] = 99;
        original[0]; // => 1
        ```

    - Rest parameter: é o oposto do spread, ajuntando elementos em um array. Pode ser usado na **definição de uma função**, quando você não sabe a quantidade de parâmetros que ela vai ter (*rest* vem de "resto"). Você aplica o rest parameter como o **último** parâmetro da função, *agrupando* em um array todos os argumentos que vierem a mais quando a função é invocada. 

        ```js
        function foo(paramA, paramB, ...otherParams); 
        // tudo que vier a partir do 3º parâmetro será agrupado em um array aqui definido como otherParams, que pode ser usado dentro da função
        ```

### Criação (ES6)

O ES6 adicionou dois métodos estáticos para criação de arrays:

`Array.of()`: retorna um array com base nos valores passados por argumento;

`Array.from()`: recebe como primeiro argumento um iterable ou um objeto array-like e retorna um array com os elementos desse objeto. Possui um segundo argumento, opcional, em que você pode passar uma função a ser aplicada em cada elemento para retornar um valor diferente (parecido com um map interno) para aquele elemento.

> Objetos array-like são objetos que possuem uma propriedade de nome `length` e propriedades cujos nomes sejam inteiros - outros nomes são ignorados pelo `Array.from()`. Apesar de poderem ser usados em funções que aceitam objetos array-like, eles não herdam métodos do `Array.prototype`.

```js
Array.from({ length: 4, 0: "foo", 1: 'yummy', 3: true })
// => ['foo', 'yummy', undefined, true]

Array.from({ length: 4, 0: "foo", banana: 'yummy', 3: true })
// =>['foo', undefined, undefined, true]
```

### Iteração

#### `forEach()`

Você pode usar o `for` clássico ou o `for/of` para iterar sobre os elementos do array. No entanto, uma maneira mais completa e funcional é por meio do método `forEach()`.

O `forEach(callbackFn(elemento, indice, arr), thisValue)` espera uma função como primeiro argumento (callback). Essa função será executada para cada elemento do array e, por sua vez, pode receber três argumentos, providenciados pelo forEach: o primeiro é o elemento de cada iteração, o segundo é o índice desse elemento, e o terceiro é o array como um todo. 

O callback pode ser definido dentro ou fora do loop, mas é uma prática comum fazer dentro, com uma arrow function. Em muitos casos você só está interessado no elemento, então o segundo e terceiro parâmetros podem ser ignorados e você escreve sua função com somente um parâmetro.

O `forEach` tem um segundo parâmetro opcional, aqui representado pelo `thisValue`. Ele pode ser utilizado para passar um outro contexto de `this` que a função vai acessar ao ser executada. É mais avançado e não tão comum, e a [MDN tem um exemplo](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#using_thisarg) de utilização. 

> Diferente dos loops `for` tradicionais, o `forEach` **não termina antecipadamente** usando o `break`, ou seja, o `forEach` **obrigatoriamente itera sobre todos os elementos**. Se você precisa de um loop que possa terminar antes de chegar ao final, utilize um dos `for` clássicos.

O `forEach()` **ignora (pula) índices inexistentes**, no caso de array esparso. Já o `for/of` **não ignora**, e o acesso a índices inexistentes retorna `undefined`.

```js
let soma = 0;

// array esparso de tamanho 7
const numeros = [2, 4,,,, 10, -3];

// forEach ignora índices vazios
numeros.forEach((valor, indice) => {
    soma += valor;
    console.log('índice: ', indice);
    // => índice:  0
    // => índice:  1
    // => índice:  5
    // => índice:  6
})
soma; // => 13

// forOf acessa índices vazios e retorna undefined
soma = 0;
// numeros.entries() retorna um array de arrays
// [ [idx0, val0], ..., [idxN, valN] ]
for (let [indice, valor] of numeros.entries()) {
    soma += valor ?? 0; // evitar undefined na soma
    console.log('indice: ', indice);
    // => índice:  0
    // => índice:  1
    // => índice:  2
    // => índice:  3
    // => índice:  4
    // => índice:  5
    // => índice:  6
}
soma; // => 13
```

#### `map()`

O método `map()` é parecido com o `forEach()` com algumas diferenças, listadas abaixo. Ele espera uma função como argumento, que é executada para cada elemento do array, com exceção daqueles inexistentes caso seja esparso. Essa função também pode receber os três argumentos `(elemento, indice, arr)`.

- o método **retorna um novo array** com os valores retornados pela função que você criou. Ou seja, sua função deve retornar um valor;

- apesar de a função não ser executada em índices inexistentes, o array retornado **também será esparso** do mesmo jeito que o array original.

```js
const numeros = [1, 2, 3];
const quadrados = numeros.map(val => val * val)
quadrados; // => [1, 4, 9]
```

O `map()` é um método muito utilizado quando você trabalha com React e precisa fazer um loop dentro de um JSX, por exemplo.

#### `reduce()`

`reduce(callbackFn(acc, elemento, indice, arr), valorInicial);`

Esse é outro método de iteração, cujo objetivo é **reduzir o array para um único valor**. Em outras palavras, ele combina os elementos do array para produzir um único valor a ser retornado, resultante de uma função que você passa como callback, também chamada de função redutora. O segundo argumento (`valorInicial`) é opcional e passa um valor inicial para o callback.

Diferente dos métodos anteriores, a função redutora vai receber **quatro** argumentos: `(acc, elemento, indice, arr)`. O primeiro argumento é o resultado obtido até o momento pela função, geralmente chamado de acumulador. Os outros três são os mesmos mencionados anteriormente.

Na primeira iteração, os passos serão os seguintes: 

- é atribuído a `acc` o `valorInicial`; se esse valor não foi passado, o primeiro elemento do array (`arr[0]`) é atribuído a `acc`. 

- o callback então é executado para o `elemento` da iteração. Se `valorInicial` foi passado, o `elemento` será  `arr[0]`; senão, será `arr[1]`;

- o retorno do callback é **atribuído** a `acc`;

Nas iterações seguintes, a função é passada para os próximos elementos e o retorno vai sendo atribuído em `acc`, por isso `acc` seria um "acumulador".

```js
let totalVendas = 100;
const novasVendas = [25, 13, 9.99, 7];
totalVendas = novasVendas.reduce(
    // 1º argumento, callbackFn
    (total, venda) => {
        console.log(`acumulador: ${total}, elemento: ${venda}`);
        // acumulador: 100, elemento: 25
        // acumulador: 125, elemento: 13
        // acumulador: 138, elemento: 9.99
        // acumulador: 147.99, elemento: 7
        return total + venda;
    },
    // 2º argumento, valorInicial
    totalVendas
);
totalVendas; // => 154.99
```

Existe também o método `reduceRight()`, que é o mesmo procedimento, porém fazendo a iteração partindo do último índice até o primeiro.

#### Outros métodos

Existem vários outros métodos e listá-los aqui seria exaustivo. Vale a pena consultar a documentação da MDN ou o livro mencionando no início dessas anotações.

- [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter);
- [`find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find);
- [`findIndex()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex);
- [`every()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every);
- [`some()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some);
- [Veja mais](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#instance_methods).

### Matriz

JS não oferece suporte a matrizes (ou arrays multidimensionais em geral). O que pode ser feito nesse caso é criar um **array de arrays** (um array cujos elementos também são arrays) e assim por diante. Fique ciente, no entanto, que o **tamanho** do array é definido pela sua **quantidade de elementos diretos**, ou seja, se um elemento também é um array, o seu tamanho não é adicionado ao tamanho do array-pai. Veja no exemplo:

```js
// criando as linhas: array esparso de 10 posições
let matriz = new Array(10); 
for(let i = 0; i < matriz.length; i++){
    matriz[i] = new Array(5); // criando as colunas
}
matriz.length; // => 10
matriz[0].length; // => 5
```

Você pode usar o método `flat()` para "achatar" um array de arrays, isto é, para transformá-lo em um único array. O método recebe como argumento o `depth`, ou seja, a profundidade com que o método deve achatar o array, levando em conta que subelementos do array podem conter outros arrays, e assim por diante. Por padrão, o valor é 1. Entenda melhor no exemplo:

```js
let arr = [1, [2, [3, [4]]]];
// flat() e flat(1) são a mesma coisa
arr.flat() // => [1, 2, [3, [4]]]
arr.flat(1) // => [1, 2, [3, [4]]]

// acessando novos níveis de aninhamento
arr.flat(2) // => [1, 2, 3, [4]]
arr.flat(3) // => [1, 2, 3, 4]

// isso não causa erro
arr.flat(4) // => [1, 2, 3, 4]
```

## Objetos 

Declarados entre `{}`. É uma coleção de **propriedades**, cada uma sendo um par de `chave: valor`, **separadas por vírgula**. Quando uma propriedade é uma **função**, ela também é chamada de **método** do objeto.

```js
let objeto = {
    texto: 'texto', // não precisa usar var nem let
    numero: 1, 
    valido: true,
    itens: ['item 1'],
    // propriedades também podem ser outros objetos
    objetoInterno: {
        textoInterno: 'texto interno'
    },
    // métodos (funções) podem ser declarados assim...
    ola: function(){
        console.log('Olá, sou um objeto!');
    },
    // ... ou assim (ES6)
    tchau() {
        console.log('Até mais!');
    }
};
```

> Também é possível criar objetos utilizando o operador `new` ou o método `Object.create()` (este último é uma maneira poderosa de criar um objeto e selecionar o seu prototype). Veja mais na [Seção de funções construtoras](#funções-construtoras).

- propriedades são acessadas pelo ponto (`objeto.numero`) ou colchetes (`objeto['numero']`);

    - o acesso pelo `.` não funciona se a propriedade tiver espaços, pontuação, ou se for um número ou uma expressão. Nestes casos, opte pelo `[]`;

    - se a propriedade não existir, será retornado `undefined`;

        - indo mais fundo: o JS irá procurar por essa propriedade no prototype do objeto e, caso não encontre, irá procurar no prototype do prototype do objeto, e assim por diante, até não haver mais prototypes nessa cadeia de prototypes. Caso não encontre, aí sim é retornado o `undefined`.

    - você também pode acessar subpropriedadades. Exemplo: `objeto.objetoInterno.textoInterno`;

    - assim como em arrays (que são um tipo especial de objeto), caso o objeto **não exista**, o acesso a uma propriedade resulta em um `ReferenceError`; caso seja atribuído `null` ou `undefined` ao objeto (por exemplo, quando recebe o resultado de uma função), o acesso irá resultar em um `TypeError`. Se quiser evitar isso, utilize o `?.` ou `?.[]` para acessar (ES2020); neste caso, será retornado `undefined` se o objeto for `null` ou `undefined`. O mesmo vale para acesso a subpropriedades;

        - outro jeito de evitar é usando o curto-circuito com &&:

            `const texto = objeto && objeto.objetoInterno && objeto.objetoInterno.textoInterno`;

    - caso acesse um índice que não tem valor, o resultado será `undefined`;

- métodos são invocados pelo ponto + parênteses: `objeto.ola()`;

- posso selecionar mais de uma propriedade de uma vez (é o ["destructuring assignment"](#destructuring-assignment)): `const { texto, valido } = objeto`;

- posso adicionar novas propriedades: `objeto.novaPropriedade = 'sou uma nova propriedade'`. O mesmo é utilizado para sobrescrever o valor de uma propriedade que já existe.

- posso deletar propriedades com o comando `delete`; ao tentar acessá-las após remoção, irá retornar `undefined`. Exemplo: `delete objeto.novaPropriedade`;

    - propriedades herdadas **não** são deletadas com `delete`.

- posso também inicializar um objeto vazio: `let novoObj = {}`;

- `Object.values(objeto)`: retorna um **array** com os **valores** de todas as propriedades do objeto;

- `Object.keys(objeto)`: retorna um **array** com o nome de todas as **propriedades** (menos as não herdadas);

- `Object.entries(objeto)`): retorna um **array de arrays bidimensionais**, sendo que cada cada elemento é uma `[chave, valor]`;

- O operador `in` pode ser usado para verificar se um objeto possui uma propriedade (`'numero' in objeto` retornaria `true`).

    - você também pode usar os métodos `hasOwnProperty()` e `propertyIsEnumerable()`. Por exemplo: `objeto.hasOwnProperty('numero')` retornaria `true`.

- `Object.assign(target, source1)`: copia as propriedades **próprias** (não herdadas) e enumeráveis de `source1` para `target`. Se `target` já tiver a propriedade, ela é sobrescrita com o valor de `source1`. Você pode passar outros objetos como argumento (`source2, ..., sourceN `) que também serão copiados para `target`, sendo que cada source novo irá sobrescrever as propriedades já existentes.

    - você também pode fazer isso com o **spread operator** (ES2018): `target = {...target, ...source1}`. Ele também copia somente as propriedades próprias e sobrescreve propriedades que já existem.

### Acessando propriedades com get e set

Você também pode definir **métodos** para acessar ou modificar uma propriedade (os chamados "getters" e "setters"). Isso foi introduzido no ES5. 

Apesar de serem definidos como métodos, o acesso/modificação **não** é feito por meio de invocação. Você acessa o valor da propriedade utilizando o `.` ou `[]` e modifica o valor utilizando atribuição (`=`). Veja o exemplo:

```js
const ponto = {
    x: 3,
    y: 4,

    // opte por usar o mesmo nome para o getter e o setter
    get r() {
        return Math.hypot(this.x, this.y)
    },
    set r(newVal) {
        // posso acessar outro getter do próprio objeto
        const theta = this.theta;
        this.x = newVal * Math.cos(theta);
        this.y = newVal * Math.sin(theta);
    },

    // posso definir somente o getter
    // criando uma propriedade read-only
    get theta() {
        return Math.atan2(this.y, this.x);
    }
}

// para acessar/modificar, NÃO uso o parênteses
ponto.r; // 5
ponto.theta; // 0.9272952180016122

// theta é read-only
ponto.theta = 10;
ponto.theta; // 0.9272952180016122
```

Como demonstrado no exemplo acima, quando crio somente um getter, a propriedade é read-only. Se eu atribuir um valor a essa propriedade, este valor será **ignorado**. 

Também é possível criar uma propriedade write-only definindo somente um setter. Propriedades write-only retornam undefined quando são acessadas.

> Observe que, dentro do objeto, quando quero acessar alguma de suas propriedades, **utilizamos o `this`**. Isso é **importante** e necessário para garantir que o acesso seja ao objeto que está chamando o método ou acessando a propriedade. Por exemplo, se o objeto for utilizado como modelo para criação de novos objetos (com o `Object.create()`, por exemplo), o uso do `this` garante que o novo objeto acesse os valores de suas próprias propriedades e não os valores do objeto usado de modelo.

**Não** é possível utilizar **arrow function** em getters e setters. Primeiro por conta do comportamento do `this` em arrow functions; segundo por conta da sintaxe dos getters e setters, que precisa de uma função nomeada. O nome dessa função também será o nome to setter/getter.

Por fim, vale lembrar que em objetos literais (ou seja, objetos criados diretamente usando `{}`) não existe a definição de propriedades privadas, ou seja, você **consegue acessar qualquer propriedade do objeto**, mesmo sem getter e setter. A criação de getter e setter é uma prática de desenvolvimento e uma convenção entre desenvolvedores para evitar esse acesso direto (é o conceito de encapsulamento de dados). Já quando utilizamos classes, a partir do ES2022 foi introduzida a definição de [campos privados](#campos-privados) (campo no caso é sinônimo de propriedade).

### Prototype

No JS, **todos os objetos herdam propriedades e métodos de um prototype**. Se eu acesso a propriedade de um `obj`, será primeiro procurado se ele possui esta propriedade; se não possuir, vai subindo na cadeia procurando por ela nos pais e assim por diante.

`Object.prototype`: é o objeto no **topo** da cadeia de objetos do JS. Todos os objetos herdam propriedades dele. A única exceção é quando você cria um objeto sem prototype usando `Object.create(null)`.

`objeto.__proto__`: propriedade que expõe o protótipo do objeto instanciado, exibindo suas propriedades. Ela está sendo descontinuada e no seu lugar podemos usar `Object.getPrototypeOf(<nome_do_objeto>)`.

### Funções construtoras

É possível **criar objetos a partir de um modelo**, por meio de **funções construtoras**. Esta era a maneira que o JS oferecia para se trabalhar antes de possibilitar o uso de `class` na linguagem. A maneira moderna de criar objetos é por meio de classes.

- A criação antes era feita por meio de protótipos e cadeia de protótipos. Internamente, continua sendo assim - a maneira moderna é somente um "syntax sugar", ou seja, uma sintaxe mais fácil de ler por um humano.

Hoje em dia, trabalha-se mais com classes, mas entender protótipos é ideal para entender as particularidades do JS e também no caso de cair em um projeto legado. Veja mais sobre classes na [Seção sobre Classes](#classes).

```js
// função construtora
// por convenção, o nome da função começa com maiúscula
function User(nome, email) { 
    // "new User(...)" vai executar essa função User 
    // e criar um novo objeto vazio. No corpo da 
    // função construtora, podemos adicionar
    // propriedades a esse novo objeto por meio do this.
    this.nome = nome; 
    this.email = email;

    this.exibirInfos = function() {
        return `Nome: ${this.nome}; e-mail: ${this.email}`;
    }
}

// Criando um novo objeto a partir desse modelo User.
// O this da função User é determinado por esse novo 
// objeto criado e atribuído a novoUsuario
const novoUsuario = new User('Matheus', 'meu@email.br');
console.log(novoUsuario.exibirInfos()); // => Nome: Matheus; e-mail: meu@email.br
```

Objetos criados por meio de funções construtoras têm o **mesmo protótipo** que é retornado pela **propriedade `prototype` da função que o criou** (`Object.getPrototypeOf(novoUsuario) === User.prototype` retorna true).

Outra forma de criar objetos a partir de um modelo é com o  [`Object.create()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create). Esse método estático cria um novo objeto usando como modelo outro objeto, que é passado como primeiro argumento. Esse outro objeto se torna o prototype do novo objeto.

Em resumo: **`new`** é usado para criar um novo objeto a partir de uma **função construtora**. **`Object.create()`** é usado para criar um novo objeto a partir de **outro objeto**.

### JSON

Formato `chave: valor`. Muito utilizado para comunicação entre back-end e front-end (APIs). Notação parecida com objetos, porém, mais restritiva com relação ao formato (o nome das chaves é entre aspas, não pode ter comentários, não pode ter uma vírgula no final da última propriedade (trailing comma), etc).

`JSON.parse(dadosJSON)`: converte **JSON para um objeto JavaScript**. Comumente usado quando **recebemos** um JSON da API;

`JSON.stringify(objetoJS)`: converte um **objeto JavaScript para o formato JSON**. Comumente usado quando **enviamos** dados para uma API. Esse processo também é conhecido como **serialização**.

- propriedades cujo valor seja `undefined`, assim como funções, **não** são serializáveis, ou seja, são **omitidos** no resultado da conversão. 

- O valor `null`, no entanto, **é** serializável;

- existem outras particularidades, como a serialização de um objeto do tipo Date. Consulte a [documentação](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#description).

## Classes Set e Map

Podemos usar as propriedades de objetos para mapear strings a valores. Dependendo da estrutura e operações que você precisa, trabalhar com as classes Set e Map pode ser mais vantajoso do que utilizar objetos.

### Set

Em JS, um set (conjunto em português) é um conjunto de valores únicos, não ordenados e não indexáveis.

- **valores únicos**: valores duplicados não são permitidos; ao tentar inserir um valor já existente em um set, este valor é ignorado;

- não ordenado: a ordem dentro de um set é **baseada na ordem de inserção**. Ao iterar em um set (usando `for/of` ou `forEach`), os elementos serão enumerados pela ordem de inserção;

- **não indexáveis**: não é possível recuperar um elemento pelo seu índice, como em um array, pois não há índices em um set.

A **maior vantagem** do set é quando você precisa **verificar se elementos fazem parte de um conjunto**. O set é otimizado para fazer essa verificação de maneira mais rápida do que em outras classes, como Arrays. A verificação é feita com o método `has()`. Além disso, com um set você tem a garantia de que não há repetição de elementos.

Por ser um conjunto, também estão disponíveis ao set [operações matemáticas de conjuntos](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#set_composition), como intersecção e união.

```js
// um set pode ser iniciado sem valores ou passando algum
// objeto iterável como argumento.
const valoresUnicos = new Set(); 

// método add adiciona UM valor por vez e retorna o set
// com o valor adicionado. 
valoresUnicos.add(5);

// posso ter valores de diferentes tipos
valoresUnicos.add(true);

// a comparação leva em conta o tipo, então 5 e "5" 
// serão valores distintos
valoresUnicos.add("5");

// devolve o tamanho do set
valoresUnicos.size; // => 3

// Se você passar um array, a REFERÊNCIA ao array é 
// adicionada ao set como valor, e não seus elementos.
valoresUnicos.add([0, 2, 4]);
valoresUnicos; // => Set(4) {5, true, '5', Array(3)}

// remove UM valor por vez e retorna true se o elemento 
// foi encontrado e removido, senão retorna false
valoresUnicos.delete(5); // => false
valoresUnicos.size; // => 3

// tentar remover o array não irá dar certo, a não ser 
// que você passe a referência ao mesmo array adicionado
// ao set
valoresUnicos.delete([0, 2, 4]); // => false
valoresUnicos.size; // => 3

// clear() remove todos os elementos de uma vez
valoresUnicos.clear();
valoresUnicos.size; // => 0
```

O método `add()` insere um valor **por vez**. O que pode ser feito é inicializar o set (`new Set()`) e passar um iterável como argumento, um array por exemplo. Neste caso, o array é desmembrado e seus elementos únicos passam a ser elementos do set. Como o `add()` retorna o set atualizado, é possível fazer o method chaining (`valoresUnicos.add(5).add(10).add('texto')`)

**Cabe ressaltar**: passar um **array para `add()` não insere seus elementos**, mas sim uma **referência em memória ao array**. Isso acaba se **refletindo também na remoção**: passar um array para `delete()` não necessariamente irá deletar um elemento array no set, mesmo se os valores são iguais - você precisa **passar a referência ao array como argumento do `delete()`**.

Por fim, um set pode ser convertido em array utilizando o spread operator. Com isso, podemos utilizar o set como uma forma útil de remover elementos duplicados de um array ou string:

```js
const cidade = 'Araraquara';
const valoresRepetidos = [2, 3, 5, 2, 3, 5, 5, 5, 2, 3, 10, 5, 7, 2, 3]
const c = [...new Set(cidade)]; 
const v = [...new Set(valoresRepetidos)];
console.log(c); // => ['A', 'r', 'a', 'q', 'u']
console.log(v); // => [2, 3, 5, 10, 7]
```

### Map

Um map é parecido com um objeto: é um **conjunto de pares chave-valor**, onde um valor é mapeado a uma chave. Diferente de objetos, no entanto, as **chaves** em um map podem ser de **qualquer valor**, incluindo funções, objetos e primitivas. Em objetos, as chaves são somente strings ou Symbol.

Outras diferenças em relação a objetos:

- map tem a propriedade `size` que informa sua quantidade de items;

- map é otimizado e tem melhor performance em **cenários que involvem operações frequentes de adição e remoção de elementos**. A busca por um elemento, no entanto, tem performance similar.

Você pode inicializar um map vazio ou passar um iterável que contenha como elementos arrays de 2 elementos `[chave, valor]`.

Da mesma forma que um set, map tem os métodos `has()`, `delete()` e `clear()`. A chave é passada como argumento para `has()` e `delete`.

Adicionar uma nova chave/valor, no entanto, é por meio do **método `set(<chave>, <valor>)`**, que adiciona um valor por vez e retorna o map atualizado (permitindo o method chaining). Além disso, as **chaves também são únicas**, ou seja, adicionar um valor a uma chave já existente sobrescreve o antigo valor. Com isso, `set()` também funciona como um método de update. Como em um set, a **comparação do valor da chave leva em conta seu tipo**, e quando é passado um array ou objeto como valor da chave, é **utilizada a referência em memória**.

Recuperar um valor é por meio do método `get()`, que retorna o valor da chave passada como argumento. Caso a chave não exista no map, é retornado `undefined`. Por conta de a chave utilizar a referência em memória quando definida como um array ou objeto, vale a mesma ressalva feita para set:

```js
let m = new Map(); 
m.set({}, 1); // mapeia a referência a um objeto vazio
m.set({}, 2); // mapeia a referência a OUTRO objeto vazio
m.size // => 2
m.get({}) // => undefined: referência a um TERCEIRO objeto vazio
```

Maps são iteráveis e retornam um array `[chave, valor]` em cada iteração. A ordem em um map é baseada na ordem de inserção dos elementos, como em um set. Também é possível iterar somente nas chaves ou valores, por meio dos métodos `keys()` e `values()`.

Vale uma ressalva ao utilizar o `forEach`: o callback passado ao forEach tem como **primeiro argumento o valor**, e como **segundo argumento a chave**: `m.forEach((valor, chave) => {...});`. Cuidado para não confundir a ordem, que é inverso do chave/valor.

## Classes 

Classes são uma representação para grupos de objetos que compartilham de certas propriedades. Em outras palavras, grupos de objetos que **herdam propriedades de um mesmo objeto protótipo** (prototype object). Membros da classe (chamados de instâncias da classe) podem ter suas próprias propriedades, mas também possuem propriedades e métodos definidos pela classe. Por conta disso, a **herança em classes no JS é baseada em protótipo** (prototype-based inheritance).

👉 Se dois objetos herdam propriedades de um mesmo protótipo, podemos dizer que são instâncias de uma mesma classe.

A vantagem de utilizar classes ao invés de uma função que gera objetos vem dessa característica de herança baseada em protótipo. Ao definir métodos em uma classe, esses **métodos são criados somente uma vez** e **compartilhados entre todas as instâncias** criadas por esta classe - quando uma instância invoca o método, ela acessa a referência a esse método e executa seu corpo. Se ao invés disso tivéssemos uma função que retornasse objetos, e dentro desses objetos definíssemos os métodos, esses métodos seriam criados para cada objeto novo, o que impactaria na performance em termos de alocação de memória.

Classes sempre estiveram presentes no JS, mas foi a partir do **ES6** que se introduziu a palavra-chave **`class` para criação de classes**. É somente  uma *syntax sugar*: por trás dos panos, o que há são objetos, que herdam métodos e propriedades de protótipos.

As seções abaixo mostram primeiro como se trabalhava com classes "antigamente", e depois mostra como é feito com `class`, o "jeito moderno". Entender como era feito antes é bom para saber o que está acontecendo por trás dos panos no jeito moderno, bem como pode ser necessário quando se trabalha com projetos legados.

### Criação com `Object.create()` e função do tipo factory

> O livro usa o termo "prototype object", traduzido como "objeto protótipo". No entanto, eu prefiro seguir com as palavras em inglês.

Uma das maneiras antigas de se definir uma classe é criar um objeto com propriedades que serão compartilhadas com outros objetos (esse seria o prototype object). Utilizando o método `Object.create()` e passando o prototype object como argumento, podemos criar um novo objeto que herda do prototype object, e com isso, temos a definição de uma classe em JS. 

Podemos ainda utilizar uma função factory para criar as instâncias da classe e já inicializá-las.

> Uma função factory (função de fabricação ou função fábrica) em JS implementa o padrão Factory. No caso, é uma função que é invocada para criar e retornar um objeto.

Exemplo simples para entendimento:

```js
// criação do prototype object
const carroPrototipo = {
    // criação de métodos estilo ES6
    acelerar() {
        console.log('Carro acelerando.');
    },

    frear() {
        console.log('Carro parado.');
    },

    toString() {
        // o this será associado ao objeto que 
        // invocar este método
        return `Carro da marca ${this.marca}, modelo ${this.modelo}.`
    }
}

// função factory que cria objetos (instâncias)
// baseado em carroPrototipo
function carro(marca, modelo) {
    // quando invocada, cria uma instância baseada 
    // em carroPrototipo
    const novoCarro = Object.create(carroPrototipo);

    // inicializa as propriedades da instância
    novoCarro.marca = marca;
    novoCarro.modelo = modelo;

    return novoCarro;
}

const fox = carro('Volks', 'Fox');

// como é uma instância de carroPrototipo, 
// posso utilizar seus métodos
fox.acelerar(); // 'Carro acelerando.'
fox.toString(); // => 'Carro da marca Volks, modelo Fox.'

// confirmando o protótipo da instância
Object.getPrototypeOf(fox) === carroPrototipo; // => true
carroPrototipo.isPrototypeOf(fox); // => true

// veja que sem uma instância o this não tem 
// valor definido
carroPrototipo.toString(); // => 'Carro da marca undefined, modelo undefined'
```

### Criação com função construtora e `new`

Um jeito mais natural (idiomático) de criar uma classe em JS, antes da introdução de `class`, é por meio de [funções construtoras](#funções-construtoras). Nesse caso, a função que inicializa um objeto pode ser chamada de "construtora" (constructor). Novamente, apesar da tradução, vou preferir usar o nome em inglês.

Novos objetos são criados por meio da **palavra chave `new`**, que invoca o constructor, que então inicializa o novo objeto criado. Observe que neste caso não é necessário utilizar o método `Object.create()`. O `new` irá **criar um novo objeto**, depois irá **invocar o constructor** como método desse objeto, para finalmente retornar o objeto. Ou seja, o valor do **`this` será esse novo objeto** e estará acessível na invocação da função. Isso funciona por conta do uso do `new` para invocar o constructor, e não funcionará corretamente se o constructor for chamado da maneira usual.

Mais do que isso, funções construtoras possuem uma **propriedade `prototype`**, que é **usada como o protótipo do novo objeto**. Com isso, podemos acessar `prototype` do constructor, que é um objeto, e **incluir nele as propriedades que serão herdadas** pelas instâncias que forem criadas por meio do `new`.

- o objeto `prototype` possui uma propriedade predefinida `constructor`, que faz referência à própria função construtora. Se você criar um `prototype` do zero ao invés de adicionar propriedades ao objeto `prototype` de uma função construtora, você perde essa propriedade `constructor`. No exemplo abaixo, optamos por estender `prototype`, ou seja, adicionar novos métodos usando `Carro.prototype.<nomeDoNovoMétodo>`.

Lembre-se: por convenção, o **nome** de uma função construtora **inicia com letra maiúscula**, para diferenciá-la de uma função "normal".

Exemplo anterior, modificado com esta outra abordagem:

```js
// função construtora. Quando invocada, inicializa o objeto
function Carro(marca, modelo) {
    // this faz referência ao objeto que a invocou
    this.marca = marca;    
    this.modelo = modelo;
}

// inclusão dos métodos ao protótipo
Carro.prototype.acelerar = function() {
    console.log('Carro acelerando.');
};

Carro.prototype.frear = function() {
    console.log('Carro parado.');
}

Carro.prototype.toString = function() {
    // o this será associado ao objeto que 
    // invocar este método
    return `Carro da marca ${this.marca}, modelo ${this.modelo}.`;
};

// um novo objeto é criado, Carro é invocada para 
// inicializar as propriedades desse novo objeto, 
// e o novo objeto é atribuido à variável kwid.
const kwid = new Carro('Renault', 'KWID');
kwid.acelerar(); // 'Carro acelerando.'
kwid.toString(); // => 'Carro da marca Renault, modelo KWID.'

// verificando o prototype da instância
Carro.prototype.isPrototypeOf(kwid); // => true
kwid instanceof Carro; // => true

// sem usar new, o `this` não será associado 
// corretamente e provavelmente será undefined 
// (modo restrito)
const usoErrado = Carro('marca', 'modelo');
usoErrado.toString(); // => Cannot read properties of undefined
```

Em resumo: o uso de funções construtoras, invocadas com a palavra chave `new`, faz o seguinte:

- cria um novo objeto vazio;

- define o protótipo desse novo objeto como a propriedade `prototype` da função construtora;

- invoca a função construtora, associando o `this` ao novo objeto criado;

- retorna automaticamente o novo objeto, a menos que a função construtora retorne algo explicitamente.

Por fim, vale mencionar que **não é possível utilizar arrow functions** nos métodos ou na função construtora, seja nessa abordagem ou na anterior: arrow functions não possuem a propriedade `prototype` e não lidam com o `this` da maneira desejada para este caso.

### Criação com `class`

A introdução da palavra-chave `class` pelo ES6 veio para facilitar a sintaxe de criação de classes. O uso de `class` é só outra maneira de definir uma função construtora e métodos a serem herdados.

> Classes não são "hoisted", ou seja, **não** é possível utilizá-las ou modificá-las **antes de serem definidas**.

Dentro do **corpo da classe**, você **cria o constructor** que irá inicializar o objeto, usando a palavra-chave `constructor`. Caso você não defina `constructor` (por exemplo, se não é necessário inicializar o objeto), um constructor vazio será implicitamente criado. O **constructor será chamado** quando você cria uma nova instância usando **`new`** e o **nome da classe** (`const novoObj = new NomeDaClasse();`).

Também dentro do corpo da classe, você **instancia variáveis e métodos**, mas sem a necessidade de separá-los por vírgula ou usar a abordagem `chave: valor`. Variáveis em uma classe costumam ser chamadas de **"campos" (fields)**, e seriam similares a propriedades em objetos.

> O código no corpo da classe é executado em **modo restrito**.

```js
class Carro {
    constructor(marca, modelo) {
        this.marca = marca;
        this.modelo = modelo;
    }

    // criação de métodos estilo ES6
    acelerar() {
        console.log('Carro acelerando.');
    }

    frear() {
        console.log('Carro parado.');
    }

    toString() {
        return `Carro da marca ${this.marca}, modelo ${this.modelo}.`;
    }
}

const captiva = new Carro('Chevrolet', 'Captiva');
captiva.acelerar(); // 'Carro acelerando.'
captiva.toString(); // => 'Carro da marca Chevrolet, modelo Captiva.'

// verificando o prototype da instância
Carro.prototype.isPrototypeOf(captiva); // => true
captiva instanceof Carro; // => true
```

#### Métodos estáticos

Ao definir um método em uma classe, se você adicionar a palavra-chave `static` antes do nome deste método, você o transforma em método estático. Isso significa que este método **é chamado diretamente pela classe**, sem precisar criar uma instância dela. É o que acontece quando chamamos, por exemplo, `Math.abs()` ou `Date.now()`, etc.

Métodos estáticos devem ser invocados através da classe, e não de instâncias, por isso, também são conhecidos como "métodos de classe". Ao tentar invocar um método de classe por meio de uma instância, você provavelmente irá receber um erro parecido com esse: "TypeError: instancia.nomeDoMetodoEstatico is not a function".

> Internamente, métodos estáticos se tornam **propriedades da função construtora**, e não do prototype object (`NomeDaClasse.prototype`). Por essa razão, não estão disponíveis às instâncias da classe.

Nada impede de você ter um método de instância e um método estático com o mesmo nome definido em uma classe. Basta adicionar o `static` ao método que será da classe. Quando chamado pela classe, o método estático é executado; quando chamado pela instância, o método de instância é executado.

Apesar de incomum (e não recomendado), existe sim um jeito de invocar um método estático por meio de uma instância da classe: utilizando a propriedade `constructor` (ou seja, `nomeDoObjeto.constructor.nomeDoMetodoEstatico`).

### Adição de métodos em classes existentes

Se uma classe já foi criada e definida, você pode adicionar novos métodos a ela criando uma nova propriedade no `prototype` da classe e atribuindo a essa propriedade uma função. Esses métodos ficam disponíveis a **todas as instâncias**, inclusive àquelas que já tenham sido criadas.

Da mesma forma, é possível também adicionar métodos estáticos diretamente na classe, atribuindo uma nova propriedade à própria classe ao invés de ao `prototype`.

Exemplo considerando a classe Carro construída anteriormente:

```js
// adicionando novo método
Carro.prototype.buzinar = function() {
    console.log('FONK!'); 
}

// adicionando método estático
Carro.describe = function () { 
    console.log('Este é um carro');
}

captiva.buzinar(); // "FONK"
Carro.describe(); // "Este é um carro"
```

Também é possível adicionar novos métodos em classes próprias do JS (Math, String, Date, etc), mas **não é recomendado**, pois pode causar confusão e incompatibilidade quando houver novas versões do JS.

#### Campos (até ES6)

Um campo (field) é o nome que damos às propriedades da classe ou de instâncias da classe que não são métodos (são as "variáveis" locais). 

Até o ES6, para **definir campos** em instâncias de uma classe, eles devem ser criados e inicializados **na função construtora ou dentro dos métodos**, usando o `this.NomeDoCampo = valor;` (lembre-se: ao atribuir um valor a uma propriedade do objeto, se esta propriedade não existe, ela é criada e o valor é associado a ela). Isso foi visto no exemplo da classe `Carro` acima, com a criação dos campos marca e modelo dentro do constructor, acessíveis pelas instâncias da classe (ou dentro dela por meio do `this`).

É também possível criar **campos estáticos**, acessados diretamente da classe (`Math.PI`, por exemplo). Para criá-los, primeiro é preciso definir a classe, e depois adicionar estes campos, atribuindo um valor a eles. Ou seja, campos estáticos são **criados fora do corpo da classe**.

```js
class Carro {
    // código omitido
}

// criando um campo estático (ES6), após 
// a classe ter sido definida
Carro.qtdePneus = 4;

const dolphin = new Carro('BYD', 'Dolphin');

Carro.qtdePneus; // => 4
dolphin.qtdePneus; // => undefined. É um campo estático.

```

A partir do **ES2022**, novas sintaxes foram atribuídas, facilitando e ampliando a criação e uso de campos, como pode ser visto na [próxima seção](#novidades-es2022). Vale, no entanto, estar atento a isso, caso se trabalhe com versões mais antigas do JS.

#### Novidades ES2022

> Como a edição do livro que eu usei é de 2020, esta seção é baseada na [documentação da MDN sobre classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_classes).

A partir do ES2022, foi introduzida a **sintaxe para campos**, incluindo **campos estáticos**, bem como **métodos e campos privados**.

##### Criação de campos

Anteriormente, os campos só podiam ser declarados dentro do constructor ou dentro de métodos. Agora, campos podem ser declarados **fora do constructor** e sem o uso do `this`. Isso possibilita declarar todos os campos no topo do corpo da classe, o que facilita a leitura do código.

- não é necessário inicializá-los. Neste caso, o valor será `undefined`;

- note, no entanto, que para **acessar** esses campos dentro da classe você ainda precisa **referenciá-los com o `this`**.

##### Campos privados

É possível também criar **campos privados**, que são **acessíveis somente dentro da própria classe**. Você cria um campo privado prefixando `#` ao identificador (`#nomeDoCampoPrivado`). Para acessá-los, você continua usando o `#` (`this.#nomeDoCampoPrivado`).

- propriedades privadas **não podem** ser acessadas nem modificadas **fora da classe** em que foram declaradas. Nem mesmo instâncias da classe possuem acesso a essas propriedades. Subclasses **também não** têm acesso nem podem modificar essas propriedades;

- tentar acessar um campo privado por meio da instância resulta em SyntaxError;

- métodos definidos na classe **conseguem acessar campos privados de outras instâncias**, desde que essas instâncias **sejam da mesma classe**. Isso pode acontecer, por exemplo, quando um método da classe recebe como parâmetro uma instância da mesma classe - nesse caso, o método pode acessar os campos privados dessa instância;

Para possibilitar o acesso ou modificação a campos privados, é necessário criar getters e setters, ou outros métodos que possibilitem o acesso e modificação desses campos privados.

Exemplo:

```js
class Conta {
    #saldo = 1000; // propriedade privada
    nome = 'Fulano'; // propriedade pública

    // getter para acesso ao saldo
    get saldo() { return this.#saldo }

    // função que modifica o saldo
    deposito(valor) { 
        if (valor > 0) {
            this.#saldo += valor;
        }
    }
}

// manipulando uma instância de Conta
const minhaConta = new Conta();
console.log(minhaConta.saldo); // => 1000
minhaConta.deposito(450);
console.log(minhaConta.saldo); // => 1450
```

**Vale uma observação:** no DevTools do navegador é possível que você consiga criar classes e acessar propriedades e métodos privados por meio das instâncias sem erros. Isso é um "relaxamento" da própria ferramenta para auxílio dos desenvolvedores. 

##### Métodos privados

Métodos também podem ser privados, bastando adicionar `#` antes da declaração do método. Para serem executados, devem ser chamados com a `#`. (`this.#metodoPrivado()`).

##### Campos estáticos

Do mesmo jeito que métodos estáticos, campos estáticos podem ser criados adicionando `static` antes do nome do campo, e são acessados pela classe ao invés da instância.

```js
class Conta {
    static nomeDoBanco = 'Super Banco';
}

console.log(Conta.nomeDoBanco); // => Super Banco
```

### Herança

É possível aplicar herança tanto a classes criadas com `class` quanto da maneira anterior (pré-ES6). Para saber como era na abordagem anterior, veja no livro a Seção 9.5.1. Aqui irei explicar somente herança aplicada a classes criadas com `class` e `extends`.

Uma classe `B` pode herdar de outra classe `A`. Nesse caso, chamamos **`A` de superclasse** e **`B` de subclasse**. 

Como `B` é uma subclasse de `A`, isso significa que `B` **herda todos os métodos (estáticos e de classe) e campos públicos** de `A`. Além disso, `B` pode **definir seus próprios campos e métodos**, bem como pode **sobrescrever métodos de `A`** (o que chamamos de *overriding*).

> O overriding está associado ao conceito de polimorfismo. **No JS**, apesar de ser possível o overriding, em que os métodos nas subclasses podem ter um comportamento diferente do da superclasse, **não é possível a sobrecarga (*overloading*)**, que ocorre quando **alteramos a assinatura de um método** (adicionando ou removendo parâmetros).

Podemos **invocar o constructor e os métodos públicos da superclasse** utilizando a palavra reservada **`super`**. Seguem algumas considerações:

- Se a subclasse tiver um **constructor**, é **obrigatório usar `super()`** para invocar o constructor da superclasse. Isso é necessário para que a instância seja inicializada corretamente e você tenha acesso a ela. 

    - mesmo se a superclasse não possuir um constructor, caso a subclasse possua, é obrigatório chamar `super()`;

    - Você só poderá usar o `this` no constructor após ter invocado `super()`;

- Se a subclasse não tiver um constructor, o JS implicitamente irá criar um para você e chamar `super()`;

- Ao **sobrescrever um método**, podemos invocar o método da superclasse utilizando **`super.nomeDoMetodo()`**. Se o método receber argumentos, podemos passá-los ao `super`. 
    
    - No caso de sobrescrita de métodos, o `super` pode ser invocado quantas vezes quiser e em qualquer posição do método;
    
    - observe que **não** é obrigatório invocar o método da superclasse se você não quiser;

    - também observe que você pode invocar qualquer outro método da superclasse (e não somente o que está sendo sobrescrito) usando `super`, desde que o método não seja privado.

Exemplo:

```js
class Animal {
    constructor(nome) {
        this.nome = nome
    }

    falar() {
        console.log('O animal está falando.');
    }

    toString() {
        console.log(`Sou um animal. Meu nome é ${this.nome}.`);
    }
}

class Cao extends Animal {
    constructor(nome, raca) {
        // obrigatório invocar o construtor 
        // da superclasse
        super(nome);

        // uso do this somente após ter
        // invocado super
        this.especie = 'Cão';
        this.raca = raca;
    }

    // override usando super
    falar() {
        super.falar();
        console.log('Au au au!');
    }

    // override sem super
    toString() {
        console.log(`Sou um ${this.especie} da raça ${this.raca} e meu nome é ${this.nome}.`);
    }
}

// instância da superclasse
const gata = new Animal('Filó');
gata.falar(); // "O animal está falando."
gata.toString(); // "Sou um animal. Meu nome é Filó."

// instância da subclasse
const pet = new Cao('Tobi', 'Pastor');
pet.toString(); // "Sou um Cão da raça Pastor e meu nome é Tobi."
pet.falar(); // "O animal está falando. Au au au!"
```

### Composição

Na herança, o relacionamento é do tipo "É um" (exemplo: "um carro *é um* veículo"). Na composição, o relacionamento é do tipo "Tem um" (exemplo: "um carro *tem um* motor"). Com essa ideia em mente, você consegue se orientar entre usar herança ou composição.

No JS, **não há herança múltipla**. Neste caso, você pode utilizar de **composição**, em que **uma classe pode conter instância de uma ou várias outras classes**: você pode criar um campo dentro dessa nova classe, e atribuir a este campo a instância de outra classe (você dá um `new` dessa outra classe e atribui ao campo). Você define sua nova classe como sendo a composição de uma ou mais classes, sem necessariamente herdar destas classes.

Composição pode ser usada quando você quer usar o comportamento de outra classe, e não necessariamente **ser** uma instância por completo daquela classe. Nesse caso, você cria uma instância dessa outra classe para delegar comportamentos a ela, sem criar uma hierarquia rígida de herança (em outros termos, elas não seriam fortemente acopladas).

Exemplo de composição (adaptado de um código gerado pelo ChatGPT):

```javascript
// Classe que será composta
class Motor {
  constructor() {
    this.velocidade = 0;
  }

  acelerar(incr) {
    this.velocidade += incr;
  }
}

// Um Carro não é uma subclasse de Motor, mas faz 
// uso de comportamentos de Motor, então podemos
// aplicar composição
class Carro {
  constructor() {
    this.motor = new Motor(); 
    // Carro agora tem um campo "motor", 
    // que é um objeto da classe Motor,
    // podendo acessar seus comportamentos
  }
}

// o comportamento de acelerar será delegado 
// ao motor
const meuCarro = new Carro();
meuCarro.motor.acelerar(50);
console.log(`Velocidade atual: ${meuCarro.motor.velocidade}`); // "Velocidade atual: 50"
```

Optar por herança ou composição é o famoso "depende". Depende do seu problema e da solução que você quer propor. Se a sua solução resultar em várias classes em que você enxerga alguma forma de hierarquia bem definida, optar por herança parece ser mais útil, pois todas compartilharão de campos e métodos e poderão definir/sobrescrever seus próprios métodos. Caso sua solução resulte em classes que precisam de comportamentos de outras classes, mas não necessariamente sejam uma subclasse, optar por composição pode fazer mais sentido. Lembre-se sempre da relação "é um" (herança) e "tem um" (composição).

> 👉 Segundo o livro, há uma máxima de OOP e design pattern: **"favoreça composição ao invés de herança"**.

### Classes abstratas

Em OOP, uma **classe abstrata** é aquela que define métodos (comportamentos) que subclasses devem ter, mas que **não os implementa** - fica **a cargo de cada subclasse implementá-los**. É uma forma de criar uma classe genérica que seria um "contrato" que subclasses devem seguir ao estender dela.

- uma classe abstrata não precisa ser completamente abstrata: ela também pode conter alguns métodos "concretos" (isto é, implementados) e campos.

No JS **não há** uma definição formal nem palavra-chave para classes e métodos abstratos (como o `abstract` em outras linguagens, por exemplo). Mas você pode simular este comportamento criando uma superclasse com métodos que lançam erros informando que devem ser implementado pela subclasse. Dessa forma, você obriga as subclasses a sobrescrever estes métodos e implementá-los de fato.

## Modularização

Modularização de código significa quebrar seu código em diferentes arquivos / blocos independentes, e disponibilizá-los para que possam ser utilizados por outros códigos. Isso simplifica o projeto, permite a utilização de projetos de terceiros, além de melhorar o encapsulamento ou acesso ao código, explicitando o que será ou não disponibilizado para uso em outros códigos. 

Por meio da modularização em JS, também conseguimos criar nossas próprias variáveis e funções sem a preocupação de sobrescrever alguma variável/função que venha de outro módulo - cada um tem seu escopo  próprio (chamado de "namespace"). Isto é, para usar alguma variável/função do módulo, você geralmente vai utilizar `nomeDoModulo.nomeDaFuncao`. Ou seja, se você criar uma função `contaCaracteres` e importar um módulo `Contador` que tenha uma função `contaCaracteres`, elas poderão existir no mesmo código e não se sobrescreverão (sua função será invocada por `contaCaracteres()` e a do módulo será invocada como `Contador.contaCaracteres()`).

O **Node** tem sua própria maneira de trabalhar com módulos, usando a função **`require()`**. Já em **JS**, o ES6 introduz as palavras-chaves **`import`** e **`export`**.

> Novas versões do Node (a partir da v12) também suportam `import`/`export` (parte do chamado "ES Modules") ao adicionar `type: "module"` no `package.json`. O `require()` é parte do chamado "CommonJS".

### Módulos em Node

Exportando funções de um módulo utilizando o objeto global `exports` do Node:

```js
// meuprojeto/calculadora.js

const soma = (x, y) => x + y;
const subtracao = (x, y) => x - y;
const funcaoInterna = () => console.log('função indisponível para outros módulos');

// somente o que estiver no objeto exports pode ser 
// exportado por outros arquivos
module.exports = { soma, subtracao }
```

Importando módulos próprios e de bibliotecas com `require()`:

```js
// meuprojeto/main.js

// necessário informar o caminho do arquivo (relativo ou 
// absoluto) para módulos próprios
const calc = require('./calculadora.js'); // o .js é opcional

// posso também importar somente aquilo que me interessa
const { soma } = require('./calculadora.js'); 

// para módulos do Node ou instalados via package manager
// não é preciso informar o caminho, somente o nome
const fs = require('fs'); 

console.log(soma(1, 3)); // 4
console.log(calc.soma(1, 3)); // 4
```

### Módulos com ES6

O mesmo exemplo anterior, mas agora usando a sintaxe do ES6 para `import` e `export`:

```js
// meuprojeto/calculadora.js

// opção 1: exportar individualmente cada variável/função/classe
export const soma = (x, y) => x + y;
export const subtracao = (x, y) => x - y;
const funcaoInterna = () => console.log('função indisponível para outros módulos');

// opção 2: fazer um export único no final do arquivo
// export { soma, subtracao };

// --------

// meuprojeto/main.js
// OBRIGATÓRIO informar o caminho do arquivo
import { soma, subtracao } from './calculadora.js'; // o .js é OBRIGATÓRIO

// ou se quiser importar tudo e definir um alias:
import * as calc from './calculadora.js'

// para módulos instalados via package manager, e caso 
// você utilize um bundler (Webpack, Vite, etc), seu 
// bundler pode reconhecer esses módulos instalados e, 
// nesse caso não é necessário passar o caminho 
// completo, somente o nome do módulo.

console.log(soma(1, 3)); // 4
console.log(calc.soma(1, 3)); // 4
```

Quando o módulo exporta **somente um valor** (uma classe, por exemplo), podemos utilizar o **`export default`**.

```js
// meuprojeto/Automovel.js
export default class Automovel {
    // ...
}

// meuprojeto/carros.js
// para export default, não é necessário utilizar {} no import
import voceDecideONome from './Automovel.js';
```

**Observações**:

- `export` e `import` não podem ser usados dentro de classes, funções, loops ou condicionais, ou seja, devem ser colocados no nível mais externo do seu código;

- a convenção é declarar todos os imports no começo do código. Eles podem ser colocados em qualquer lugar (desde que não dentro de blocos internos, como mencionado anteriormente), pois são elevados pelo hoisting, mas isso deixa o código ruim de ler;

- você pode renomear um valor importado utilizando `as`: `import { soma as adiciona } from './calculadora.js'`. O mesmo pode ser feito para valores exportados, mas é algum incomum;

- você pode criar um módulo que importa diferentes módulos e os exporta novamente. Isso acontece em projetos React, por exemplo, em que vários componentes ficam dentro de uma mesma pasta: você pode criar um arquivo `index.js` que importa e re-exporta os componentes, facilitando para que outros componentes possam importá-los em um único import.

    ```js
    // meuprojeto/components/Form/index.js

    // assumindo que os componentes são export default
    import Input from './Input/index.jsx';
    import Select from './Select/index.jsx';
    import Checkbox from './Checkbox/index.jsx';
    export { Input, Select, Checkbox};

    // outra maneira, combinando export e from:
    export { default as Input } from './Input/index.jsx';
    export { default as Select } from './Select/index.jsx';
    export { default as Checkbox } from './Checkbox/index.jsx';

    // caso os componentes não fossem export default
    // poderia simplificar para
    // export * from './';  
    ```

- O ES2020 introduziu o **operador `import('caminhoDoModulo')`**. Ele possibilita **imports dinâmicos**, retornando uma Promise, que é "fullfiled" quando o import termina de carregar. Neste caso, o import pode acontecer em qualquer parte do código, inclusive dentro de funções, loops, condicionais ou qualquer outro bloco de código. O módulo é carregado sob demanda (somente quando necessário), o que pode melhorar a performance.

## Erros

- `throw`: usado para **criar seu próprio erro**; pode substituir um return na função, jogando um erro que pode ser capturado e tratado. Se não for tratado na função, o erro "propaga" pela stack até encontrar um bloco que trata erros. Se nenhum for encontrado, o programa retorna um erro ao usuário. O erro dado pelo `throw` pode ser uma string, um número, ou uma instância da classe `Error`.

- `try...catch`: captura e manipulação de um erro; coloca o código dentro do try e captura/manipula o erro com o catch. O `catch` também pode jogar um erro com `throw`, para ser tratado por quem chamou a função, propagando o erro para cima.

- `finally`: bloco de código que **será executado**, independente de existir ou não um erro pego no try...catch, ou até se houver um return ou um break no try. É opcional. Pode ser usado, por exemplo, para fazer alguma "limpeza" no código.

```js
try {
    // bloco a ser executado
}
catch(e) {
    // se um erro "e" for jogado, será manipulado aqui
}
finally {
    // será executado, mesmo se acontecer um erro no bloco do try
}
```

> O `catch` e o `finally` são opcionais, mas pelo menos um deve estar presente se você usar um bloco `try`.

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

11
pag. 479
