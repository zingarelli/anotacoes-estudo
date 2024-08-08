# Anotações sobre Docker

Anotações baseadas no curso ["Docker: criando e gerenciando containers"](https://www.alura.com.br/curso-online-docker-criando-gerenciando-containers), ministrado pelo [Daniel Artine](https://linkedin.com/in/danielartine) na Alura.

> O [Docker](https://www.docker.com) é um software que permite gerenciamento de contêineres.

Para entender contêineres, vamos primeiro entender o que é uma máquina virtual.

**Máquina virtual:** virtualização de um ou mais **sistemas operacionais** (SOs) rodando em **uma única máquina**. Vários SOs isolados do SO original (aquele realmente instalado na máquina).

**Contêineres (em inglês: *containers*)**: são **processos** rodando em **um único SO**, sem a necessidade de virtualizar um SO completo, reduzindo o consumo de recurso computacional. O isolamento é garantido para cada contêiner e em difentes níveis de isolamento (*namespaces*). Por meio de *cgroups* é possível definir quanto de recursos (memória, processamento, etc) cada contêiner pode consumir.

**Isolamento**: garantia de que aquilo que está instalado **não** vai entrar em **conflito** com outras instalações, sejam máquinas virtuais ou contêineres. Por exemplo, um contêiner pode ter uma aplicação rodando na versão 1.3.4 na porta 80 e outro contêiner pode ter a mesma aplicação rodando na versão 3.8 na porta 80. Por conta do isolamento, ambas podem coexistir na mesma máquina sem entrar em conflito.

**Host**: nome dado à máquina que está rodando o Docker.

## Imagem

Uma imagem é formada por um conjunto de camadas (*layers*), usadas para a criação de um ou mais contêineres. Isso permite o reaproveitamento e portabilidade de uma imagem para criar contêineres em diferentes máquinas.

As camadas são criadas a partir de instruções a serem executadas. Elas ficam em um arquivo chamado **`Dockerfile`** (sem extensão mesmo). É esse arquivo que define como uma imagem é construída. Ou seja, é como se fosse um script/arquivo bat.

Em outras palavras, a imagem é uma "receita" do que precisa ser instalado, arquivos do host que precisam ser copiados para alguma pasta do contêiner, comandos que precisam ser executados para que o contêiner seja criado e funcione, enfim, tudo que é necessário para rodar uma aplicação no contêiner. Ela pode até fazer uso de outras imagens que necessite como dependência.

### Exemplo

Este exemplo de um `Dockerfile` simples cria uma imagem que irá rodar um ambiente de execução Node para subir um servidor que vai exibir uma página Web. Considere que tanto o `Dockerfile` quanto os arquivos do projeto estão na mesma pasta.

```dockerfile
# nossa imagem vai se basear na imagem do node, 
# versão (tag) 14.
FROM node:14

# pasta padrão do container. Vai ser criada quando 
# o container for criado.
WORKDIR /app-node

# informação sobre a porta que a aplicação usa
# esse comando é opcional
EXPOSE 3000

# copia o conteúdo da pasta atual do host (.), que
# é a pasta onde está o Dockerfile, para a pasta 
# atual do container, que no caso é aquela que foi
# definida no comando WORKDIR.
COPY . .

# ao criar a IMAGEM, executa o comando
RUN npm install

# comando a ser executado quando o CONTAINER
# for iniciado.
ENTRYPOINT npm start 
```

Existem vários outros comandos que podem ser usados. A [documentação](https://docs.docker.com/reference/dockerfile/) possui explicação e exemplo de todos eles.

Para criar uma imagem, usamos o comando abaixo. A flag `-t` define uma tag (um nome) para a imagem. O ponto no final (`.`) indica que o diretório atual em que você está no terminal será usado para o build, então certifique-se de que esteja no diretório do `Dockerfile` ao rodar o comando:

    docker build -t nome_da_imagem .

> No Docker Desktop, além dos contêineres, você pode também ver todas as imagens que estão salvas na sua máquina. Na linha de comando, você pode usar `docker images`.

## Comandos

> O Docker Desktop deve estar **rodando** na sua máquina para poder usar os comandos.

> No Windows, execute os comandos abaixo no **PowerShell**.

---

Criar e executar um contêiner baseado em uma imagem:

    docker run caminho_ou_nome_da_imagem

Se a imagem não estiver em sua máquina, por padrão o Docker irá procurar por ela no [Docker Hub](https://hub.docker.com). Caso não a encontre, irá gerar um erro.

> O Docker Hub é um repositório de imagens. Você também pode fazer upload de uma imagem sua, mediante a criação de um login. É parecido com o GitHub.

---

Você pode usar a flag `-d` para criar e executar o contêiner em modo "detached", para que o contêiner rode em segundo plano, sem travar o terminal.

    docker run -d caminho_ou_nome_da_imagem

---

Você também pode mapear uma porta do host para uma porta do contêiner com a flag `-p` (p minúsculo):

    docker run -d -p porta_do_host:porta_do_container caminho_ou_nome_da_imagem

Por exemplo, se você tem um contêiner que roda na porta 80 e quer executá-lo na porta 8080 do host (a porta "verdadeira" da sua máquina), você pode fazer `docker run -d -p 8080:80 caminho_ou_nome_da_imagem`.

---

Listar os contêineres em execução:

    docker ps

ou

    docker container ls

---

Para listar todos os contêineres, incluindo os que não estão em execução, use a flag `-a`:

    docker ps -a

ou

    docker container ls -a

---

Parar a execução de um contêiner (use o `docker ps` para saber o id ou nome do contêiner que vai ser parado):

    docker stop containerId_ou_name

--- 

Executar um contêiner já criado:

    docker start containerId_ou_name

---

Remover um contêiner:

    docker rm containerId_ou_name

Se o contêiner estiver em execução, você pode usar a flag `--force` para parar o contêiner e removê-lo:

    docker rm containerId_ou_name --force

--- 

Executar um comando em um contêiner que está em execução:

    docker exec containerId_ou_name comando

## Docker Compose

É uma ferramenta de coordenação (não confundir com orquestração, que é outro conceito), em que conseguimos, em um único arquivo, compor a execução de múltiplos contêineres em um mesmo ambiente, de maneira coordenada. Ao invés de sempre ficarmos subindo os contêineres pela linha de comando, conseguimos fazer isso em um arquivo.

É utilizado um arquivo YAML (*Yet Another Markup Language* ou *YAML Ain't Markup Language*). Em versões mais antigas, o arquivo era `docker-compose.yml` ou `docker-compose.yaml`. Em versões atuais, o arquivo é `compose.yaml` (preferível) or `compose.yml`.

Com o terminal na pasta em que se encontra esse arquivo, usamos o comando `docker compose up` para iniciar os contêineres e configurações listados no arquivo.

O exemplo abaixo irá subir dois contêineres: um com o mongodb e outro com uma aplicação front-end chamada Alura Books. A alurabooks irá aguardar o contêiner do mongodb estar em execução antes de ser iniciada (por conta do `depends_on`). Ambos irão rodar em uma mesma rede, também configurada no arquivo.

```yaml
services:
  mongodb:
    image: mongo:4.4.6
    container_name: meu-mongo
    networks:
      - compose-bridge

  alurabooks:
    image: aluradocker/alura-books:1.0
    container_name: alura-books
    networks:
      - compose-bridge
    ports:
      - 3000:3000
    depends_on:
      - mongodb

networks:
  compose-bridge:
    driver: bridge
```

- para rodar em modo "detached": `docker compose up -d`;

- para listar os serviços criados pelo docker compose: `docker compose ps`;

- para remover os contêineres criados pelo docker compose: `docker compose down`;

## Outros conceitos

Explicação breve de outros conceitos vistos em aula. Meu foco foi nas aulas iniciais e na final, porque eram os conteúdos que eu precisava para avançar nos cursos de Front End (Next.js). Os conceitos e comandos das outras aulas foram mais a fundo nas funcionalidades do Docker. Embora importantes, são mais voltados para a área de DevOps, então não vou fazer um resumo tão detalhado dessa parte.

**Volume:** é uma das maneiras de persistir dados ao usar contêineres (e a recomendada pelo Docker). Persistência significa manter no host os dados gerados ou usados pelo contêiner, mesmo no caso em que o contêiner é removido. Quando não usada alguma técnica de persistência, os dados gerados pelo contêiner são removidos quando ele é removido. Volumes também podem ser compartilhados entre contêineres. 

- Outras estratégias de persistência são: bind mount e tmpfs (tmpfs faz a persistência temporária em memória e funciona somente em Linux).

**Bridge**: É uma das soluções do Docker para gerenciamento de rede (*network*) para os contêineres. Por padrão, os contêineres são colocados na mesma rede denominada *bridge*, cada um com seu próprio IP. Com isso, eles podem se comunicar via IP. Nisso a *bridge* se comporta como se fosse um switcher.

- Podemos também criar nossa própria *bridge*, com configurações de rede mais customizadas. Isso possibilita, por exemplo, definir sub-redes, gateways e outras opções específicas. Contêineres nessa *bridge* customizada também podem se comunicar usando seus nomes (*hostnames*), ao invés do IP. Isso simula o DNS de uma rede interna.