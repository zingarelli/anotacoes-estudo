# Scrum

👉 [Online Scrum Guide](https://scrumguides.org/scrum-guide.html)

Conteúdo baseado no curso ["Scrum: agilidade em seu projeto"](https://cursos.alura.com.br/course/scrum-agilidade-seu-projeto), ministrado pela [Maíra Nicoletti](https://www.linkedin.com/in/mairanicoletti/) na Alura.

Imagem do framework Scrum, retirado do [Scrum.org](https://www.scrum.org/learning-series/sprint/):

![imagem mostrando os eventos e alguns artefatos dentro de uma Sprint](https://scrumorg-website-prod.s3.amazonaws.com/drupal/inline-images/2023-08/the-sprint-2.0.png)

## Introdução

Criadores do Scrum: **Ikujiro Nonaka** e **Hirotaka Takeuchi**. Inicialmente para gestão de produtos em fábrica, ou seja, não estava voltado para desenvolvimento de software.

O Scrum é adaptado por Jeff Sutherland e Ken Schwaber para o desenvolvimento de software. Acabam se tornando os "pais do Scrum".

Scrum é um **framework ágil**, estruturado em **etapas** e com **papéis** bem definidos para a equipe, voltado para o gerenciamento de projetos de forma eficiente e eficaz.

## Definições

- **Papéis**: a função, a ocupação de cada membro do time. Três papéis são fundamentais: Scrum Master (SM), Product Owner (PO) e Development Team (Devs).

- **Artefatos**: são "registros tangíveis" do trabalho desenvolvido, ou seja, aquilo que foi produzido pelo trabalho em equipe, como documentos, códigos, protótipos ou até mesmo o produto final. Três artefatos são essenciais: Product Backlog, Sprint Backlog, Increment. 

- **Eventos**: encontros fixos entre o time. São cinco eventos, sendo a Sprint um evento em si mesma (e não um encontro), dentro do qual ocorrem os outros quatro eventos (estes sim são encontros): Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective.

## Papéis

- **Product Owner**: é a pessoa que conhece tudo sobre o produto. É responsável por **entender os requisitos** solicitados pelo cliente e **materializá-lo em um produto**. É o elo de ligação (ou o porta-voz) do cliente com a empresa, conciliando os interesses do cliente (o que ele quer) com os interesse da empresa (os recursos disponíveis para o projeto). 
  
  - define os requisitos
  - gerencia o cronograma de entregas
  - gerencia o **Product Backlog**
  - tem autoridade para cancelar um Sprint

- **Scrum Master**: é como o maestro de uma orquestra. Entende como o Scrum funciona e é responsável por garantir que o framework está sendo **aplicado**, **orientando** e **capacitando** os outros membros se necessário. Estabelece a **comunicação** e **colaboração** entre o time e procura **remover quaisquer barreiras** que atrapalhem o progresso. Também **organiza os eventos** Scrum.

  - **não** precisa entender sobre programação ou tecnologias - seu entendimento principal é sobre o Scrum
  - **não** toma decisões de projeto: esse é o papel do PO

- **Development Team**: desenvolvem as tarefas que culminam no produto. Tem autonomia para desenvolver suas atividades, mas também deve ter um perfil colaborativo, para se comunicar com outros membros. É guiado pelo PO para a priorização das tarefas.

  - em conjunto, elaboram o **Sprint Backlog**
  - não são somente programadores: QAs, designers, DBAs, etc. também fazem parte do time

## Artefatos

- **Product Backlog**: lista com todos os detalhes do produto - requisitos, funcionalidades, necessidades, desafios, etc. Aquilo que o cliente deseja e aquilo que a empresa está comprometida a entregar. Está associado ao **Product Goal**, isto é, o objetivo/meta do produto, aquilo que o projeto quer atingir.

- **Sprint Backlog**: composta por um objetivo a ser atingido (**Sprint Goal**) e as tarefas a serem desenvolvidas durante a Sprint para atingir este objetivo.

- **Increment**: é uma melhoria, uma nova funcionalidade adicionada ao produto. Ocorre durante uma Sprint. O time elabora uma Definição de Feito (**Definition of Done**), que estabelece os critérios para considerar que um Incremento está pronto para ser entregue e avaliado.

## Eventos

- **Sprint**: evento com uma duração fixa (entre 2 e 4 semanas). Os outros quatro eventos ocorrem dentro da Sprint. Ao final da Sprint, uma entrega é feita (uma melhoria é adicionada ao produto) e avaliada. A essas entregas é dado o nome de **Incremento**, e mais de uma pode ocorrer dentro de uma mesma Sprint. Sprints vão ocorrendo em sequência até chegar ao produto final .

- **Sprint Planning**: 1º evento dentro da Sprint. São selecionados itens do Product Backlog e é definida a Sprint Goal. São listadas as tarefas a serem desenvolvidas para atingir o objetivo. Para cada tarefa será definido um responsável por ela e estimado um tempo de entrega. O resultado do evento é o **Sprint Backlog**.

- **Daily Scrum**: reunião diária de cerca de 15 minutos, usada para alinhar o andamento das tarefas realizadas por cada um da equipe. Momento para troca de ideias e remoção de barreiras.

- **Sprint Review**: apresentação do Incremento, aquilo que foi entregue, para os stakeholders (o cliente, o usuário, as partes interessadas no projeto), para que seja **avaliado e dado um feeback do produto**. Também apresenta o que havia sido esperado para a Sprint e o que foi de fato entregue. Todos alinham as expectativas para as próximas Sprints e o Product Backlog é atualizado.

- **Sprint Retrospective**: diferente da Review, aqui é **avaliado o processo de trabalho** do time durante a Sprint que terminou, identificando problemas e melhorias para as próximas Sprint (um plano de ação). A Retrospective ocorre após a Review.

## Epic, story e task

> Conteúdo adaptado da interação com a Luri, a IA Generativa da Alura.

- **Epic**: É como um grande objetivo, uma meta a ser alcançada no desenvolvimento do projeto. Para sua realização, ela pode ser quebrada em Stories.

- **Story (ou user story)**: São as tarefas menores que precisam ser feitas para atingir o objetivo de uma Epic. Podem resumir alguma funcionalidade a ser implementada. 

- **Task**: é um nível adicional de granularidade, em que você quebra uma Story em várias tarefas ainda menores. É uma forma de auxiliar na divisão das atividades entre o time e no planejamento de tempo para conclusão. 

O Product Backlog lista todas as epics e stories que você precisa fazer para o seu produto. Durante uma Sprint Planning, o time seleciona as Stories do Product Backlog que serão trabalhadas naquela Sprint, ou seja, essas Stories são incluídas no Sprint Backlog. É possível também quebrar as Stories em Tasks e adicioná-las ao Sprint Backlog. Durante a Sprint, o time se organiza para finalizar essas Tasks e assim completar as Stories planejadas para aquela Sprint. A cada Sprint, novas Stories vão sendo finalizadas e, por consequência, as Epics vão sendo cumpridas.