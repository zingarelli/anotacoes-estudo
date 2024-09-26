# Tailwind CSS

Anotações sobre o framework [Tailwind CSS](https://tailwindcss.com/).


## Créditos

Essas anotações são baseadas no curso [Tailwind CSS: estilizando a sua página com classes utilitárias](https://cursos.alura.com.br/course/tailwind-css-estilizando-pagina-classes-utilitarias), oferecido pela [Alura](https://www.alura.com.br) e ministrado pela [Beatriz Moura](https://www.linkedin.com/in/beatrizmouradev/). 

Os arquivos iniciais do curso foram disponibilizados [no repositório deste link](https://github.com/alura-cursos/aluranewsletter/tree/49ad22e834517cd7204f1f723a1b8a0a0c1178ce). 

## Sobre o Tailwind CSS

É **framework CSS**. No caso, ele possui ferramentas e padrões CSS que podem ser aplicados a uma página pelo uso das chamadas **"classes utilitárias"**, aplicando regras de estilo e layout pré-definidos (mas que também podem ser customizáveis). Dessa forma, ele **otimiza e simplifica** a inserção de estilos a uma página sem a necessidade de escrever várias linhas de código CSS ─ pode ser até que você não precise sair do arquivo HTML do projeto, pois somente utilizando as classes utilitárias já é possível aplicar diversos estilos.

Segundo a [documentação](https://tailwindcss.com/docs/installation), o Tailwind irá verificar os arquivos do projeto, procurando pelas classes utilizadas no HTML, para então gerar os estilos correspondentes em um arquivo CSS estático.

Baseado nas [pesquisas do State of CSS](https://2023.stateofcss.com/en-US/css-frameworks/), desde 2019 o Tailwind aparece em 1º lugar dentre os frameworks CSS no quesito "retenção" (quando a pessoa experimenta uma tecnologia e a continua utilizando). No quesito "uso" (quando a pessoa está utilizando a tecnologia), desde 2021 o Tailwind permanece na 2ª posição, atrás do Bootstrap.

### Bootstrap x Tailwind

O [Bootstrap](https://getbootstrap.com/) é outro framework CSS bastante popular. Uma das diferenças entre ele e o Tailwind é no **resultado do uso das classes**: enquanto no Bootstrap o uso das classes predefinidas geram estilos padronizados para **componentes** completos (por exemplo, um botão, um menu, modal, etc), o Tailwind faz o uso de classes mapeadas a **propriedades** do CSS (as tais "classes utilitárias"), permitindo uma **customização mais granular**. 

Em outras palavras, a classe no Bootstrap já define um estilo completo de um componente (exemplo: `class="btn btn-primary"`); caso você queira **customizar, é necessário sobrescrever o CSS criado pelo Bootstrap**. Já no Tailwind, cada classe adicionada modifica uma propriedade específica do componente, sendo que o conjunto dessas classes como um todo, escolhidas por você, é que define o estilo final (exemplo: `class="bg-green-500 text-white text-center"`). Entra aquele custo-benefício entre ter um componente pronto porém mais genérico, ou um componente mais customizado porém cheio de classes pré-definidas.

Mais diferenças, vantagens e desvantagens de cada um, podem ser visto [neste artigo do instrutor Matheus Alberto](https://www.alura.com.br/artigos/tailwind-framework-bootstrap-tailwind).

## Instalação no HTML

A [documentação do Tailwind](https://tailwindcss.com/docs/installation) possui guias para instalação do framework em diversas tecnologias, como Next, Laravel, Angular, etc. Para páginas simples, envolvendo somente HTML, é possível "instalar" o Tailwind adicionando um script no `<head>` do arquivo HTML: 

```html
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>
</head>
```

Ao adicioná-lo, um conjunto de estilos base, chamado Preflight, é aplicado ao HTML (semelhante a um CSS Reset).

## Classes utilitárias

As chamadas "classes utilitárias" são classes pré-definidas pelo Tailwind (ou criadas por você), que são **adicionadas ao atributo `class`** de um elemento HTML. O Tailwind mapeia essas classes para propriedades específicas do CSS. Por exemplo: a classe utilitária `bg-gray-50` está mapeada para a propriedade CSS `background-color: rgb(249 250 251);`.

A [**documentação do Tailwind**](https://tailwindcss.com/docs/) será sua melhor fonte para pesquisar as classes utilitárias e as respectivas propriedades CSS mapeadas. Use e abuse do quick search (`Ctrl + K`) para procurar por alguma propriedade CSS e verificar qual classe utilitária a representa.

### Modificadores

Podemos também aplicar as classes utilitárias a pseudo-classes e pseudo-elementos. Para isso utilizamos modificadores, que são palavras adicionadas antes da classe utilitária, separadas por `:`, ou seja: `modificador:classe_utilitaria`.

Exemplo adicionando cores diferentes ao texto e ao background durante o hover em um botão:

```html
<button class="hover:text-red-400 hover:bg-gray-500">Cadastrar</button>
```

Novamente, a [documentação do Tailwind](https://tailwindcss.com/docs/hover-focus-and-other-states) tem uma seção completa sobre uso de modificadores e exemplos.

### Estilização baseada no elemento pai ou irmão

O Tailwind possibilita aplicar estilos a um elemento baseado no estado de um elemento pai ou irmão. 

Imagine, por exemplo, que você tem uma div e dentro dela outros elementos, entre eles um input. Aí você deseja alterar a borda deste input durante o hover em qualquer parte da div. O Tailwind possibilita isso utilizando a classe `group` no elemento pai e adicionando `group-` ao modificador na classe do elemento que deve ser estilizado.

O exemplo abaixo adiciona uma borda azul a um input quando o efeito de hover acontece em qualquer elemento dentro da div pai do input.

```html
<div class="group">
    <h2>Passe o mouse sobre mim</h2>
    <input class="border group-hover:border-cyan-800" type="text">
</div>
```

O mesmo pode ser feito entre irmãos, só que usando a classe `peer`.

Mais sobre isso pode ser [visto na documentação](https://tailwindcss.com/docs/hover-focus-and-other-states#styling-based-on-parent-state).

## Mobile first

O Tailwind CSS segue o **princípio mobile-first**, ou seja, os estilos padrão são aplicados primeiro para dispositivos móveis (telas menores), e depois você pode adicionar modificações para telas maiores usando prefixos de breakpoints específicos do Tailwind (ou customizados por você).

Breakpoints padrões:

| Prefixo | Largura mínima | Equivalente CSS |
|--|--|--|
| sm | 640px | @media (min-width: 640px) { ... }
| md | 768px | @media (min-width: 768px) { ... }
| lg | 1024px | @media (min-width: 1024px) { ... }
| xl | 1280px | @media (min-width: 1280px) { ... }
| 2xl | 1536px | @media (min-width: 1536px) { ... }

Abaixo vemos um exemplo de aplicação de responsividade a uma tag `<main>`. O código irá adicionar um `display: flex`, posicionando os elementos-filhos de `<main>` em coluna. Por meio do prefixo `lg`, informamos que o posicionamento será em linha para telas com uma largura a partir de 1024 pixels. 

```html
<main class="flex flex-col lg:flex-row">
```

## Numeração para tamanhos

Propriedades que definem tamanhos (por exemplo, margin, padding, etc) possuem uma **numeração padrão** utilizada no Tailwind. O valor 1 representa 4px ou 0.25rem. O valor 2 representa 8px ou 0.5rem. E por aí vai.

Fica fácil mapear o tamanho definido pelo Tailwind e sua representação no CSS pelas seguintes **regras**:

- para obter o valor em pixel, multiplique por 4. Exemplo: `p-3` é um padding de 12px ($3 \times 4$);
- para obter o valor em rem, divida por 4 (ou multiplique por 0.25): Exemplo: `p-4` é um padding de 1 rem ($4 \div 4$).

O inverso também é válido: se você quer uma margem de 24px, utilize a classe utilitária `m-6` ($24 \div 4$); se quer um margin-bottom de 2rem, utilize `mb-8` ($2 \times 4$).

> Existem outras numerações, como **frações** (para representar porcentagem), expressões como **`sm`** e **`xl`** (representando padrões para small e extra-large), palavras como **`light`** e **`bold`** (representando peso de fonte), etc. Nesses casos, consulte a documentação para saber suas representações em CSS.

## Customização

É possível criar suas próprias classes utilitárias ou estender definições do próprio Tailwind para, por exemplo, adicionar novas cores, fontes, etc, ou até mesmo modificar as definições padrões.

Podemos fazer isso dentro do próprio HTML, adicionando um novo script e nele criar a variável `tailwind.config` e atribuir a ela um objeto com as customizações. 

> **Atenção**: esse script deve vir **após** o script de instalação do framework.

Exemplo adicionando novas cores e família de fontes: 

```html
<head>
<script src="https://cdn.tailwindcss.com"></script>
<script>
    tailwind.config = {
        theme: {
            // o extends preserva os valores default
            // do Tailwind e permite criar novos 
            extend: {
                // adicionando novas cores
                colors: {
                    azul: {
                        claro: '#C5DFFF',
                        escuro: '#061E3C',
                    }
                },
                // adicionando nova font-family
                fontFamily: {
                    inter: ['Inter', 'sans-serif']
                }
            }
        }
    }
</script>
</head>

<!-- Usando as customizações -->
<body class="bg-azul-claro font-inter">
```

> Em projetos maiores, podemos adicionar essas customizações em um arquivo separado `tailwind.config.js`.

Um guia sobre customização e opções que podem ser configuradas podem ser vistas na [documentação do Tailwind](https://tailwindcss.com/docs/configuration).

## Exemplo completo

Abaixo temos um exemplo completo de uma página utilizando algumas classes utilitárias do tailwind. Os comentários indicam o que cada classe adiciona de estilo CSS.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        // customizando o Tailwind
        tailwind.config = {
            theme: {
                // o extends preserva os valores default 
                // do Tailwind permite criar novos
                extend: {
                    // nova cor
                    colors: {
                        azul: '#061E3C'
                    }
                }
            }
        }
    </script>
    <title>Olá Tailwind!</title>
</head>

<!-- aplica um linear-gradient para a direita, começando 
<!-- com uma cor roxa e indo para uma cor azul -->
<body class="bg-gradient-to-r from-purple-400 to-blue-600">

    <!-- aplica um background-color branco; usa flexbox -->
    <!-- com elementos posicionados em coluna, alinhados -->
    <!-- e justificados no centro. Em telas maiores, os  -->
    <!-- elementos são posicionados em linha. -->
    <!-- Largura de 80%, margem horizontal auto e vertical -->
    <!-- de 160px (10 rem), padding 48px (3 rem), -->
    <!-- box-shadow predefinido pelo tailwind, -->
    <!-- opacity de 50%. -->
    <main
        class="bg-white flex flex-col justify-center items-center lg:flex-row w-4/5 mx-auto my-40 p-12 shadow-2xl opacity-50">
        
        <!-- largura de 40% (ou 10% para telas maiores) -->
        <img class="w-2/5 lg:w-1/5" src="./image/logo-tailwind.png" alt="Logo Tailwind CSS">

        <!-- fonte 36px (ou 60 para telas maiores) -->
        <!-- font-weight 900 e texto centralizado -->
        <!-- cor customizada em tailwind.config -->
        <h1 class="text-4xl lg:text-6xl font-black text-center text-azul">Olá Tailwind CSS!</h1>

        <!-- largura de 40% (ou 10% para telas maiores) -->
        <img class="w-2/5 lg:w-1/5" src="./image/logo-tailwind.png" alt="Logo Tailwind CSS">
    </main>
</body>
</html>
```

> Um exemplo ainda mais completo, incluindo estilizações de elementos HTML e SVG, além de efeitos de hover, focus e até animações, pode ser visto no [neste repositório](https://github.com/zingarelli/alura_newsletter_tailwind).
