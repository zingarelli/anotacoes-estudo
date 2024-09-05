# MySQL

## Créditos

Estas anotações são baseadas nos cursos da Alura para a formação ["Consultas com MySQL"](https://www.alura.com.br/formacao-consultas-mysql)[^1], ministrados por:

- [Beatriz Magalhães](https://cursos.alura.com.br/user/beatriz280197);

- [Victorino Vila](https://www.linkedin.com/in/victorino-vila-1a160/);

- [Danielle Oliveira](https://www.linkedin.com/in/danielle-oliveira-071550134/);

- [Igor Nascimento Alves](https://www.linkedin.com/in/igor-nascimento-alves-145910127/).

[^1]: complementado por tópicos da formação [Conhecendo SQL](https://www.alura.com.br/formacao-conhecendo-sql).

## Pratique SQL

O site [SQL Murder Mystery](https://mystery.knightlab.com) (em inglês) permite praticar SQL por meio de um jogo interativo em que você é um detetive tentando solucionar um assassinato.

## Sobre o MySQL

É um **SGBD** (Sistema de Gerenciamento de Banco de Dados) que utiliza a **linguagem SQL** (Structured Query Language) para manipulação de **dados** armazenados em forma de **tabelas**.

> **Bancos de dados relacionais** armazenam os dados em tabelas, que se "comunicam"/"relacionam" entre si. Ou seja, as tabelas possuem informações que se ligam de alguma forma. O relacionamento é estabelecido por meio de chaves primária e estrangeira.

Documentação versão 8.0: [link](https://dev.mysql.com/doc/refman/8.0/en/).

Download da versão MySQL Community Edition: [link](https://dev.mysql.com/downloads/). 

- Esta é a versão open source para desenvolvedores. No curso, foi utilizada a versão 8.0.36.

- Baixamos o arquivo MySQL Installer for Windows e selecionamos a opção full, para instalar tanto o servidor quanto as ferramentas para desenvolvimento. Nessa opção, é instalado o **MySQL Workbench**, que é uma interface gráfica utilizada no curso para se trabalhar com o MySQL. Se você prefere uma ferramenta baseada em linha de comando, também é instalado o MySQL Shell.

> Posteriormente, quando você abrir o MySLQ Workbench e se deparar com erros tipo "Target host is configured as windows, but seems to be a different OS." ou "Current profile has no WMI enabled.", abra os Serviços do Windows e verifique se o MySQL está em execução. Se não estiver, inicie o serviço e reabra o MySQL Workbench.

## Convenções do MySQL

Seguem algumas convenções de nomes e definições que o MySQL usa (ou que os instrutores usam) e que podem ser diferentes das utilizadas por outros sistemas ou na literatura sobre banco de dados.

- Schema: é mesmo que banco de dados (database); é o conjunto de tabelas, views, procedures e funções;

- Palavras reservadas do SQL: comandos como `SELECT`, `CREATE`, etc., podem ser utilizados tanto com letras maiúsculas ou minúsculas, e o MySQL Workbench irá entender;

    - uma convenção da área é manter os comandos em letras maiúsculas e os nomes de tabelas, funções, etc, em letras minúsculas.

- Modelo físico do banco de dados: conceito parecido com o DER (Diagrama Entidade-Relacionamento), só que mais detalhado. No MySQL Workbench, podemos ter uma versão "enhanced" do DER na opção Database -> Reverse Engineer...;

- tupla: linha de uma tabela. Às vezes também escrevo nas anotações como sendo "registro" ou "entrada". Entenda todas como sinônimos.

- Uso do ponto e vírgula (`;`): indica o fim de um comando. Embora não seja obrigatório em todas as consultas, a ausência dele pode dar dores de cabeça ou efeitos indesejados, por exemplo, quando você quer executrar múltiplos comandos de uma vez. Então é considerada boa prática adicioná-lo ao final de cada comando SQL;

- Uso de aspas simples: quando tratar de strings, opte pelo uso de aspas simples. O MySQL também aceita aspas duplas, mas [há exceções](https://dev.mysql.com/doc/refman/8.4/en/string-literals.html).

## Tipo de dados

A [documentação do MySQL](https://dev.mysql.com/doc/refman/8.4/en/data-types.html) possui seções com os diversos tipos de dados que podem ser utilizados na criação das tabelas.

Exemplos de tipos: INT (e variações TINYINT, SMALLINT, MEDIUMINT, BIGINT), FLOAT, DOUBLE, DECIMAL, DATE, CHAR, VARCHAR, TEXT, BOOLEAN, ENUM, SET.

Tipos aceitos como **chave primária**: 

- INT (incluindo a opção de AUTO_INCREMENT); 

- UUID (sequência de números hexadecimais);

- CHAR/VARCHAR;

- combinação de outras colunas da tabela, desde que elas gerem valores que sejam únicos.

## Comandos

```sql
CREATE DATABASE <nome_da_base>;
```

- Cria o schema com o nome que você digitou em `nome_da_base`.

---

```sql
USE <nome_da_base>;
```

- Ativa o schema; assim, os próximos comandos serão baseados nesse schema.

- No MySQL Workbench, você pode simplesmente dar dois cliques no schema.

---

```sql
CREATE TABLE <nome_da_tabela> (
<nome_da_coluna> <tipo_da_coluna>,
-- outras colunas...
);
``` 

- Cria uma tabela `nome_da_tabela` e suas colunas. 

- Segue um exemplo mais completo, incluindo chaves primária e estrangeiras:


    ```sql
    CREATE TABLE alugueis (
        aluguel_id VARCHAR(255) PRIMARY KEY,
        cliente_id VARCHAR(255),
        hospedagem_id VARCHAR(255),
        data_inicio DATE,
        data_fim DATE,
        preco_total DECIMAL(10, 2),
        FOREIGN KEY (cliente_id) REFERENCES clientes(cliente_id),
        FOREIGN KEY (hospedagem_id) REFERENCES hospedagens(hospedagem_id)
    );
    ```
    
---

```sql
INSERT INTO <nome_da_tabela> VALUES (
    <valor_coluna_1>,
    -- valor das demais colunas, separadas por vírgula
);
```

- Insere um novo dado (linha) na tabela. Os valores de cada coluna devem ser na mesma ordem em que as colunas estão na tabela.

- Se quiser inserir múltiplas linhas, basta colocar os novos valores em parênteses, cada linha separada por vírgula.

---

```sql
INSERT INTO <nome_da_tabela> (
    <nome_coluna_a>, 
    <nome_coluna_b>
    <nome_coluna_c>, 
    -- ...
) 
VALUES (
    <valor_coluna_a>, 
    <valor_coluna_b>, 
    <valor_coluna_c>, 
    -- ...
);
```

- Você pode optar por escolher para quais colunas quer inserir dados, especificando o nome dessas colunas e colocando os valores na mesma sequência das colunas especificadas.

---

```sql
ALTER TABLE <nome_da_tabela>
ADD COLUMN <nome_da_coluna> <tipo_da_coluna>
-- opcionalmente você pode adicionar mais colunas, 
-- separadas por virgula
```

- Adiciona uma nova coluna à tabela. É inserido NULL como valor dessa coluna nos registros existentes.

---

```sql
ALTER TABLE <nome_da_tabela>
RENAME TO <novo_nome>;
```

- Altera o nome da tabela.

---

```sql
ALTER TABLE  <nome_da_tabela> 
RENAME COLUMN <nome_da_coluna> TO <novo_nome_da_coluna>
-- opcionalmente você pode renomear mais colunas, 
-- separadas por virgula
```

- Altera o nome de uma ou mais colunas

---

```sql
UPDATE <nome_da_tabela>
SET <nome_da_coluna> = <novo_valor>
WHERE <condicao>;
```

- atualiza uma coluna da tabela com um novo valor.

- **não esqueça** de adicionar a cláusula `WHERE` para indicar em quais linhas esse update deve ser feito. Caso não use o `WHERE`, o update será feito em **todas** as linhas.

- uma dica para ter certeza de que está fazendo corretamente: rode previamente um `SELECT` para verificar se as linhas que você quer modificar serão as retornadas por esse `SELECT`.

---

```sql
DELETE FROM <nome_da_tabela>
WHERE <condicao>;
```

- apaga os registros na tabela que atendem à condição especificada.

- aqui valem as mesmas observações feitas para a cláusula de `UPDATE`.

    - **não esqueça da cláusula `WHERE`**;

    - **não esqueça da cláusula `WHERE`**;

    - **não esqueça da cláusula `WHERE`**;

    - **não esqueça da cláusula `WHERE`**;

- sempre vale a pena **ter um backup** do banco antes de prosseguir com qualquer operação de remoção de dados.

- **atenção para o caso de chave estrangeira!** Caso a entrada a ser apagada contenha alguma coluna que é referenciada como chave estrangeira em uma ou mais tabelas, o comando **não** será executado e um erro será mostrado. Isso é feito para garantir a integridade do banco.

    - nesses casos, você deve verificar na mensagem de erro qual tabela possui a chave estrangeira e aplicar a remoção também a esta tabela, e aí então proceder com a remoção na tabela principal.

    - existem algumas cláusulas na criação de uma tabela com chave estrangeira que possibilitam informar como a remoção do registro será tratada (por exemplo: `ON DELETE CASCADE`, `ON DELETE SET NULL`, etc); por padrão, quando não especificado, a estratégia é restringir a remoção (`ON DELECT RESTRICT`).

---

```sql
DROP TABLE <nome_da_tabela>;
```

- apaga a tabela inteira (tabela e seu conteúdo). Use com cautela e seguro de que realmente quer fazer isso.

- as considerações sobre chave estrangeira mencionadas no comando `DELETE` também valem aqui.

## Consultas

```sql
SELECT * FROM <nome_da_tabela>;
```

- Consulta simples que traz tudo (`*`) da tabela informada.

---

```sql
SELECT <nome_da_coluna_x>, <nome_da_coluna_y> FROM <nome_da_tabela>;
```

- Você pode consultar colunas específicas de uma tabela, informando o nome de cada uma delas (como cadastradas na tabela).

---

```sql
SELECT * FROM <nome_da_tabela> WHERE <condicao>;
```

- Uso da cláusula `WHERE` para fazer uma filtragem mais específica, baseado em uma ou mais condições.

- Exemplo

    ```sql
    SELECT * FROM hospedagens
    WHERE tipo = 'hotel' AND ativo = 1;
    ```

---

```sql
SELECT DISTINCT <nome_da_coluna> FROM <nome_da_tabela>;
```

A cláusula `DISTINCT` elimina os valores repetidos para a coluna (ou colunas) que você especificou, ou seja, retorna somente uma única ocorrência dos valores para esta coluna(s).

### Exemplos de consultas

Para estes exemplos, suponha que a base de dados é de uma plataforma de aluguel de imóveis por temporadas curtas, tipo Airbnb.

```sql
SELECT * FROM hospedagens
WHERE hospedagem_id IN ('1', '10', '100');
```

- Consulta limitando o retorno a somente 3 correspondências de ID de hospedagem.

- A cláusula `IN` possibilita informar um conjunto de valores que serão aceitos nas condições do filtro.

---

```sql
SELECT 
	(SELECT COUNT(*) FROM proprietarios) AS qtd_proprietarios,	
	(SELECT COUNT(*) FROM hospedagens) AS qtd_hospedagens,
	(SELECT COUNT(*) FROM alugueis) AS qtd_alugueis;
```

- Uma consulta bem simples para mostrar que é possível usar `SELECT` de forma aninhada, ou seja, aproveitar o resultado de um (ou mais) `SELECT` em outro `SELECT`. Neste caso, estamos retornando de uma única vez a quantidade de registros nas tabelas proprietarios, hospedagens e alugueis.

- `COUNT()` é uma função que conta as linhas **não nulas**. No caso, como estamos usando `COUNT(*)`, ele irá também contar as linhas nulas, se houver, para cada proprietário (a consulta está agrupada por `nome`).

- `AS` é utilizado quando queremos dar um "apelido" (*alias*) a uma coluna/resultado. Esse apelido aparece somente no resultado da consulta, ou seja, a coluna na tabela não é renomeada.

---

```sql
SELECT cliente_id, AVG(preco_total) AS ticket_medio
FROM alugueis
GROUP BY cliente_id;
```

- Consulta que está agrupando os clientes pelo ID e calculando uma média que eles gastam dentre os vários alugueis que já pagaram. 

- `AVG()` é uma **função de agregação** que calcula a média.

- `GROUP BY` costuma ser utilizado em conjunto com funções de agregação, como a `AVG()`. Quando os dados são agrupados, as funções de agregação são aplicadas separadamente para o conjunto de dados **de cada agrupamento.**

    - após agrupado, você pode aplicar um filtro à consulta usando o `HAVING`. É semelhante ao `WHERE`, porém, específico para conjuntos agrupados.

---

```sql
SELECT 
  cliente_id, 
  AVG(DATEDIFF(data_fim, data_inicio)) AS media_dias_estadia 
FROM alugueis
GROUP BY cliente_id
ORDER BY media_dias_estadia DESC
```

- Função que verifica a média de dias de cada cliente quando fica hospedado, ordenando a média de maneira decrescente.

- `DATEDIFF()` é uma função para subtração entre valores do tipo `DATE`.

- `ORDER BY` é a cláusula para ordenação do resultado baseado em alguma coluna. `DESC` indica ordenação decrescente. Por padrão, a ordem é ascendente (`ASC`).

---

```sql
SELECT 
  p.nome AS nome_proprietario, 
  COUNT(*) AS total_hospedagens_ativas
FROM proprietarios p
JOIN hospedagens h ON p.proprietario_id = h.proprietario_id
WHERE h.ativo = 1
GROUP BY p.nome
ORDER BY total_hospedagens_ativas DESC
LIMIT 10;
```

- Consulta envolvendo 2 tabelas, para verificar o "top 10" dos proprietários que tenham a maior quantidade de hospedagens ativas cadastradas em seu nome.

- `p` e `h` são *aliases* (apelidos) para as tabelas `proprietarios` e `hospedagens`. O uso de um alias permite facilitar na hora de identificar quais colunas pertencem a cada tabela, de modo a diferenciá-las na consulta (por exemplo, `p.proprietario_id` e `h.proprietario_id` possibilita diferenciar a coluna `proprietario_id` presente nas duas tabelas). Usamos o `.` para acessar as colunas a partir de um alias.

- `JOIN <nome_da_tabela> ON <condicao>` é a cláusula usada para combinar duas ou mais tabelas. No caso do MySQL, a cláusula `JOIN` executa um `INNER JOIN`. Com ela, podemos cruzar os resultados de duas tabelas baseado em uma ou mais condições. No caso dessa consulta, estamos cruzando os resultados baseado na condição de trazer os dados de `h` que tenham o mesmo ID de proprietarios da tabela `p`. Ou seja, se em `h` houver um ID de proprietário que não existe em `p`, essa tupla não será considerada.

    - Veja mais sobre o `JOIN` [nesta Seção específica](#tipos-de-join);

- `LIMIT` é a cláusula que usamos para limitar a quantidade de resultados vindos da consulta. Como queremos o "top 10", limitamos a consulta aos 10 primeiros resultados (que foram previamente ordenados de forma decrescente pelo total de hospedagens ativas).

---

```sql
SELECT 
  YEAR(data_inicio) AS ano,
  MONTH(data_inicio) AS mes,
  COUNT(*) AS total_reservas
FROM alugueis
GROUP BY ano, mes
ORDER BY total_reservas DESC;
```

- Consulta do total de reservas (aluguéis) feitas por ano e mês, de modo a verificar os meses de maior e menor demanda em cada ano. Uma consulta assim pode indicar a sazonalidade.

- As funções `YEAR()` e `MONTH()` extraem o ano e o mês de uma dada data.

- A cláusula `GROUP BY` aceita **agrupamento composto**, fazendo o agrupamento na ordem em que as colunas foram indicadas. No caso do exemplo, o resultado é agrupado por ano e depois, dentro de cada ano, é agrupado por mês, e o `COUNT()` faz a soma de cada um desses agrupamentos compostos.

---

```sql
-- Transformando o CPF, armazenado como uma string somente com números, 
-- para um formato com pontos e traço (XXX.XXX.XXX-XX).
-- A função UPPER passa todas as letras para maiúsculo.
-- A função TRIM elimina espaços em branco no início e final da string.
-- A função CONCAT concatena strings, separadas por vírgula.
-- A função SUBSTR(campo, inicio, qtd) retorna parte de uma string de 
-- "campo", a partir de "inicio" (o índice começa em 1) e extraindo 
-- uma quantidade "qtd" de caracteres, incluindo o caracter de inicio
SELECT 
	UPPER(TRIM(nome)) as nome,
	CONCAT(SUBSTR(cpf, 1, 3), '.',
		SUBSTR(cpf, 4, 3), '.',
		SUBSTR(cpf, 7, 3), '-',
		SUBSTR(cpf, 10, 2)) as CPF,
	contato as 'e-mail'
FROM clientes;
```

- Consulta para mostrar nome, CPF e e-mail dos clientes, utilizando algumas funções para manipular strings.

---

```sql
SELECT 
	h.tipo tipo, 
    AVG(a.nota) media_avaliacao,
    TRUNCATE(AVG(a.nota), 2) media_truncada,
    ROUND(AVG(a.nota), 2) media_arredondada,
    ROUND(AVG(a.nota), 3) media_arredondada_tres_casas,
    CEIL(AVG(a.nota)) media_arredondada_pra_cima,
    FLOOR(AVG(a.nota)) media_arredondada_pra_baixo
FROM avaliacoes a
JOIN hospedagens h ON h.hospedagem_id = a.hospedagem_id
GROUP BY h.tipo;
```

- Média geral das avaliações, baseada no tipo da hospedagem, utilizando algumas funções para manipular números. Exemplo de retorno:

| tipo | media_avaliacao | media_truncada | media_arredondada | media_arredondada_tres_casas | media_arredondada_pra_cima | media_arredondada_pra_baixo |
| - | - | - | - | - | - | - |
| 'apartamento' | '2.9565' | '2.95' | '2.96' | '2.957' | '3' | '2' |
| 'casa' | '2.9756' | '2.97' | '2.98' | '2.976' | '3' | '2' |
| 'hotel' | '3.0618' | '3.06' | '3.06' | '3.062' | '4' | '3' |

## Tipos de JOIN

As cláusulas `JOIN` combinam linhas de duas ou mais tabelas, permitindo recuperar dados de todas essas tabelas envolvidas no `JOIN`. Com isso, seu `SELECT` pode **retornar colunas de tabelas diferentes em uma mesma consulta**.

- `INNER JOIN`: Retorna linhas quando há pelo menos uma correspondência em **ambas** as tabelas. No MySQL, você pode usar somente `JOIN` e será efetuado um `INNER JOIN`;

- `LEFT JOIN`: Retorna todas as linhas da tabela à esquerda (a primeira tabela da consulta) e as linhas correspondentes da tabela à direita (a segunda tabela, indicada após o comando de join). Inclui também os resultados em que **não houver correspondência** com a tabela à direita, marcados como NULL no lado direito;

- `RIGHT JOIN`: É o inverso do `LEFT JOIN`, ou seja, retorna todas as linhas da tabela à direita e as correspondentes da tabela à esquerda, marcando NULL quando não há correspondência do lado esquerdo;

- `FULL JOIN`: Combina `LEFT JOIN` e `RIGHT JOIN`, retornando todas as linhas em que houve uma correspondência em uma das tabelas, marcando NULL para as colunas da tabela quando não há correspondência.

    - O **MySQL não tem suporte nativo ao `FULL JOIN`**. Nesse caso, você pode combinar `LEFT JOIN` e `RIGHT JOIN` por meio da cláusula `UNION`.

    - A cláusula `UNION` faz a união dos resultados de dois ou mais `SELECT`s, exibindo somente os valores distintos (ou seja, caso haja linhas que se repitam nos `SELECT`s, somente uma é retornada). Se precisar que todas as linhas sejam retornadas, incluindo duplicatas, use `UNION ALL`. É necessário que as colunas em cada `SELECT` sejam as mesmas ou compatíveis (mesmo número de colunas e tipo de dados).

        - Exemplo criado pela ChatGPT: 

            ```SQL
            (
                -- LEFT JOIN: retorna todas as linhas de TabelaA e as correspondências em TabelaB
                SELECT A.id, A.valor AS valor_A, B.valor AS valor_B
                FROM TabelaA A
                LEFT JOIN TabelaB B ON A.id = B.id
            )
            UNION
            (
                -- RIGHT JOIN: retorna todas as linhas de TabelaB e as correspondências em TabelaA
                SELECT B.id, A.valor AS valor_A, B.valor AS valor_B
                FROM TabelaB B
                RIGHT JOIN TabelaA A ON B.id = A.id
            );
            ```

## View

View é uma forma de criar novas tabelas "virtuais", baseadas no retorno de consultas SQL feitas com as tabelas do banco. 

- é uma tabela "virtual" pois ela não armazena dados, mas sim uma consulta SQL. Isso significa que, toda vez que a view é acessada, a consulta SQL definida para essa view é **executada novamente**, garantindo trazer os dados mais atualizados. Isso traz o trade-off com relação a **desempenho**, especialmente para views muito complexas.

Uma de suas vantagens é criar uma ou mais tabelas que serão disponibilizadas para consulta. Por exemplo, por meio de uma view podemos decidir quais colunas serão "expostas" para consulta, bloqueando o acesso a consulta às tabelas originais. Assim, limitamos o acesso ao banco de dados a somente algumas views que queremos tornar públicas, protegendo dados sensíveis. 

Exemplo de view que cria uma tabela com os aluguéis e um desconto promocional baseado na quantidade de dias de estadia. O cálculo do desconto e o valor promoocional estão sendo feitos por duas funções customizadas cujo código não está no exemplo. Veja mais sobre [funções nesta Seção](#funções).

```sql
CREATE OR REPLACE VIEW viewAluguelPromocional AS
SELECT 
	aluguel_id, 
    cliente_id, 
    preco_total,
    getDesconto(aluguel_id) AS desconto,
    calculaDesconto(aluguel_id) AS preco_promocional
FROM alugueis;

-- usando a view
SELECT * FROM viewaluguelpromocional;
```

### Diferença entre WITH e VIEW

A cláusula `WITH` é outra forma de obter dados de uma ou mais tabelas e utilizar esses dados em outra consulta SQL. Diferente de uma View, a consulta criada com a cláusula `WITH` não fica armazenada no banco, ou seja, após a query ser executada, o resultado da consulta com `WITH` é apagado.

- lembre-se: uma View fica salva no banco (a consulta, e não o resultado), podendo ser reutilizada em outras consultas.

O resultado de uma cláusula `WITH` é chamado de **CTE (Common Table Expression)**. É uma maneira de criarmos uma ou mais **tabelas temporárias** que nos auxiliem em consultas mais complexas. Com isso, conseguimos simplificar a complexidade de uma query fazendo a quebra em consultas menores, que podem ser referenciadas.

- Exemplo criado pela ChatGPT, retornando a quantidade de vendas a partir de 1º de janeiro de 2024 (as vendas recentes, nomeada como RecentSales), filtradas a partir daquelas cuja quantidade ultrapassou 100:

    ```sql
        -- CTE
        WITH RecentSales AS (
            SELECT sale_id, sale_date, amount
            FROM sales
            WHERE sale_date > '2024-01-01'
        )
        -- we can reference the CTE in a more simplified query
        SELECT sale_id, amount
        FROM RecentSales 
        WHERE amount > 100;
    ```

## Trigger

É um procedimento armazenado no banco de dados, que é executado a partir de alguma condição. Com isso, triggers podem ser utilizadas para automatizar tarefas.

Por exemplo, uma trigger pode ser executada para atualizar uma coluna da tabela X quando um quando um novo item é adicionado a uma tabela Y.

- Exemplo da aula, adequado pela ChatGPT para o MySQL. Esta trigger refaz os dados da tabela faturamentodiario a cada nova inserção na tabela itenspedidos:

    ```sql
    -- uma trigger bem ineficiente
    DELIMITER //

    CREATE TRIGGER calculaFaturamentoDiario
    AFTER INSERT ON itenspedidos
    FOR EACH ROW
    BEGIN
        -- apaga o que já tinha
        DELETE FROM faturamentodiario;

        -- refaz os cálculos de faturamento de cada dia
        INSERT INTO faturamentodiario (dia, faturamento_diario)
        SELECT 
            DATE(p.datahorapedido) AS dia, 
            SUM(i.precounitario) AS faturamento_diario
        FROM pedidos p
        JOIN itenspedidos i
        ON i.idpedido = p.id
        GROUP BY dia
        ORDER BY dia;
    END //

    DELIMITER ;
    ```

## Transações

São blocos de comandos SQL que queremos ter certeza de que foram executados corretamente para que as atualizações seja de fato salvas no banco de dados. Caso algum comando falhe por algum motivo, queremos garantir que nada seja salvo no banco. Ou seja, esse bloco de comandos funciona como se fosse uma coisa única (princípio da atomicidade, veja mais sobre [ACID](#acid) abaixo). 

Em uma transação, confirmamos a gravação de fato dos resultados dos comandos executados por meio de um `COMMIT`. Caso algo saia errado, podemos reverter o banco ao estado anterior à transação por meio de um `ROLLBACK`.

### ACID

Acrônimo para Atomicidade, Consistência, Isolamento e Durabilidade. São princípios a serem seguidos pelos SGBDs no que tange as transações.

- Atomicidade: em uma transação, todas operações são concluídas com sucesso ou nenhuma é aplicada. Ocorrendo algum erro, o banco reverte as operações da transação que já tenham sido realizadas e volta ao estado original;

- Consistência: após uma transação, o banco sai de um estado válido e entra em outro estado válido. Isso significa que todas as regras e restrições definidas no schema continuam valendo;

- Isolamento: cada transação é executada de maneira isolada, evitando problemas de concorrência. Por exemplo, os efeitos de uma transação A não ficam visíveis às transações B, C e D enquanto A não for concluída. Cada banco decide como tratar transações que competem pelo mesmo recurso;

- Durabilidade: após o COMMIT de uma transação, é garantido que os dados foram persistidos e as mudanças no banco foram permanentes, mesmo se houver alguma falha de conexão, falta de energia, etc.

## Stored procedures

A maioria dos SGBDs criam suas próprias "extensões" ao SQL, para prover novas funcionalidades como laços, condicionais, funções e procedures.

Uma Stored Procedure é um bloco de comandos a serem executados, incluindo a possibilidade de definir variáveis, laços e condições. Elas são armazenadas no banco e podem ser reutilizadas. Com isso, você pode encapsular a lógica e regras de negócio relacionadas a alguma ação no banco, automatizando alguma tarefa repetitiva. 

Você pode entendê-las como sendo pequenos programas.

> Uma transação, apesar de ser também um bloco de comandos, não permite o uso de lógica estruturada nem de parâmetros. Sua finalidade é efetuar operações garantindo o cumprimento dos princípios ACID. Além disso, uma stored procedure pode ter uma ou mais transações, mas o inverso não se aplica.

O exemplo a seguir cria uma procedure que lista todos os clientes:

```sql
DELIMITER $$
CREATE PROCEDURE lista_clientes()
BEGIN
	SELECT * FROM clientes;
END$$
DELIMITER ;
```

O uso de delimitadores (`DELIMETER`) como `//` ou `$$` é para diferenciar o fim de comandos que são do bloco (que usam o `;` por padrão) e o fim de comandos que são da procedure em si. Após a declaração da procedure, é chamado o `DELIMITER ;` para voltar ao `;` padrão.

Para usar uma procedure, usamos o comando `CALL`:

```sql
CALL lista_clientes;
```

- Podemos chamar procedures dentro de outras procedures, inclusive passando parâmetros como referência usando a a palavra reservada `INOUT`. Um [exemplo](#passagem-de-parâmetro-por-referência) é dado na próxima Seção. 

Para apagar:

```sql
DROP PROCEDURE lista_clientes;
```

Para alterar uma stored procedure, apagamos ela e a criamos novamente, com as alterações (veja os [exemplos na próxima Seção](#exemplos-de-procedures)). Também podmeos alterar interativamente pelo MySQL Workbench, clicando com o botão direito na procedure e selecionando "Alter Stored Procedure...".

### Exemplos de procedures

Esses exemplos são evoluções de uma mesma procedure que insere um novo registro na tabela reservas. 

---

#### Uso de parâmetros e variáveis

Calculando o valor total da reserva baseado no número de dias e valor da diária. 

Observe que é possível **declarar parâmetros e variáveis** locais na procedure.

```sql
DROP PROCEDURE IF EXISTS nova_reserva;
DELIMITER //
-- procedure com parâmetros
CREATE PROCEDURE nova_reserva(
	vReserva VARCHAR(10),
	vCliente VARCHAR(10),
	vHospedagem VARCHAR(10),
	vDataInicio DATE, 
	vDataFim DATE,
    vPrecoUnitario DECIMAL(10,2) -- valor da diária
)
BEGIN
	-- variáveis para cálculo do preço total da reserva
    DECLARE vDias INTEGER DEFAULT 0;
    DECLARE vPrecoTotal DECIMAL(10,2);
    
    -- inicialização das variáveis
    SET vDias = DATEDIFF(vDataFim, vDataInicio);
    SET vPrecoTotal = vDias * vPrecoUnitario;
    
    INSERT INTO reservas VALUES (
		vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
	);
END//
DELIMITER ;
```

Chamando a procedure com argumentos:

```sql
CALL nova_reserva('10004', '1004', '8635', '2024-02-15', '2024-02-17', 180);
```

---

#### Tratamento de erros 

**Tratamento de erros** baseado em um código de erro gerado pelo SGBD.

- Para descobrir o código de erro, você pode tentar executar um comando para o erro que você quer tratar.

**Importante:** você deve **declarar todas as variáveis** que sua procedure for utilizar **antes** de usar `DECLARE EXIT HANDLER`. 

> Usar o `EXIT` diz para que o bloco de código seja encerrado caso o erro ocorra. Se quiser que o bloco de código continue, pode ser usado o `CONTINUE`.

```sql
DROP PROCEDURE IF EXISTS nova_reserva;
DELIMITER //
CREATE PROCEDURE nova_reserva(
	vReserva VARCHAR(10),
	vCliente VARCHAR(10),
	vHospedagem VARCHAR(10),
	vDataInicio DATE, 
	vDataFim DATE,
    vPrecoUnitario DECIMAL(10,2) -- valor da diária
)
BEGIN
	-- variáveis para cálculo do preço total da reserva
    DECLARE vDias INTEGER DEFAULT 0;
    DECLARE vPrecoTotal DECIMAL(10,2);
    
    -- variável para exibir mensagem de erro
    DECLARE vMensagem VARCHAR(100);
    
    -- tratamento de erro para chave estrangeira (erro 1452)
    DECLARE EXIT HANDLER FOR 1452
    BEGIN
		SET vMensagem = 'Problema de chave estrangeira';
        -- use o select para exibir a mensagem como resultado da consulta
        SELECT vMensagem;
    END;
    
    -- cálculo do valor total da reserva
    SET vDias = DATEDIFF(vDataFim, vDataInicio);
    SET vPrecoTotal = vDias * vPrecoUnitario;
    
    INSERT INTO reservas VALUES (
		vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
	);
    
    SET vMensagem = 'Aluguel incluído na base com sucesso!';
    SELECT vMensagem;
END//
DELIMITER ;
```

---

#### Condicional IF

No próximo exemplo, a inserção da reserva é feita passando o nome do cliente ao invés do ID. Procuramos o id do cliente pelo nome na tabela `clientes` e atribuímos este ID a uma variável. Também tratamos os casos de mais de um cliente possuir o mesmo nome, ou de o cliente não existir, usando um **condicional `IF`**.


```sql
DROP PROCEDURE IF EXISTS nova_reserva;
DELIMITER //
CREATE PROCEDURE nova_reserva(
	vReserva VARCHAR(10),
	vClienteNome VARCHAR(255),
	vHospedagem VARCHAR(10),
	vDataInicio DATE, 
	vDataFim DATE,
    vPrecoUnitario DECIMAL(10,2) -- valor da diária
)
BEGIN
	-- a partir do nome, vamos encontrar o id consultando a tabela clientes
    DECLARE vCliente VARCHAR(10);
    
    -- vamos verificar se há mais de um cliente com o mesmo nome
    DECLARE vQtdCliente INTEGER;
	
	-- variáveis para cálculo do preço total da reserva
    DECLARE vDias INTEGER DEFAULT 0;
    DECLARE vPrecoTotal DECIMAL(10,2);
    
    -- variável para exibir mensagem de erro
    DECLARE vMensagem VARCHAR(100);
    
    -- tratamento de erro para chave estrangeira (erro 1452)
    DECLARE EXIT HANDLER FOR 1452
    BEGIN
		SET vMensagem = 'Problema de chave estrangeira';
        -- use o select para exibir a mensagem como resultado da consulta
        SELECT vMensagem;
    END;
    
    -- verificando a quantidade de clientes com o nome informado
    -- podemos usar o resultado do SELECT como valor da variável
    SET vQtdCliente = (SELECT COUNT(*) FROM clientes WHERE nome = vClienteNome);
    
    IF vQtdCliente > 1 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois há mais de um cliente com o mesmo nome.';
        SELECT vMensagem;
	ELSEIF vQtdCliente = 0 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois o cliente não existe.';
        SELECT vMensagem;
	ELSE
		-- cálculo do valor total da reserva
		SET vDias = DATEDIFF(vDataFim, vDataInicio);
		SET vPrecoTotal = vDias * vPrecoUnitario;
		
		-- ao invés de um SET, podemos usar o SELECT INTO para atribuir 
        -- o valor a uma variável
		SELECT cliente_id INTO vCliente FROM clientes WHERE nome = vClienteNome; 
		
		INSERT INTO reservas VALUES (
			vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
		);
		
		SET vMensagem = 'Aluguel incluído na base com sucesso!';
		SELECT vMensagem;
    END IF;
END//
DELIMITER ;
```

---

#### Condicional CASE

Pedaço de código mostrando como seria o `IF` anterior utilizando **outra condicional, o `CASE`**:

```sql
CASE vQtdCliente
WHEN 0 THEN
    SET vMensagem = 'Não é possível fazer a inclusão, pois o cliente não existe.';
    SELECT vMensagem;

WHEN 1 THEN
    -- cálculo do valor total da reserva
    SET vDias = DATEDIFF(vDataFim, vDataInicio);
    SET vPrecoTotal = vDias * vPrecoUnitario;
    
    -- ao invés de um SET, podemos usar o SELECT INTO para atribuir 
    -- o valor a uma variável
    SELECT cliente_id INTO vCliente FROM clientes WHERE nome = vClienteNome; 
    
    INSERT INTO reservas VALUES (
        vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
    );
    
    SET vMensagem = 'Aluguel incluído na base com sucesso!';
    SELECT vMensagem;

ELSE
    SET vMensagem = 'Não é possível fazer a inclusão, pois há mais de um cliente com o mesmo nome.';
    SELECT vMensagem;
END CASE;

```

---

Outra versão do `CASE`, colocando a condição dentro de cada `WHEN`:

```sql
CASE 
WHEN vQtdCliente > 1 THEN
    SET vMensagem = 'Não é possível fazer a inclusão, pois há mais de um cliente com o mesmo nome.';
    SELECT vMensagem;
WHEN vQtdCliente = 0 THEN
    SET vMensagem = 'Não é possível fazer a inclusão, pois o cliente não existe.';
    SELECT vMensagem;
WHEN vQtdCliente = 1 THEN
    -- cálculo do valor total da reserva
    SET vDias = DATEDIFF(vDataFim, vDataInicio);
    SET vPrecoTotal = vDias * vPrecoUnitario;
    
    -- ao invés de um SET, podemos usar o SELECT INTO para atribuir 
    -- o valor a uma variável
    SELECT cliente_id INTO vCliente FROM clientes WHERE nome = vClienteNome; 
    
    INSERT INTO reservas VALUES (
        vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
    );
    
    SET vMensagem = 'Aluguel incluído na base com sucesso!';
    SELECT vMensagem;
END CASE;
```

---

#### Loop WHILE

Exemplo utilizando o **loop `WHILE`**. Nesta procedure, incluímos uma reserva de aluguel baseado na data inicial e número de dias, excluindo finais de semana (finais de semana são adicionados à reserva, mas não são cobrados, então não entram na contagem de número de dias). Também é mostrado o uso de `INTERVAL` e a função `dayofweek` para se trabalhar com datas.

```sql

DROP PROCEDURE IF EXISTS nova_reserva;
DELIMITER //
CREATE PROCEDURE nova_reserva(
	vReserva VARCHAR(10),
	vClienteNome VARCHAR(255),
	vHospedagem VARCHAR(10),
	vDataInicio DATE, 
	vDias INTEGER,
    vPrecoUnitario DECIMAL(10,2) -- valor da diária
)
BEGIN
	-- novas regras: 
    -- 1. calcular a data final com base na inicial e número de dias
    -- 2. finais de semana NÃO contam para o cálculo dos dias e são
    -- incluídos de graça no total da reserva
    DECLARE vDataFim DATE;
    DECLARE vContador Integer;
    DECLARE vDiaSemana Integer;

	-- a partir do nome, vamos encontrar o id consultando a tabela clientes
    DECLARE vCliente VARCHAR(10);
    
    -- vamos verificar se há mais de um cliente com o mesmo nome
    DECLARE vQtdCliente INTEGER;
	
	-- variável para cálculo do preço total da reserva
    DECLARE vPrecoTotal DECIMAL(10,2);
    
    -- variável para exibir mensagem de erro
    DECLARE vMensagem VARCHAR(100);
    
    -- tratamento de erro para chave estrangeira (erro 1452)
    DECLARE EXIT HANDLER FOR 1452
    BEGIN
		SET vMensagem = 'Problema de chave estrangeira';
        -- use o select para exibir a mensagem como resultado da consulta
        SELECT vMensagem;
    END;
    
    -- verificando se há mais de um cliente com o mesmo nome
    -- podemos usar o resultado do SELECT como valor da variável
    SET vQtdCliente = (SELECT COUNT(*) FROM clientes WHERE nome = vClienteNome);
    
    CASE 
	WHEN vQtdCliente > 1 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois há mais de um cliente com o mesmo nome.';
        SELECT vMensagem;
    WHEN vQtdCliente = 0 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois o cliente não existe.';
        SELECT vMensagem;
	WHEN vQtdCliente = 1 THEN
		-- cálculo dos dias, aplicando as novas regras
        SET vContador = 0;
        
        -- a data de início será incluída como sendo o
        -- primeiro dia das diárias. Isso é corrigido no loop
        SET vDataFim = vDataInicio - INTERVAL 1 DAY; 
        WHILE vContador < vDias DO
			-- independente do dia, adicionamos à data final
            SET vDataFim = vDataFim + INTERVAL 1 DAY;
			SET vDiaSemana = dayofweek(vDataFim);
             -- 1 é domingo e 7 é sábado
            IF (vDiaSemana <> 1 AND vDiaSemana <> 7) THEN
				SET vContador = vContador + 1;
            END IF;
        END WHILE;        
		
        -- cálculo do valor total da reserva
        SET vPrecoTotal = vDias * vPrecoUnitario;
		
		-- ao invés de um SET, podemos usar o SELECT INTO para atribuir 
        -- o valor a uma variável
		SELECT cliente_id INTO vCliente FROM clientes WHERE nome = vClienteNome; 
		
		INSERT INTO reservas VALUES (
			vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
		);
		
		SET vMensagem = 'Aluguel incluído na base com sucesso!';
		SELECT vMensagem;
    END CASE;
END//
DELIMITER ;
```

---

#### Passagem de parâmetro por referência

Neste exemplo, separamos a lógica do `WHILE` do exemplo anterior em uma nova procedure, chamada `calcula_data_fim`, que faz o cálculo e retorna a data final do aluguel. Esse retorno é dentro do próprio parâmetro `vDataFim` passado à procedure, indicando com `INOUT` que este **parâmetro é passado como referência**, ou seja, modificações de valor feito nesse parâmetro serão mantidos ao retornar à procedure que chamou `calcula_data_fim`.

```sql
-- procedure que calcula a data final e utiliza passagem de 
-- parâmetro por referência para retornar o resultado
DROP PROCEDURE IF EXISTS calcula_data_fim;
DELIMITER $$
CREATE PROCEDURE calcula_data_fim(
	vDataInicio DATE,
    INOUT vDataFim DATE, -- passagem de parâmetro por referência
    vDias INTEGER
)
BEGIN
	DECLARE vContador INTEGER;
    DECLARE vDiaSemana INTEGER;
    
	SET vContador = 0;
	SET vDataFim = vDataInicio - INTERVAL 1 DAY; 
    
	WHILE vContador < vDias	DO
		SET vDataFim = vDataFim + INTERVAL 1 DAY;
		SET vDiaSemana = dayofweek(vDataFim);
		IF (vDiaSemana <> 1 AND vDiaSemana <> 7) THEN
			SET vContador = vContador + 1;
		END IF;
	END WHILE;   
END$$
DELIMITER ;

DROP PROCEDURE IF EXISTS nova_reserva;
DELIMITER //
CREATE PROCEDURE nova_reserva(
	vReserva VARCHAR(10),
	vClienteNome VARCHAR(255),
	vHospedagem VARCHAR(10),
	vDataInicio DATE, 
	vDias INTEGER,
    vPrecoUnitario DECIMAL(10,2)
)
BEGIN
    DECLARE vCliente VARCHAR(10);
    DECLARE vQtdCliente INTEGER;
    DECLARE vDataFim DATE;
	DECLARE vPrecoTotal DECIMAL(10,2);
    DECLARE vMensagem VARCHAR(100);
    
    -- tratamento de erro para chave estrangeira (erro 1452)
    DECLARE EXIT HANDLER FOR 1452
    BEGIN
		SET vMensagem = 'Problema de chave estrangeira';
        SELECT vMensagem;
    END;
    
    -- verificando se há mais de um cliente com o mesmo nome
    SET vQtdCliente = (SELECT COUNT(*) FROM clientes WHERE nome = vClienteNome);
    
    CASE 
	WHEN vQtdCliente > 1 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois há mais de um cliente com o mesmo nome.';
        SELECT vMensagem;
    WHEN vQtdCliente = 0 THEN
		SET vMensagem = 'Não é possível fazer a inclusão, pois o cliente não existe.';
        SELECT vMensagem;
	WHEN vQtdCliente = 1 THEN
        
        -- executando outra procedure
        CALL calcula_data_fim(vDataInicio, vDataFim, vDias);
		
        -- cálculo do valor total da reserva
        SET vPrecoTotal = vDias * vPrecoUnitario;
		
		SELECT cliente_id INTO vCliente FROM clientes WHERE nome = vClienteNome; 		
		INSERT INTO reservas VALUES (
			vReserva, vCliente, vHospedagem, vDataInicio, vDataFim, vPrecoTotal
		);
		
		SET vMensagem = 'Aluguel incluído na base com sucesso!';
		SELECT vMensagem;
    END CASE;
END//
DELIMITER ;
```

---

#### Usando CURSOR e TEMPORARY TABLE

Neste exemplo, criamos duas novas procedures e fazemos uso de tabela temporária e cursor para fazer a inclusão de uma reserva para um grupo. Assumimos que o grupo é uma string com nomes separados por vírgula. No caso, será feita a uma reserva para cada nome, utilizando os mesmos dados de hospedagem, dias e valores. Considere que a procedure `nova_reserva` agora calcula automaticamente o ID da reserva. É apenas um exemplo didático para entender tabela temporária e cursor, pois na prática seria muito mais fácil percorrer a string e adicionar cada nome diretamente da lista recebida.

> Uma **tabela temporária** é outra forma de armazenarmos dados temporariamente no banco. Diferente de uma View ou CTE, a tabela temporária persiste **durante a sessão** do banco (enquanto a conexão ao banco estiver ativa) ou caso seja explicitamente excluída ("dropada"). Ela pode ser acessada por outras queries dentro da mesma sessão.

> Um **cursor** é um mecanismo que permite que tenhamos **acesso linha a linha dos resultados de uma consulta**. Com ele, o resultado da consulta é armazenado em memória e podemos acessar cada linha de maneira interativa utilizando o `FETCH`. Veja no exemplos os passos para a declaração, abertura, iteração e fechamento do cursor.

```sql
-- percorre uma lista de nomes e adiciona cada um
-- em uma tabela temporaria
DROP PROCEDURE IF EXISTS inclui_novo_grupo;
DELIMITER //
CREATE PROCEDURE inclui_novo_grupo(
	grupo VARCHAR(255) -- lista de nomes separados por vírgula    
)
BEGIN
	DECLARE nome VARCHAR(255);
    DECLARE proximo_nome VARCHAR(255);
    DECLARE pos INTEGER;
    
    SET proximo_nome = grupo;
    SET pos = INSTR(proximo_nome, ',');
    
    WHILE pos > 0 DO 
		-- adiciona nome à tabela temporária
        SET nome = LEFT(proximo_nome, pos - 1);
        INSERT INTO temp_nomes VALUES (nome);
        
        -- remove nome do grupo
        SET proximo_nome = SUBSTR(proximo_nome, pos + 1);
        SET pos = INSTR(proximo_nome, ',');
    END WHILE;
    
    IF TRIM(proximo_nome) <> '' THEN
		INSERT INTO temp_nomes VALUES (TRIM(proximo_nome));
    END IF;
END//
DELIMITER ;

-- adiciona um grupo em uma tabela temporária e usa
-- um cursor para percorrer esta tabela e adicionar
-- uma reserva para cada nome do grupo
DROP PROCEDURE IF EXISTS nova_reserva_grupo;
DELIMITER //
CREATE PROCEDURE nova_reserva_grupo(
	grupo VARCHAR(255),
	vHospedagem VARCHAR(10),
	vDataInicio DATE,
	vDias INTEGER,
    vPrecoUnitario DECIMAL(10,2)
)
BEGIN
	DECLARE vNome VARCHAR(255);
    
    -- sentinela para sair do loop quando não houver mais nomes
    DECLARE fimLoop INTEGER DEFAULT 0; 

    -- cursor que percorre linha a linha o resultado do select
    DECLARE cursorNome CURSOR FOR SELECT nome FROM temp_nomes;
    
    -- tratando erro quando chega ao fim do cursor, atualizando
    -- a sentinela e continuando a execução da procedure 
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET fimLoop = 1;
    
    -- criação da tabela temporária
	DROP TEMPORARY TABLE IF EXISTS temp_nomes;
    CREATE TEMPORARY TABLE temp_nomes(nome VARCHAR(255));
    CALL inclui_novo_grupo(grupo);
    
    -- percorrendo a lista de nomes e adicionando a reserva
    OPEN cursorNome; -- executa o select da declaração do cursor
    FETCH cursorNome INTO vNome;
    WHILE fimLoop = 0 DO 
		CALL nova_reserva(vNome, vHospedagem, vDataInicio, vDias, vPrecoUnitario);
		FETCH cursorNome INTO vNome;
    END WHILE;
    CLOSE cursorNome; -- libera a memória do cursor
    
    -- por precaução, vamos apagar a tabela temporária também ao final
    DROP TEMPORARY TABLE IF EXISTS temp_nomes;
END//
DELIMITER ;

-- exemplo de chamada
CALL nova_reserva_grupo('Gabriel Carvalho,Erick Oliveira,Catarina Correia,Lorena Jesus', '8635', '2023-06-03', 7, 30);

```

## Funções

Assim como Stored Procedures, funções encapsulam códigos e são armazenadas no banco, podendo ser reutilizadas em outras consultas SQL, inclusive podendo ser chamadas dentro de outras funções, procedures e triggers. 

Diferente de Stored Procedures, elas **obrigatoriamente retornam algum valor**. Além disso, elas costumam ser utilizadas em consultas SQL como **parte de uma expressão** (por exemplo, fazer alguma conversão e retornar o valor para uma variável), por isso são mais "simples". Por fim, as funções **não fazem modificações no banco de dados**, ou seja, elas não são utilizadas para operações de inserção, atualização, remoção, etc.

> Funções **não** trabalham com transações nem com tratamento de erros com `DECLARE ... HANDLER`. Você pode, no entanto, usar `IF` ou `CASE` para fazer alguma validação da entrada ou resultado esperado e retornar algum valor que simbolize um erro.

Exemplo simples que retorna somente uma frase:

```sql
DELIMITER $$
CREATE FUNCTION helloWorld()
RETURNS VARCHAR(50) -- tipo que será retornado
DETERMINISTIC -- resultado do retorno é sempre o mesmo
BEGIN
	RETURN 'Hello world!'; -- retorno de fato da função
END$$
DELIMITER ;
```

- quando a função retorna **sempre o mesmo resultado** para **os mesmos valores de entrada**, adicionamos o modificador **`DETERMINISTIC`** para auxiliar o MySQL na otimização. Dependendo das regras configuradas no SGBD, pode ser obrigatória a declaração explícita do modificador.

Para ver a saída da função, podemos fazer um `SELECT`:

```sql
SELECT helloWorld();
```

### Exemplos de funções

---

Neste exemplo, temos o uso de **parâmetros e variáveis locais**. A função recebe o id de um cliente e retorna o seu CPF, no formato (XXX.XXX.XXX-XX):

```sql
DROP FUNCTION IF EXISTS formata_cpf;
DELIMITER //
CREATE FUNCTION formata_cpf(id VARCHAR(255))
RETURNS VARCHAR(50) DETERMINISTIC
BEGIN
	DECLARE cpf_formatado VARCHAR(50);
    -- podemos usar SET ou fazer um SELECT..INTO    
    SET cpf_formatado = (
		SELECT CONCAT(SUBSTR(cpf, 1, 3), '.',
			SUBSTR(cpf, 4, 3), '.',
			SUBSTR(cpf, 7, 3), '-',
			SUBSTR(cpf, 10, 2)) 
		FROM clientes
        WHERE cliente_id = id
	);
	RETURN cpf_formatado;
END//
DELIMITER ;
```

Podemos chamar as funções criadas no meio de uma consulta SQL:

```sql
SELECT TRIM(nome), formata_cpf(cliente_id) FROM clientes LIMIT 10;
```

---

No próximo exemplo, mostramos como é possível utilizar o `SELECT INTO` para inicializar mais de uma variável. A função retorna uma string com o nome e o valor da diária de um aluguel, baseado no id do aluguel passado como argumento.

```sql
DROP FUNCTION IF EXISTS infoAluguel;
DELIMITER //
CREATE FUNCTION infoAluguel(id VARCHAR(255))
RETURNS VARCHAR(255) DETERMINISTIC
BEGIN
	DECLARE nome VARCHAR(255);
	DECLARE total_aluguel DECIMAL(10, 2);
	DECLARE total_dias INT;
    DECLARE valor_diaria DECIMAL(10, 2);
    
    -- podemos usar SELECT INTO para atribuir valores
    -- a mais de uma variável
    SELECT 
		c.nome, 
		a.preco_total, 
        datediff(a.data_fim, a.data_inicio)
	INTO nome, total_aluguel, total_dias
    FROM alugueis a
    JOIN clientes c ON c.cliente_id = a.cliente_id
    WHERE a.aluguel_id = id;
    
    SET valor_diaria = ROUND(total_aluguel / total_dias, 2);
    
    RETURN CONCAT('Cliente: ', nome, '. Valor Diária: R$', valor_diaria);
END//
DELIMITER ;
```