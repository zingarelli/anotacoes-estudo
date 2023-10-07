# Anotações sobre Git e GitHub

## Instrutores

[Vinicius Dias](https://www.linkedin.com/in/cviniciussdias/) 

Cursos: 

- ["Git e GitHub: Controle e compartilhe seu código"](https://www.alura.com.br/curso-online-git-github-controle-de-versao--amp)

- ["Git e GitHub: estratégias de ramificação, Conflitos e Pull Requests"](https://www.alura.com.br/curso-online-git-github-branching-conflitos-pull-requests)

*Com algumas anotações do curso de GIT do Bootcamp TQI | DIO*.

## Introdução

Git é uma ferramenta para controle de versões de arquivos. Auxilia não ter que criar diversos arquivos com conteúdos semelhantes e nomes tipo "versao_2", "ultima_edicao", "final", "final_revisado", etc. Também permite trabalho em grupo em um mesmo conjunto de arquivos, controlando quem fez as edições e possibilitando tratar de conflitos de edições em um mesmo arquivo.

As letras que formam GIT não têm um significado, mas é alvo de sarcasmo do próprio criador, Linus Torvalds.

Git precisa ser instalado na máquina (no Linux às vezes já vem instalado). 

- Link: https://git-scm.com/

Uma das **vantagens** do Git sobre outros sistemas de controle de versão é que com o Git você tem uma versão no seu repositório local, na sua máquina, podendo trabalhar em cima dessa versão e depois fazer o push para o repositório remoto. Possível **trabalhar "offline" e de modo distribuído** (cada pessoa tem o arquivo em seu repositório local, depois devem ser resolvidos os problemas de merge entre alterações diferentes em um mesmo arquivo).

## Definições

**VCS**: Version Control System.

O **Git** é a **ferramenta** para o versionamento; o **GitHub** é um **repositório remoto** de código

- o GitHub é muito utilizado pelos programadores e também pode servir de "vitrine"/portifólio para aqueles que procuram por um emprego na área, de modo a poderem mostrar aos recrutadores os projetos que desenvolvem ou já desenvolveram.

**Git Bash**: é um terminal de comando que pode ser instalado durante a instalação do Git. Lembra o Vim. Os comandos para navegar em pastas e tal são iguais aos do Linux. Também tem umas cores para tornar a interface mais amigável e facilitar a leitura/execução dos comandos. 

- Mas é possível também chamar o Git pelo CMD do Windows (acertando as variáveis no PATH). 

- Você também pode clicar com o botão direito na pasta em que está seu código e selecionar para abrir com o Git Bash; com isso, ele será aberto já no caminho para esta pasta.

## Versionamento e repositórios

O Git versiona **repositórios**, ou seja, é necessário **criar uma pasta** para os arquivos que você quer versionar. Essa pasta será a raiz do seu repositório. Subpastas são permitidas e entram no versionamento.

**HEAD**: pensando em uma linha do tempo de versionamento, o HEAD indica a posição atual nos arquivos versionados. Geralmente é o topo do versionamento, fazendo referência ao último commit feito (mais sobre commits [nesta Seção](#commit)).

**master**: antigamente era o nome padrão dado à branch principal do repositório (mais sobre branch [nesta Seção](#branch)). Hoje em dia é considerado um nome depreciativo (lembra master/slave, ou mestre/escravo) e tem-se utilizado mais a denominação de "main".

### `.gitignore`

É possível informar ao Git para **não versionar** alguns arquivos ou pastas; para isso, crie um arquivo `.gitignore` (com o ponto no começo, sem extensão) na raiz do repositório e nele insira o nome dos arquivos e pastas a serem ignorados 

- no caso de pastas, digite o caminho até a pasta (relativo à raiz do repositório) e insira a barra / no final do nome da pasta;

- informar o nome de uma pasta faz com que **todo conteúdo dentro dela (incluindo subpastas)** também seja ignorado.

### Repositório remoto

É o nome dado ao repositório que servirá como o **"servidor"** do projeto que foi versionado. É ele que indicará a versão corrente dos arquivos versionados e cuidará de todo o versionamento. É nele que faremos o [`push`](#git-push) de modificações e também é nele que as pessoas poderão clonar o código para trabalhar localmente. 

- Eu tenho o costume de chamá-lo de "repositório global", então às vezes utilizo esse termo nas anotações. 

- Pode ser tanto uma pasta na sua máquina, como também hospedado externamente (no GitHub, por exemplo).

No GitHub, estando logado no site, podemos criar um novo repositório remoto. Após criado, o próprio GitHub irá passar as instruções para adicioná-lo ao repositório local e fazer o `push` dos arquivos. É possível também criar tudo do zero dentro do próprio GitHub.

### Detalhes sobre o funcionamento do versionamento

Segue um resumo de algumas tecnologias e definições envolvidas no versionamento de arquivos. 

**SHA1 (Secure Hash Algorithm)**: o instrutor fala "Xá1". Algoritmo de **encriptação** que gera um conjunto caracteres de 40 dígitos, servindo como um identificador único para o arquivo. É o que permite ao Git **identificar cada mudança nos arquivos versionados** (cada mudança gera uma nova chave SHA1). Quando é feita uma alteração no conteúdo do arquivo, mas essa alteração faz com o que o arquivo tenha um conteúdo igual a alguma versão anterior, a chave SHA1 será a **mesma**, pois o código gerado para o conteúdo daquele arquivo resulta na mesma chave.

**Blob (bolha)**: objeto no Git. É um arquivo que contém o conteúdo de um arquivo + metadados que o GIT inclui sobre o arquivo. É o blob que irá passar pelo SHA1 para gerar a chave.

**Tree**: outro objeto no Git. A árvore que armazena um conjunto de blobs + SHA1 de cada blob + nome do arquivo contido em cada blob + outros metadados da árvore. Pode também armazenar outras árvores, de maneira recursiva. Pense como se fosse um explorador de arquivos: **as árvores são as pastas e os blobs, os arquivos**. A árvore também tem um SHA1 próprio. Qualquer alteração em um arquivo, irá alterar o SHA1 do blob e, consequentemente, irá alterar o SHA1 da árvore. 

**Commit**: também pode ser visto como um objeto no Git. Aponta para uma árvore, para um parent (o último commit antes dele), para um autor e para uma mensagem. Possui também um timestamp de quando o objeto foi criado. Novamente, o commit também tem um SHA1 próprio.

Essa estruturação em blobs, trees e commit é uma das formas que o Git **garante a segurança do versionamento**. Aquela versão é única, ou seja, qualquer alteração em algum conteúdo ou metadado irá gerar um novo SHA1, gerando incompatibilidade de SHA1 que impossibilita que haja uma manipulação maliciosa dos arquivos.

Ciclo de vida dos arquivos versionados: Untracked -> Unmodified -> Modified -> Staged

## Commit

É a etapa inicial para **salvar alterações ao repositório** (ou seja, enviar ao repositório os arquivos que foram modificados, criados e removidos). 

Ao digitar o comando para commit, a flag `-m` é usada para escrever uma mensagem descritiva curta do que foi feito na alteração/criação do arquivo (ou arquivos).

Cada commit recebe um HASH code, que serve como **identificador único** daquele commit, conforme mencionado em seção anterior. 

Você pode fazer vários commits para um mesmo arquivo, ou diferentes arquivos, antes de efetuar o `push` (que é quando efetivamente as modificações são salvas no repositório remoto).

Você decide quando fazer ou não um commit, no entanto, uma convenção é **nunca commitar um código que não funciona**, principalmente se estiver trabalhando em um projeto em equipe, pois seu commit pode quebrar o projeto ou o código em que outra pessoa está trabalhando. Uma recomendação é **commitar a cada alteração significativa do seu projeto**: nova feature, bug corrigido.

- quando você trabalhar em uma empresa, possivelmente haverá uma "etiqueta" a ser seguida pelo time para fazer commits e pushes de maneira padronizada.

## Branch 

É uma maneira de criar **vertentes/ramos de desenvolvimento** dentro de um repositório, de modo a não interferir no código principal (geralmente chamada de branch main). 

O uso de branches serve, por exemplo, para desenvolver uma nova feature do projeto ou dividir o trabalho de uma equipe.

O site https://git-school.github.io/visualizing-git/ é um bom exemplo para entender branches de maneira visual.

A branch pode ser incluída à main por meio do **merge**. Os comandos para isso são o [`merge`](#git-merge) e o [`rebase`](#git-rebase). A ação de merge pode gerar conflitos quando um mesmo arquivo está diferente entre diferentes branches. Nesse caso, o Git irá adicionar linhas nos arquivos que estão gerando conflito, indicando aonde esses conflitos acontecem, e **você é responsável por resolvê-los** (eliminando as linhas que você considera que não são mais necessárias para o atual estado de seu projeto, por exemplo). 

Resolvidos os conflitos, segue o processo de commit dessas mudanças e, aí então o merge/rebase é finalizado (no caso do rebase, quando eu testei, foi necessário executar o comando `git rebase --continue` para encerrar o rebase. Mais sobre comandos na [próxima Seção](#comandos-git)).

## Comandos git

### `git init`

```bash
git init
```

Inicia o versionamento em um diretório (cria um repositório). Necessário navegar até a pasta onde estão os códigos a serem versionados e então executar este comando.

- flag `--bare` indica que o repositório é "puro", ou seja, só irá conter arquivos e receber modificações. Pode ser utilizado quando criamos localmente o repositório remoto, que será o repositório acessado por todos para baixar arquivos e subir mudanças.

### `git config`

```bash
git config --local user.name "Seu nome aqui"
git config --local user.email "seu@email.aqui"
```

Os dois comandos acima servem para informar o nome e e-mail de quem está trabalhando naquele repositório local. É útil quando se trabalha em equipe, assim é possível identificar os autores das mudanças. 

- O comando `--local` adiciona essas informações ao repositório **corrente**; caso queira que seja uma informação para **todos** os seus repositórios locais, use o `--global`. Essas duas configs são necessárias, caso contrário vai dar problema na hora do commit.

`git config --list` : mostra a lista de configurações salvas. 

`git config --local --unset <nome_da_configuracao>`: apaga da lista de configuração local a propriedade informada.

### `git status`

```bash
git status
```

Dá detalhes sobre os arquivos em seu repositório. Por exemplo, o comando informa se há arquivos que foram criados ou modificados e que ainda não foram commitados ou enviados ao repositório central. Também informa em qual branch você está trabalhando.

### `git add`

```bash
git add nome_do_arquivo
```

Informa ao git que o arquivo deverá ser adicionado ao próximo commit a ser executado. O arquivo vai para **"stage"** (é o estado em que o arquivo está no aguardo para ser "commitado").

- caso sejam vários arquivos, pode ser usando o ponto (`.`): 

   - `git add .`

   - **Atenção**: nesse caso, serão adicionados **todos** os arquivos que foram alterados/criados/removidos no repositório local. 
   
- Para selecionar os arquivos manualmente, você pode inseri-los separados por espaço: 

   - `git add arq_1 arq_2 arq_3`

   - A tecla `tab` auxilia no autocomplete dos nomes.

### `git commit`

```bash
git commit -m "descricao_curta_de_seu_commit"
```

Commita suas alterações que estão em "stage", para serem salvas no repositório remoto. As alterações são de fato salvas no repositório remoto por meio do [`git push`](#git-push).

### `git reset`

```bash
git reset --soft HEAD~1
```

**Desfaz** seu último commit, mas **mantém** as alterações que haviam sido feitas. Útil quando, por exemplo, você commitou, mas viu que faltou uma pequena alteração. Ao invés de aplicar a alteração e criar um novo commit, você pode usar esse comando para desfazer o commit, aplicar a alteração, e então commitar. Seria como um "`Ctrl+Z`".

### `git log`

```bash
git log
```

Existe o histórico de commits feitos, em ordem decrescente de data, trazendo nome e email de quem commitou, o código Hash e a mensagem de cada commit.

`git log --oneline`: histórico resumido; mostra somente parte do hash e mensagem de cada commit. Facilita quando você só quer ler as mensagens de cada commit;

`git log -p`: histórico detalhado: mostra também as alterações feitas por cada commit.

### `git remote`

```bash
git remote add nome_do_repositorio_remoto endereco_para_o_repositorio_remoto
```

Cria uma ligação entre o repositório local e um repositório remoto. O endereço pode ser tanto uma pasta da própria máquina (precisa ser preparada com o `git init --bare`), quanto uma URL (para um repositório online criado no GitHub, por exemplo). O `nome_do_repositorio_remoto` é o nome que você dará para esse repositório remoto, e que será necessário na hora do `push`/`pull`. A convenção é usar o nome "origin".

`git remote -v`: lista os repositórios remotos.

### `git push` 

```bash
git push nome_do_repositorio_remoto nome_da_branch
```

Envia as mudanças commitadas para o repositório remoto.

`git push -u`: memoriza o repositório e a branch desse `push` e deixa você usar o comando `git push` sem precisar informar repositório e branch toda vez. **Use com cautela**: caso você comece a trabalhar com outras branches e se esqueça de alterar na hora de fazer o `push`, poderá estar fazendo o push na branch ou repositório errados, então o recomendado é fazer o push completo, com nome do repositório remoto e nome da branch.

### `git pull`

```bash
git pull [nome_do_repositorio_remoto]
```

Comando oposto ao `push`, baixa os arquivos do repositório remoto e tenta adicioná-los ao seu repositório local (caso encontre conflito entre o código do repositório remoto e o local, será necessário verificar e resolver). Ele pode ser visto como um combo de `git fetch` + `git merge`.

**Atenção**: minhas anotações estavam anteriormente **ERRADAS**. Eu havia considerado que era obrigatório enviar no comando do git pull dois argumentos: `[nome_do_repositorio_remoto]` e `[nome_da_branch]`. Eles são opcionais. Mais detalhes sobre eles:

- O argumento `[nome_do_repositorio_remoto]` é  usado caso você tenha mais de um repositório remoto e queira indicar de onde as mudanças devem ser baixadas. Quando não passado, o pull será feito no repositório remoto padrão (geralmente é o `origin` do GitHub);

- Há um segundo argumento `[nome_da_branch]` que você pode passar para indicar que você quer atualizar sua branch local com o conteúdo da branch remota indicada. **Tome cuidado para não atualizar a branch local errada** ao usar esse argumento. Quando não passado, o pull será feito da branch local em que você está. 

O que acontece com os arquivos que você alterou e ainda não commitou após um `pull`? Perde os dados? Os arquivos alterados não são baixados?

Resposta: se os arquivos estiverem alterados no repositório remoto e também foram modificados no repositório local (mas ainda não commitados), ao fazer o `pull` o git irá emitir um erro de que o arquivo será sobrescrito pelo merge. A solução que o git informa é commitar o arquivo que está modificado no repositório local ou usar o `git stash` antes de fazer o pull (ver mais abaixo sobre o [`git stash`](#git-stash)).

- Se optar por commitar, ao fazer o `git pull` o próprio git irá tentar fazer o merge das modificações que estão no repositório remoto e no local;

- Se optar pelo `stash`, ao fazer o `git pull` o arquivo será atualizado somente com as modificações do repositório remoto e você fica responsável por trazer as modificações que estão no stash e resolver os conflitos de merge.

### `git stash`

```bash
git stash
```

Salva os arquivos em um **local temporário** da sua máquina, por exemplo, para alterações que eu ainda não terminei mas que não vou commitar por enquanto. Desse modo, essas alterações não são perdidas. Isso não afeta branches, ou seja, você pode salvar arquivos no stash e fazer modificações/navegações entre branches. Atenção: ao fazer isso, seu código será revertido para o do último commit.

`git stash list`: lista todas as modificações que foram salvas no stash (caso você tenha usado o comando git stash mais de uma vez)

`git stash apply numero_da_lista`: traz de volta as modificações que foram salvas e faz um merge com as outras alterações que foram feitas desde então. O `numero_da_lista` é o numero que vem entre chaves na lista informada pelo `git stash list` e que você quer recuperar (`stash@{0}:`).

`git stash drop numero_da_lista`: remove o que foi salvo na stash

`git stash pop`: traz as modificações do último `git stash` feito e o remove da lista do stash (é uma combinação de `git stash apply 0` e `git stash drop`); é como um pop de pilha.

### `git diff`

```bash
git diff
```

Mostra as **diferenças** no código atual e o código do último commit, caso você tenha modificações no arquivo. 

E se for mais de um arquivo? Ele vai mostrar as diferenças arquivo por arquivo; a linha de comando dá indicações de quando um arquivo termina e um novo será mostrado.

`git diff hash_commmit_X..hash_commmit_Y`: mostra as diferenças desde o `hash_commmit_X` até o `hash_commmit_Y`

- o mesmo pode ser aplicado para branches.

### `git clone`

```bash
git clone caminho_do_repositorio_remoto nome_para_a_pasta_local
```

Clona um projeto, ou seja, baixa os arquivos de um repositório remoto para a máquina e salva em uma pasta com o `nome_para_a_pasta_local`. 

- se quiser salvar o repositório na pasta corrente (e ela estiver **vazia**), basta usar o ponto (`.`) no lugar.
- se você não passar o `nome_para_a_pasta_local`, será criada uma pasta com o nome do repositório clonado. Por exemplo, caso o `caminho_do_repositorio_remoto` seja `https://github.com/zingarelli/anotacoes-estudo.git`, será criada a pasta `anotacoes-estudo` e o conteúdo do repositório será salvo dentro dela.

Com o projeto clonado, é possível alterar os arquivos, commitar e fazer push para o repositório remoto. 

O caminho pode ser a URL de um repositório online ou o caminho para uma pasta na máquina local.

O  nome do repositório remoto será "`origin`" para o projeto clonado, mas pode ser renomeado com `git remote rename nome_atual novo_nome`.

### `git branch`

```bash
git branch nome_da_nova_branch
```

Cria nova branch, porém, **não** a ativa como principal; você continuará na branch atual.

`git branch -M novo_nome_da_branch`: comando para renomear a branch atual. 

`git checkout nome_da_branch` : altera a branch que será a ativa (move seu HEAD para esta nova branch), carregando a versão mais atual dos arquivos que estão nessa branch.

`git checkout -b nome_da_nova_branch` : cria nova branch **e** já altera para ser a ativa.

### `git checkout`

```bash
git checkout HASH_do_commit
```

Navega para um commit específico do log. Serão carregados os arquivos desse commit, **PORÉM**, qualquer **alteração** feita no código, inclusive commits, **NÃO** entrará no histórico. Para que essas alterações entrem no histórico do projeto, você deve **criar uma branch** (pode usar o `git checkout -b nome_da_nova_branch` após ter navegado para o commit desejado).

### git merge

```bash
git merge outra_branch
```

Faz um merge (combinação) da branch atual com o commit mais atual da `outra_branch`, para que as **duas ramificações se unam em uma só**. 

Será aberto um texto no VIM (ou no editor de texto que você configurou) para incluir uma mensagem de commit de merge (no VIM, se não quiser escrever nada, digite `:x` e pressione `ENTER`; caso esteja no modo de INSERT, pressione `ESC` antes de digitar o `:x`). 

No processo de merge, os commits anteriores da `outra_branch` **não entram** para a branch atual (mas eles ainda estarão no histórico da `outra_branch`, caso você faça um checkout nela).

### `git rebase`

```bash
git rebase outra_branch
```

Traz todos os commits da `outra_branch` para a branch atual e atualiza as modificações. Os commits da `outra_branch` serão inseridos na "linha do tempo" dos commits a partir do momento em que houve a criação da `outra_branch`, e na frente destes seguirão os commits que foram feitos na branch atual. O último commit da branch atual será considerado como o ativo. Não gera um commit de merge.

- lembre-se do site para uma experiência visual de merge, rebase, etc: https://git-school.github.io/visualizing-git/

### `git restore`

```bash
git restore nome_do_arquivo
```

Comando antigo (versões mais velhas do Git): `git checkout -nome_do_arquivo`.

Funciona como um `Ctrl+Z` para um arquivo, quando eu quero **descartar** as alterações feitas em um arquivo que está com o **status "modified"**. Irá restaurar o arquivo corrente ao meu repositório local. 

**Atenção**: você **perde** suas modificações ainda não commitadas no arquivo ou ainda não adicionadas ao stage. 

**Atenção 2**: se você já tiver adicionado o arquivo ao stage (por meio do `git add nome_do_arquivo`), esse comando de checkout não irá funcionar; primeiro você precisará fazer o comando abaixo.

```bash
git restore --staged nome_do_arquivo
```

Serve para **remover** um arquivo que foi adicionado para ser commitado (ou seja, desfaz a ação de um `git add nome_do_arquivo`). Seu versionador voltará a olhar para o HEAD como sendo o estado atual e o arquivo voltará a aparecer como modified ao dar um `git status`.

Comando antigo (versões mais velhas do Git): `git reset HEAD nome_do_arquivo`.

### `git revert`

```bash
git revert HASH_do_commit
```

**Desfaz** o commit cujo hash você informou no comando. Irá desfazer esse commit e **gerar um novo commit para informar essa alteração**.

- não precisa passar o código hash completo. Pode passar somente os 7 caracteres iniciais

### `git tag`

```bash
git tag -a nome_da_tag
```

Cria uma tag, que seria como uma "versão" finalizada do projeto, um release do projeto, que não será mais modificado a partir daquele ponto. Novas modificações irão para a próxima tag (próxima release). 

Pense como se você estivesse entregando a versão 1.0 do seu projeto, por exemplo (`git tag -a v1.0`).

- flag opcional `-m "mensagem"`: adiciona uma mensagem.

- antes, o GitHub já adicionava a tag como release automaticamente, mas agora cabe ao próprio desenvolvedor navegar até a tag e gerar uma release

`git tag`: mostra todas as tags criadas.

`git push nome_do_repositorio_remoto nome_da_tag`: comando necessário para enviar a tag para o repositório remoto.

## Funcionalidades do GitHub

### Issues

É uma seção presente nos repositórios do GitHub, em que as pessoas podem enviar um texto à equipe do repositório, reportando algum problema que encontraram no código/projeto.

A seção de issues também pode ser utilizada para tirar dúvidas e entrar em contato com os desenvolvedores. É possível utilizá-la para o controle de melhorias/erros apontados pelos usuários, podendo fazer buscas com filtros e categorizar as issues com tags.

### Fork 

Fork é outra funcionalidade do GitHub, que permite que você **copie** para seu repositório remoto um **projeto de outro usuário do GitHub**. Toda a estrutura e arquivos do projeto serão copiados para seu repositório, junto com uma informação de que seu projeto é um fork desse outro projeto.

Você poderá fazer modificações no projeto, inserir novos arquivos, novas branches, commmits, etc. O `push` dessas modificações **ficarão somente em seu projeto**, não serão enviadas para o projeto original do qual o fork foi feito.

Atualizações no projeto original **não** serão pegas pelo `git pull`. Caso queira atualizar o seu repositório com o original, é necessário criar um remote para o repositório original, baixar as atualizações com o comando `git fetch` e depois fazer um merge ou rebase da branch principal desse remote com sua branch

**Diferença para o git clone**: o clone faz o download do repositório para a sua máquina, mas a referência (o remote) continua sendo o repositório original -> ao fazer o `push`, será tentado subir as mudanças para o repositório original (irá falhar caso você não tenha permissão para fazer push nesse repositório). 

### Pull Request (PR)

Caso deseje enviar as modificações feitas no seu projeto "forkado" para projeto original, você deve utilizar a funcionalidade de **Pull Request (PR)** do GitHub.

É possível enviar um título e um texto com as modificações feitas. O GitHub irá mostrar o diff dos arquivos modificados e também já avisa se consegue fazer o merge automático.

O responsável pelo repositório original poderá analisar os PR e decidir se irá fazer ou não o merge da solicitação. Tudo isso é feito dentro do GitHub.

## União de commits

É possível juntar commits utilizando o `git rebase`. Isso só funciona com commits que ainda **não foram enviados ao repositório remoto** via `git push`. Até funciona você fazer a união após um push, mas quando você fizer o push dessa união, o git irá reportar erro de que seu repositório local está atrasado com relação ao remoto e irá sugerir um merge (ou seja, não adiantará de nada).

Exemplos:

```bash
git rebase -i HEAD~3
```

   - `-i` de interativo
   - HEAD~3 informa para selecionar os 3 últimos commits a partir do HEAD

```bash
git rebase -i HASH_do_commit
```
   
   - irá selecionar todos os commits **posteriores** a HASH_do_commit (ele não estará incluso)

Em ambos os comandos, será aberto o editor de texto com a lista dos commits, para você analisar e marcar como eles ficarão. Digitar `pick` (ou `p`) mantém o commit; digitar `squash` (ou `s`) é o que une o commit ao anterior na lista; há outras opções que o editor fornece.

Após essa interação, será gerado um novo commit para informar da ação que foi feita. É possível editar a mensagem utilizando o texto dos outros commits relacionados.

## Cherry-pick

É o nome que se dá à ação de selecionar um commit específico e trazer para o HEAD da branch atual. Imagine, por exemplo, que há uma branch em que uma nova funcionalidade está sendo desenvolvida, mas ainda não está pronta. No entanto, uma modificação feita nessa branch está sendo necessária na branch principal (correção de um bug, por exemplo). É possível, neste caso, usar o `cherry-pick` para trazer para a branch principal somente o commit que possui essa modificação. Será feito um merge (ou deverão ser resolvidos os conflitos) e um novo commit será gerado.

Quando a nova funcionalidade na outra branch estiver finalizada e for feito o `git rebase`, não irá haver conflito com o commit que foi trazido pelo cherry-pick: o Git irá entender e organizar o histórico da maneira correta.

```bash
git cherry-pick HASH_do_commit
```

## Bisect

É uma maneira de navegar pelos commits e ver o que foi alterado, em busca, por exemplo, do momento em que alguma alteração indesejada foi feita ou um bug foi introduzido.
Necessário indicar o commit onde algo indesejado aconteceu (bad) e o commit em que possivelmente estava tudo ok (good). 

O git irá percorrer os commits intermediários, possibilitando olhar as alterações que foram feitas e indicar ao git se o commit é good ou bad. Por meio da navegação, é possível ver o HASH do commit em que estava tudo OK e reverter o código a esse commit ou verificar as alterações indesejadas e removê-las do código atual.

É uma alternativa mais interativa para não precisar ficar fazendo manualmente `git checkout` ou `diff`,

## Blame

Apesar do nome (culpar), o comando informa o último usuário que alterou cada linha do código em um arquivo. Informa também o hash do commit e data da modificação.

```bash
git blame nome_do_arquivo
```

Acredito que o VS Code tem uma extensão que faz a mesma coisa.

## Dicas e convenções

Convenções sobre main e outras branches (mais para o caso de estar trabalhando em uma empresa): 

- **não** desenvolver na main. Deixar nela o código que está pronto para ir em produção. 

- criar branches para desenvolver seu código. Depois de finalizado e validado, aí sim fazer o merge/rebase com a main para que vá para produção.

Sugestão: criar uma branch de development e, dentro dela, criar branches para cada feature nova. À medida que essas features vão sendo finalizadas e validadas, faz o merge para a branch de development. Ao finalizar todas as features especificadas e tendo feita todas as validações, cria-se uma nova branch de release e faz o merge com a branch de development. Nesta branch de release é possível fazer novos testes e correções e, estando tudo validado, aí então faz-se o checkout para a main e merge com a release (possivelmente também gerando uma tag de nova release).

Dica: bugs que precisam ser corrigidos com urgência poderiam ficar dentro de uma branch de hotfix. Após corrigidos, é feito o merge com a main e também com development, para manter ambos atualizados com a correção do bug.

 - Essa estratégia de organização é chamado de Git Flow
 - Imagem de exemplo
 
 ![Imagem de exemplo de um git flow](https://cdn-images-1.medium.com/max/1200/1*8-zDz1s5Atux_yNW_mXmfg@2x.png)

## Hooks

Ações a serem executadas quando certos eventos do git acontecem. Por exemplo: executar um script de testes no código antes de todo commit (seria um "pre-commit"); executar um script de deploy após um push (seria um "post-receive").

Scripts de exemplo para alguns eventos se encontram na pasta hook, dentro da pasta .git, e o final do nome de cada arquivo é .sample. Você pode criar um arquivo dentro dessa pasta com o mesmo nome menos o sample para executar o hook.

Lista de hooks e mais infos: https://githooks.com.

O GitHub possui algo parecido, chamado "GitHub Actions".

Sugestões de softwares com GUI para facilitar a visualização/utilização do Git: Git Cola, GitHub Desktop, GitKraken (este último tem até a implementação da estratégia do Git Flow).

O próprio VS Code possui algumas funcionalidades para versionar com o git e linkar com o GitHub.
