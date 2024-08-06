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
SELECT cliente_id, avg(preco_total) AS ticket_medio
FROM alugueis
GROUP BY cliente_id;
```

- Consulta que está agrupando os clientes pelo ID e calculando uma média que eles gastam dentre os vários alugueis que já pagaram. 

- `avg()` é uma **função de agregação** que calcula a média.

- `AS` é utilizado quando queremos dar um "apelido" (*alias*) a uma coluna/resultado. Esse apelido aparece somente no resultado da consulta, ou seja, a coluna na tabela não é renomeada.

- `GROUP BY` pode ser utilizado em conjunto com algumas funções de agregação, como a `avg()`. Precisamos explicar qual tipo de agrupamento dos dados será feito para que a função seja corretamente aplicada.

---

```sql
SELECT cliente_id, avg(datediff(data_fim, data_inicio)) AS media_dias_estadia 
FROM alugueis
GROUP BY cliente_id
ORDER BY media_dias_estadia DESC
```

- Função que verifica a média de dias de cada cliente quando fica hospedado, ordenando a média de maneira decrescente.

- `datediff()` é uma função para subtração entre valores do tipo `DATE`.

- `ORDER BY` é a cláusula para ordenação do resultado baseado em alguma coluna. `DESC` indica ordenação decrescente. Por padrão, a ordem é ascendente (`ASC`).

---

```sql
SELECT p.nome AS nome_proprietario, count(*) AS total_hospedagens_ativas
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

- `count()` é uma função que conta as tuplas. No caso, estamos contando todas as ocorrências (`*`) para cada proprietário (a consulta está agrupada por `nome`).

- `LIMIT` é a cláusula que usamos para limitar a quantidade de resultados vindos da consulta. Como queremos o "top 10", limitamos a consulta aos 10 primeiros resultados (que foram previamente ordenados de forma decrescente pelo total de hospedagens ativas).

---

```sql
SELECT 
	year(data_inicio) AS ano,
    month(data_inicio) AS mes,
    count(*) AS total_reservas
FROM alugueis
GROUP BY ano, mes
ORDER BY total_reservas DESC;
```

- Consulta do total de reservas (aluguéis) feitas por ano e mês, de modo a verificar os meses de maior e menor demanda em cada ano. Uma consulta assim pode indicar a sazonalidade.

- As funções `year()` e `month()` extraem o ano e o mês de uma dada data.

- A cláusula `GROUP BY` aceita **agrupamento composto**, fazendo o agrupamento na ordem em que as colunas foram indicadas. No caso do exemplo, o resultado é agrupado por ano e depois por mês, e o `count()` faz a soma de cada um desses agrupamentos compostos.

## Tipos de JOIN

- `INNER JOIN`: Retorna linhas quando há pelo menos uma correspondência em ambas as tabelas. No MySQL, você pode usar somente `JOIN` e será efetuado um `INNER JOIN`;

- `LEFT JOIN`: Retorna todas as linhas da tabela à esquerda (a primeira tabela da consulta) e as linhas correspondentes da tabela à direita (a segunda tabela, indicada após o comando de join). Inclui também os resultados em que **não houver correspondência** com a tabela à direita, marcados como NULL no lado direito;

- `RIGHT JOIN`: É o inverso do `LEFT JOIN`, ou seja, retorna todas as linhas da tabela à direita e as correspondentes da tabela à esquerda, marcando NULL quando não há correspondência do lado esquerdo;

- `FULL JOIN`: Combina `LEFT JOIN` e `RIGHT JOIN`, retornando todas as linhas em que houve uma correspondência em uma das tabelas, marcando NULL para as colunas da tabela quando não há correspondência.

    - O **MySQL não tem suporte nativo ao `FULL JOIN`**. Nesse caso, você pode combinar `LEFT JOIN` e `RIGHT JOIN` por meio da cláusula `UNION`.