# Next.js

Estas anotações estão baseadas na **versão 14** do Next, então alguns conceitos, códigos e estruturas podem mudar em versões anteriores. 

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

> Em projetos React, estamos acostumados a verificar erros na aplicação olhando o Dev Tools do navegador. Já no Next, como algumas coisas serão feitas pelo servidor, também precisamos ficar de olho no console onde rodamos o `npm run dev`, pois é nele que serão informados erros do servidor.

## Estrutura de um projeto Next

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

Exemplo importando a fonte `Prompt` (não variante). Observe que a fonte importada é utilizada como uma função, que recebe as configurações via parâmetro, por meio de um objeto. Essa função, por sua vez, retorna um objeto e utilizamos a propriedade `className` desse objeto para fazer o link entre a fonte e o HTML onde ela deve ser aplicada ─ no caso do exemplo, estamos aplicando a fonte à aplicação toda (atributo `className` da tag `<html>`). Observe que dessa forma não precisamos configurar o `font-family` no CSS global:

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

### Criando endpoints

Além de exibir páginas com o arquivo `pages.js`, você também pode usar a App Router para criar **endpoints no servidor para retornar ou receber dados**, ou seja, usar o Next como uma API para tratar requests e responses. O Next define isso como [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers).

Para isso, você cria um **arquivo `route.js`**. Dentro deste arquivo, você pode criar funções para os verbos HTTP, como GET, POST, etc. Estas funções possuem dois parâmetros opcionais: o `request`, um [objeto representando a Request](https://nextjs.org/docs/app/api-reference/functions/next-request), e o `context`, um objeto cuja única propriedade atualmente é a `params`, que por sua vez é o objeto que o Next disponibiliza para acessar as rotas dinâmicas.

Um exemplo de organização de projeto é, dentro da pasta `app`, criar uma pasta `api` e nela definir as rotas (endpoints) para lidar com requisições.

O exemplo abaixo cria uma função que irá retornar as respostas a um comentário usando o endpoint `api/comment/[id]/replies`, cujo id é acessado por uma rota dinâmica (`[id]`). A consulta no banco está sendo feita com [Prisma](#prisma).

```js
// -- api/comment/[id]/replies/route.js
import db from "../../../../../../prisma/db"

// we add underline to indicate that the 
// request parameter will not be used
export const GET = async (_request, { params }) => {
    const replies = await db.comment.findMany({
        where: {
            parentId: parseInt(params.id)
        },
        include: {
            author: true
        }
    });

    // Response is an interface of the Fetch API
    return Response.json(replies);
}
```

## Fetch de dados pelo servidor

Por padrão, os componentes no Next são renderizados pelo servidor (SSR). Por conta disso, requisições de fetch de dados são feitos pelo lado do servidor e você pode utilizar o resultado diretamente no componente, **sem a necessidade de um `useEffect` ou `useState`** gerenciando o estado dos dados. Isso ocorre porque o servidor irá obter os dados necessários para montar a tela e enviá-la ao cliente quando tudo estiver pronto.

- Essa solução está disponível nas versões novas do Next, usando **App Router**.

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

## Página de erro

Quando acontece algum erro na aplicação que não tenha sido tratado, você pode gerar uma página para exibir alguma informação (*fallback UI*) ao invés daquela tela padrão de erro do React. Isso é feito criando um arquivo `error.js` para a definição de um componente que irá retornar o que deve ser renderizado na página de erro.

Você pode criar um arquivo `error.js` em cada rota da aplicação, de modo a ter uma página de erro personalizada para cada rota. Quando um erro ocorre em uma rota que não tem esse arquivo, o Next irá procurar pelo arquivo de erro na rota pai e assim por diante.

Exemplo de componente de erro. Note que neste caso deve ser criado um **client component** para lidar com comportamento de erros dinâmicos e possibilitar alguma interação com a pessoa usuária.

```jsx
'use client'; // error components MUST BE client components
import { useEffect } from "react";

export default function Error({ error }) {
    useEffect(() => {
        // this will be displayed in the browser dev tools
        console.log(`Erro na página inicial: ${error}`);
    }, [error]);

    return (
        <div><h2 style={{ color: '#FFF' }}>Ops, aconteceu um erro!</h2></div>
    )
}
```

## Server Actions

Server Actions são **funções assíncronas** que você pode criar em seu projeto Next, e que podem ser invocadas tanto por client components quanto por server components. Essas funções são **executadas no lado do servidor**. 

Um exemplo comum é invocá-las na submissão de um formulário, usando o atributo `action` do elemento `form`. Um aspecto interessante do Next é que essa submissão **não** irá causar um recarregamento da página.

- Podemos passar argumentos para uma Server Action usando a função `bind`. Isso é necessário, pois a função está rodando no servidor, então temos que "emprestá-la" do servidor para o componente que vai invocá-la;

- Caso uma Server Action resulte em uma ação que atualiza algum campo da página, você pode usar a função `revalidatePath` para que o Next faça as alterações necessárias na UI (sem recarregar a página inteira).

- Server Actions também podem ser invocadas da maneira tradicional, por meio de eventos ou hooks como `useEffect`.

- Podemos utilizar o hook `useFormStatus` do React para verificar se uma ação está pendente. Isso é útil para exibirmos um ícone de carregamento enquanto a ação não termina, por exemplo.

  - até 2024, este hook se encontra [disponível de forma experimental](https://react.dev/reference/react-dom/hooks/useFormStatus) no React;

  - o hook só funciona se o componente for renderizado dentro de um elemento `form`;

  - o componente que utiliza o hook deve ser um client component. Você pode, por exemplo, abstrair o pedaço de código que usa o hook em um componente separado e aí informar que será um client component.

Ao criar uma server action, é uma **boa prática deixar explícito** no arquivo que a função deve ser executada no servidor, utilizando a diretiva `'use server'`. 

Outra boa prática é colocar as actions em uma pasta separada. Por exemplo, criar um arquivo `src/actions/index.js` e dentro dele exportar as actions.

Exemplo de Server Action para incrementar o número de curtidas em uma postagem. Observe que usamos o método `update` do [Prisma](#prisma): 

```js
// explicit tell Next that this file is to run in the server
'use server';

import { revalidatePath } from "next/cache";
import db from "../../prisma/db";

// server action to increment the number of likes for a post
export async function incrementThumbsUp(post){
    await db.post.update({
        where: { 
            id: post.id 
        },
        data: {
            // we can pass an object with an "increment" property
            // to let Prisma increment the current value of a field 
            // by a value of X (increment likes by 1 in this case)
            likes: {
                increment: 1
            }
        }
    });

    // clear cache to update the UI of pages affected by this action
    revalidatePath('/');
    revalidatePath(`/${post.slug}`);
}
```

Exemplo de chamada utilizando a `action` de um elemento `form`. Parte não relevante do código foi omitida para economizar espaço. 

```jsx
import { incrementThumbsUp } from '@/actions';

export const CardPost = ({ post }) => {
    // using bind to pass additional arguments to the Server Action
    const submitThumbsUp = incrementThumbsUp.bind(null, post);
    
    return (
        <form action={submitThumbsUp}>
              <ThumbsUpButton />
              <p>{post.likes}</p>
        </form>
    );
}
```

Mesmo exemplo, dessa vez utilizando o evento de submit do `form`. Neste caso, é necessário transformar o componente em um client component e, por conta disso, impedir o recarregamento da página com `preventDefault`:

```jsx
'use client'

import { incrementThumbsUp } from '@/actions';

export const CardPost = ({ post }) => {
    // using bind to pass additional arguments to the Server Action
    const submitThumbsUp = incrementThumbsUp.bind(null, post);

    const handleSubmit = e => {
        e.preventDefault();
        submitThumbsUp();
    }
    
    return (
        <form onSubmit={handleSubmit}>
            <ThumbsUpButton />
            <p>{post.likes}</p>
        </form>
    );
}
```

O `ThumbsUpButton` é um client component que vai renderizar um botão ou um spinner, baseado no estado da ação (por meio do `useFormStatus`). Parte não relevante do código foi omitida para economizar espaço. :

```jsx
'use client';

import { useFormStatus } from "react-dom"

export const ThumbsUpButton = () => {
    const { pending } = useFormStatus();
    return <IconButton disabled={pending}>
        {pending ? <Spinner /> : <ThumbsUp />}
    </IconButton>
}
```

#### Valores de formulário

Quando uma Server Action é chamada via **`action` de um elemento `form`**, o Next automaticamente **injeta um objeto [`formData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData/FormData)** como último argumento da função. Este objeto contém os valores de cada elemento dentro do formulário, que podem ser acessados por um método `get` passando o atributo `name` desses elementos.

O exemplo abaixo é parte de um código de uma Server Action que faz a inserção de comentários em um post. Observe que `formData` aparece como último parâmetro da função. Esse parâmetro **não** é passado pelo componente que chama a função, e sim injetado automaticamente pelo Next. Também observe que usamos o **método `create`** do Prisma para fazer a **inserção no banco de dados**:

```js
export async function postComment(post, formData) {
    await db.comment.create({
        data: {
            text: formData.get('text'),
            authorId: author.id,
            postId: post.id
        }
    });
}
```

## Prisma

> Para um exemplo de uso do Prisma, incluindo os arquivos necessários, consulte o projeto do [Code Connect](https://github.com/zingarelli/code-connect/tree/postgres_prisma/prisma).

O Prisma é um [ORM (Object Relational Mapper)](https://www.prisma.io/dataguide/types/relational/what-is-an-orm). Isso significa que ele atua, no caso do projeto, como um **intermediador entre as linguagens SQL e JavaScript** (ele também trabalha com outras linguagens). Assim, podemos focar nas estruturas e códigos no Next, criando tabelas e consultas utilizando objetos em JS, e deixar que o Prisma se responsabilize por se comunicar com o banco de dados e "traduzir" em SQL aquilo que queremos. 

Para adicionar o Prisma a um projeto, usamos o comando 

```shell
npm i prisma
```

Para criar os arquivos iniciais para utilização do Prisma, o comando é 

```shell
npx prisma init
```

Caso ele ainda não esteja instalado na máquina, este comando também irá fazer a instalação. 

Iniciado o Prisma, uma pasta `prisma` será criada na raiz do projeto com um arquivo **`schema.prisma`**. Neste arquivo definimos qual SGBD será utilizado e também criamos os objetos que representarão as tabelas e seus relacionamentos (daí o nome *Object Relational Mapper*). 

Também será criado um arquivo `.env`, onde são definidas variáveis de ambiente. O Prisma irá consultar esse arquivo para obter as credenciais de conexão ao banco.

> O arquivo `.env` contém dados sensíveis de acesso ao projeto, então **não o versione** nem o compartilhe em ambiente de produção.

### Criação do banco de dados

Supondo que vamos criar o banco do zero, definimos as tabelas e relacionamentos no arquivo `schema.prisma` e rodamos o comando abaixo para efetuar a chamada "migração" (*migration*). Esta é a ação que irá criar de fato o banco de dados e suas tabelas no Postgres.

```shell
npx prisma migrate dev --name init
```

- `dev` indica que estamos em um ambiente de desenvolvimento. A outra opção, `prod` é para ambientes de produção e possui diferenças (não mencionadas aqui);

- `--name init` é a forma de darmos um nome a essa migração, de modo a facilitar identificá-la quando houver outras migrações. Você pode escolher o nome que quiser;

- a pasta `prisma/migrations` contém subpastas com os arquivos SQL criados pelo Prisma.

Exemplo de criação de duas tabelas (models), Post e User, no `schema.prisma`, incluindo chaves primárias e estrangeira, e relacionamentos:

```prisma
// @id indicates this property as primary key
// @default is a default value; in this case, it will use the autoincrement function to generate an integer
// @unique indicates that the value cannot be repeated between records (rows)
// Post Post[] indicates a 1:N relationship between User and Post
model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
  Post Post[]
}

// @updatedAt automatically updates the time when a record is updated
// @relation configures foreign keys to indicate a connection between tables
model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  authorId Int
  author User @relation(fields: [authorId], references: [id])
}
```

### Prisma Client

É uma **classe** disponibilizada pelo Prisma para fazer consultas e as demais operações de CRUD na base de dados, sem usar SQL. 

O client é criado pelo seguinte comando:

```shell
npx prisma generate
```

Esse comando deve ser **utilizado toda vez que houver alguma mudança no banco** (alguma alteração no `schema.prisma`), para garantir que o client esteja atualizado com qualquer alteração dos modelos, tipos e relacionamentos.

Para disponibilizar o client para uso na aplicação, uma sugestão é criar um arquivo `db.js` na pasta prisma e exportar o client. 

```js
// --- prisma/db.js
import { PrismaClient } from '@prisma/client';
const db = new PrismaClient();
export default db;
```

### Seed de dados

> Link do Prisma sobre seeding, incluindo exemplos em JS e TS: https://www.prisma.io/docs/orm/prisma-migrate/workflows/seeding.

Você pode usar o Prisma para popular (semear) o banco de dados. Para isso, criamos um comando `seed` no `package.json`, e usamos o comando `npx prisma db seed` para popular o banco.

O exemplo a seguir mostra como seria o comando `seed` incluído no `package.json`. O arquivo `prisma/seed.js` será executado pelo Node e irá popular o banco de dados:

```json
{
  "prisma": {
    "seed": "node prisma/seed.js"
  }
}
```

O próximo exemplo mostra a inserção de um novo dado à tabela "Author":

```js
// --- prisma/seed.js
const { PrismaClient } = require('@prisma/client'); // neste caso, precisamos usar require
const prisma = new PrismaClient();

async function main() {
    // --- criando autora "Ana Beatriz"
    const author = {
        name: "Ana Beatriz",
        username: "anabeatriz_dev",
        avatar: "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/authors/anabeatriz_dev.png",
    };

    // upsert faz a criação ou atualização na base de dados
    // baseado na condição passada em "where"
    const ana = await prisma.user.upsert({
        where: { username: author.username },
        update: {}, // vazio indica que nenhum update será feito
        create: author
    });
}

main()
    .then(async () => {
        await prisma.$disconnect()
    })
    .catch(async (e) => {
        console.error(e)
        await prisma.$disconnect()
        process.exit(1)
    })
```

### Fetch de dados

Por meio do prisma client, podemos acessar as **tabelas** do banco (os models no `schema.prisma`) como se fossem **objetos do client**. Esses objetos possuem métodos para fazer consultas.

No exemplo a seguir, usamos o método **`findMany`** para recuperar os dados da tabela Post. Essa tabela possui uma relação com a tabela Author (uma chave estrangeira para o id do autor), então podemos passar via parâmetro um objeto de configuração com a propriedade `include` para também recuperar dados da tabela Author (na prática, é como se estivéssemos fazendo um `SELECT` com `JOIN` no SQL). Além disso, também está implementada a lógica para paginação (utilizando as propriedades `take` e `skip`) e a ordenação pela data de criação do post (propriedade `orderBy`);

```js
import db from '../../prisma/db';

const ITEMS_PER_PAGE = 6;

// fetch no lado do servidor
const getAllPosts = async (page) => {
  try {
    // lógica para a página anterior
    const prev = page > 1 ? page - 1 : null;

    // lógica para a próxima página, baseado no número
    // de items salvos na base
    const totalItems = await db.post.count(); // SELECT count(*) FROM post
    const totalPages = Math.ceil(totalItems / ITEMS_PER_PAGE); 
    const next = page < totalPages ? page + 1 : null;

    // lógica para trazer os items da próxima página
    const skip = (page - 1) * ITEMS_PER_PAGE;

    // SELECT * FROM post
    const posts = await db.post.findMany({
      // Uso do include para trazer dados de outra tabela.
      // Possível porque há uma relacionamento entre as
      // tabelas definido no schema.prisma
      include: {
        author: true
      },
      // paginação
      take: ITEMS_PER_PAGE,
      skip,
      // ordenação
      orderBy: { createdAt: 'desc' }
    });
    return { data: posts, prev, next };
  }
  catch (error) {
    console.log(`[${new Date().toString()}] Função getAllPosts --> erro de conexão com a API: ${error}`);
    return { data: [], prev: null, next: null };
  }
}
```

## Deploy FullStack na Vercel usando Prisma

A Vercel possibilita criarmos um banco de dados e integrá-lo ao deploy da aplicação. Para isso, precisamos fazer alguns ajustes no projeto e também na Vercel.

Para o uso do Prisma, a Vercel solicita duas variáveis de ambiente, então precisamos alterar o `schema.prisma`:

```prisma
datasource db {
  provider  = "postgresql"
  // Uses connection pooling
  url       = env("POSTGRES_PRISMA_URL")
  directUrl = env("POSTGRES_URL_NON_POOLING")
}
```

Para manter o sincronismo com o projeto rodando localmente, adicionamos essas variáveis também ao `.env`:

```
POSTGRES_PRISMA_URL="postgresql://postgres@localhost:5432/codeconnect_dev"
POSTGRES_URL_NON_POOLING="postgresql://postgres@localhost:5432/codeconnect_dev"
```

- Lembrando que, em **produção**, **não compartilhamos o `.env`**.

Por fim, ajustamos o `package.json`, adicionando os passos do Prisma para o script de **`build`** (a Vercel irá chamar esse comando durante o deploy):

```json
{  
  "scripts": {
    "dev": "next dev",
    "build": "prisma generate && prisma migrate dev && prisma db seed && next build",
    "start": "next start",
    "lint": "next lint"
  },
}
```

Na Vercel, precisamos adicionar um banco Postgres, disponibilizado pela plataforma. Para o caso deste projeto, a versão free (hobby) está disponível. 

- Acesse a página do projeto na Vercel;

- Vá até a aba "Storage";

- Clique no botão "Create" ao lado do item "Postgres";

- Use todas as opções padrão nas próximas janelas.

> A página do projeto na Vercel só aparece após o primeiro deploy. Então faça o primeiro deploy, que irá gerar um erro por não ter um banco de dados, e aí então crie um banco Postgres e faça um redeploy.