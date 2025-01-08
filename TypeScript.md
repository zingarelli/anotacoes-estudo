# Anotações sobre TypeScript

As seções a seguir dão uma noção geral do TypeScript como linguagem. Ao final, há uma seção específica tratando do [TypeScript aplicado ao React](#typescript-e-react), com informações e códigos exemplificando as tipagens em um projeto React.

Muitas vezes, para economizar digitação, faço referência ao JavaScript como JS e ao TypeScript como TS.

## Documentação oficial 

Site para a documentação oficial: https://www.typescriptlang.org/docs/.

## Instrutor Alura

- [Flavio Henrique de Souza Almeida](https://www.linkedin.com/in/fl%C3%A1vio-henrique-almeida-a6315747/)

# TypeScript

É uma **linguagem** e também uma **camada adicional** ao JavaScript (chamam também de "superset" do JS), que dá ao JS um sistema de tipos para as variáveis, uma tipagem estática e previsível. Com o TS, haverá uma **compilação do código**, já informando ao programador qualquer erro de tipagem que for encontrado. Sem o TS, erros de tipagem apareceriam somente em tempo de execução, ou seja, após o código ter sido deployado (imagine descobrir esse erro somente em produção). 

TS foi desenvolvido e é mantido pela Microsoft, por isso ele possui uma ótima integração com o VS Code (autocomplete, ajudas e avisos de erros em tempo de desenvolvimento). 

Para utilizar o TS, é **necessário ter o Node.js instalado**, pois precisaremos do npm para instalações e start do projeto.

O framework Angular utiliza o TS por padrão. Além disso, é possível utilizar o TS com o React e o Vue, por meio de configurações adicionais.

Arquivos TypeScript possuem a extensão `.ts` (ou `.tsx`, se você quiser sinalizar que seu código retorna JSX).

**Vantagem**: garantir que os tipos certos estejam sendo utilizados nas variáveis, evitando erros em operações e facilitando na manutenção do código (ao saber exatamente o que uma função, objeto, etc, aceita como argumento, propriedade, etc). Ela também pode obrigar que as variáveis sejam inicializadas com algum valor.

**Desvantagem**: adiciona um tempo de compilação ao deploy do código; aumenta a complexidade do código; tem uma curva de aprendizado.

## Instalando o TypeScript

Utilize o comando abaixo para instalar o TypeScript standalone(versão 4.2.2) em um ambiente de desenvolvimento (não será instalado em produção):

    npm install typescript@4.2.2 --save-dev

### Compilação

Para compilar um arquivo `.ts` manualmente, utilizamos o comando

    tsc <nome_do_arquivo>.ts

Isso irá gerar como saída um arquivo com a extensão `.js`, transformando o código TS em JS, que pode ser então executado por navegadores ou pelo node. Se houver erros na compilação, eles serão exibidos na tela.

Podemos também automatizar com um arquivo [`package.json`](#arquivo-packagejson).

### Arquivo `tsconfig.json`

Neste arquivo podemos adicionar configurações sobre como o TypeScript irá se comportar e como irá lidar com os arquivos, podendo deixá-lo mais "flexível" ou mais restritivo.

Existem inúmeras possibilidades de configuração. A documentação tem uma [listagem de todas elas](https://www.typescriptlang.org/tsconfig/), com detalhes e exemplos. No GitHub, a comunidade também mantém alguns [templates pré-configurados](https://github.com/tsconfig/bases/?tab=readme-ov-file) para determinados projetos, permitindo que você estenda a partir deles e modifique por meio das suas configurações particulares (a [documentação](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#tsconfig-bases) explica como fazer isso).

Segue exemplo simples de um `tsconfig.json` com configurações a respeito de onde encontrar os códigos para compilação e onde deve ser salva a saída. Cada chave nesses objetos de configuração também podem ser usados como flags caso você esteja compilando usando o `tsc` via linha de comando.

```ts
{
    // configurações para compilador
    "compilerOptions": {
        // pasta em que serão salvos os arquivos TS compilados
        "outDir": "dist/js",

        // versão do ECMASCRIPT que será utilizada para conversão para JS
        // hoje em dia, a maioria dos navegadores já utilizan essa versão
        "target": "ES6",
    }, 

    // informação de onde estão os arquivos a serem compilados
    "include": [
        // inclui todas as subpastas de app (**) e todos os arquivos dentro dela (*)
        "app/**/*",
    ]
}
```

### Arquivo `package.json`

O `package.json` é outro arquivo para configuração. Neste caso, podemos definir nele alguns scripts para execução do node, na parte de `scripts`. O exemplo abaixo possui um script para compilação de um projeto:

```json
"scripts": { 
    "compile": "tsc"
},
```

Para compilar, por exemplo, basta digitar no terminal:

    npm run compile

É possível também automatizar a compilação para ocorrer toda vez que um arquivo TS for modificado e salvo (somente arquivos que estão na pasta marcada no include do `tsconfig.json`). Para isso, podemos adicionar o seguinte comando nos scripts do `package.json`: 

    "watch" : "tsc -w"

E para rodar, digitar no terminal:

    npm run watch

É possível automatizar ainda mais para que a compilação ocorra ao mesmo tempo em que o servidor da aplicação está rodando. Para isso, basta adicionar uma nova entrada nos `scripts` do `package.json` para que ambos os comandos rodem concomitantemente. Essa entrada foi nomeada de `start`:
    
    "start": "concurrently \"npm run watch\" \"npm run server\""

Com isso, `npm start` irá rodar os dois comandos. Isso só funciona se nas suas `devDependencies` existir o pacote "concurrently".

```json
"devDependencies": {
    "concurrently": "^6.0.0",
    "lite-server": "^2.6.1",
    "typescript": "^4.2.2"
}
```

## Tipagem

No TS, caso você não informe de qual tipo é a variável, ele irá tentar inferir seu tipo com base no valor atribuído à variável. Caso não consiga fazer isso, ele irá considerá-la como `any`, ou seja, qualquer coisa. Isso pode ser desativado incluindo a seguinte entrada no `compilerOptions` do `tsconfig.json`: 

    "noImplicitAny": true
    
Isso deixa o compilador do TS mais **rígido** ao avaliar o código. Caso uma variável não tenha um tipo definido, o TS irá emitir um erro ao invés de considerá-la `any`.

> O `any` é um tipo do TS usado quando você não quer que seja feita a checagem de tipo. Acaba fazendo com que o TS não sirva para nada neste caso, pois ele não irá verificar possíveis erros. Portanto, evite seu uso.

**Para tipar** suas variáveis/parâmetros/propriedades, você digita **`nomeDaVariavel: tipo`**. Isso é chamado de "type annotation". Os tipos podem ser os já conhecidos de JS (`Date`, `number`, etc.), mas também podem ser tipos do HTML (`HTMLElement`, `HTMLInputElement`, etc.). 

Um exemplo simples (e talvez ineficiente, já que o TS implicitamente conseguiria inferir o tipo destas variáveis por meio do valor da inicialização; valeria mais a pena se o tipo fosse mais complexo, como será visto mais adiante):

```js
let nomeCliente: string = 'Matheus';
const dataCompra: Date = Date.now();
```

Exemplo tipando os parâmetros de um constructor:

```ts
constructor(data: Date, quantidade: number, valor:number) { ... }
```

Exemplo tipando uma variável privada:

```ts
private _inputData: HTMLInputElement;
```

É possível também **tipar o retorno de funções/métodos** (porém, não é obrigatório):

```ts
//função que não retorna nada
adiciona(): void { ... }

// função que irá retornar uma instância da classe NomeDeUmaClasse
criaNegociacao(): NomeDeUmaClasse { ... }
```

### Tipar ou não tipar

- Construtores **não** devem ser tipados (mas seus parâmetros **sim**). 

- Variáveis dentro de métodos **não** precisam ser tipadas; você pode deixar que o TS infira o tipo delas quando recebem um valor. 

- Já as **propriedades de uma classe**, é melhor que sejam tipadas. A tipagem serve para auxiliar na programação, garantindo que o desenvolvedor trabalhe com os tipos certos das variáveis, funções, parâmetros, retornos, etc.

### Tipagem de `Array`

É possível utilizar `Array` na tipagem, mas neste caso seu array **precisa ter um tipo** (suporte ao `Generics`) É denominado **`Array<T>` **(diamante T), em que o `T` significa o tipo do array e é somente uma convenção, poderia ser chamado de `K`, `D`, etc.

```ts
private _listaNegociacoes: Array<Negociacao> = [];
private _listaDeFrutas: Array<string> = [];
```

Também é possível declarar de **forma mais simplificada** (syntax sugar): 

```ts
private _listaNegociacoes: Negociacao[] = [];
private _listaDeFrutas: string[] = [];
```

O TS oferece um tipo de array **somente leitura**, que pode ser vantajoso caso você tenha uma classe que retorna uma lista, mas não quer que ela seja editável (não possa fazer `push()`, `pop()`, etc). Para isso, utilize o `ReadonlyArray<T>` (only em minúsculo mesmo). Isso impede mutabilidade da lista (em tempo de compilação).

```ts
lista(): ReadonlyArray<Negociacao> {...}

// ou

lista(): readonly Negociacao[] {...}
```

### Union types

É possível que a variável aceite mais de um tipo. O TS irá verificar de qual tipo a variável é quando ela receber um valor.

Para atribuir mais de um tipo a uma variável, basta informar cada tipo separado pelo operador pipe (`|`).

```ts
let variavel_flex = string | number;

variavel_flex = 10; // ok
variavel_flex = 'dez'; // ok
variavel_flex = false; // erro
```

### Tipagem de funções

Em funções em geral, é possível tipar cada **parâmetro** e também seu **retorno**. A tipagem de **retorno**, embora **opcional**, é **recomendável**. Quando o retorno é algum tipo primitivo, como booleano ou string, o TS é capaz de inferir o tipo automaticamente. 

Em funções mais complexas, é recomendável tipar o retorno para **deixar o código mais claro e legível**, além de auxiliar em **verificar erros e inconsistências**, pois o TS saberá de antemão o que a função pode retornar e se seu retorno é compatível com a atribuição de uma variável, por exemplo.

Exemplo criado pela ChatGPT (a tipagem do retorno está aqui para exemplificar; na prática, não seria necessária, pois o TS consegue inferir o retorno baseado no tipo dos parâmetros e na operação):

```ts
// o tipo após o parênteses é para tipar o retorno da função
function add(a: number, b: number): number {
  return a + b;
}

// mesmo exemplo, mas como  uma arrow function
const add = (a: number, b: number): number => a + b;
```

## Classes e atributos privados 

### Em versões mais novas do JS

> Não consegui localizar a partir de qual versão começou a valer o que está escrito abaixo. *Acredito* que seja a partir do ES2022.

Os atributos **privados** só podem ter seus valores definidos pelo **construtor** da classe ou **métodos setters**. Não é possível atribuir um novo valor somente acessando o atributo. Caso tente fazer isso, pode ser até que seja criado um novo atributo com esse valor. Não é possível nem mesmo acessar esse atributo individualmente (precisa de um getter).

Para declarar um atributo como privado, utilizamos `#`.

```js
export class Negociacao {
    // atributos privados
    #data;
    #quantidade;
    #valor;

    // atribuição de valor no construtor
    constructor(data, quantidade, valor) {
        this.#data = data;
        this.#quantidade = quantidade;
        this.#valor = valor;
    }

    // acesso aos valores por meio de getters
    get data() {
        return this.#data
    }

    get quantidade() {
        return this.#quantidade;
    }

    get valor() {
        return this.#valor;
    }
}
```
### No TypeScript

O TS tem sua **própria sintaxe** para definição de atributos privados: a **keyword `private`**. Por uma **convenção** antiga do JS, o nome de atributos privados **começam com `_`**. O código fica assim:

```ts
export class Negociacao {
    // atributos privados
    private _data: Date;
    private _quantidade: number;
    private _valor: number;

    constructor(data: Date, quantidade: number, valor: number) {
        this._data = data;
        this._quantidade = quantidade;
        this._valor = valor;
    }

    get data() {
        return this._data
    }
    // ... e assim por diante
}
```

**Ponto de atenção**: no código JS gerado, os atributos **não** serão privados. O TS garante o encapsulamento somente em **tempo de compilação**. Acaba servindo mais como um alerta para que o programador escreva um código correto, de acordo com as regras do TS.

## Atributos públicos com `readonly`

No TS, podemos também criar atributos como sendo públicos, mas **somente leitura**, utilizando a palavra-chave `readonly` antes do nome da variável:

```ts
public readonly data: Date,
public readonly quantidade: number, 
public readonly valor: number
```

Neste caso, não é preciso criar métodos getter para esses atributos, pois eles são públicos, então passam a ter escopo global e ser acessíveis (mas não podem ser modificados nem ter um novo valor atribuído fora da classe).

### Falha do `readonly` e programação defensiva

O `readonly` possui uma falha: quando sua variável ou atributo é de um tipo mais complexo, como um `Date`, mesmo se você marcá-la como `readonly`, a variável ainda possuirá **acesso aos métodos `set`** do objeto Date, podendo então ser **modificada**.

Nesse caso, podemos aplicar o que o instrutor chama de *programação defensiva*, em que, ao invés de retornarmos a variável/atributo que seja de um tipo complexo, retornamos uma **cópia**. Assim, caso seja modificada, a modificação não irá alterar a referência original. Exemplo:

```ts
private _data: Date

get data(): Date {
    // programação defensiva
    return new Date(this._data.getTime());
}
```

> Mas nesse exemplo ele usou um atributo privado, e não um readonly. Não entendi... Talvez ele tenha se confundido com readonly quando na verdade ele estava falando sobre private.

## `constructor` simplificado no TS

No TS, você pode simplificar o `constructor` de uma classe para que, dentro do próprio construtor, sejam criadas as propriedades da classe (sejam privadas ou públicas).

Exemplo no modo convencional:

```ts
export class Negociacao {
    private _data: Date;
    private _quantidade: number;
    private _valor: number;

    constructor(data: Date, quantidade: number, valor:number) {
        this._data = data;
        this._quantidade = quantidade;
        this._valor = valor;
    }
    // ...
}
```

Exemplo no modo simplificado (o instrutor chama de 'atalho'):

```ts
export class Negociacao {
    // crio somente o construtor, sem declarar as propriedades fora dele
    // elas serão inicializadas quando o construtor for chamado
    constructor( 
        private _data: Date, 
        private _quantidade: number, 
        private _valor:number
    ) {} //bloco vazio, caso não haja nenhuma operação a ser feita no construtor, somente atribuição das propriedades
    // ...
}
```

Pelo TS, é possível **declarar as propriedades dentro do próprio construtor**, como argumentos, e o TS na compilação irá se encarregar de transformá-las em atributos da classe, que poderão ser utilizados pelos outros métodos da classe. Além disso, quando o objeto é construído, o TS se encarrega de inicializar os atributos com os valores recebidos nos parâmetros. 

Alternativa com atributos `public readonly`

```ts
constructor( 
    public readonly data: Date,
    public readonly quantidade: number, 
    public readonly valor:number
) {}
```

Caso a classe possua outras propriedades, mas que não são passadas para o construtor, é possível declará-las do modo convencional.

## Parâmetros e propriedades opcionais

No TS, as funções podem ter parâmetros opcionais, ou seja, elas podem ser invocadas sem alguns parâmetros. Para indicar que um parâmetro é opcional, você coloca `?` depois do nome do parâmetro (`param?: tipo`):

```ts
function add(num1: number, num2: number, num3?: number) {...}

add(1, 2); // OK
add(3, 5, 9); // também OK
```

**Atenção:** os parâmetros opcionais **devem vir por último**, após todos os parâmetros obrigatórios (você não pode, por exemplo, declarar um parâmetro como opcional e em seguida, declarar um parâmetro como obrigatório).

Na implementação da função, é necessário verificar se o parâmetro foi passado ou não, para evitar erros. 

```ts
// ...
if (typeof(num3) !== 'undefined') {
    // ...
}
// ...
```

> O mesmo vale para **tipagem de propriedades opcionais de um objeto**. Para indicar propriedades opcionais, adicione `?`após o nome da propriedade. Vale a mesma recomendação sobre a verificação da propriedade ser `undefined` antes de ser acessada.

## Herança no TS

Não tem segredo, basta utilizar `extends` na classe filha.

```ts 
export class NegociacoesView extends View {...}
```

Por padrão, todos os métodos e atributos são `public`, então **não** precisa colocar esse modificador nas declarações se não desejar (talvez seja uma boa prática, apesar de verboso, pois deixa evidente para quem está lendo). Caso queira modificar o acesso a essas propriedades, utilize os modificadores `protected` e `private`.

### Modificador `protected`

Em herança, uma classe filha **não tem acesso** aos atributos e métodos **privados** da classe mãe. Caso queira que atributos/métodos da classe mãe sejam acessados pelas classes filhas (e somente por elas), utilize o modificador `protected`.

```ts
protected elemento: HTMLElement;
```

**Somente** as classes filhas terão acesso a esses atributos. Objetos instanciados **não** terão acesso (a não ser se que faça métodos getter/setter).

No caso de métodos `protected` herdados, caso haja **sobrescrita** do método (veja seção abaixo), é necessário **declará-los novamente** como `protected` na classe filha, pois por padrão ele será tratado como `public`.

### Sobrescrita e sobrecarga de métodos

Quando quiser que as classes filhas implementem um método que está na classe mãe (sobrescrita, ou override em inglês), você pode fazer a classe mãe jogar um erro, auxiliando o dev a lembrar que o método deve ser implementado na classe filha. Exemplo:

```ts
template(texto: string): string {
    throw Error('A classe filha precisa implementar este método');
}
```

**Observação:** esse erro irá acontecer em tempo de *execução*. O melhor, nesse caso, é transformar essa classe em uma classe abstrata e esse método em um método abstrato (ver seção sobre [classe abstrata](#classe-abstrata)).

No caso de o método possuir um argumento cujo tipo *muda* na classe filha, é necessário o **uso de `Generics`**. A classe mãe será de um tipo T (`<T>`) e os argumentos que serão modificados também serão marcados com esse mesmo `T`. Desse jeito, você informa que o tipo é "type", cabendo ao dev colocar qual tipo ele irá utilizar e manter a consistência do tipo informado. Exemplo:

```ts
// classe Mãe
export class View<T> {
    template(texto: T): string { ... }
    // ...
}

// classe Filha
// o tipo T agora é ListaNegociacoes e deve ser mantida essa 
// consistência em todos os métodos que possuem um tipo T
export class NegociacoesView extends View<ListaNegociacoes> {
    template(listaNegociacoes: ListaNegociacoes): string { ... }
}

```

> não estou certo se isso seria considerado como sobrecarga (overload) do método ou simplesmente uma sobrescrita.

É possível dar **mais de um tipo** genérico, dando um nome a cada um deles (`T`, `K`, por exemplo); cada um pode ser então utilizado em diferentes posições da classe (como um tipo para um argumento, outro tipo para um retorno, etc):

```ts
class GenericDAO<T, K> {
    adiciona(objeto: T): K {
        // o parâmetro é de um tipo T e o retorno é de um tipo K
    }
    // ...
}
```

**Pergunta:** e se na classe filha eu quiser fazer a sobrecarga de um método com mais de um parâmetro? 

**Resposta:** aparentemente não é possível. O que pode ser feito nesse caso é a sobrecarga na **classe mãe**, ou seja, disponibilizar na classe mãe métodos com mesmo nome mas diferentes números e tipos de parâmetros. Porém, é necessário obedecer a algumas regras:

1. Escrever *somente* as assinaturas dos métodos e seus parâmetros;

2. Por último, escrever um método que tenha todos os parâmetros (para ser compatível com as assinaturas) e somente nesse método fazer a implementação; este último método não será invocado; a invocação deve ser dos métodos que receberam assinatura.

Exemplo:

```ts
update(texto: T): void;
update(texto: T, numero: number): void;
update(numero: number): void; 
update(): string;

// método pode ter de zero a dois parâmetros
// necessário verificar cada caso para evitar errors
update(arg1?: any, arg2?: any): any {
    if(arg1 != undefined && arg2 != undefined) {
        return 'updated';
    }
    else if(arg2 != undefined) {
        console.log(arg1);
    }
    else {
        console.log(arg1, arg2);
    }
}
```

**Ainda não tenho certeza se isso está totalmente correto**, pois não consegui testar a sobrescrita do método por classes filhas (dá erro de incompatibilidade de tipos). Mais informações [neste link](https://howtodoinjava.com/typescript/function-overloading/).

> Na documentação do TS, eles mostram essa possibilidade, mas consideram como sendo um override. No caso, na classe filha você adiciona o parâmetro (ou parâmetros) como opcional e faz uma condição no corpo do método para chamar o `super()` da mãe caso o parâmetro não seja recebido: ver a seção "Overriding Methods" [neste link](https://www.typescriptlang.org/docs/handbook/2/classes.html#extends-clauses).

### Classe abstrata

Não é possível criar uma instância diretamente de uma classe abstrata. Ela é herdada por uma classe filha, e será essa classe filha que o dev irá utilizar para criar instâncias. 

Em classes abstratas, você define os atributos e os métodos, sendo que alguns métodos podem ser abstratos (somente a assinatura do método). Cabe às classes filhas *implementar estes métodos abstratos* (*obrigatoriamente*).

Para tornar uma classe/método abstrato, utiliza-se o modificador `abstract`.

```ts
// exemplo de classe genérica E abstrata, com um método abstrato
export abstract class View<T> {
    // template deverá ser obrigatoriamente implementado pelas classes filhas
    abstract template(modelo: T): string;
}
```

O modificador `abstract` **não** precisa ser utilizado ao implementar o método na classe filha.

### Polimorfismo

É a capacidade que um objeto tem de ser referenciado de múltiplas formas. 

Quando um objeto "extende" de outro, ele também pode assumir o tipo de sua classe mãe. Ou seja, na hora de criar uma variável, o tipo dela pode ser **tanto o da classe filha quanto o da classe mãe**.

## Interfaces

Uma interface é criada com a palavra chave `interface`. Nela, podemos informar propriedades e seus tipos. No entanto, **não podemos** atribuir valores, nem usar modificadores nas propriedades e nem definir implementações a ela.

A interface pode ser utilizada para **tipar** um objeto. Uma vantagem é que haverá autocomplete das propriedades que ela possui, e será informado erro caso o nome da propriedade seja digitado errado. Além disso, facilita a renomeação de uma propriedade (no VS Code, utlizando a tecla `F2` fará com que a propriedade seja automaticamente renomeada onde ela for chamada no código).

### Interfaces e API

A interface é uma maneira de tipar dados recebidos de uma API. Ela pode ser utilizada para definir a forma que esses dados serão recebidos.

```ts
export interface NegociacoesDoDia {
    // o nome das propriedades deve ser igual ao nome que a API retorna
    valor: number;
    quantidade: number;
}
```

### Interfaces e classes

Interfaces também podem possuir métodos e serem utilizadas por classes. Nesse caso, a interface funcionaria como um "contrato"; quando a classe implementa uma interface (keyword `implements`), ela assume que irá implementar os métodos definidos na interface. Ou seja, neste caso a interface define como uma classe é e **o que** deve fazer, mas não define **como** ela vai fazer isso.

```ts
// declaração de interface
export interface Imprimivel {
    paraTexto(): string; // declaração de método a ser implementado
}
```

```ts
// uso de interface por uma classe
export class Negociacao implements Imprimivel{
    // ...
}
```

#### E a classe abstrata?

Uma interface acaba sendo parecida com uma classe abstrata, mas como a interface não possui implementações nem valores, ela serve mais para obrigar uma classe a implementar certos métodos. Já a classe abstrata pode ter alguns métodos implementados nela, além de métodos abstratos que classes filhas deverão implementar. 

Além disso, uma diferença importante é que não existe herança múltipla no TS para classes, mas isso é possível para interfaces, ou seja, **uma interface pode estender outras interfaces**.

### Herança múltipla e composição

Como informado anteriormente, **é possível herança múltipla** em interfaces. Quando uma interface estende de outras, ela está adquirindo os comportamentos dessas outras interfaces (mas não os está implementando!). Uma classe que implementa esta interface deverá implementar **todos** os comportamentos, seja os próprios da interface, quanto os herdados das interfaces pai. 

```ts
// suponha que Imprimivel e Comparavel são duas outras interfaces
export interface Modelo<T> extends Imprimivel, Comparavel<T> {}
```

Uma classe **pode implementar mais de uma interface**. Isso, no entanto, não é um caso de herança múltipla (a classe não herda da interface, apenas concorda em implementar seus métodos), mas sim um caso de **combinação**.

**Pergunta**: o arquivo JS gerado é somente uma linha de código `export {};`. Por quê?

**Resposta**:  em TS, as interfaces não geram código em tempo de execução. Elas são apenas uma forma de definir a estrutura dos objetos em TS, mas não são convertidas em código JS. Basicamente, o export {}; é uma instrução que informa ao compilador que esse arquivo é um módulo válido e pode ser importado por outros arquivos, mesmo que não haja nenhum outro tipo ou declaração nele. É o funcionamento interno do TS para lidar com interfaces e permitir que elas funcionem na conversão para JS.

### `interface` ou `type`

Outra maneira de tipagem é por meio de um "type alias", isto é, um nome para **identificar um tipo** que criamos e queremos **reusar**. Para isso, basta usar a palavra-chave `type`, dar um nome ao tipo e então definir este tipo.

```ts
// criando um tipo para um objeto e dando a
// este tipo o nome "Point"
type Point = {
    x: number;
    y: number;
}

// criando um tipo que aceita string ou número
type ID = string | number;

// utilizando os tipos criados
const myPoint: Point = { x: 10, y: 5 };
const myId: ID = '11259333663';
```

**Qual escolher: `type` ou `interface`?**

Ambos são semelhantes e, na maioria dos casos, você **escolhe baseado em sua preferência pessoal**. Há, no entanto, algumas diferenças entre eles, a começar pela sintaxe de criação. Este [artigo no StackOverflow](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript/52682220#52682220) tem uma explicação bem detalhada e com exemplos. Há também explicação do [próprio handbook do TypeScript](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces).

> O Handbook do TypeScript tende a favorecer o uso de `interface`.

Aqui seguem duas diferenças importantes:

1. ambos podem ser estendidos, mas a sintaxe muda: 

    - `interface` usa a palavra-chave `extends`; 
    
    - type alias utiliza `&` (interseção);

2. interfaces podem ser definidas múltiplas vezes e suas definições são **agregadas** (são adicionadas às definições anteriores). Já **type alias não** possibilita isso e irá gerar um erro se você tentar defini-lo outra vez.

```ts
interface Animal {
    nome: string;
}

type Veiculo = {
    marca: string;
}

// estendendo uma interface
interface Vaca extends Animal {
    mugir(): string;
}

// estendendo um type alias
type Carro = Veiculo & {
    qtdPortas: number;
}

// posso adicionar novos campos à interface 
// definindo ela novamente (aglutinação)
interface Animal {
    especie: string;
}

// o objecto cachorro agora reflete ambas
// as definições inseridas em Animal
const cachorro: Animal = {
    nome: 'Charlotte',
    especie: 'cão'
}

// tentar redefinir um type gera erro:
// Error: Duplicate identifier 'Veiculo'.
type Veiculo = {
    modelo: string;
}
```

## Enumerações (`enum`)

O TS oferece a possibilidade de se trabalhar com enumerações, cuja keyword é `enum`.

Uma enum funciona como um conjunto de constantes, em que seus elementos não podem ser modificados. Você pode, por exemplo, criar uma enum e exportá-la para ser utilizada em outras partes do código, concentrando suas constantes em um lugar só.

### Enums numéricas

Caso não sejam inicializados, por padrão os elementos terão um valor *incremental a partir de zero*:

```ts
enum diasDaSemana {
    DOMINGO, // valor 0
    SEGUNDA, // valor 1
    TERCA,   // e assim por diante
    QUARTA,
    QUINTA,
    SEXTA,
    SABADO   // valor 6
}

// para acessar
diasDaSemana.DOMINGO;
```

Podemos inicializar cada elemento da enum, garantindo que os valores serão os corretos, mesmo se a ordem dos elementos for mudada:

```ts
enum diasDaSemana {
    SEGUNDA = 1,
    DOMINGO = 0,
    SEXTA = 5,
    QUARTA = 3,
    SABADO = 6
    QUINTA = 4,
    TERCA = 2,
}
```
Caso um dos elementos não seja inicializado, seu valor será o valor do elemento anterior + 1;

```ts
enum diasDaSemana {
    SEGUNDA = 1,
    DOMINGO, // valor 2
    SEXTA = 5,
    QUARTA,  // valor 6
    ...
}
```

## `strictNullChecks`

Por padrão, o TS não irá verificar se uma atribuição a uma variável pode ter um valor nulo (por exemplo, quando uma variável recebe o retorno de uma função e essa função pode retornar nulo). É possível alterar esse comportamento adicionando uma nova opção ao `compilerOptions` no `tsconfig.json`:

```json
"compilerOptions": {
    "strictNullChecks": true
},
```

Ao ativar essa opção, o TS irá reportar erro em **todas** as atribuições que podem resultar em um valor `null` ou `undefined`. Caberá ao dev tratar cada variável tipada para evitar que seja recebido nulo ou que seja feita uma operação com valores possivelmente nulos. Nesse caso, ele pode  **explicitamente informar** que a variável pode aceitar o tipo `null`, ou **forçar** que a atribuição não será nula, por meio de **cast com a keyword `as`**, e também **tratar** as operações que podem envolver valores nulos (por exemplo, adicionar uma cláusula `if` e jogar erro se for nulo).

```ts
// opção 1: informar que a variável pode aceitar null
private _inputData: HTMLInputElement | null;

// opção 2: fazer o casting do que a variável deve receber na atribução
// aqui, você está assumindo a responsabilidade de que receberá um valor não-nulo (se vier um null, haverá erro em tempo de execução)
this._inputData = document.querySelector('#data') as HTMLInputElement;

// outra forma de cast (menos recomendada)
this._inputData = <HTMLInputElement>document.querySelector('#data');

// uma variável quando não é tipada é tratada como any (que já aceita null)
const form = document.querySelector('.form');

// tratando possível valor nulo
if (form) { ... }
else throw Error('O formulário é nulo');
```

**Atenção**: quando `strictNullChecks` é ativado (`true`) pode gerar uma *cascata de erros*, já que a checkagem de valores `null` é feita para todas as variáveis e operações com essas variáveis. 

A vantagem do `strictNullChecks` é perceber, em tempo de compilação, momentos que podem gerar um erro na aplicação por uso de valores `null` ou `undefined` e já fazer um tratamento desses erros.

## Decorator

> Esse é um conceito estranho e complicado. Necessário paciência e testes para entender.

O TS possui um recurso chamado "Decorator", que é uma função que possui um código que pode ser inserido dentro de outros códigos para executar algum comando que se repete ou é comum a todos eles. Diferente de uma função, partes do Decorator podem executar antes de um bloco de código onde ele é inserido e partes depois (por exemplo, para fazer um teste de performance de um bloco de código).

Para habilitar utilização de Decorators, é necessário informar no `tsconfig.json`:

```json
"compilerOptions": {        
    "experimentalDecorators": true
}
```
Ele possui esse nome "experimental" porque a função de "Decorator" ainda **não foi formalmente definida no JavaScript**, mas, segundo o instrutor, já tem sido utilizada em projetos em produção (Angular utiliza muito o Decorator).

O Decorator para métodos/funções retorna uma função, que possui três parâmetros:

- `target: any`: retorna o prototype da classe em que o Decorator for incluído, ou o construtor da classe, no caso de ser incluído em um método estático;

- `propertyKey: string`: é o nome do método em que o Decorator foi incluído;

- `descriptor: PropertyDescriptor`: é uma referência para o método original, onde será incluído o Decorator;

A sacada é sobrescrever o `descriptor`, adicionando os comandos do Decorator, mesclados ao método original em que o Decorator foi incluído, e retornar esse descriptor sobrescrito. Com isso, quando o método original for invocado, ele será executado junto com os comandos do Decorator.

Caso o método original possua parâmetros, podemos incluí-los no Decorator como sendo um Array do tipo `any`, assim aceitamos um número indefinido de parâmetros, com diferentes tipos. O método original será invocado com `apply(this, args)` para executar com os parâmetros originais.

Exemplo: 

```ts
export function logarTempoDeExecucao() {
    return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        // guardo a referência ao método original
        const metodoOriginal = descriptor.value;

        // sobrescrevo o método, mantendo seus parâmetros e incluindo os comandos do Decorator
        descriptor.value = function(...args: Array<any>){
            const t1 = performance.now();
    
            // execução do método original usando o apply com this para manter o contexto e os argumentos do método original.
            const retorno = metodoOriginal.apply(this, args);
                
            const t2 = performance.now();
            console.log(`Método ${propertyKey}: tempo de execução = ${(t2-t1)/1000} segundos`);
            
            // esse é o retorno do método original, caso tenha algum retorno
            return retorno;
        }

        // esse é o retorno da função do Decorator
        return descriptor;
    }    
}
```

Para incluir (prefiro falar "injetar") o Decorator, você utiliza a `@nomeDoDecorator()` antes da declaração de uma função ou método.

```ts
@logarTempoDeExecucao()
adiciona(): void { ... }
```

### Parâmetros

O Decorator também pode ter parâmetros próprios. Neste caso, basta informar na declaração do Decorator qual parâmetro ele pode receber:

```ts 
// parâmetro com um valor padrão é uma alternativa ao parâmetro opcional
export function logarTempoDeExecucao(emSegundos: boolean = false)
```

### Simplificando o Decorator

Caso o Decorator **não receba parâmetros**, é possível simplificar a declaração dele, retornando somente a função que ele retorna:

```ts
// exportando diretamente a função de retorno do Decorator e dando a ele o nome do Decorator
export function InspecionaMetodo(target: any, propertyKey: string, descriptor: PropertyDescriptor) { 
    // ... 
}
```

Neste caso, para adicionar o Decorator em um método, não é preciso colocar o `()`:

```ts
@InspecionaMetodo
update(texto: T): void { ... }
```

### Mais de um Decorator

É possível aplicar mais de um Decorator a um método/função, etc. Eles são colocados um por linha:

```ts
@InspecionaMetodo()
@logarTempoDeExecucao(true)
update(texto: T): void { ... }
```

Os Decorators serão executados **em ordem (de cima para baixo)**. No entanto, na compilação é o inverso (de baixo para cima), pois aquilo que está mais "para fora" precisa saber daquilo que está mais "para dentro" da função. Ou seja, no exemplo anterior, `InspecionaMetodo()` será executado primeiro, mas ele já sabe que existe `logarTempoDeExecucao()`, pois na compilação este último foi inserido antes. Isto significa que, na hora em que `InspecionaMetodo()` sobrescrever `update()`, os comandos de `logarTempoDeExecução()` já estarão injetados em `update()`. Com isso, na hora da execução, `InspecionaMetodo()` é executado primeiro e, em algum momento, irá chamar `logarTempoDeExecução()`, por este já fazer parte do método original.

### Decorator para propriedade

É possível também adicionar o Decorator para uma propriedade. Com isso, você aplica à propriedade um código que o Decorator faz. Isso pode auxiliar na legibilidade do código, abstraindo alguma lógica para o Decorator. Por exemplo, propriedades que contém um campo de um form: é possível utilizar o Decorator para fazer o código de buscar o elemento no DOM e adicioná-lo à propriedade.

O esqueleto de um Decorator para propriedade muda um pouco, pois não é necessário o terceiro argumento, `descriptor`, já que não haverá sobrescrita de um método. O segundo argumento, `propertyKey` guarda o nome da propriedade. Já `target` continua o mesmo, retorna o prototype da classe ou seu construtor (no caso de propriedade estática).

```ts
// Decorator com um parâmetro "seletor"
export function DOMInjector(seletor: string) {
    return function(target: any, propertyKey: string) { 
        //... 
}
}
```

Para poder acessar a propriedade, é necessário criar um getter no Decorator e adicionar esse getter à propriedade na hora que a classe for instanciada em runtime. Para isso, é utilizado o `target`, já que ele é um prototype da classe. A propriedade é modificada utilizando `Object.defineProperty()`. Ou seja, o Decorator não instancia um valor à propriedade, mas faz com que ela retorne o que o Decorator definir quando for acessada pelo código (ela se torna um getter).

Exemplo:

```ts
export function DOMInjector(seletor: string) {
    return function(target: any, propertyKey: string) {
        let elemento: HTMLElement;
        
        // cria um getter para definir o que a propriedade irá retornar quando ela for acessada
        const getter = function() {
            // esse if evita procurar pelo elemento toda vez que a propriedade for acessada
            if(!elemento){ // elemento ainda não foi procurado no DOM
                elemento = document.querySelector(seletor) as HTMLElement;
                console.log(`Elemento do DOM injetado na ${propertyKey}`);
            }
            return elemento;
        }

        // modifica a propriedade da classe para ser esse getter
        Object.defineProperty(target, propertyKey, {get: getter})
    }
}
```

Para utilizar:

```ts
export class NegociacaoController {
    @DOMInjector('#data')
    private _inputData: HTMLInputElement;
    // _inputData.value agora irá retornar o getter criado no Decorator
    ...
}
```

## Curiosidades diversas

### Formatação de data

Para mostrar uma data no formato da localidade do usuário, você pode utilizar o objeto `Intl.DateTimeFormat` (Intl vem de *Internationalization*):

```ts
new Intl.DateTimeFormat().format(negociacao.data);
```

Quando nenhum parâmetro é passado ao construtor, ele utilizará o formato padrão do navegador.

### Remoção de comentários

Ao compilar os arquivos TS, os comentários por padrão são incluídos nos arquivos JS gerados. Se desejar, é possível marcar para que os comentários não sejam incluídos, adicionando uma nova opção ao `compilerOptions` no `tsconfig.json`:

```json
"compilerOptions": {
    "removeComments": true;
},
```

### Teste de performance

É possível testar a performance do tempo de execução de uma função/método. Para isso, chamamos o método `performance.now()` (que retorna um tempo em milissegundos) antes e após a execução dos comandos da função/método, logando a diferença dos tempos ao final. Exemplo: 

```ts
update(texto: T): void {
    // teste de performance - início
    const t1 = performance.now();

    /* comandos do método ... */

    // teste de performance - fim
    const t2 = performance.now();

    //log do tempo de execução
    console.log(`Tempo de execução do método update: ${(t2 - t1)/1000} segundos`);
}
```

> Este método faz **parte do JS** e pode ser utilizado em projetos não-TS.

### Debugando o código TS

Como podemos colocar um breakpoint para debugar o código, se trabalhamos somente no código TS, e não no JS? É possível fazer isso, adicionando uma nova configuração no `tsconfig.json`:

```ts
"compilerOptions": {
    // possibilita debugar arquivos TS no navegador
    "sourceMap": true
}
```

Essa configuração fará com que, durante a compilação, além do arquivo JS correspondente ao arquivo TS, é gerado também outro arquivo com a extensão `.js.map`. Será esse arquivo que possibilitará colocar breakpoints no arquivo TS para debugar o código no navegador.

Além da configuração, é necessário também que seu **código-fonte esteja na mesma pasta que o código compilado**. Isso é necessário para que o Chrome consiga baixar seu código TS.

**Atenção**: você deve fazer isso somente em **ambiente de desenvolvimento**. Em produção você não deve deixar o código-fonte disponível.

*Dica para debug*: ao abrir o Dev Tools, estando na aba "Sources", use Ctrl+P para procurar pelo arquivo que deseja debugar.

# TypeScript e React

## Instrutores Alura

- [Luiz Fernando Ribeiro](https://www.linkedin.com/in/lfrprazeres/);

- [Paulo Silveira](https://www.linkedin.com/in/paulosilveira/);

- [Vinicios Neves](https://www.linkedin.com/in/vinny-neves/).

## Migrando um projeto React para o TypeScript (TS)

Quando você **já possui um projeto React desenvolvido** e desejá **migrá-lo** para o TS, é necessário **instalar** o TS e algumas bibliotecas adicionais:

    npm install --save typescript @types/node @types/react @types/react-dom @types/jest

- `--save`: flag que informa para salvar no `package.json` os pacotes que serão instalados. É uma flag opcional a partir das versões 5.0.0 do npm;

- `@types/{node, react, react-dom, jest}`: são **bibliotecas com tipagens adicionais** (que não vêm por padrão no TS) para node, react, react-dom e jest. Se seu projeto não tem o jest como dependência, por exemplo, não há necessidade de instalar a tipagem para ele.

Após instalado, precisamos criar um **arquivo de configuração `tsconfig.json`**, que contém as "regras" que desejamos que o TS siga ao avaliar/validar o código. Essas configurações dão flexibilidade para que a validação seja mais (ou menos) rigorosa. Este arquivo pode ser criado na mão ou via NPX (que já cria um arquivo com algumas configurações iniciais):

    npx tsc --init

No `tsconfig.json`, você pode adicionar a linha a seguir para informar que o JSX gerado está vindo de um projeto React, para que não seja necessário fazer o import do React explicitamente em todo arquivo que for usar React:

    "jsx": "react-jsx",

[Documentação do tsconfig](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) e [propriedades explicadas](https://www.typescriptlang.org/tsconfig).

## Criando um projeto novo React + TypeScript com CRA

Quando se trata de um **projeto React novo**, é possível criá-lo com um template TypeScript utilizando o CRA (Create React App do npm). O comando para criar um projeto dessa forma é: 

    npx create-react-app --template typescript nome_do_projeto

- Se quiser, também pode adicionar o comando `--use-npm` para forçar a utilização do NPM na criação do app. Isso é útil quando se tem tanto o NPM quanto o Yarn (que é outro gerenciador de pacotes) instalados na máquina - o CRA dá preferência ao Yarn neste caso;

## Criando um projeto novo React + TypeScript com Vite

Atualmente (2023), o **CRA tem sido preterido** por outras tecnologias mais modernas e performáticas, como Next.js. Alguns devs até recomendam não utilizar mais o CRA. Além do Next.js, outra alternativa é usar o [Vite](https://vitejs.dev/guide/).

[Este artigo da Alura](https://www.alura.com.br/artigos/vite-criar-aplicacao-react-typescript) mostra como usar o Vite para criar um projeto novo em React, usando também o TypeScript. Basicamente, você tem duas opções para criação:

Opção interativa, em que você segue os prompts na tela. Comece a interação digitando:

    npm create vite@latest

Opção direta, em que você especifica o React e o TypeScript:

    npm create vite@latest nome-do-projeto -- --template react-ts

Seja utilizando o CRA ou o Vite, será criado um arquivo `tsconfig.json` já com algumas configurações. 

## Plugin para o CSS modules

    npm install -D typescript-plugin-css-modules

Esse plugin auxilia o VS Code a entender o CSS modules como sendo um módulo TypeScript, podendo oferecer sugestões sobre as propriedades disponíveis (auxilia no autocomplete).

- O `-D` indica que será utilizado somente em tempo de desenvolvimento. É sinônimo da flag `--save-dev` e é opcional para versões do NPM ^5.0.0.

O seguinte código também deve ser adicionado ao arquivo `tsconfig.json`:

```json
{
  "compilerOptions": {
    "plugins": [{ "name": "typescript-plugin-css-modules" }]
  }
}
```

O TS não possui suporte a CSS Modules por padrão, então é necessária a adoção de uma biblioteca adicional para esse suporte. No entanto, se você criar o projeto usando CRA, aí então o suporte vai vir por padrão devido às configurações do CRA.

## Absolute imports em TypeScript

Em React, é possível criar absolute imports por meio de um arquivo `jsconfig.json`. No caso de TypeScript, não é preciso este arquivo, pois a configuração pode ser feita no próprio `tsconfig.json`, utilizando o mesmo formato de configuração:

```json
{
  "compilerOptions": {    
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

Eu explico mais sobre absolute imports nas [anotações sobre React](./React.md).

## Extensão dos arquivos

Quando seu componente **retornar um JSX**, **nomeie o arquivo como `.tsx`**. Outros arquivos podem ser nomeados como `.ts`. Isso auxilia tanto a IDE quanto outros programadores a diferenciarem o que se espera do arquivo.

## Tipos `JSX.Element`, `ReactNode` e `ReactElement`

O `ReactElement` é um objeto com um tipo e props.

O `ReactNode` pode ser um `ReactElement`, um `ReactFragment`, uma string, um number, uma lista de `ReactNode`, null, undefined ou boolean.

O `JSX.Element` é um `ReactElement` com o tipo genérico para props e type sendo any. Ele existe para permitir que outras bibliotecas implementem o JSX de seu próprio jeito customizado.

## Tipagem de componentes usando `React.FC`

Resposta do Vinicios à pergunta no fórum sobre a tipagem com `React.FC`: 

"O tipo `React.FC` é uma forma de tipar componentes que retornam JSX. Ele é uma abreviação para Function Component, ou seja, é uma forma simples e direta de definir um componente funcional.

A vantagem de utilizar o `React.FC` é que ele já inclui algumas propriedades padrão, como children, que é uma propriedade implícita que representa o conteúdo entre as tags do componente. Além de tipar o retorno da função.

Maaaas é importante lembrar que o uso do `React.FC` não é obrigatório. Algumas pessoas preferem utilizar interfaces para tipar as props, como você mencionou e como utilizamos nos outros cursos.

Hoje em dia, na prática em projetos em produção, não se usa mais essa abordagem ([aqui tem uma discussão no github interessante sobre isso](https://github.com/facebook/create-react-app/pull/8177)). Ele era o padrão e, no meio do caminho, **o time do React acabou por não encorajar mais esse tipo de abordagem**."

## Exemplos de Tipagem com Class Components

### Tipagem e uso de props

```ts
// Button.tsx
// tipando a prop "texto"
class Button extends React.Component<{ texto: string }> {
    render() {
        return (
            <button>{this.props.texto}</button>
        )
    }
}
export default Button;

// usando o componente
<Button texto='Adicionar' />
```

### Tipagem e uso de children

É o mesmo exemplo anterior, agora utilizando o children ao invés de uma prop personalizada.

```ts
// Button.tsx
class Button extends React.Component<{ children: ReactNode }> {
    render() {
        return (
            <button>{this.props.children}</button>
        )
    }
}
export default Button;

// usando o componente
<Button>Adicionar</Button>
```

## Exemplos de Tipagem com Function Components

### Tipagem e uso de props

Fazendo um destructuring das props e depois tipando cada uma individualmente:

```ts
// Item.tsx
// primeiro declara as props, depois informa o tipo de cada uma
export default function Item({ task, duration }: { task: string, duration: string }) {
    return (
        <li>
            <h3>{task}</h3>
            <span>{duration}</span>
        </li>
    )
}
```

### Alternativa utilizando interfaces

Um padrão que pode ser seguido para **tipar as props** de um componente (seja function ou class component) é **usando interfaces**. Dentro do próprio componente (ou em um arquivo externo, se preferir), podemos definir uma interface e nessa interface listar as props que são aceitas e seus tipos. 

Uma convenção de nome para interfaces é usar `NomeDoComponenteProps` ou só `Props`. Outra convenção de nome é colocar o prefixo `I` (de interface): `INomeDaInterface`. 

Uma opção de organização é criar uma pasta "types" para armazenar essas interfaces de tipo:

Exemplo 1: 

```tsx
// types/ITasks.tsx
export interface ITasks {
    task: string;
    duration: string
}

// components/Item.tsx
import { ITasks } from '../types/ITasks';

// a tipagem é mais simples e fácil de ler
export default function Item({ task, duration }: ITasks) {
    return (
        <li>
            <h3>{task}</h3>
            <span>{duration}</span>
        </li>
    )
}
```

Exemplo 2`:

```ts
// Banner.tsx
interface BannerProps {
    enderecoImagem: string;
    textoAlternativo?: string; // o ? ao lado do nome significa que é uma prop opcional (aceita undefined)
}

// posso aplicar o destruct para obter as props da interface
const Banner = ({ enderecoImagem, textoAlternativo}: BannerProps) => {
    return (
        <header className="banner">
            <img src={enderecoImagem} alt={textoAlternativo} />
        </header>
    )
}

export default Banner;
```

## Tipagem de props que são objetos

Quando você passa um **objeto inteiro** como prop, na hora de tipá-lo, você tem que defini-lo como sendo uma prop e depois informar o tipo da prop (`{nome_da_prop}: {nome_da_prop}: tipo_da_prop`):

```tsx
interface Props {
    title: string,
    description: string, 
    photo: string, 
    price: number,
    id: number
}

// necessário informar o nome da prop e depois tipá-la
export default function Item({ item }: { item: Props }) {
    //...
}

// usando o componente
{menu.map(item => (
    // passando o objeto inteiro como uma única prop "item"
    <Item key={item.id} item={item} />
))}
```

Você também pode **desestruturar o objeto** na chamada do componente (`{...nome_do_objeto}`), fazendo com que cada propriedade desse objeto se torne uma prop do componente. Isso facilita a tipagem na declaração do componente, pois posso agregar todas as props em uma só e tipar essa prop diretamente:

```tsx
interface Props {
    title: string,
    description: string, 
    photo: string, 
    price: number,
    id: number
}

// posso dar qualquer nome à prop (que está agregando todas as props recebidas) e já tipá-la diretamente
export default function Item(item: Props) {...}

// usando o componente
{menu.map(item => (
    // desestruturando o objeto e passando todas as suas propriedades como sendo props individuais
    <Item key={item.id} {...item} />
))}
```

Outra alternativa é inserir o objeto dentro da interface. Assim, não é necessário destruct na chamada do componente:

```tsx
interface Props {
    item: {
        title: string,
        description: string, 
        photo: string, 
        size: number,
        serving: number, 
        price: number,
        id: number
    }
}

export default function Item(item: Props) {...}

// usando o componente
{menu.map(item => (
    // não preciso fazer o destruct
    <Item key={item.id} item={item} />
))}
```

## Tipagem utilizando `type` e `typeof`

Uma alternativa à tipagem **sem o uso de interfaces** é declarar o tipo com a palavra reservada `type` e informar seu tipo como sendo `typeof` alguma coisa. 

Por exemplo, se você recebe um arquivo JSON com um array de objetos de propriedades semelhantes, você pode fazer o uso do `typeof` para declarar que seu tipo será do tipo das propriedades desse objeto (nesse caso, você pode pegar somente o primeiro item do array, senão o tipo será um array). O TS irá inferir o tipo de cada propriedade desse objeto.

```tsx
import menu from '../items.json';

// utilizando a primeira entrada do arquivo JSON 
type Props = typeof menu[0];

// usando o tipo criado
export default function Item(item: Props) {...}
```

Você pode utilizar `&` para combinar tipagens e assim extender de outros tipos: 

```tsx
import React, { ButtonHTMLAttributes, HtmlHTMLAttributes } from "react";

// custom properties and also native HTML button properties
type buttonProps = {
    children: React.ReactNode;
} & React.ButtonHTMLAttributes<HTMLButtonElement>

export const Button = ({ children, className }: buttonProps) => {
    return <button className={className}>{children}</button>
}
```

## Tipagem de eventos

Eventos passados como argumentos para uma função também precisam ser tipados. 

Segue uma **dica para descobrir o tipo do evento**: escreva uma função para o evento e depois passe o mouse no argumento de evento para que o VS Code dê uma dica do tipo dele. 

Caso não queira ser específico no tipo do evento, é possível utilizar um tipo mais genérico: `React.SyntheticEvent<InputEvent>`

```ts
// No VS Code, passe o mouse sobre "e" para ver o tipo e copiálo
onChange={ e => ... }
```

### Tipagem de funções como props

Quando passo uma função como prop, ela também deve ser tipada. Para isso, deve-se adicionar tipagem aos **argumentos** e também ao **retorno**. Posso fazer isso informando o nome da função e seu tipo como uma arrow function, fazendo a tipagem dentro dessa arrow function. Caso a função não retorne nada, seu tipo de retorno é `void`.

```tsx
interface IProps {
    // tipando uma função "onSelected", que possui um parâmetro "task" tipado por uma interface, e que não retorna nada
    onSelected: (task: ITask) => void
}
```

A tipagem de funções em **class components é feita da mesma maneira**.

## Organização de pastas

Uma boa prática é criar uma pasta `shared/interfaces` (ou somente `interfaces`) para colocar as interfaces de entidades que são utilizadas por diferentes componentes da aplicação. Cria-se uma interface por arquivo. Desse modo, centralizo as tipagens de entidades, facilitando o reúso e a manutenção.

```ts
// shared/interfaces/IColaborador.ts:
export interface IColaborador {
    nome: string;
    cargo: string;
    imagem: string;
}

// componentes/Time/index.tsx:
import { IColaborador } from '../../shared/interfaces/IColaborador';

// posso até usá-la como tipagem dentro de outra interface
interface TimeProps {
    corPrimaria: string;
    corSecundaria: string;
    nome: string;
    colaboradores: IColaborador[] // notação para indicar que é um array
}
```

Outra convenção é criar a pasta `types` para guardar as tipagens comuns entre componentes.

## Responsabilidade por retorno nulo

Quando usamos a exclamação (`!`) logo após o nome de uma variável (`let birthday = userInput!`) ou chamada de uma função que pode retornar nulo (`let birthday = getBirthday()!`), estamos garantindo ao TS (e nos responsabilizando) que aquela função **não** retornará nulo. Dessa forma, o TS irá ignorar o possível erro que isso poderia causar durante a compilação. É uma forma de forçar o TS a aceitar uma situação que poderia ocasionar em retorno de null. Deve ser usado com parcimônia.

# Continuar em

https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types