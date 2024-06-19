# Next.js

## Créditos

Estas anotações são baseadas nos cursos da Alura para a formação [Next.js 14: desenvolvendo aplicações robustas com alta produtividade](https://www.alura.com.br/formacao-next-js-14-aplicacoes-robustas-alta-produtividade), ministrados pela [Patrícia Silva](https://github.com/gss-patricia) e pelo [Vinicios Neves](https://www.linkedin.com/in/vinny-neves/).


## Introdução

O [Next.js](https://nextjs.org) é um **framework construído em cima do React**. Ou seja, ele dá poderes adicionais ao React para construção de aplicações Web, proporcionando muita **otimização** da aplicação e **ferramentas para facilitar o building/deploy** do produto final.

- Atualmente (2023), o Next.js é um dos frameworks [recomendados pelo React](https://react.dev/learn/start-a-new-react-project) para criação de um novo projeto React. **O CRA já não aparece na recomendação e tende a ser preterido pelos desenvolvedores**.

O framework possui sua **própria solução para roteamento**, mas também é possível utilizar outras bibliotecas, como a React Router. 

Em questões de otimização, é possível fazer uma renderização híbrida dos componentes, com aqueles que exigem interatividade sendo renderizados no cliente (no navegador, como estamos acostumado com aplicações React), e outros **componentes sendo renderizados pelo servidor**, que entrega o HTML pronto ao cliente - isso ajuda também na indexação da página pelos buscadores (questão de SEO). Há também otimização no uso de imagens e fontes.

- A documentação do Next.js usa os nomes **"Client Components"** e **"Server Components"** para diferenciar os componentes renderizados pelo cliente (CSR - Client Side Rendering) e os renderizados pelo servidor (SSR - Server Side Rendering);

---> revisar essa parte

- Por padrão, os componentes dentro da pasta `app` são Server Components. Para indicar que um componente deve ser renderizado pelo cliente, é utilizada a diretiva `use client` **no topo do código** no arquivo do componente.

    - Client Components são utilizados quando há necessidade de interatividade por meio de eventos ou quando se usa hooks.

<---

## Instalação

Para as versões atuais do Next.js (versão 13), recomenda-se o Node versão 16.8 ou superiores.

É possível fazer uma [instalação manual](https://nextjs.org/docs/getting-started/installation#manual-installation) de um projeto Next, mas também há a opção automatizada, por meio de um processo interativo utilizando o npx: 

    npx create-next-app@latest

O comando acima irá sempre utilizar a versão mais atual estável do Next. Caso você queira utilizar uma versão específica, é só substituir o `latest` pelo número da versão. Exemplo criando um projeto na versão 14 do Next:

    npx create-next-app@14

> Use o comando `create-next-app` na pasta onde você deseja salvar o projeto.

Com isso, um projeto Next é criado já com alguns arquivos e pastas. Para rodá-lo, digite:

    npm run dev

O projeto irá rodar em http://localhost:3000

> Em projetos React, estamos acostumados a verificar erros na aplicação olhando o Dev Tools do navegador. Já no Next, como algumas coisas serão feitas pelo servidor, também precisamos ficar de olho no console onde rodamos o `npm run dev`, pois é nele que serão informado erros do servidor.

## Estrutura de um projeto Next

O curso é baseado na **versão 14** do Next, então alguns conceitos, códigos e estruturas podem mudar em versões diferentes. 

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

- jsconfig.json: configurações do JavaScript. Caso você tenha marcado "No" para a pergunta "Would you like to customize the default import alias (@/*)" durante o `create-next-app`, será configurado neste arquivo que toda vez que você iniciar um import com `@`, o Next irá entender que deve procurar a partir da pasta `src`, o que facilita na hora de informar o caminho para um arquivo. Exemplo: `import { Aside } from '@/components/Aside'`. O componente `Aside` será buscado em `src/components/Aside`.

- globals.css: estilos globais da aplicação;

- layout.js: template básico de toda a aplicação. Aqui você pode definir tudo aquilo que é comum de aparecer em todas as páginas (os metadados do html, por exemplo, e até as tags `html` e `body`);

- page.js: 

- page.module.css: estilos específicos para `page.js`. Por padrão, o Next oferece CSS Modules.

> Uma dica de organização é criar uma pasta `components` dentro de `src` para guardar seus componentes. 

### Pasta `public`

Quando criada, esta pasta será entendida pelo Next.js como sendo o local em que serão armazenados os **arquivos estáticos** (imagens, fontes, arquivo robots.txt, etc). Esse arquivos ficarão públicos, ou seja, acessíveis diretamente da raiz do projeto (da base URL, sem necessidade de incluir `public`).

Por exemplo, caso você queira utilizar uma imagem que está em `public/images/logo.svg` como sendo o `src` de uma tag `img`, você passa o caminho relativo dessa forma: `/images/logo.svg`. 

Da mesma forma, caso queira acessar o arquivo pelo navegador, basta informar: `http://www.nomedoseusite.com.br/images/logo.svg`.

## Otimizando imagens

O Next oferece um componente `Image` que pode ser utilizado em substituição ao elemento `<img>` do HTML. A vantagem de utilizar o `Imagem` é que o Next se encarrega de otimizar automaticamente a imagem para você, melhorando o carregamento da página, por exemplo. 

```js
import logo from './logo.png';

export const Aside = () => {
    return (<aside>
        <Image src={logo} alt="Logo do Code Connect" />
    </aside>)
}
```

Para aproveitar ainda mais das otimizações, você pode fazer o import da imagem (`import logo from './logo.png'`) ao invés de passar o caminho para ela (`scr="/images/logo.png"`, por exemplo). Pelo import, o Next automaticamente determina o width e height da imagem. Caso você opte por enviar o caminho da imagem (ou a URL de uma imagem externa), você obrigatoriamente precisa passar as dimensões nas props `width` e  `height`.

---> revisar essa parte

## App Router

A partir da versão 13, o Next.js introduziu o conceito de App Router (anteriormente, isso era feito pelo [Pages Router](https://nextjs.org/docs/pages/building-your-application/routing)). 

O **roteamento acontece dentro da pasta `app`**, considerada a raiz da aplicação (a homepage). Novas rotas são definidas por meio da **criação de subpastas** dentro de `app`. A página que será exibida por cada rota é incluída em um arquivo **`page.js`** (ou .jsx ou .tsx).

- é possível criar **rotas aninhadas**, por meio da criação de outras subpastas dentro das subpastas, mantendo a mesma estrutura.

O App Router suporta criação de layouts, templates, solução para o erro 404, etc, por meio do uso de **"special files"**, que seriam arquivos que utilizam uma nomenclatura específica: `page.js`, `layout.js`, `template.js`, `not-found.js`, etc.

- você também pode adicionar outras pastas e arquivos dentro de `app` não relacionados a roteamento. Elas **não** serão acessíveis ou consideradas como uma rota da aplicação, a não ser que esteja presente um `page.js` ou `route.js` nesta pasta;

- alternativamente, nada te impede de criar outra pasta fora de `app` para colocar os demais arquivos de seu projeto. Por exemplo, ter a pasta `app` para lidar somente com rotas e páginas, e outra pasta irmã `components` para criar seus componentes. O dev e sua equipe são livres para decidir como o projeto será estruturado.

<---