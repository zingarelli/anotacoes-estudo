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

A palavra já significou "no SQL" (não SQL), mas hoje evoluiu para **Not only SQL** (não somente SQL). É uma abordagem para gerenciar bancos de dados que armazenam **dados não estruturados ou semiestruturados**, mas que também podem suportar alguns comandos SQL.

Dados não estruturados são aqueles que **não possuem uma estrutura pré-definida ou organização consistente**. Eles podem conter campos que são diferentes entre si, ou que mudam constantemente. Podem também se tratar de informações não textuais, como vídeos, imagens, mapas, etc. Já os dados **semiestruturados** têm uma **organização parcial**, como campos nomeados, mas **sem um schema rígido** (um JSON, por exemplo, que segue uma estrutura chave-valor, mas aceita diferentes tipos de valores e quantidades de campos por registro). Por conta dessas características, eles não podem ser armazenados em bancos de dados relacionais em seu formato "puro", ou seja, precisariam ser transformados para se adequar à estrutura do banco relacional.

Bancos de dados NoSQL constumam ser divididos em quatro categorias, baseado no tipo de dados que armazenam:

- Documentos: coleção de documentos, cada um possuindo sua estrutura e tipos de dados próprios. Esses documentos costumam vir em formatos como JSON, XML, BSON, YAML, etc;

- Chave-valor: os dados são representados como coleção de pares chave-valor, ou seja, cada registro possui uma chave única, que armazena algum valor de algum tipo. Por meio da busca pela chave, você obtém o valor;

- Formato colunar: os registros de dados são armazenados separadamente em colunas, não em linhas. Isso aumenta a eficiência de acesso a uma coluna, quando o interesse é em resgatar várias linhas para um atributo específico (para, por exemplo, obter alguma métrica);

- Grafos: representam os dados como nós e usam as arestas para estabelecer relacionamento entre esses dados. É uma estrutura útil para se verificar como diferentes conjuntos de dados se relacionam entre si e obter informações a respeito de relacionamentos mais complexos.

## Escalabilidade

Um dos principais pontos de diferença entre bancos SQL e NoSQL é com relação à escalabilidade, ou seja, o crescimento da base conforme novos dados vão sendo adicionados. 

Dizemos que bancos de dados SQL tradicionalmente escalam verticalmente, enquanto noSQL escalam horizontalmente.

- Escalar **verticalmente** significa que os dados estão armazenados em uma mesma máquina (servidor) e, quando novos recursos são necessários (mais poder de processamento, RAM, armazenamento) você **transfere os dados para um servidor mais potente** ou **adiciona mais recursos ao servidor atual**. 

- Escalar **horizontalmente** acontece quando os **dados estão distribuídos** em várias máquinas (servidores) organizadas em uma rede, onde se comunicam e se organizam para manter e disponibilizar esses dados. É um sistema distribuído. Quando novos recursos são necessários, você **adiciona novos servidores à rede**.

> Bancos SQL podem escalar horizontalmente por meio de técnicas como sharding ou particionamento lógico, mas isso geralmente envolve maior complexidade e não é nativamente suportado como nos bancos NoSQL.

## Quando usar bancos SQL e NoSQL

É o **famoso depende**. Depende do seu objetivo e do uso que você vai fazer dos dados a serem armazenados.

Bancos relacionais são úteis para dados que precisam estar em conformidade com algum tipo de regulação ou segurança, como operações financeiras, dados de saúde, estoque de produtos. Por conta das transações ACID, esse bancos garantem a integridade e consistência dos dados. 

Bancos de dados relacionais também são úteis quando os dados estão, de fato, relacionados. Ou seja, você consegue enxergar uma estrutura definida nos dados que serão armazenados. Esses bancos foram feitos e são otimizados para isso.

Bancos NoSQL foram desenvolvidos para lidar com volume massivo de dados. Devido à escalabilidade horizontal, são indicados para processar e armazenar grandes volumes de dados. Eles também são úteis para dados que mudam constantemente, já que trabalham com dados não estruturados. 

Dado a característica de estarem distribuídos, os bancos NoSQL também são úteis quando se tem um alto volume de leitura e escrita, ou para dados que muitas vezes precisam estar disponíveis de maneira rápida, ao custo de uma perda aceitável de inconsistência ou consistência eventual. Por exemplo, dados de sensores, dispositivos IoT, análise de logs.

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