# NoSQL

A introdução dessas anotações foram baseadas nos seguintes artigos em inglês:

- [Understanding SQL vs NoSQL Databases](https://www.mongodb.com/resources/basics/databases/nosql-explained/nosql-vs-sql);

- [SQL vs. NoSQL Databases: What’s the difference?](https://www.ibm.com/think/topics/sql-vs-nosql);

- [SQL vs. NoSQL: full comparison of features, differences, and more](https://www.testgorilla.com/blog/sql-vs-nosql/).

## Relembrando

- SQL (Structured Query Language) é uma **linguagem** utilizada em banco de dados **relacionais**.

- Banco de dados relacionais: armazenam dados em linhas (registros) e colunas (atributos), que compõem uma **tabela**. Tabelas podem se relacionar entre si, ou seja, estabelecer uma ligação entre seus dados, por meio de **chaves estrangeiras**.

- Chaves estrangeiras são identificadores presentes em uma tabela, que **referenciam** algum um dado presente em **outra tabela**. Dessa forma, os dados ficam organizados de uma maneira pré-definida e podem ser **combinados durante uma consulta**.

- Dados estruturados: são dados que seguem um **schema fixo**, isto é, uma organização lógica previamente definidas. Sabemos quais serão as colunas da tabela, quais serão seus tipos, relacionamentos, quais colunas são obrigatórias, etc. Ao inserir um **dado novo**, ele deve **obedecer à estrutura predefinida**. Isso garante **consistência** nos dados. 

- Transações em bancos de dados relacionais seguem os **princípios ACID** (Atomicidade, Consistência, Isolamento e Durabilidade).

## O que é NoSQL

A palavra já significou "No SQL" (não SQL), mas hoje evoluiu para **Not only SQL** (não somente SQL). É uma abordagem para gerenciar bancos de dados que armazenam **dados não estruturados ou semiestruturados**, mas que também podem suportar alguns comandos SQL.

Dados não estruturados são aqueles que **não possuem uma estrutura pré-definida ou organização consistente**. Eles podem conter campos que são diferentes entre si, ou que mudam constantemente. Podem também se tratar de informações não textuais, como vídeos, imagens, mapas, etc. Já os dados **semiestruturados** têm uma **organização parcial**, como campos nomeados, mas **sem um schema rígido** (um JSON, por exemplo, que segue uma estrutura chave-valor, mas aceita diferentes tipos de valores e quantidades de campos por registro). Por conta dessas características, eles não podem ser armazenados em bancos de dados relacionais em seu formato "puro", ou seja, precisariam ser transformados para se adequar à estrutura do banco relacional.

Bancos de dados NoSQL constumam ser divididos em **quatro categorias**, baseado no tipo de dados que armazenam:

- **Documentos**: coleção de documentos, cada um possuindo sua estrutura e tipos de dados próprios. Esses documentos costumam vir em formatos como JSON, XML, BSON, YAML, etc;

- **Chave-valor**: os dados são representados como coleção de pares chave-valor, ou seja, cada registro possui uma chave única, que armazena algum valor de algum tipo. Por meio da busca pela chave, você obtém o valor;

- **Formato colunar**: os registros de dados são armazenados separadamente em colunas, não em linhas. Isso aumenta a eficiência de acesso a uma coluna, quando o interesse é em resgatar várias linhas para um atributo específico (para, por exemplo, obter alguma métrica);

- **Grafos**: representam os dados como nós e usam as arestas para estabelecer relacionamento entre esses dados. É uma estrutura útil para se verificar como diferentes conjuntos de dados se relacionam entre si e obter informações a respeito de relacionamentos mais complexos.

## Escalabilidade

Um dos principais pontos de diferença entre bancos SQL e NoSQL é com relação à escalabilidade, ou seja, o crescimento da base conforme novos dados vão sendo adicionados. 

Dizemos que bancos de dados **SQL tradicionalmente escalam verticalmente**, enquanto **NoSQL escalam horizontalmente**.

- Escalar **verticalmente** significa que os dados estão armazenados em uma mesma máquina (servidor) e, quando novos recursos são necessários (mais poder de processamento, RAM, armazenamento) você **transfere os dados para um servidor mais potente** ou **adiciona mais recursos ao servidor atual**. Uma analogia seria um prédio, que vai aumentando verticalmente à medida de novos andares vão sendo construídos para suportar mais salas;

- Escalar **horizontalmente** acontece quando os **dados estão distribuídos** em várias máquinas (servidores) organizadas em uma rede, onde se comunicam e se organizam para manter e disponibilizar esses dados. É um sistema distribuído. Quando novos recursos são necessários, você **adiciona novos servidores à rede**. Seguindo a analogia, ao invés de aumentar um prédio, você tem um conjunto de prédios interligados e, quando precisa de novas salas, constrói novos prédios e caminhos.

> Bancos SQL também podem escalar horizontalmente por meio de técnicas como sharding ou particionamento lógico, mas isso geralmente envolve maior complexidade e não é nativamente suportado como nos bancos NoSQL.

## Quando usar bancos SQL e NoSQL

É o **famoso depende**. Depende do seu objetivo e do uso que você vai fazer dos dados a serem armazenados.

Bancos **relacionais** são úteis para **dados que precisam estar em conformidade com algum tipo de regulação ou segurança**, como operações financeiras, dados de saúde, estoque de produtos. Por conta das transações ACID, esse bancos garantem a **integridade e consistência** dos dados. 

Bancos de dados relacionais também são úteis quando os dados estão, de fato, relacionados. Ou seja, você consegue **enxergar uma estrutura definida** nos dados que serão armazenados. Esses bancos foram feitos e são otimizados para isso.

Bancos **NoSQL** foram desenvolvidos para lidar com **volume massivo de dados**. Devido à escalabilidade horizontal, são indicados para processar e armazenar grandes volumes de dados. Eles também são úteis para dados que **mudam constantemente**, já que trabalham com dados não estruturados. 

Dado a característica de estarem distribuídos, os bancos NoSQL também são úteis quando se tem um **alto volume de leitura e escrita**, ou para dados que muitas vezes precisam estar **disponíveis de maneira rápida**, ao custo de uma perda aceitável de inconsistência ou consistência eventual. Por exemplo, dados de sensores, dispositivos IoT, análise de logs.

## Teorema CAP

> Esse texto foi inicialmente escrito pelo ChatGPT e depois revisado por mim, baseado [neste artigo da IBM](https://www.ibm.com/topics/cap-theorem).

O Teorema CAP (ou Teorema de Brewer) é um princípio fundamental na escolha e design de bancos de dados distribuídos. Ele estabelece que um **sistema distribuído** consegue **garantir somente dois dos três atributos** abaixo:

- **Consistência (Consistency)**: Todos os nós do sistema distribuem a mesma versão dos dados, garantindo que uma leitura sempre retorne o valor mais recente. Quando um dado é escrito em um nó, deve ser replicado em todos os nós para que a escrita seja considerada bem-sucedida, por exemplo.

- **Disponibilidade (Availability)**: Todos os pedidos de leitura e escrita recebem uma resposta, mesmo que parte do sistema falhe.

- **Tolerância a Partições (Partition Tolerance)**: O sistema continua funcionando mesmo que haja falhas de comunicação entre partes da rede. Essa falha ou quebra na comunicação é denominado "partição".

Bancos de dados NoSQL geralmente escolhem:

- Consistência e Tolerância a Partições (CP): caso haja uma partição, o sistema fica indisponível até a situação ser resolvida, para evitar que nós retornem dados antigos ou desatualizados. Exemplo: MongoDB.

- Disponibilidade e Tolerância a Partições (AP): quando ocorre uma partição, o sistema continua disponível, mas alguns nós podem retornar dados antigos até a partição ser resolvida. As inconsistências são então tratadas. É o que chamamos de "consistência eventual". Exemplo: Apache Cassandra.

Essa escolha tem impacto na arquitetura do banco e ajuda a decidir qual NoSQL usar dependendo do cenário e das necessidades da aplicação.

## MongoDB

Anotações baseadas no curso da Alura ["MongoDB: conhecendo um banco de dados NoSQL"](https://cursos.alura.com.br/course/mongodb-banco-dados-nosql), ministrado pela [Danielle Oliveira](https://www.linkedin.com/in/danielle-oliveira-071550134/).

[Documentação MongoDB](https://www.mongodb.com/pt-br/docs/) em português.

### Introdução

É um banco de dados NoSQL que armazena **documentos**. Um conjunto de documentos é chamado de **coleção** (que seria um análogo a uma tabela de BDs relacionais).

- documentos **BSON** (/bizon/ ou /beizon/), uma representação binária de um JSON, que trabalha com mais tipos de dados. Continua sendo um conjunto de pares chave-valor. Por ser binário, é mais compacto e mais otimizado pelo MongoDB para consultas.

Open source, com versões gratuitas (Community Edition) e pagas (Enterprise Edition, com mais recursos e suporte), além de uma versão na nuvem (Atlas, com planos gratuitos e pagos).

Na instalação da [Community Edition](https://www.mongodb.com/try/download/community), há a opção de instalar também o MongoDB Compass. Esse software permite trabalhar com o MongoDB por meio de uma interface gráfica. 

Se quiser trabalhar na linha de comando, uma opção é instalar o [MongoDB Shell](https://www.mongodb.com/try/download/shell) (ou mongosh). Ao instalar o executável (`.msi`), basta abrir o prompt de comando e digitar `mongosh` para começar a usar o MongoDB Shell.

### Comandos mongosh

`show databases`: mostra suas bases de dados.

---

`use nome_da_base`: conecta em uma base. Se ela não existir, será criada.

- nomes **NÃO** são case sensitive (minhaBase e MinhaBase serão o mesmo banco);

- máximo 64 caracteres;

- caracteres proibidos: `/. "$*<>:|?` (Windows) e `/. "$` (Linux);

- o banco só é de fato criado se ao menos uma coleção for criada.

---

`db.createCollection('nome_da_colecao')`: cria uma nova coleção de documentos. 

- nomes **são** case sensitive (minhaColecao e MinhaColecao serão coleções diferentes);

- pode usar aspas simples ou duplas para o nome da coleção;

- Há um segundo parâmetro que aceita um objeto com opções de configuração.

---

`show collections`: mostra as coleções para o banco que está conectado.

---

`db.nome_da_colecao.drop()`: remove uma coleção.

- se não houver mais coleções, o banco também é removido.

---

`db.dropDatabase()`: remove um banco de dados.

- precisa estar **conectado** ao banco que você quer remover.

---

#### Inserção

`db.nome_da_colecao.insertOne({documento})`: método para inserir um novo documento na coleção;

`db.nome_da_colecao.insertMany([{documento1}, {documento2}, ...])`: inserção de múltiplos documentos. Listados como se fossem elementos de um array.

- no mongosh, o nome das chaves pode ser escrito sem aspas se forem uma única palavra. Caso use mais de uma palavra (não é uma boa prática), use aspas (simples ou duplas).

---

#### Consultas

`db.nome_da_colecao.find()`: retorna **todos** os documentos da coleção mencionada.

---

`db.nome_da_colecao.find(<filtro>)`: o **primeiro parâmetro** do método `find` aceita um objeto com **filtro** dos documentos a serem retornados. Semelhante à cláusula `WHERE` em uma consulta SQL. 

- O exemplo a seguir retorna documentos da coleção `series` que tenham um campo 'Ano de lançamento' cujo valor seja 2018 ou 2019:

  ```js
  db.series.find({'Ano de lançamento': {$in: [2018, 2019]}})
  ```

A [documentação do MongoDB](https://www.mongodb.com/docs/manual/reference/operator/query/#query-selectors) lista os operadores disponíveis para queries.

---

`db.nome_da_colecao.find({}, <projecao>)`: o **segundo parâmetro** do método é um objeto em que você especifica quais **campos** devem ser retornados na consulta. Semelhante a especificar quais colunas devem ser retornados em um `SELECT` do SQL.

- No exemplo abaixo, especificamos que queremos todos os documentos (objeto vazio `{}` no primeiro parâmetro), mas retornando somente os campos `Série` e `Ano de lançamento`. Utilizamos `1` para especificar que esses campos devem ser retornados. O campo `_id` é sempre retornado pela busca e por isso utilizamos o valor `0` para especificar que ele não deve ser retornado.

  ```js
  db.series.find({}, {'Série': 1, 'Ano de lançamento': 1, '_id': 0})
  ```

---

O próximo exemplo mostra o uso de outros métodos para **modificar o comportamento** da consulta. Aqui, buscamos documentos que possuam o campo `Genero` sendo "Ação" ou "Comédia" e selecionamos os campos a serem retornados. Além disso, após o `find()`, usamos o `sort()` para ordenar o resultado pelo campo `Série` em ordem crescente e o método `limit()` para limitar o número máximo de documentos retornados. 

```js
db.series.find({'Genero': {$all: ['Ação', 'Comédia']}}, {'Série': 1, 'IMDb Avaliação': 1, '_id': 0}).sort({'Série': 1}).limit(5)
```

A [documentação do MongoDB](https://www.mongodb.com/docs/manual/reference/method/db.collection.find/#examples) traz uma série de outros exemplos.

--- 

#### Atualização

`db.nome_da_colecao.updateOne(<filtro>, <update>)`: atualiza os campos de um documento na coleção mencionada.

- o primeiro parâmetro é um objeto com o filtro para encontrar o documento;

- o segundo parâmetro é um objeto com o operador `$set` e o campo (ou campos) a ser atualizado. Se o campo não existir, será criado.

- Exemplo atualizando o campo "Temporadas disponíveis" da série "Grimm":

  ```js
  db.series.updateOne({'Série': 'Grimm'}, {$set: {'Temporadas disponíveis': 6}})
  ```

---

`db.nome_da_colecao.updateMany(<filtro>, <update>)`: método para atualizar mais de um documento. Os parâmetros são os mesmo de `updateOne`, sendo que no `filtro` você utiliza algum seletor para encontrar mais de um documento.

- No exemplo abaixo, atualizamos dois campos para duas séries:

  ```js
  db.series.updateMany({'Série': {$in: ['Four More Shots Please', 'Fleabag']}}, {$set: {'Classificação': '18+', 'Temporadas disponíveis': 2}})
  ```

--- 

#### Remoção

`db.nome_da_colecao.deleteOne(<filtro>)`: remove um documento da coleção mencionada, correspondente ao `filtro` passado.

- se `filtro` retornar mais de um documento, somente o primeiro será removido.

---

`db.nome_da_colecao.deleteMany(<filtro>)`: remove todos os documentos da coleção mencionada retornados pelo `filtro` passado.

- o exemplo a seguir removerá todos os documentos que tenham menos de 3 temporadas disponíveis:

```js
db.series.deleteMany({'Temporadas disponíveis': {$lte: 2}})
```

### Detalhes sobre inserção de documentos

Para inserir um documento a uma coleção, você passa os dados em um objeto com um conjunto de chave-valor. No MongoDB **Compass**, chaves são strings e **deve ser utilizado aspas duplas** (`"`). O mesmo para valores do tipo string.

Exemplo de um documento no Compass:

```json
{
  "Série": "Pataal Lok",
  "Ano de lançamento": 2020,
  "Temporadas disponíveis": 1,
  "Linguagem": "Hindi",
  "Genero": ["Drama"],
  "IMDB Avaliação": 7.5,
  "Classificação": "18+"
}
```

Documentos no MongoDB possuem um campo `_id`, de valor único, utilizado como chave primária. Quando **não informado, o próprio MongoDB irá criar esse campo**, com um valor hexadecimal. Por exemplo: `"_id": { "$oid": "6703f9afcb60596fb9698dcd" }`.