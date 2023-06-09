# Anotações sobre acessibilidade em páginas Web

## Instrutor 

[Natan Souza](https://www.linkedin.com/in/designernatan/) - curso ["Acessibilidade web parte 1: tornando seu front-end inclusivo"](https://cursos.alura.com.br/course/acessibilidade-web-front-end)

## Leitores de tela

- NVDA (mais utilizado);

- JAWS (é pago);

- VoiceOver (Mac).

- Windows (a partir do 10) já vem com o Narrator instalado nativamente. Pode ser utilizado em combinação com o NVDA para utilizar a voz do Narrator (que é melhor) no NVDA.

## Links Úteis

- https://webaim.org/projects/screenreadersurvey9/ : Survey sobre acessibilidade, em que usuários com algum tipo de disability informam quais navegadores mais usam, tipo de SO, tipo de leitor de tela, etc.

- https://www.estudoinclusivo.com.br: pesquisa brasileira sobre leitores de tela (já tem até de 2022);

- https://www.tpgi.com/basic-screen-reader-commands-for-accessibility-testing/ : atalhos de teclado para diferentes tipos de leitores de tela.

## NVDA

Pessoas com deficiência visual utilizam a tecla H para ler os headings da página (`h1`, `h2`, ...) para ter uma "visão geral" do que se trata a página. Por isso, é importante utilizar os headings da maneira correta, para informar os textos mais importantes da página. 

- segundo a pesquisa do site [estudoinclusivo](https://www.estudoinclusivo.com.br/), outras opções utilizadas, além do heading, é navegar item por item (seta para baixo) e pelos links (`Tab` ou tecla `K`).

## Boas práticas HTML

Adicionar `lang="pt-br"` à tag `<html>`, para que o leitor de tela fale em português.

- O atributo `lang` também pode ser colocado em outras tags. Caso em alguma parte da página tenha alguma citação ou texto em outro idioma, é possível utilizar o atributo `lang` e setar para que o leitor de tela leia no idioma correto.

O tributo `alt` é um texto alternativo para quando a imagem não carrega, mas que também é utilizado pelo leitor de tela para "ler" a imagem. Como será lido, utilize o português corretamente neste atributo, incluindo acentuação e pontuação.

- O NVDA fala "gráfico" toda vez que lê uma imagem, então fica redundante escrever no alt "foto" ou "imagem". Você pode descrever somente o que a imagem significa;

- Não utilize o atributo `title` na imagem em conjunto com o `alt`, pois o leitor de tela irá ler os dois;

- Iconografia (ícones sem valor semântico) normalmente tem caráter visual, e o que for visual temos que fazer no CSS. Seja usando um `background-image` ou um `::after`/`::before`. Ou seja, quando a imagem **não for relevante**, **não use** a tag `<img>`; use talvez uma div e adicione a imagem via CSS.

    - Caso não seja possível, devido a alguma questão de design ou qualquer outra coisa, você pode deixar o `alt` vazio para que o leitor de tela ignore a imagem. Necessário verificar em cada leitor como ele faz isso (um pode ser somente o atributo `alt`, outro pode ser `alt=""`, etc)

Para elementos `<sgv>`, você pode utilizar a tag `<title>` dentro do SVG para substituir o atributo `alt`.

- Caso o SVG esteja dentro de uma tag `<a>`, você pode utilizar o atributo `title` da tag `<a>` para descrever o SVG, pois o leitor irá ler o `title` do link neste caso.

No HTML5 existem também as tags `<figure>` e `<figcaption>` que funcionam parecido com o `alt`; 

- em uma aula, o instrutor menciona as WAI-ARIA, que seriam como uma "expansão" do HTML5, adicionando novos atributos aos elementos. Nelas, existe o atributo `aria-labelledby`, que você pode usar para indicar o `id` de um `figcaption`, que será então utilizado pelo leitor de tela para descrever a imagem. O `alt` será ignorado pelo leitor (e será usado para o caso de, por exemplo, a imagem não carregar).

Tags de `<input>` e `<label>` precisam seguir aquela combinação de atributos (`for` e `id`) para que o leitor de tela consiga associar qual label pertence a qual input.

Propriedades `display: none` e `visibility: hidden` no CSS escondem um elemento visualmente e **também do leitor de tela**. Necessário tomar cuidado com questões de acessibilidade, caso os elementos precisem ser lidos.

- Uma solução é utilizar `position` e `left`/`right` e marcar para uma posição infinita (`-9999px`). Não irá aparecer visualmente, mas o leitor conseguirá ler. Indicado pela [WebAim](https://webaim.org/techniques/css/invisiblecontent/).

- Parece que a melhor solução é `aria-hidden="true"` (ver [tópico no fórum](https://cursos.alura.com.br/forum/topico-escondido-mas-correto-178132))

Crie um link "Pular para conteúdo principal" logo no começo da página, para que seja o primeiro elemento a ser lido quando o usuário der `Tab`. Você pode esconder esse link para que seja lido somente pelos leitores de tela, ou pode fazer alguma animação em que esse link apareça somente quando o Tab é pressionado (o Facebook e LinkedIn fazem isso).

- Na tag `<a>`, você pode usar um id para pular para uma seção da própria página. Cuidado ao escolher qual elemento receberá o `id` para ser o conteúdo principal, pois o leitor de tela irá ler todo o conteúdo dentro desse elemento.

- Alguns leitores de tela já possuem atalhos que vão para o conteúdo que é marcado como principal. Neste caso, você deve incluir o atributo `role` na tag que será a principal

```html
<alguma_tag role="main">
```

Cuidado com o atributo `disabled` (utilizado, por exemplo, para desabilitar um campo de input). Ele não será lido pelo leitor de tela. 

- Uma opção é utilizar o atributo `readonly`. Tem um funcionamento semelhante ao `disabled` e é lido pelo leitor de tela (inclusive informando ao usuário que é somente leitura).

Zoom no celular: cuidado para não colocar o atributo `user-scalable=no` ou `maximum-scale=1.0` na tag ``<meta>``, pois isso desabilita/limita o zoom no celular do usuário, o que pode prejudicar a acessibilidade.

O HTML5 também possui tags para vídeos, inclusive tag para incluir legendas e outras opções para melhorar a acessibilidade. 