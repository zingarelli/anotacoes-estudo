# Next.js

## Créditos

Estas anotações são baseadas nos cursos da Alura para a formação [Next.js 14: desenvolvendo aplicações robustas com alta produtividade](https://www.alura.com.br/formacao-next-js-14-aplicacoes-robustas-alta-produtividade), ministrados pela [Patrícia Silva](https://github.com/gss-patricia) e pelo [Vinicios Neves](https://www.linkedin.com/in/vinny-neves/).

Alguns complementos foram inseridos baseados na própria [documentação do Next](https://nextjs.org/docs) (em inglês).


## Introdução

O [Next.js](https://nextjs.org) é um **framework construído em cima do React**. Ou seja, ele dá poderes adicionais ao React para construção de aplicações Web, proporcionando muita **otimização** da aplicação e **ferramentas para facilitar o building/deploy** do produto final.

- Atualmente (2023), o Next.js é um dos frameworks [recomendados pelo React](https://react.dev/learn/start-a-new-react-project) para criação de um novo projeto React. **O CRA já não aparece na recomendação e tende a ser preterido pelos desenvolvedores**;

- O framework foi criado e é mantido pela Vercel;

- A documentação possui um curso introdutório para quem está nessa jornada do React para o Next: https://nextjs.org/learn.

> Nas palavras do Vinny em seu [artigo sobre o Next](https://www.alura.com.br/artigos/next-js): "Vale reforçar: **o Next.js não substitui o React; ele o potencializa**. Ele oferece ferramentas e funcionalidades que tornam a criação de sites e aplicações web mais rápidas, acessíveis e eficientes, simplificando muitos dos processos que, no React, demandariam soluções adicionais ou mais complexas."

O framework possui sua **própria solução para roteamento**, mas também é possível utilizar outras bibliotecas, como a React Router. 

- na verdade, o Next oferece duas soluções de roteamento, a Pages Router, que é mais antiga, e a App Router, a mais recente, voltada para aproveitar as features mais novas do React. Essas anotações são baseadas na **App Router**.

Em questões de otimização, é possível fazer uma renderização híbrida dos componentes, com aqueles que exigem interatividade sendo renderizados no cliente (no navegador, como estamos acostumado com aplicações React), e outros **componentes sendo renderizados pelo servidor**, que entrega o HTML pronto ao cliente - isso ajuda também na indexação da página pelos buscadores (questão de SEO). Há também otimização no uso de imagens e fontes.

- A documentação do Next.js usa os nomes **"Client Components"** e **"Server Components"** para diferenciar os componentes renderizados pelo cliente (CSR - Client Side Rendering) e os renderizados pelo servidor (SSR - Server Side Rendering).

## Instalação

Para as versões atuais do Next.js (versão 13), recomenda-se o Node versão 16.8 ou superiores.

É possível fazer uma [instalação manual](https://nextjs.org/docs/getting-started/installation#manual-installation) de um projeto Next, mas também há a opção automatizada, por meio de um processo interativo utilizando o npx: 

    npx create-next-app@latest

O comando acima irá sempre utilizar a versão mais atual estável do Next. Caso você queira utilizar uma versão específica, é só substituir o `latest` pelo número da versão. Exemplo criando um projeto na versão 14 do Next (pega a sub-versão mais estável disponível para a versão 14):

    npx create-next-app@14

> Use o comando `create-next-app` na pasta onde você deseja salvar o projeto.

Com isso, um projeto Next é criado já com alguns arquivos e pastas. Para rodá-lo, digite:

    npm run dev

O projeto irá rodar em http://localhost:3000

> Em projetos React, estamos acostumados a verificar erros na aplicação olhando o Dev Tools do navegador. Já no Next, como algumas coisas serão feitas pelo servidor, também precisamos ficar de olho no console onde rodamos o `npm run dev`, pois é nele que serão informado erros do servidor.

## Estrutura de um projeto Next

A formação baseia-se na **versão 14** do Next, então alguns conceitos, códigos e estruturas podem mudar em versões diferentes. 

Exemplo da estrutura de pastas criada pelo `create-next-app`:

```
public
src
|── app
    |── globals.css
    |── layout.js
    |── page.js
    |── page.module.css
jsconfig.json
next.config.mjs
package-lock.json
package.json
README.md
```

Uma descrição sobre alguns arquivos:

- jsconfig.json: configurações do JavaScript. Caso você tenha marcado "No" para a pergunta "Would you like to customize the default import alias (@/*)" durante o `create-next-app`, este arquivo terá uma configuração em que, toda vez que você iniciar um import com `@`, o Next irá entender que deve procurar a partir da pasta `src`, o que facilita na hora de informar o caminho para um arquivo. Exemplo: `import { Aside } from '@/components/Aside'`. O componente `Aside` será buscado em `src/components/Aside`.

- globals.css: estilos globais da aplicação;

- layout.js: template básico de **toda** a aplicação. Dentro dele é implementado o componente `RootLayout`. Aqui você pode definir tudo aquilo que é comum de aparecer em todas as páginas (os metadados do html, por exemplo, e até as tags `html` e `body`);

- page.js: página inicial da raiz da aplicação;

- page.module.css: estilos específicos para `page.js`. Por padrão, o Next oferece CSS Modules.

> Os arquivos `layout.js` e `page.js` (ou `.jsx` ou `.tsx`) são os necessários para começar a aplicação Next.

> Uma dica de organização é criar uma pasta `components` dentro de `src` para guardar seus componentes. 

### Pasta `public`

Quando criada, esta pasta será entendida pelo Next.js como sendo o local em que serão armazenados os **arquivos estáticos** (imagens, fontes, arquivo robots.txt, etc). Esse arquivos ficarão públicos, ou seja, acessíveis diretamente da raiz do projeto (da base URL, sem necessidade de incluir `public`).

Por exemplo, caso você queira utilizar uma imagem que está em `public/images/logo.svg` como sendo o `src` de uma tag `img`, você passa o caminho relativo dessa forma: `/images/logo.svg`. 

Da mesma forma, caso queira acessar o arquivo pelo navegador, basta informar: `http://www.nomedoseusite.com.br/images/logo.svg`.

## Otimizando imagens

O Next oferece um componente `Image` que pode ser utilizado em substituição ao elemento `<img>` do HTML. A vantagem de utilizar o `Image` é que o Next se encarrega de otimizar automaticamente a imagem para você, melhorando o carregamento da página, por exemplo. 

```js
import logo from './logo.png';

export const Aside = () => {
    return (<aside>
        <Image src={logo} alt="Logo do Code Connect" />
    </aside>)
}
```

Para aproveitar ainda mais das otimizações, você pode fazer o import da imagem (`import logo from './logo.png'`) ao invés de passar o caminho para ela (`scr="/images/logo.png"`, por exemplo). Pelo import, o Next automaticamente determina o width e height da imagem. Caso você opte por enviar o caminho da imagem (ou a URL de uma imagem externa), você obrigatoriamente precisa passar as dimensões nas props `width` e  `height`.

### Imagens de fontes externas

Quando você passa uma **URL** para a prop `src`do componente `Image`, por questões de segurança, essa URL tem que estar configurada como **fonte confiável** pelo Next para o download de imagens. Essa configuração é feita no arquivo `next.config.js` (ou .mjs), criado automaticamente quando você usa o `create-next-app`.

Anteriormente, a configuração era feita por uma propriedade `domains`, que agora está depreciada e foi substituída pela `remotePatterns`, que oferece mais opções de configuração. 

Exemplo liberando tudo o que vier do domínio `raw.githubusercontent.com`. Você pode usar `pathname` e `port` para liberar o conteúdo de somente parte do caminho do domínio:

```js
const nextConfig = {
    images: {
        // jeito antigo, deprecated
        // domains: [
        //     'raw.githubusercontent.com'
        // ],

        // jeito atual com remotePatterns
        remotePatterns: [
            {
                protocol: 'https',
                hostname: 'raw.githubusercontent.com',
                port: '',
                pathname: '**', // tudo o que estiver dentro de hostname
            },
        ],
    }
}
```

## Otimizando fontes

O Next também entrega uma solução para otimizar a importação de fontes, incluindo aquelas disponíveis no Google Fonts.

Existem fontes chamadas de "variantes". Este é um formato de fonte que consegue se adaptar automaticamente a diferentes "pesos" ou "estilos" de fonte. [Neste link](https://fonts.google.com/variablefonts) há a lista de fontes variantes no Google Fonts. Para fontes que **não** sejam variantes, será necessário adicionar a propriedade `weight` no import da fonte, indicando quais pesos queremos importar, por exemplo.

Exemplo importando a fonte `Prompt` (não variante). Observe que a fonte importada é utilizada como uma função, que recebe as configurações como um objeto. Essa função retorna um objeto e utilizamos a propriedade `className` desse objeto para fazer o link entre a fonte e o HTML onde ela deve ser aplicada ─ no caso do exemplo, estamos aplicando a fonte à aplicação toda (atributo `className` da tag `<html>`). Observe que dessa forma não precisamos configurar o `font-family` no CSS global:

```jsx
import { Prompt } from 'next/font/google';

// obtendo a fonte via uma função
const prompt = Prompt({
  weight: ['400', '600'],
  subsets: ['latin'], // importando somente os caracteres latinos
  display: 'swap' // mostre a fonte fallback enquanto a fonte externa é carregada
})

export default function RootLayout({ children }) {
  return (
    <html lang="pt-br" className={prompt.className}>
      {/* restante do código */}
    </html>
  );
}
```

Para fontes cujo nome é mais de uma palavra (Roboto Mono, por exemplo), você usa o underscore `_` entre as palavras: `import { Roboto_Mono } from 'next/font/google'`.

## App Router

### Introdução

A partir da versão 13, o Next.js introduziu o conceito de App Router (anteriormente, isso era feito pelo [Pages Router](https://nextjs.org/docs/pages/building-your-application/routing)). 

O roteamento é baseado em um sistema de arquivos (*file-system based router*) e **começa dentro da pasta `app`**, considerada a raiz da aplicação (a homepage). Novas rotas são definidas por meio da **criação de subpastas** dentro de `app`. 

É possível criar **rotas aninhadas**, por meio da criação de outras subpastas dentro das subpastas, mantendo a mesma estrutura.

Cada pasta e subpasta corresponde a um **segmento da URL** (segumento de rota) da aplicação. 

A página que será exibida por uma rota é incluída em um arquivo **`page.js`** (ou `.jsx` ou `.tsx`) na pasta para aquela rota. É esse arquivo que o Next irá procurar quando precisar exibir a página para a rota acessada na URL.

Por exemplo, para a estrutura a seguir

```
app
|── profile
    |── settings
      |── page.js
```

a URL a ser acessada é `seu-site.com.br/profile/settings`.

> Por padrão, os componentes dentro da pasta `app` são **Server Components**. Para indicar que um componente deve ser renderizado pelo cliente, é utilizada a diretiva `'use client'` **no topo do código** no arquivo do componente. Client Components são utilizados quando há necessidade de interatividade por meio de eventos ou quando se usa hooks.

O App Router suporta criação de layouts, templates, página 404, etc, por meio do uso de arquivos que seguem uma convenção de nomenclatura: `page.js`, `layout.js`, `template.js`, `not-found.js`, etc.

- você também pode adicionar outras pastas e arquivos dentro de `app` não relacionados a roteamento. Elas **não** serão acessíveis ou consideradas como uma rota da aplicação ─ o roteamento somente olha para arquivos `page.js` ou `route.js`;

- alternativamente, nada te impede de criar outra pasta fora de `app` para colocar os demais arquivos de seu projeto. Por exemplo, ter a pasta `app` para lidar somente com rotas e páginas, e outra pasta irmã `components` para criar seus componentes. O dev e sua equipe são livres para decidir como o projeto será estruturado.

### Rotas dinâmicas

Você pode criar nomes para as rotas de modo dinâmico (segmentos dinâmicos) utilizando a convenção de nomenclatura `[nome_segmento_dinamico]`. Os colchetes `[]` indicam que o nome daquela pasta será alterado dinamicamente pela a aplicação. Por exemplo, pode ser substituída por um slug ou id obtido de uma consulta à API.

Para recuperar o nome de um segmento dinâmico (o nome dado à pasta entre colchetes), você pode usar a prop `params` disponibilizada pelo Next ao componente implementado em um arquivo `page.js`. Por exemplo, `params.nome_segmento_dinamico`.

Exemplo mais completo:

```jsx
// -- app/posts/[slug]/pages.js
const PagePost = ({ params }) => {
    return (
        <h1>{params.slug}</h1>
    );
}
export default PagePost;
```

Neste exemplo, caso a URL fosse: `seu-site.com.br/posts/aprendendo-nextjs`, seria exibido na tela `<h1>aprendendo-nextjs</h1>        `

### Componente Link

O Next fornece o componente `Link` para substituir a tag `<a>`. Por meio dele é possível fazer a navegação entre as rotas da aplicação sem a necessidade de recarregar a página toda, mantendo o conceito de SPA. Exemplo:

```jsx
import Link from 'next/link';
<Link href={`/?page=${prev}`}>Página anterior</Link>}
```

### Prop searchParams

No caso de server components, podemos obter a query string da URL (aquilo que vem após um `?` em uma URL) por meio de uma prop provida pelo Next chamada `searchParams`. Ela retorna um **objeto** com os parâmetros de busca contidos na query string.

| URL | searchParams |
| --- | --- |
|/shop?a=1&b=2	| { a: '1', b: '2' } |

Exemplo de uso:

```js
export default async function Home({ searchParams }) {
    // exemplo de URL: http://localhost:3000/?page=3
    const currentPage = searchParams?.page || 1;

    // restante do código...
}
```

## Fetch de dados pelo servidor

Por padrão, os componentes no Next são renderizados pelo servidor (SSR). Por conta disso, requisições de fetch de dados são feitos pelo lado do servidor e você pode utilizar o resultado diretamente no componente, **sem a necessidade de um `useEffect` ou `useState`** gerenciando o estado dos dados. Isso ocorre porque o servidor irá obter os dados necessários para montar a tela e enviá-la ao cliente quando tudo estiver pronto.

- Essa solução está disponível nas versões novas do Next, usando App Router.

- Por ser feito pelo servidor, o erros e `console.log` que você fizer irão **aparecer no terminal**, e não no Dev Tools do navegador.

Exemplo: 

```jsx
// fetch do lado do servidor
const getAllPosts = async () => {
  const resp = await fetch('http://localhost:3042/posts');
  if (!resp.ok) {
    // esse console.log será mostrado no TERMINAL
    // por ser um server component
    console.log('Função getAllPosts --> erro ao obter as postagens da API');
    return [];
  }
  return resp.json();
}

export default async function Home() {
  // sem necessidade de useEffect/useState
  const posts = await getAllPosts();
  return (
    <main>
      {posts.map(post => <CardPost post={post} key={post.id} />)}
    </main>
  );
}
```