# Anotações sobre HTML e CSS

## Instrutores Alura

- [Pedro Marins](https://www.linkedin.com/in/pedromarins/);

- [Matheus Alberto](https://www.linkedin.com/in/matheus-alberto-marcus/);

- [Mônica Mazzochi Hillman](https://www.linkedin.com/in/monicamhillman/);

*Com algumas anotações de cursos do Bootcamp TQI | DIO*

#  Dicas VS Code

- Aperte o F2 para renomear tags. Assim, se você alterar o nome da tag de abertura, a tag de fechamento também será alterada.

- Emmet abbreviation:

	- `div.nova_classe` : cria a tag e adiciona a classe a ela;

	- `div.nova_classe*3`: cria TRÊS tags div com a classe;

	- se tiver um arquivo `.html` em branco, digitando `html:5` e apertando ENTER, o VS Code irá criar uma estrutura completa para o arquivo HTML com algumas tags já preenchidas (necessário trocar o lang, pois o padrão é en)

    - mais atalhos podem ser vistos no [cheat sheet](https://docs.emmet.io/cheat-sheet/).

- quando quiser mudar vários elementos, variáveis, texto, etc, de uma vez, clique no início de cada um **segurando o ALT**, e todos ficarão selecionados; alterando um, os outros também serão alterados;

	- também é possível utilizar CTRL+ALT+seta para selecionar múltiplas linhas, no mesmo nível de coluna (se houver linhas em branco, o início delas também será selecionado);

- ao selecionar uma palavra, é possível selecionar as outras ocorrências da mesma palavra com o CTRL+D. Com isso, é possível utilizar o comando para ir selecionando uma a uma, e depois editá-las ao mesmo tempo. Útil também com códigos e outros caracteres de pontuação;

- `CTRL + P`: abre um campo de busca para você digitar o nome de um arquivo que você quer abrir (útil para projetos muito grandes);

- `CTRL + '` (ou `CTRL + J`): abre/fecha terminal;

- `SHIFT + ALT + F`: dá um "beautify" no código, organizando os tabs;

- Extensão Live Server: é uma extensão que permite abrir arquivos HTML no navegador, habilitados com hot-reload (automaticamente recarrega a página depois de qualquer mudança salva no código, sem necessidade de dar F5 constantemente. Após instalado, aparece um link "Go Live" no canto direito da barra que fica na parte inferior do VS Code.

- VS Code dentro do GitHub: abrindo a página principal de um repositório no GitHub, ao apertar ponto no teclado irá abrir uma nova janela, que irá carregar o seu projeto dentro do VS Code no próprio navegador. É meio pesado para carregar, mas é muito legal caso você esteja em outra máquina e precise fazer algum pequeno ajuste no código de maneira mais rápida.

# Links úteis

- Site para pesquisar e usar caracteres Unicode (copyright, emoji, letras japonesas, etc.): https://unicode-table.com/en/;
	
- Emojipedia: https://emojipedia.org;

- [TinyPNG](https://tinypng.com/): remove metadados e comprime a imagem, otimizando o espaço.

# HTML

História:

- 1991: Tim Berners-Lee projeta o HTML (Hyper Text Markup Language);

- 1995: criação do CSS (Cascading Style Sheet);

**Tags estruturais**: São tags de informação, que organizam o documento ou a página. Exemplos: `<header>`, `<main>`, `<nav>`.

**Tags de conteúdo**: são as tags que incluem aquilo que a pessoa usuária irá ver na página/documento. Exemplo: `<p>`, `<h1>`, `<img>`, `<a>`.

**Elementos vazios**: são elementos HTML que não possuem nós filhos, ou seja, eles possuem somente a tag de abertura, mas **não possuem a tag de fechamento**. São chamados de *void elements* em inglês.

- isso é uma *tag* de abertura: `<p class='intro'>`

- isso é uma *tag* de fechamento: `</p>`

- isso é um *elemento* HTML: `<p class='intro'>Olá mundo!</p>`

- isso é um *elemento* vazio (e também a *tag* em si): `<input type='text'>`

- apesar desta distinção entre tag e elementos, vez ou outra elas serão usadas como sinônimos no texto;

- em XHTML, elementos vazios recebem uma barra ao final da tag. Essas são chamadas tags auto-fecháveis (*self-closing tags*). Por exemplo: `<input type='text'  />`. No HTML essa barra ao final de um elemento vazio é ignorada, ou seja, não é necessária, mas algumas pessoas desenvolvedoras preferem usá-las para deixar o código mais fácil de ler e válido como documento XML.

## Algumas tags e suas definições

**Use sempre o [site da MDN](https://developer.mozilla.org/) como referência!**

`<!DOCTYPE html>` : tag estrutural. Por padrão, DOCTYPE vem escrita em maiúsculo; escrever somente `html` indica ao navegador para aplicar a versão mais recente disponível do HTML. É a primeira tag a ser escrita na sua página HTML.

- se fosse uma versão anterior, teria que informar também o DTD (Document Type Definition). Exemplo HTML 4: 

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

`<html>`: tag estrutural. Engloba todo o conteúdo a ser renderizado na página. O atributo `lang` informa em qual idioma a página foi criada (o Chrome, por exemplo, usa essa informação para dar a opção de traduzir o conteúdo da página para outro idioma). Exemplo: `<html lang="pt-br">...<html>`.

`<meta charset="UTF-8">`: outra tag estrutural. Vai dentro da tag `<head>`. Passa informações ao navegador. O atributo `charset` informa qual conjunto de caracteres o navegador deve usar (UTF-8, por exemplo, possibilita um texto com acentos e outros caracteres utilizado na maioria dos idiomas).

`<title>` : é a tag de conteúdo que informa ao navegador o título para aparecer na aba do navegador, na barra de tarefas, etc. Vai dentro da tag `<head>`.

`<strong>` : tem significado **semântico** de que aquele conteúdo é um destaque; serve para dar importância ao conteúdo que essa tag engloba. Na tela, os navegadores costumam renderizar seu conteúdo em negrito.

`<em>` : tem significado **semântico** de dar ênfase ao conteúdo. Na tela, os navegadores costumam renderizar seu conteúdo em itálico.

`<b>` e `<i>` : **não** possuem significado semântico dentro do HTML. São tags somente de **renderização** do texto. Já `<strong>` e `<em>` são chamados de "estados lógicos", fazem a separação entre apresentação e conteúdo.

`<h1>` : tag que dá muita importância ao conteúdo dentro dela. Pode ser aplicada também em imagens. Deixa o navegador entender que aquela parte é muito importante semanticamente (esse significado semântico foi introduzido pelo HTML5). Embora nada impeça de você utilizá-la várias vezes, o recomendável é ser usada somente **uma vez por página**, por questões de SEO.

`<img>` : Um dos void elements, insere uma imagem à página.

- `alt`: é o atributo utilizado para descrever a imagem (principalmente para questões de acessibilidade);

- `srcset`: atributo que informa ao navegador outras resoluções da imagem, permitindo que ele decida qual baixar e mostrar ao usuário, dependendo da largura da tela. Exemplo:

```html
<img 
	src="imagem-alta-qualidade.png" 
	srcset="imagem-baixa-qualidade.png 340w, imagem-media-qualidade.png 1080w, imagem-alta-qualidade.png 2400w"
	alt="Descrição da imagem"
>
```

`<picture>`: essa tag possibilita ao navegador mostrar imagens diferentes, dependendo do tamanho ou orientação da tela, que podem ser definidos pelos atributos `srcset` e `media`. Ela precisa conter um ou mais elementos `<source>`, colocados no início, e um elemento `<img>`, que é colocado no final. O `<img>` mostrará a imagem escolhida em algum dos elementos `<source>` ou, caso nenhum deles sirva, irá exibir o conteúdo de seu próprio atributo `src` (o que é chamado de "*fallback*"). Essa abordagem com a tag `<picture>` otimiza o tráfego de dados sem a necessidade de uma regra no CSS para qual imagem mostrar. Exemplo:
	
```html
<picture>
	<source srcset="assets/img/banner-desktop.jpg" media="(min-width: 1024px)" >
	<img src="assets/img/banner-mobile.jpg" alt="Capa do Instalura">
</picture>
```

`<header>`: tag introduzida no HTML5, específica para o cabeçalho de uma página, uma tag mais semântica (mas ainda diz respeito ao conteúdo). Diz ao navegador que aquela é a primeira informação a ser apresentada. 

- Fica dentro da tag `<body>`; 

- não confundir com a tag `<head>`. 

- Pode ser utilizada como um menu da página. Pode também ser utilizada novamente dentro de uma tag `<section>` para sinalizar o título da section (junto com as tags `<h2>`, etc). O mesmo vale para a tag `<article>`.

`<main>`: tag semântica utilizada para indicar o conteúdo principal da página; deve ser única em cada página. Vai dentro da tag `<body>`.

`<section>`: tag que pode substituir uma `<div>` para ter um contexto mais semântico do seu conteúdo. 

- A `div` é só uma divisão **visual**, quando há elementos que você gostaria de alterar visualmente no CSS; nesse caso, a div engloba esses elementos facilitando a manipulação como uma coisa só. 

- A `section` tem um significado mais semântico; todo o conteúdo dentro dela é complexo e semanticamente estão relacionados, fazem um mesmo sentido. Exemplo: uma seção sobre um tópico, uma seção listando uma série de itens e imagens relacionadas, uma seção para contato, etc.

`<footer>`: tag introduzida no HTML5; representa o rodapé da página, informações que aparecerão no final da página.

`<nav>`: tag introduzida no HTML5, serve para indicar que aquele conteúdo é um menu de navegação.

`<iframe>`: ainda existe e é utilizado geralmente para acesso a algum serviço externo (exibir um mapa, um vídeo, algum conteúdo de rede social). É inline ( iframe significa "inline frame"), então, se quiser fazer alguma estilização visual (margin, por exemplo), é necessário envolvê-lo em um div e então aplicar o CSS (dica: deixe a width da div no mesmo tamanho da width do CSS inline do iframe).

### Formulários

`<input>`: toda tag de input tem que ter um tipo (apesar de que o `type="text"` seja padrão) e um identificador. Por padrão, seu display é inline-block.

- input do tipo `submit` já tem um texto padrão implementado pelos navegadores, caso nenhum texto seja atribuído. Esse texto padrão é baseado na língua definida para a página. No caso de pt-BR, é "Enviar";

- input do tipo `radio`: quando quero que somente uma opção seja selecionada, dentre várias, devo inserir o mesmo valor para o atributo name em todos os input do tipo `radio`;

- http://mobileinputtypes.com: esse site mostra visualmente como serão os inputs em um celular. Bom para verificar acessibilidade do seu site em um aparelho celular (tela pequena);

- `required`: atributo que vai dentro da tag `input` para indicar que aquele campo é obrigatório. Em formulários, o botão do submit não irá processar a requisição enquanto o campo não for preenchido. Também faz algumas validações como, por exemplo, se o e-mail foi preenchido no padrão de e-mail. Também pode ser utilizado na tag `<textarea>`;

- `placeholder`: é o texto que aparece esmaecido no input como sugestão do que deve constar no campo.

`<label>`: sempre anda em par com uma tag `<input>` ou `<textarea>`. Se quiser dar uma informação sobre o que o campo input significa, use a tag `label` (ao invés de uma tag `p`, `span`, `div`, etc.). Também inclua o atributo `for` com o mesmo nome dado para o `id` do input. 

 - Exemplo: 
 
 ```html
 <label for="id_input">Teste</label>
 ```
 
 - se eu clicar na label, automaticamente vou para o campo do input. Por padrão, seu display é inline.
	
- outra forma de relacionar um label a um input é colocar a tag `<input>` como filha da tag `<label>`. Com isso não preciso dos atributos `for` e `id`.

`<fieldset>`: é uma tag que pode ser usada nos formulários para agrupar um ou mais diferentes campos referentes a um assunto específico. Ao invés de usar uma `div`, podemos utilizar essa tag específica para este propósito

- `<legend>`: é uma tag para dar um título para o fieldset (como se fosse uma tag `<p>`); diferente da tag `<label>`, que está relacionada a um input específico.

### Tabelas

`<thead>`, `<tbody>`: são tags semânticas em tabelas para indicar o conteúdo que é o cabeçalho e o conteúdo que são os dados 

- há também tag para o footer da tabela `<tfoot>`

`<th>`: tag utilizada para o conteúdo de cada coluna do cabeçalho dentro da tag `<thead>` 

- substitui o `<td>`; 

- por padrão, o texto vem centralizado

Atributo `colspan`: funciona como a função de mesclar células do Excel, você informa o número de colunas e ela tratará essas colunas como uma célula única.	

# CSS

**Lembrando**: Use o [site da MDN](https://developer.mozilla.org/) como referência.

O portal da [W3Schools](https://www.w3schools.com/) também é outro que pode ser usado como referência, além de trazer tutoriais e um espaço para testar o código online.

CSS significa **C**ascading **S**tyle **S**heets.

Para aplicar estilos em seu HTML, a estrutura básica de um código CSS é:

```
seletor {
	propriedade_x: valor_1;
	propriedade_y: valor_2;
	...
}
```

- seletor: é a tag HTML, ou o atributo id (`#nome_do_id`), ou o atributo class (`.nome_da_classe`). Existem outros seletores avançados.

- propriedade: são as propriedades CSS que podem ser adicionadas, tais como cor, largura, altura, posicionamento, etc.

- valor: o valor atribuído à propriedade, que pode ser um número, uma cor, uma palavra reservada, podendo também incluir unidade de medida ou porcentagem.

## Convenções e boas práticas

- geralmente, o tamanho padrão da **fonte** no navegador é **16px**;

- uma boa **largura para a página** (levando em conta que será carregada em um computador) é **940px**, pois a maioria dos monitores tem uma resolução acima dessa e irá reproduzir bem dentro desse tamanho de 940px;

- `id` e `class`, segundo o instrutor: "Sempre que queremos falar de **estilo**, usamos uma **classe**. Sempre que queremos falar de **comportamento**, usamos um **identificador**";

- é melhor aplicar estilos a **classes** ao invés de tags HTML, pois as tags podem ser alteradas no decorrer do desenvolvimento, e aí os estilos aplicados a elas podem não corresponder mais ao que era esperado na apresentação da página. Aplicando o estilo à classe garante que, mesmo se a tag for alterada, os estilos se mantenham para aquela parte da página. Além disso, uma mesma classe pode ser utilizada por diferentes tags, todas recebendo o mesmo estilo.

- o nome da classe deve ser algo que remeta à função do elemento a ser estilizado: "titulo-principal", "banner", "input-padrao", "box-menu". O comportamento (como será alinhado, tamanho da fonte, cor) deve ser inserido no CSS e não no nome da classe.

	- isso, no entanto, não parece ser um padrão. Em alguns cursos foi dito o oposto.

## Como adicionar CSS

- inline: aplicação de CSS **dentro** de um elemento HTML, com o uso do atributo `style`:

```html
<h1 style="color:red; font-size:22px;">Introdução</h1>
```

- tag `<style>`: CSS dentro do `<head>`, tag de conteúdo:

```html
<head>
	<style>
		p {
			font-weight: 600;
			margin-left: 5px;
		}
	</style>
</head>
```

- arquivo externo: quando há somente um, por convenção é nomeado de `style.css`; é adicionado dentro da tag `<head>`:

```html
<head>
	<link rel="stylesheet" href="caminho até o arquivo css">
</head>
```

**Atenção**: Quando há mais de um arquivo CSS, eles são lidos **em ordem** e causam **sobrescrita** de propriedades (é o "cascading")

- se uma propriedade de um seletor receber um valor em um CSS, e a mesma propriedade para o mesmo seletor receber outro valor em outro CSS que aparece depois no HTML, este último valor irá sobrescrever o anterior.

## CSS Reset

Os navegadores, por padrão, já aplicam alguns estilos para as tags HTML. Por isso, é comum adicionarmos um arquivo `reset.css` para remover esses estilos. 

Deve ser o primeiro CSS linkado na página, para ser o primeiro a ser aplicado ao documento.

Exemplo muito utilizado: https://meyerweb.com/eric/tools/css/reset/reset.css

## Seletores

- `:root`: é um seletor de pseudo-classe que seleciona o elemento root da árvore que representa o documento (no HTML, ele seleciona o elemento `<html>`). É uma convenção utilizá-lo para declarar variáveis globais no CSS;

- seletor `*`: é um seletor global. Quando sozinho, seleciona **TODOS** os elementos do HTML. As propriedades dentro do seletor `*` são aplicadas a todos os elementos do documento. Pode também ser aplicado em conjunto com um seletor específico para selecionar todos os filhos diretos de um seletor (exemplo: `div.wrapper > *` irá aplicar estilos a todos os elementos *diretamente* descendentes de uma div com a classe wrapper);

- precedência na aplicação do CSS: !important >  inline style (HTML) > id > class > tag

	- se eu especificar a tag e sua classe, as precedências se somam. Ou seja `p.teste` > `.teste`;

	- o mesmo vale se usar dois seletores de tag: `form p` > `p`

### Pseudo-elementos e pseudo-classes 

São seletores CSS relativos a partes/estados do HTML, tais como `:hover`, `:active`, `:visited` (estes três quando se trata de links) e `:required` (em formulários). 

**Pseudo-classes** aplicam estilos a um elemento baseado em seu **estado** ou **relação a outros elementos**. São identificadas no CSS pelos dois pontos (`:`). 

**Pseudo-elementos** aplicam estilo a uma **parte específica** do elemento. São identificados pelo duplo dois pontos (`::`).

Hoje em dia é possível usar somente os dois pontos (`:`) para ambos os casos. 

Pseudo-classes podem selecionar **posições específicas** de algumas elementos, como `<li>`. Por exemplo: `:first-child`, `:last-child`, `:nth-child(numero_do_elemento)`

- `:nth-child` começa do 1;

- `:nth-child(2n)` pega todos os elementos pares; 

- outras combinações podem ser feitas

Pseudo-elementos permitem que você aplique um estilo a uma parte específica do elemento selecionado, exemplos:

- `::first-letter`

- `::before`
		
É possível até adicionar texto com os pseudo-elementos, mas estes textos não são selecionáveis na página.

### Seletores avançados

Foram incluídos no CSS3.

Possibilidade de selecionar **filhos/irmãos específicos** de um elemento, sem a necessidade de criar uma classe só para esse propósito. 

Possibilita aplicar efeito a tags-filhas específicas, sem alterar todas as tags semelhantes a ela (irmãs ou abaixo dela)

Exemplos:

- `main > p` : seleciona os elementos `p` que são **filhos diretos** de `main`. Sub-filhos (ou seja, filhos de `p`) **não** serão selecionados

- `img + p` : seleciona somente o **primeiro** elemento `p` que vem após um `img` (primeiro **irmão**)

- `img ~ p` : seleciona **todos** os `p` que vêm após um `img` (todos os irmãos)

	- os `p` não precisam estar imediatamente após um `img`; basta ser irmão;

- `img ~ p:not(#missao)` : seleciona todos os `p` irmãos de `img`, com **exceção** do `p` que tem o `id` `missao`.

## Propriedades

- `margin`, `padding`: permitem aplicar o estilo aos quatro cantos do elemento; a ordem é "**top right bottom left**"

	- se usar somente 2 valores, será "**top/bottom left/right**";

	- `auto` diz que a distância entre top/bottom ou left/right é a mesma, calculada automaticamente (o que centraliza o elemento no caso de left/right);

	- uma boa prática é manter as margens sempre em uma mesma direção. Ou seja, se usar margin-bottom em um elemento, continue usando margin-bottom para seus elementos filhos e irmãos, mantendo o "fluxo da direção" do espaçamento.

- `vertical-align`: por padrão, os elementos são alinhados para baixo (com a linha de baixo). Se quiser que fiquem alinhados com a linha de cima, utilize o `top`;

- `box-sizing: border-box`: garante que irá manter a largura/altura (geralmente neste caso utilizada em porcentagem) dentro do que foi definido, mesmo quando `padding`, `border` e `margin` são adicionados (o navegador vai se encarregar de manter a largura/altura que foi definida), o que evita o estouro de largura ao somar todos os espaçamentos.

	- Se a largura/altura não for definida, essa propriedade não faz nada;
	
	- Se o conteúdo não couber dentro da largura/altura definida, irá ocorrer um transbordamento (overflow), com o conteúdo saindo da "caixa" do elemento.

- `transition`: propriedade incluída no CSS3, possibilita uma animação de um certo tempo para fazer a transição entre alguma propriedade do CSS. Exemplo: 

```css
transition: 1s background
```

Caso eu tenha uma propriedade que altere a cor do background (por exemplo, uma cor diferente para o `hover`), a transição de uma cor para a outra ocorre gradualmente durante 1 segundo. 

- `transform`: outra propriedade incluída no CSS3 que possibilita aplicar algumas transformações geométricas (`rotate`) e espaciais (`scale`) aos elementos do HTML, sem necessidade de um código em JavaScript (como ocorria anteriormente). 

	- Pode ser utilizada junto com `transition` para transformações mais suaves. 
	
	- Pode ser utilizada mais de uma transformação, basta ir adicionando entre espaços;

- `z-index`: altera a "profundidade" do elemento. Essa propriedade permite dizer se um elemento está mais a frente ou atrás de outros elementos. 

	- Para funcionar, o elemento precisa ter a propriedade `position` como sendo diferente de `static`. 
	
	- Por padrão, todos os elementos estão com o `z-index auto`, ou seja, no mesmo plano.
	
	- Quanto **maior o valor** definido no z-index, **mais à frente** o elemento estará dos demais. O contrário acontece para valores negativos, deixando o elemento mais para trás.
	
	- Evite usar valores negativos, para não complicar a lógica. Opte por utilizar valores positivos e sequenciais para definir diferentes camadas para os elementos.

- `opacity`: propriedade que define opacidade de uma imagem, no intervalo de 0 a 1 (ou 0 a 100%); quanto mais próximo de **zero**, mais **opaca** fica a imagem (mais difícil de enxergar);

- `box-shadow`: propriedade que cria uma sombra na "caixa" do elemento. Você informa quantos pixels a sombra deve se deslocar no eixo X e o no eixo Y, podendo também informar quantos pixels deve ser o blur, e também quantos por quantos pixels ela deve se espalhar no elemento (spread). Por fim, você informa a cor da sombra.

	- é possível também adicionar mais de uma sombra dentro da propriedade, separando cada uma por vírgula;

	- inset: é uma keyword que pode ser usada para especificar que a sombra é interna ao elemento (não funciona em imagens);

	- se quiser centralizar a sombra, coloque 0 e 0 para os eixos X e Y;
	
	- Exemplo que cria uma sombra deslocada 10pixels para a direita (eixo X), 5px para baixo (eixo Y), aplicando efeito de blur por 30px e se espalhando pelo elemento por 20px, na cor preta.

```css
box-shadow: 10px 5px 30px 20px #000000
```

- `text-shadow`: sombra em texto; a lógica dos valores é a mesma do `box-shadow`;

## Funções

- `linear-gradient(cor1, cor2, ..., corN)`: função introduzida no CSS3 e utilizada com a propriedade `background` para criar um degradê de cores, começando na `cor1` e transicionando para a `cor2` e demais.
	
	- necessário no mínimo 2 cores;
	
	- também é possível alterar a **inclinação** do gradiente, adicionando em quantos graus deve começar; é o primeiro parâmetro da função;
	
	- e também é possível indicar a partir de que porção cada cor deve começar; por padrão, é feita uma divisão igualitária	

```css
background: linear-gradient(45deg, orange 30%, blue)
```

- `radial-gradient`: outra função, esta cria um degradê circular; neste caso não tem a opção de inclinação;

- `calc()`: mais uma função introduzida no CSS3, possibilita fazer cálculos nas propriedades. Parece ser usado mais quando estamos mexendo com dimensionamento e queremos que o resultado seja algo específico. Exemplo: 

```css
width: calc(40% - 26px);
```

## Variáveis

São entidades que irão **definir um estilo** que pode ser **reutilizado** em outras parte do CSS, evitando repetição e facilitando a manutenção e a leitura do CSS. Utilizado, por exemplo, para padronizar valores de cores, margin e padding, etc.

Recebem o prefixo `--` antes do nome. Exemplo: 

```css
--main-color: #FDFDFD;
```

Para usar a variável, utiliza-se a função `var()`. Exemplo

```css
color: var(--main-color);
```

Como informado anteriormente, é comum declarar variáveis no seletor `:root`, para que elas sejam globais (acessadas por qualquer outro seletor), mas também podem ser declaradas dentro de um seletor específico (e neste caso, o escopo da variável é o bloco do seletor, podendo ser acessada por ele e seus "filhos").

## Padrão de nomenclatura BEM 

**B**lock, **E**lement, **M**odifier.

Padrão para nomear classes no CSS, de modo a minimizar conflitos e organizar a estrutura do HTML e do CSS.

**Block**: pense nele como um componente, um bloco de HTML que poderá ser reutilizável em diferentes páginas ou seções. Será nomeado como uma classe normal.

**Element**: uma parte específica do bloco (uma tag, filhos de uma tag, etc). Será nomeado utilizando underline duplo (`__`) após o nome do block: `<nome-do-bloco>__<nome-do-elemento>`.

Modifier: define um estilo visual específico para um elemento, que modifica somente aquele elemento, sem modificar a todos do mesmo tipo. Será nomeado utilizando hífen duplo (`--`) após o nome do block ou do element: 

 `<nome-do-bloco>--<nome-do-modificador>`
 
 `<nome-do-bloco>__<nome-do-elemento>--<nome-do-modificador>` 

 Exemplo:

```html
<div class="bloco bloco--borda">`
	<p class="bloco__elemento" >...</p>
	<p class="bloco__elemento--erro">...</p>
</div>
```

## Cores

É possível utilizar diferentes sistemas para aplicar valor de cores:

- Nome da cor em inglês. Neste caso, o navegador que decide como é a cor informada com relação ao tom, saturação.

- sistema RGB: valores no intervalo 0,1,2,...,255;

	- possui um espectro de cores menor, mas também pode ser utilizado no CSS;

	- um valor para R (*red*, vermelho), outro para G (*green*, verde), outro para B (*blue*, azul);

		- `rgb(0, 255, 0)`;

	- é possível também adicionar um **quarto parâmetro**, que mexe na camada alfa (para opacidade)

		- `rgba(0, 255, 0, 0.3)` : adiciona uma opacidade de 30% à cor

- sistema hexadecimal: `0123456789ABCDEF`

	- a cor é representada por 6 dígitos, dois  para cada cor do RGB;

	- adiciona o prefixo `#` antes dos dígitos;

	- `#CCFF22`: vermelho terá o valor `CC`, verde será `FF` e azul, `22`

## Bordas

- `border`: declaração unificada para informar o tamanho, o estilo e a cor da borda (a ordem não importa):

```css
border: 2px solid #000000;
```

- `border-radius`: versão 3 do CSS possibilita fazer cálculos para obter uma borda arredondada (imagine que cada canto tem um círculo, conforme você aumenta o radius, maior é esse círculo e maior é o espaço arredondado da borda). Você pode definir um tamanho diferente para cada canto.

## Fontes

- https://fonts.google.com: repositório do Google com fontes públicas e já preparadas para a Web (fontes preparadas para se comportarem de maneira semelhante, independente do dispositivo ou sistema operacional). Você inclusive pode digitar um texto e verificar como ele irá ficar para diferentes fontes exibidas.

- fontes externas precisam ser importadas para seu código. Se escolhidas do repositório do Google, ele irá informar o código completo para importar a fonte selecionada (é como um import de um CSS) e como deve ser declarada a família de fontes no CSS.

## Medidas Relativas no CSS

São medidas calculadas com base em outra medida (seu tamanho é relativo a outro tamanho), o que garante uma flexibilidade no tamanho dos elementos, quando baseados em outro elemento principal 

- alterando o valor neste elemento principal, todos os outros que são relativos a ele serão recalculados (não preciso alterar manualmente), mantendo a consistência. 

- O pixel (`px`) é uma medida **absoluta**, ou seja, sua medida é fixa e será representada da mesma forma em qualquer tela.

Exemplos de medidas relativas: `em` e `rem` 

- o significado de `em` tem a ver com a letra M (que é pronunciado como "em" em inglês)
	
**No `em`**, os elementos filhos terão um tamanho de acordo com o elemento pai. Se o pai tem `16px`, `1em` no filho será igual a `16px`, `2em` será `32px` e assim por diante. 

- Se o filho tiver um filho, `2em` será `64px` (2*32) para este filho. 

- quando utilizado na propriedade `margin`, `1em` será o mesmo tamanho da fonte definida no elemento ou em elementos acima dele.

**No `rem` (root em)**, o tamanho da fonte é relacionado com o **elemento raiz** (o tamanho que for definido em `:root` ou `html` ou `body`). Com isso, os cálculos ficam mais fáceis, pois seu referencial é sempre o elemento raiz, independente se os outros elementos são filhos ou sub-filhos, etc.

## Alinhamento de elementos

### Valores da propriedade `display`:

- `block`: instrutor explica que pode ser entendido não como "bloco", mas sim "bloqueio". Esse valor *bloqueia* qualquer outro elemento de ocupar espaço na mesma linha; o tamanho será sempre 100% da página; é o padrão para muitas tags

- `inline-block`: ocupa somente o tamanho de seu conteúdo, mas permite mexer em sua largura e espaçamento

	- quando quero que dois elementos inline-block fiquem um do lado do outro, podemos mexer no `width` de cada um em porcentagem (por exemplo, 40% e 60%), porém, nesse caso, as tags precisam estar uma do lado da outra, sem espaços ou quebra de linha no HTML

- `inline`: ocupa o tamanho de seu conteúdo e não permite mexer com seu espaçamento interno/externo

	- pode, no entanto, aplicar `margin-left` e `margin-right` para dar um espaço entre elementos `inline`

### Flexbox 

É uma maneira de organizar os elementos, possibilitando remanejá-los de maneira mais flexível dentro de um espaço.

A propriedade `display: flex`, deve ser colocada no elemento pai (que será denominado "flex container"). Seus elementos filhos diretos serão chamados de "flex items".

- se os flex items possuírem filhos que também precisam se tornar flex, é necessário declarar a propriedade `display: flex` nesse flex item, para que ele passe a ser um container flex e seus filhos se tornem flex items, e assim por diante na cadeia de aninhamento de elementos.

Por padrão, quando utilizado, o flexbox faz com que os flex items se alinhem **lado a lado na horizontal** (row), sem quebra da linha (nowrap), no começo dessa linha (flex-start) e ocupando todo o espaço do flex container (stretch).

#### Propriedades adicionais

O flex container possui algumas propriedades CSS adicionais que possibilitam brincar com o posicionamento/alinhamento dos flex items:

- `justify-content`: propriedade que redistribui o espaçamento (espaço vazio) entre os flex items, de acordo com a direção (por padrão, a direção é linha, ou seja, horizontal);

- `align-items`: propriedade que organiza os flex items no eixo **contrário** ao do justify-content (por padrão, irá definir como serão alinhados na vertical);

- `justify-content` faz o alinhamento ENTRE os elementos; já o `align-items` alinha OS elementos em si;

- é possível também usar as propriedades de alinhamento em um flex item específico, adicionando a ele as propriedades `align-self` e `justify-self`.

#### Para saber mais

- Post com um guia bem completo das propriedades do Flexbox, com imagens e exemplos: 
	https://css-tricks.com/snippets/css/a-guide-to-flexbox/
- Cheat Sheet visual: 
	https://yoksel.github.io/flex-cheatsheet/
- Site que ensina as propriedade por meio de um joguinho: 
	https://flexboxfroggy.com

### Grid

O MDN tem uma [explicação completa](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout) do funcionamento do Grid.
	
O Grid é outra forma de remanejar os elementos dentro de um container (neste caso, um Grid Container), utilizando linhas e colunas, o que lembra o design de layout que antigamente era feito usando tabelas.

- `justify-content` e `align-items`: propriedades ainda existem e funcionam como no Flexbox com `flex-direction:row`;

- `grid-template-columns`: propriedade que informa quantas colunas haverá no layout e qual tamanho de cada uma
	
	- pode ser utilizada a unidade de medida "`fr`", que fraciona as colunas de maneira proporcional à largura disponível no Grid Container;
	
	- exemplo: `grid-template-columns: 1fr 1fr 1fr`. Irá criar 3 colunas de tamanho igual (1/3 da largura disponível para cada);
	
	- também pode utilizar `px`, `%,` etc.
	
	- tamanho "`auto`" irá ocupar o tamanho do maior elemento entre as linhas/colunas (a maior "célula")

- `grid-template-rows`: a ideia é a mesma que para colunas;

- quando é necessário repetir várias vezes linhas/colunas de mesmo tamanho, é possível utilizar a função `repeat`. Exemplo repetindo o valor `1fr` quatro vezes: 

```css
grid-template-columns: repeat(4, 1fr);
```

- é possível mesclar colunas adicionando ao elemento que será modificado a propriedade `grid-column: span <número_de_colunas_a_serem_mescladas>`
	
	- o mesmo para linhas, com o `grid-row`

- as propriedades `grid-row` e `grid-column` também permitem informar qual posição o elemento irá ocupar, bastando colocar o número da linha/coluna. Se também for informar o span, coloque uma / para separar as informações:

	- `grid-row: 1 / span 2;`:  elemento irá ocupar a primeira linha e também irá mesclar 2 linhas em seu espaço

- `gap`: propriedade que coloca um espaçamento entre cada elemento do grid (linhas e colunas)
	
	- é também possível informar valores diferentes para linhas e colunas, passando dois valores separados por espaço: `gap: <valor_linha> <valor_coluna>`;

	- se quiser espaçamento somente entre linhas e colunas, é possível utilizar as propriedades `row-gap` e `column-gap`;

	- essa propriedade também pode ser utilizada como o Flexbox ou em conjunto com a propriedade `column-count`.

- Dica do instrutor para pensar no layout da página com Grid: tirar um print do design da página (que pode ser, por exemplo, um design feito no Figma ou um arquivo PSD). Com esse print, utilizar o Paint ou algum editor de imagem e desenhar à mão linhas e colunas no print. Isso dará uma noção de quantas linhas e quantas colunas serão necessárias para ajustar todos os elementos em um grid.

#### `grid-template-areas`

É possível também organizar as linhas e colunas de uma maneira mais fácil de dar manutenção utilizando a propriedade `grid-template-areas` no grid container. 

Nessa propriedade, damos um nome para cada elemento do grid e escrevemos seus nomes nas colunas que queremos que eles ocupem espaço; esses nomes vêm entre aspas, separados por espaço. 

Podemos adicionar uma nova linha utilizando novamente as aspas e separando os nomes por espaço.

Exemplo:

```css
grid-template-areas:  
	"page-title page-title page-title" 
	"featured featured recent"
;
``` 	

Neste exemplo, temos um grid de 2 linhas e 3 colunas; o elemento "page-title" irá ocupar todas as colunas da primeira linha; na segunda linha, o elemento "featured" irá ocupar as duas primeiras colunas e o elemento "recent" irá ocupar a última coluna

Deve-se adicionar esses nomes como propriedade nos elementos que representam cada um. 

Exemplo:
	
```css
.main-page-title {
	grid-area: page-title;
}
```

Neste exemplo, elementos com a classe "main-page-title" irão receber o nome "page-title", que irá mapeá-lo nas áreas informadas no `grid-template-areas` do grid container.

## Posicionamento de elementos

A propriedade `position` permite informar como o elemento deve ser posicionado, com relação aos outros elementos próximos a ele. Valores:

- `static`: é a posição natural dos elementos, posição inicial;

- `relative`: utilizado quando se quer dar uma nova posição para o elemento, que será relativa à posição inicial dele; com isso, você poderá incluir outras opções CSS de posicionamento do elemento (ex. top, left, etc.);

- `absolute`: utilizado quando se quer dar uma nova posição ao elemento, mas não que seja relativa à sua posição inicial. Muda a posição inicial desse elemento. Será absoluto em relação a outra coisa (por exemplo, em relação ao cabeçalho, a um div específico). 

	- por padrão, será absoluto em relação à pagina inteira; 
	
	- se quiser que seja absoluto em relação a outro elemento (que seja pai ou esteja acima do elemento com absolute), este outro elemento deve ter uma `position: relative`.

	- O `absolute` "levanta" o elemento e ele pode ficar por cima de outros elementos. Outra solução é usar o `float`. O `float` também levanta o elemento, porém, o espaço que ele ocupava abaixo continua sendo dele, então não sobrepõe outros elementos abaixo dele da página (eles ocupam o espaço ao redor desse elemento com float, respeitando o espaço que ele ocupava antes)
		
		- se você quiser interromper os efeitos do `float`, isto é, para que a estrutura após um elemento com float volte ao comportamento normal, utilize `clear` no elemento em que você deseja que os efeitos do `float` sejam interrompidos. O `clear` irá indicar que aquele elemento não aceita compartilhar espaço com elementos flutuantes.

## Responsividade

Websites responsivos adaptam seu layout e funcionalidades de acordo com o tamanho e o equipamento em que estão sendo exibidos.

Há uma ferramenta muito útil nos navegadores que possibilita visualizar como o conteúdo da página será exibido para diferentes tamanhos de tela. Ela se encontra no Developer Tools (Ferramentas do Desenvolvedor) presente nos navegadores.

- o atalho para acesso ao DevTools geralmente é a tecla `F12`;

- você também pode tentar abrir o DevTools clicando com o botão direito em algum elemento da página e selecionando a opção `Inspecionar elemento`. 

O ícone para visualizar o conteúdo em diferentes tamanhos (dois retângulos, um maior outro menor, simbolizando um tablet e um celular) costuma ficar no topo esquerdo do DevTools, ao lado do ícone de inspeção de elemento (uma setinha do mouse em cima de um quadrado). Veja um exemplo na imagem abaixo, capturada no navegador Google Chrome:

![a imagem mostra o DevTools aberto no Google Chrome, com um quadrado vermelho destacando o ícone que possibilita mudar o layout da página simulando outro dispositivo ou tamanho de tela](https://github.com/zingarelli/anotacoes-estudo/assets/19349339/d3773a68-f103-4860-bac1-04179e679288)

É possível ajustar diferentes tamanhos, bem como dimensões pré-definidas para alguns celulares, e também a posição (vertical ou horizontal).

### Media queries

Para **informar** ao navegador qual largura deve usar para diferentes telas, utilizam-se os atributos name e content da tag `<meta>`:

```html
<meta name="viewport" content="width=device-width">
```

O código acima informa ao navegador que o tamanho da viewport será igual ao tamanho da largura do dispositivo.

- `viewport` significa a área da janela do navegador em que é possível ver o conteúdo web (e que pode ser maior ou menor do que o tamanho da janela do navegador).

- no atributo `content` é possível informar diferentes propriedades além da width, separadas por vírgula.
	
Para **estilizar** no CSS, utilizamos as chamadas "media queries", em que especificamos, por exemplo, uma largura-alvo máxima e informamos qual CSS será utilizado para este caso:

```css
@media screen and (max-width: 480px){
	...
}
```

O código acima irá aplicar os estilos para telas de até no máximo 480px.

### Dicas para layout mobile

- celulares possuem uma DPI (densidade de pixels por polegada) menor do que uma tela de computador, cerca de 320 DPI de largura. DPI é diferente de resolução. O celular pode ter uma resolução enorme, mas é a DPI que irá informar quantos pixels cabem na largura e altura. Por isso, a resolução irá se adaptar à DPI. 

- Precisamos trabalhar com a DPI para adaptarmos o site a uma tela de celular.