# Anotações sobre Git e GitHub

Anotações baseadas em cursos da Alura e também do Bootcamp TQI | DIO. 

📖 Posteriormente atualizadas pelo livro: **Pro Git (Scott Chacon & Ben Straub)**, [disponível gratuitamente](https://git-scm.com/book/en/v2) (em inglês).

**Instrutores e cursos Alura:**

🎓 [Vinicius Dias](https://www.linkedin.com/in/cviniciussdias/) 

🎓 [Gabrielle Ribeiro Gomes](https://www.linkedin.com/in/gabrielleribeiro/)

🎓 [Rodrigo da Silva Ferreira Caneppele](https://www.linkedin.com/in/rcaneppele)

🖥️ ["Git e GitHub: Controle e compartilhe seu código"](https://www.alura.com.br/curso-online-git-github-controle-de-versao--amp)

🖥️ ["Git e GitHub: estratégias de ramificação, Conflitos e Pull Requests"](https://www.alura.com.br/curso-online-git-github-branching-conflitos-pull-requests)

🖥️ [Git e GitHub: compartilhando e colaborando em projetos](https://cursos.alura.com.br/course/git-github-compartilhando-colaborando-projetos)

🖥️ [Git e GitHub: dominando controle de versão de código](https://cursos.alura.com.br/course/git-github-dominando-controle-versao-codigo)

## Introdução

Git é uma ferramenta para **controle de versões de arquivos**. Auxilia não ter que criar diversos arquivos com conteúdos semelhantes e nomes tipo "versao_2", "ultima_edicao", "final", "final_revisado", etc. Também permite trabalho em grupo em um mesmo conjunto de arquivos, controlando quem fez as edições e possibilitando tratar de conflitos de edições em um mesmo arquivo.

Git foi criado em 2005 pela comunidade de desenvolvimento do Linux, para terem sua própria ferramenta de controle de versões distribuída.

As letras que formam GIT não têm um significado, mas é alvo de sarcasmo do próprio criador, Linus Torvalds.

Git precisa ser instalado na máquina (no Linux às vezes já vem instalado). 

> Link: https://git-scm.com/downloads

Uma das **vantagens** do Git sobre outros sistemas de controle de versão é que com o Git você tem uma versão no seu repositório local, na sua máquina, podendo trabalhar em cima dessa versão e depois fazer o push para o repositório remoto, ou seja, um repositório acessado por todos da equipe, o que centraliza todas as mudanças. Possível **trabalhar "offline" e de modo distribuído** (cada pessoa tem o arquivo em seu repositório local, depois devem ser resolvidos os problemas de merge entre alterações diferentes em um mesmo arquivo). Por isso é considerado um DVCS (Distributed Version Control System)

## Definições

**VCS**: Version Control System.

O **Git** é a **ferramenta** para o versionamento; o **GitHub** é um **repositório remoto** de código.

> O [GitHub](https://github.com) é muito utilizado pelos programadores e também pode servir de "vitrine"/portifólio para aqueles que procuram por um emprego na área, de modo a poderem mostrar aos recrutadores os projetos que desenvolvem ou já desenvolveram.

**Git Bash**: é um terminal de comando que pode ser instalado durante a instalação do Git. Lembra o Vim. Os comandos para navegar em pastas e tal são iguais aos do Linux. Também tem umas cores para tornar a interface mais amigável e facilitar a leitura/execução dos comandos, além de autocomplete de comandos e caminhos ao pressionar `Tab`. 

> É possível também chamar o Git pelo CMD do Windows (acertando as variáveis no PATH). 

> Você também pode clicar com o botão direito na pasta em que está seu código e selecionar para abrir com o Git Bash; com isso, ele será aberto já no caminho para esta pasta.

## Estados de um arquivo

Arquivos de um projeto versionado pelo Git podem possuir um de **três estados**: **modified**, **staged**, **committed**.

- modified: o arquivo foi alterado, mas essa alteração ainda não foi enviada para o gerenciamento de versão (não está na base de dados do git daquele projeto);

- staged: o arquivo alterado foi marcado para ser adicionado à próxima alteração do gerenciamento de versão (ao próximo commit). Costumamos dizer que estes arquivos estão na "staging area";

- committed: o arquivo está devidamente versionado e faz parte do diretório do git (está na base do git daquele projeto).

A staging area é importante por te dar o controle de selecionar quais arquivos farão parte do próximo commit. Isso possibilita ter um histórico de commits bem organizado, facilitando identificar quais arquivos foram alterados a cada commit, bem como facilitando a reversão para o caso de algum bug ter sido introduzido ao projeto, por exemplo. 

## Versionamento e repositórios

O Git versiona **repositórios**, ou seja, é necessário **criar uma pasta** para os arquivos que você quer versionar. Essa pasta será a raiz do seu repositório. Subpastas são permitidas e entram no versionamento.

**HEAD**: pensando em uma linha do tempo de versionamento, o HEAD indica a posição atual nos arquivos versionados. Geralmente é o topo do versionamento, fazendo referência ao último commit feito (mais sobre commits [nesta Seção](#commit)).

**master**: antigamente era o nome padrão dado à branch principal do repositório (mais sobre branch [nesta Seção](#branching)). Hoje em dia é considerado um nome depreciativo (lembra master/slave, ou mestre/escravo) e tem-se utilizado mais a denominação de "main". O GitHub começou a usar "main" em meados de 2020.

### `.gitignore`

É possível informar ao Git para **não versionar** alguns arquivos ou pastas; para isso, crie um arquivo `.gitignore` (com o ponto no começo, sem extensão) na raiz do repositório e nele insira o nome dos arquivos e pastas a serem ignorados 

- no caso de pastas, digite o caminho até a pasta (relativo à raiz do repositório) e insira a barra / no final do nome da pasta;

- informar o nome de uma pasta faz com que **todo conteúdo dentro dela (incluindo subpastas)** também seja ignorado;

- também é possível ignorar todos os arquivos de uma extensão, por meio da máscara `*.nome_da_extensao`;

- usar `**` indica que todas as pastas e subpastas devem ser consideradas. Por exemplo: `doc/**/*.pdf` irá ignorar todos os arquivos .pdf que estejam dentro da pasta doc e subpastas dentro dela.

Exemplos: 

```
# exemplo de comentario
arquivo_ignorado.c

# quando ha somente a barra no final, ignora 
# todas as pastas que tenham o nome pasta
# Ex: confidencial/, docs/confidencial, etc 
# serao ignorados
confidencial/

# uma barra no comeco ou no meio indica um
# caminho relativo, entao sera ignorada 
# somente essa pasta
# Nesse exemplo, somente docs/segredo/ sera
# ignorado
docs/segredo/

# ignorando todos os arquivos .tmp
*.tmp
```

> O site [gitignore.io](https://www.toptal.com/developers/gitignore/) disponibiliza templates de gitgnore para diversas linguagens de programação.

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

## Branching

Uma tradução em português para "branch" seria "ramo" ou "galho".

O processo de branching uma maneira de criar **vertentes/ramos de desenvolvimento** dentro de um repositório, de modo a não interferir no código principal (geralmente chamada de branch main). 

Uma branch, em termos básicos, é uma lista de commits sucessivos (um histórico de commits, ou uma linha do tempo). Cada novo commit é adicionado ao "topo" da branch, ou seja, a branch ativa tem um ponteiro apontando para o último commit feito. Criar uma nova branch significa criar um novo ponteiro a partir do último commit, sendo que novos commits irão atualizar para onde o ponteiro aponta, dependendo de qual branch estiver ativa.

Por exemplo, suponha que você tem a branch main, apontando para o commit abc1234. Você então cria e ativa uma nova branch "teste". Um ponteiro para essa branch é criado e também aponta para o commmit abc1234. Suponha que você fez um novo commit, cde5678; neste caso, o ponteiro de "teste", que é a branch ativa, irá apontar para cde5678, enquanto o ponteiro da branch main continua apontando para abc1234. Novos commits continuarão atualizando o ponteiro de "teste". Por fim, suponha que o último commit feito na branch "teste" foi ijk2345, e que agora você retornou para a branch main (main está ativa) e fez um novo commit, lmn6789. Neste caso, branch main irá apontar para este commit. Com isso, você agora tem duas ramificações na linha do tempo dos commits, iniciado no commit abc1234, mas com a branch main apontando para lmn6789 e a branch teste apontando para ijk2345.

Outra nomenclatura envolvida no branching é o conceito de **HEAD**. O HEAD é um ponteiro especial que **aponta para a branch ativa**. Ao trocar de branches, o que você está de fato fazendo é movendo o HEAD para apontar para a branch desejada.

O uso de branches serve, por exemplo, para desenvolver uma nova feature do projeto ou dividir o trabalho de uma equipe, possibilitando que cada um faça suas alterações de maneira independente, que posteriormente podem ser unidas à branch main (ou qualquer outra branch) por meio de um processo de merging.

O site https://git-school.github.io/visualizing-git/ é um bom exemplo para entender branches de maneira visual.

Os comandos para branching, tanto o antigo (`checkout`) quanto o novo (`switch`) podem ser vistos na seção do [`git branch`](#git-branch).

Os comandos envolvidos no merging são o [`merge`](#git-merge) e o [`rebase`](#git-rebase). A ação de merge pode gerar conflitos quando um mesmo arquivo está diferente entre diferentes branches. Nesse caso, o Git irá adicionar linhas nos arquivos que estão gerando conflito, indicando aonde esses conflitos acontecem, e **você é responsável por resolvê-los** (eliminando as linhas que você considera que não são mais necessárias para o atual estado de seu projeto, por exemplo). 

Resolvidos os conflitos, segue o processo de commit dessas mudanças e, aí então o merge/rebase é finalizado (no caso do rebase, quando eu testei, foi necessário executar o comando `git rebase --continue` para encerrar o rebase. Mais sobre comandos na [próxima Seção](#comandos-git)).

## Comandos git

Segue abaixo uma lista de comandos git, com uma descrição do que fazem e como executá-los no terminal. Essa lista é uma referência e não está em uma ordem específica. A maioria dos comandos, senão todos, devem ser usados na pasta raiz do projeto que esteja ou que vai ser versionado (seu repositório local, ou *working directory*).

Como alternativa, você pode optar por softwares que oferecem uma interface gráfica para a maioria desses comandos. O VS Code, por exemplo, tem uma seção na barra lateral esquerda chamada "Source Control". Por meio dela, é possível visualizar os arquivos modificados, deletados e adicionados, além de fazer commit e push, ver o histórico de commits, etc.

### `--help`

Essa é uma **opção (ou flag, se preferir)** que você pode adicionar a qualquer comando git (exemplo: `git status --help`) para obter um manual de como o comando funciona e quais opções ele aceita. Em versões atuais, esse manual será aberto no navegador, e não na linha de comando.

Se quiser somente saber as opções disponíveis para um comando git, de maneira mais concisa, pode usar a flag `-h`.

### `git config`

```bash
git config user.name "Seu nome aqui"
git config user.email "seu@email.aqui"
```

Os dois comandos acima servem para informar o nome e e-mail de quem está trabalhando no repositório local. Essas informações serão necessárias para fazer commits na sua máquina. 

O Git trabalha com diferentes níveis de arquivos `config`. Quando o comando é usado como mostrado acima, as configurações de nome e e-mail são **salvas localmente**, ou seja, somente para o repositório atual. O comando acima é o mesmo que `git config --local user.name "Seu nome aqui"`, sendo que a flag `--local` é o padrão, então pode ser omitida.

- Caso queira que a configuração de nome e e-mail seja salva e utilizada por **todos** os seus repositórios locais (no Windows, isso significa o usuário que está logado), **use o `--global`**. Na prática, é mais conveniente já setar como global e alterar para um projeto específico caso precise de nome/e-mail diferentes. Para isso, basta rodar o comando no projeto que deseja, sem a flag `--global`. As configurações locais sempre prevalecerão sobre as globais.

`git config --list` : mostra a lista de configurações salvas em todos os arquivos config, tanto o local quanto global e de sistema. 

`git config --local --unset <nome_da_configuracao>`: apaga da lista de configuração local a propriedade informada.

### `git init`

```bash
git init
```

Inicia o versionamento em um diretório, ou seja, cria um repositório local na pasta em que o comando foi executado. Portanto, se você quer versionar um projeto, é necessário navegar até a pasta onde estão os códigos a serem versionados e então executar o comando.

Será criada uma pasta oculta `.git`. Ela é a responsável por fazer todas a operações que o git precisa para versionar o projeto.

- flag `--bare` indica que o repositório é "puro", ou seja, só irá conter arquivos e receber modificações. Pode ser utilizado quando criamos localmente o repositório remoto, que será o repositório acessado por todos para baixar arquivos e subir mudanças.

### `git clone`

```bash
git clone url_ou_caminho_do_repositorio_remoto nome_para_a_pasta_local
```

Caso você vá trabalhar em um projeto que já está versionado, uma opção para obter uma cópia do projeto em sua máquina é usando o `git clone`. Como o nome diz, este comando clona o projeto, ou seja, baixa de um repositório **remoto** todos os arquivos e todo o versionamento e os salva para uma pasta `nome_para_a_pasta_local`. 

- Se quiser salvar o repositório na pasta corrente (e ela estiver **vazia**), basta usar o ponto (`.`) ao invés de `nome_para_a_pasta_local`: `git clone url_ou_caminho_do_repositorio_remoto .`

- Se você **não passar** o `nome_para_a_pasta_local`, será criada uma pasta com o nome do repositório clonado. Por exemplo, usando o comando `git clone https://github.com/zingarelli/anotacoes-estudo.git`, será criada a pasta `anotacoes-estudo` e o conteúdo do repositório será salvo dentro dela.

Com o projeto clonado, é possível alterar os arquivos, commitar e fazer push para o repositório remoto. 

> Caso o projeto não seja seu, será necessário ter permissão da pessoa dona do projeto para que seu usuário possa fazer push.

O `url_ou_caminho_do_repositorio_remoto` pode ser a URL de um repositório online ou o caminho para uma pasta na máquina local.

O  nome do repositório remoto será "`origin`" para o projeto clonado, mas pode ser renomeado com `git remote rename nome_atual novo_nome`.

### `git status`

```bash
git status
```

Dá detalhes sobre os arquivos em seu repositório. Por exemplo, o comando informa se há arquivos que foram criados ou modificados e que ainda não foram commitados ou enviados ao repositório remoto, bem como arquivos que estão na staging area prontos para serem commitados. Também informa em qual branch você está trabalhando.

Você pode utilizar a flag `--short` ou `-s` para ter uma saída mais concisa do status. Arquivos serão listados linha a linha, com uma letra no início para informar seu status:

- `??`: novo arquivo ainda não versionado;
- `M`: arquivo foi modificado mas ainda não adicionado à staging area;
- `A`: arquivo adicionado para a staging area;  
- `MM`: arquivo foi modificado, adicionado à staging area e posteriormente modificado novamente.

### `git add`

```bash
git add nome_do_arquivo
```

Informa ao git que o arquivo deverá ser adicionado ao próximo commit a ser executado. O arquivo vai para **"stage"** (é o estado em que o arquivo está no aguardo para ser "commitado").

- caso sejam vários arquivos, pode ser usando o ponto (`.`): 

   - `git add .`

   - **Atenção**: nesse caso, serão adicionados **todos** os arquivos que foram alterados/criados/removidos na pasta atual.

- caso queira adicionar todos os arquivos de uma pasta específica, informe o caminho relativo até aquela pasta, a partir da pasta em que se encontra atualmente. Isso irá incluir também subpastas e arquivos em subpastas a partir da pasta informada. 
   
- Para selecionar os arquivos manualmente, você pode inseri-los separados por espaço: 

   - `git add arq_1 arq_2 arq_3`

   - A tecla `tab` auxilia no autocomplete dos nomes.

### `git commit`

```bash
git commit -m "descricao_curta_de_seu_commit"
```

"Commita" suas alterações que estão em "stage", e as prepara para serem salvas no repositório remoto. Ou seja, é um jeito de informar alterações que foram feitas no projeto para resolver uma tarefa, corrigir um bug, etc. 

Isso ajuda a manter um histórico de modificações no projeto e quais arquivos foram alterados/adicionados/removidos. Por meio de outros comandos git, podemos usar esse histórico para "navegar no tempo" do projeto, verificando como ele estava em diferentes etapas, ou até mesmo reverter para um commit anterior.

A flag `-m` e a mensagem são opcionais. Se você usar somente `git commit`, será aberto o editor de texto padrão da sua máquina para que você inclua a mensagem. O commit é finalizado ao salvar a mensagem e fechar o editor de texto.

> As alterações "commitadas" ficam salvas no repositório **local**. Para de fato salvá-las no repositório remoto, utilizamos o comando [`git push`](#git-push).

A flag `-a` permite commitar todos os arquivos que foram **modificados** (os "untracked" não entram aqui) sem precisar enviá-los primeiro à staging area. É uma maneira de **pular a etapa de `git add`**. Use com cautela, pois eventualmente você pode estar commitando arquivos que você ainda não gostaria de commitar.

#### `git commit --amend`

```bash
git commit --amend -m "sua_mensagem_editada"
``` 

Use esse comando para incluir novas alterações ao **último commit** e/ou editar a mensagem de commit. Ao invés de criar um novo commit, o último commit é "editado" com as novas alterações/mensagem, e um novo hash é gerado para ele.

> Use a flag `--no-edit` se você não quer editar a mensagem original, mas sim somente incluir as alterações ao último commit

Novamente **atenção**: use o amend caso o commit **não** tenha sido enviado para o repositório **remoto**, para não modificar o histórico de commits que já foi compartilhado com outras pessoas. É possível forçar a alteração para o repositório remoto, mas o comando não é mencionado aqui.

### `git log`

```bash
git log
```

Exibe o histórico de commits feitos, em ordem decrescente de data, trazendo nome e email de quem commitou, o código Hash e a mensagem de cada commit.

> `git log --oneline`: histórico resumido; mostra somente parte do hash e mensagem de cada commit. Facilita quando você só quer ler as mensagens de cada commit para uma consulta rápida;

> `git log -p` (ou `--patch`): histórico detalhado: mostra também as alterações (diff) feitas por cada commit em relação ao commit anterior.

> `git log --graph`: mostra os commits com alguns símbolos auxiliares no começo de cada linha (`|`, `\`, `/`) para criar "bifurcações" na linha do tempo, de modo a facilitar a visualização de branches e merges que aconteceram. O símbolo `|` indica que os commits estão seguindo uma linha direta, ou seja, pertencem a uma mesma branch (geralmente a main). O símbolo `/` indica quando uma branch começou e o `\` indica o momento em que houve um merge dessa branch. Desse modo, você consegue ver o histórico de commits de uma maneira identada, permitindo verificar o histórico por branches.

### `git show`

```bash
git show hash_do_commit
```

Mostra o commit específico e também o diff dele com o commit anterior a ele.

> Se você omitir o `hash_do_commit`, será mostrado o commit do HEAD (geralmente é o último commit).

### `git diff`

```bash
git diff
```

Mostra as **diferenças** no código atual e o código que está na sua staging area (ou do último commit, se não houver nada em stage). 

E se mais de um arquivo tiver sido modificado? Ele vai mostrar as diferenças arquivo por arquivo; a linha de comando dá indicações de quando um arquivo termina e um novo será mostrado.

A flag `--stage` ou `--cached` mostra as diferenças que estão em staging em comparação ao último commit.

> `git diff hash_commmit_X..hash_commmit_Y`: mostra as diferenças desde o `hash_commmit_X` até o `hash_commmit_Y`. Se **X for um commit mais antigo que Y**, serão mostradas as diferenças em ordem **cronológica**. Caso X seja um commit mais novo que Y, as diferenças serão mostradas em ordem cronológica reversa (meio que mostrando o que será desfeito de X para Y).

> o mesmo comando pode ser aplicado para nome de branches.

### `git remote`

```bash
git remote add nome_do_repositorio_remoto endereco_para_o_repositorio_remoto
```

Cria uma ligação entre o repositório local e um repositório remoto. O endereço pode ser tanto uma pasta da própria máquina (precisa ser preparada com o `git init --bare`), quanto uma URL (para um repositório online criado no GitHub, por exemplo). O `nome_do_repositorio_remoto` é o nome que você dará para esse repositório remoto, e que será necessário na hora do `push`/`pull`. A convenção é usar o nome "origin" para o repositório remoto principal (esse é o nome que o Git usa quando você clona um projeto).

> Para listar todos os repositórios remotos vinculados ao seu repositório local e a URL associada a cada um, use o comando: `git remote -v`. Se quiser só ver o nome dos repositórios, omita o `-v`.

> Para remover (desvincular) um repositório remoto: `git remote remove nome_do_repositorio_remoto` ou `git remote rm nome_do_repositorio_remoto`.

> Para atualizar o caminho de um repositório remoto: `git remote set-url nome_do_repositorio_remoto novo_endereco_para_o_repositorio_remoto`

> Para renomear um repositório remoto: `git remote rename nome_atual novo_nome`

### `git fetch`

```bash
git fetch nome_do_repositorio_remoto
```

Este comando baixa os dados do repositório remoto que você ainda não tem no seu repositório local, incluindo novas branches que você ainda não tenha. No entanto, o comando **não** faz o merge automático do que foi baixado com as mudanças que você tenha feito localmente - você deve fazer isso manualmente, para evitar gerar algum conflito.

Como alternativa para um fetch + merge automático, temos o [`git pull`](#git-pull).

### `git push` 

```bash
git push nome_do_repositorio_remoto nome_da_branch
```

Envia para o repositório remoto todas mudanças commitadas localmente que ainda não tenham sido enviadas.

Atenção: caso você tente fazer um push e o repositório remoto tenha commits que você ainda não tem, seu push será rejeitado. Nesse caso, primeiro você vai ter que fazer o fetch, incorporar os commits que você ainda não tinha (e resolver qualquer conflito) para daí então "pushar" seus commits. Isso mantém o histórico de commits íntegro e único.

A flag `-u` (`git push -u nome_do_repositorio_remoto nome_da_branch`) memoriza o repositório e a branch desse `push` e deixa você usar o comando `git push` sem precisar informar repositório e branch toda vez. **Use com cautela**: caso você comece a trabalhar com outras branches e se esqueça de alterar na hora de fazer o `push`, poderá estar fazendo o push na branch ou repositório errados, então o recomendado é fazer o push completo, com nome do repositório remoto e nome da branch.

### `git pull`

```bash
git pull [nome_do_repositorio_remoto]
```

Comando oposto ao `push`, baixa os arquivos do repositório remoto e tenta adicioná-los ao seu repositório local (caso encontre conflito entre o código do repositório remoto e o local, será necessário verificar e resolver). Ele pode ser visto como um combo de `git fetch` + `git merge`.

**Atenção**: minhas anotações estavam anteriormente **ERRADAS**. Eu havia considerado que era obrigatório enviar no comando do git pull dois argumentos: `[nome_do_repositorio_remoto]` e `[nome_da_branch]`. Eles são opcionais. Mais detalhes sobre eles:

- O argumento `[nome_do_repositorio_remoto]` é  usado caso você tenha mais de um repositório remoto e queira indicar de onde as mudanças devem ser baixadas. Quando não passado, o pull será feito no repositório remoto padrão (geralmente é o `origin` do GitHub);

- Há um segundo argumento `[nome_da_branch]` que você pode passar para indicar que você quer atualizar sua branch local com o conteúdo da branch remota indicada. **Tome cuidado para não atualizar a branch local errada** ao usar esse argumento. Quando não passado, o pull será feito da branch local em que você está. 

**Pergunta**: O que acontece com os arquivos que você alterou e ainda não commitou após um `pull`? Perde os dados? Os arquivos alterados não são baixados?

**Resposta**: se os arquivos estiverem alterados no repositório remoto e também foram modificados no repositório local (mas ainda não commitados), ao fazer o `pull` o git irá emitir um erro de que o arquivo será sobrescrito pelo merge. A solução que o git informa é commitar o arquivo que está modificado no repositório local ou usar o `git stash` antes de fazer o pull (ver mais abaixo sobre o [`git stash`](#git-stash)).

- Se optar por commitar, ao fazer o `git pull` o próprio git irá tentar fazer o merge das modificações que estão no repositório remoto e no local;

- Se optar pelo `stash`, ao fazer o `git pull` o arquivo será atualizado somente com as modificações do repositório remoto e você fica responsável por trazer as modificações que estão no stash e resolver os conflitos de merge.

### `git stash`

```bash
git stash
```

Salva os arquivos em um **local temporário** da sua máquina, por exemplo, para alterações que eu ainda não terminei mas que não vou commitar por enquanto. Desse modo, essas alterações não são perdidas. Isso não afeta branches, ou seja, você pode salvar arquivos no stash e fazer modificações/navegações entre branches. Atenção: ao fazer isso, seu código será revertido para o do último commit.

> `git stash save "sua mensagem aqui"`: permite que você adicione uma mensagem ao salvar os arquivos no stash.

- o comando mais atual é `git stash push -m "sua mensagem aqui"`

> `git stash list`: lista todas as modificações que foram salvas no stash (caso você tenha usado o comando git stash mais de uma vez). Cada modificação ganha um número na lista (um índice na pilha do stash), facilitando seu acesso

> `git stash apply numero_da_lista`: traz de volta as modificações que foram salvas e faz um merge com as outras alterações que foram feitas desde então. O `numero_da_lista` é o número que vem entre chaves na lista informada pelo `git stash list` e que você quer recuperar (exemplo: `stash@{0}:`). Esse comando **não** remove o item da stash.

> `git stash drop numero_da_lista`: remove o que foi salvo na stash, no índice passado em `numero_da_lista`.

> `git stash pop`: traz as modificações do último `git stash` feito e o remove da lista do stash (é uma combinação de `git stash apply 0` e `git stash drop`); é como um pop de pilha.

> `git stash clear`: limpa todas as modificações salvas no stash. Use com cautela.

### `git branch`

```bash
git branch nome_da_nova_branch
```

Cria nova branch, porém, **não** a ativa como principal; você continuará na branch atual. Se você quiser criar uma nova branch e já ativá-la como principal, pode usar o comando:

```bash
git checkout -b nome_da_nova_branch
```

O comando acima cria a branch e faz o HEAD apontar para ela (leia sobre HEAD na [Seção sobre branching](#branching)). 

- Atualmente, também temos o comando `git switch` para trabalhar com branches, substituindo o `git checkout`. O comando para criar a branch e ativá-la como principal (flag `-c` ou `--create`) fica:
   
```bash
git switch -c nome_da_nova_branch
```

> `git branch -M novo_nome_da_branch`: comando para renomear a branch atual **localmente**. 

> `git checkout nome_da_branch` ou `git switch nome_da_branch` : altera a branch que será a ativa (move seu HEAD para esta nova branch), carregando a versão mais atual dos arquivos que estão nessa branch.

> `git branch -d nome_da_branch`: remove `nome_da_branch` do seu repositório **local**. Caso queira remover também do remoto, o comando é `git push nome_do_repositorio_remoto --delete nome_da_branch` (ou `git push nome_do_repositorio_remoto :nome_da_branch`).

- É considerada uma boa prática remover branches após finalizar a feature que estava atrelada àquela branch, para manter o projeto limpo.

### `git checkout`

```bash
git checkout HASH_do_commit
```

Navega para um commit específico do log. Serão carregados os arquivos desse commit, **PORÉM**, qualquer **alteração** feita no código, inclusive commits, **NÃO** entrará no histórico. Para que essas alterações entrem no histórico do projeto, você deve **criar uma branch** (pode usar o `git checkout -b nome_da_nova_branch` após ter navegado para o commit desejado).

> Atualmente, o git fornece um comando alternativo, mais intuitivo: o [`git restore`](https://git-scm.com/docs/git-restore). Essa mudança veio por conta da confusão da dupla responsabilidade do `git checkout`, que serve tanto para mudar de branch quanto para restaurar arquivos navegando por commits.

### `git merge`

```bash
git merge outra_branch
```

Faz um merge (combinação) da branch atual com o commit mais atual da `outra_branch`, para que as **duas ramificações se unam em uma só**. 

Existe um método de merge chamado "fast-forward", que é quando a branch atual está somente "para trás" na linha do tempo de commits em relação à `outra_branch`. Neste caso, o git somente vai avançar a branch atual, fazendo com que ela "aponte" para o último commit da `outra_branch`. 

Existe um caso diferente em que a branch atual e a `outra_branch` divergiram, isto é, a partir de um ponto em comum, cada uma evoluiu para lados diferentes (houve commits tanto na branch_atual quanto na `outra_branch` após esse ponto em comum). Neste caso, o git também vai fazer com que a branch atual aponte para o último commit da `outra_branch`, e irá tentar mesclar as mudanças entre esse dois commits (o último da branch atual e o último da `outra_branch`). Caso não haja nenhum conflito, irá solicitar a inclusão de uma mensagem de commit de merge (caso abra o editor de texto VIM, se não quiser escrever nada, digite `:x` e pressione `ENTER`; caso esteja no modo de INSERT, pressione `ESC` antes de digitar o `:x`). 

No processo de merge, os commits anteriores da `outra_branch` são **incorporados** à branch atual, ou seja, eles passam a fazer parte do histórico da branch atual. No entanto, os commits também continuam presentes no histórico da `outra_branch`, caso você faça um checkout nela. A visualização do log nestes caso fica mais fácil com o comando `git log --graph`.

### `git rebase`

```bash
git rebase outra_branch
```

Traz todos os commits da `outra_branch` para a branch atual e atualiza as modificações. Os commits da `outra_branch` serão inseridos na "linha do tempo" dos commits a partir do momento em que houve a criação da `outra_branch`, ou seja, o head da branch atual é movido para o último commit da `outra_branch` e as mudanças feitas em cada commit da branch atual serão aplicadas, commit a commit, após este último commit de `outra_branch`. Ou seja, novos commits são gerados (uma nova chave hash) e o head da branch atual vai avançando commit por commit, até chegar no último commit que havia na branch atual. Ao final, temos uma "linha do tempo" única novamente para a branch atual, unificada com as mudanças de `outra_branch`.

Como as mudanças são aplicadas commit a commit, conflitos podem acontecer e devem ser resolvidos também commit a commit.

> Lembre-se do site para uma experiência visual de merge, rebase, etc: https://git-school.github.io/visualizing-git/

### `git restore`

#### Arquivos modificados

```bash
git restore nome_do_arquivo
```

Comando antigo (versões mais velhas do Git): `git checkout -- nome_do_arquivo`.

Funciona como um `Ctrl+Z` para um arquivo (ou vários arquivos), quando eu quero **descartar** as alterações feitas em um arquivo que está com o **status "modified"**. Irá restaurar o arquivo corrente ao meu repositório local. 

   - Se quiser restaurar todos os arquivos, você pode usar o `git restore .`

**Atenção**: você **perde** suas modificações ainda não commitadas no arquivo ou ainda não adicionadas ao stage. Se você não quer perder, pode usar outras estratégia como o [`git stash`](#git-stash) ou criar uma nova [branch](#branching).

**Atenção 2**: se você já tiver adicionado o arquivo ao stage (por meio do `git add nome_do_arquivo`), esse comando de restore não irá funcionar; primeiro você precisará fazer o comando abaixo.

#### Arquivos em staging 

```bash
git restore --staged nome_do_arquivo
```

Serve para **remover** um arquivo que está na staging area, ou seja, que foi adicionado para ser commitado (ou seja, desfaz a ação de um `git add nome_do_arquivo`). Seu versionador voltará a olhar para o HEAD como sendo o estado atual e o arquivo voltará a aparecer como modified ao dar um `git status` - neste caso, você não perde as alterações feitas no arquivo.

Comando antigo (versões mais velhas do Git): `git reset HEAD nome_do_arquivo`.

> `git restore --source=hash_do_commit nome_do_arquivo`: restaura o arquivo com base em como este arquivo se encontrava no commit informado em `--source`

### `git revert`

```bash
git revert HASH_do_commit
```

**Desfaz** as mudanças efetuadas no commit cujo hash você informou no comando. Será **gerado um novo commit para informar essa alteração**, ou seja, o commit que você informou continuará acessível no histórico (no log).

> não precisa passar o código hash completo. Pode passar somente os 7 caracteres iniciais

**Atenção**: desfaz as mudanças *somente* do commit informado no commando. Imagine que você fez o revert de um commit que está bem para trás do seu histórico. Aplicar o revert irá somente desfazer o que *aquele* commit alterou. Os outros commits que estão à frente deste no histórico serão **mantidos**. Quando você quer desfazer um commit e todos os outros que o procederam, você está aplicando um `git reset`, que é mais poderoso e **perigoso**.

### `git reset`

```bash
git reset --hard HASH_do_commit_a_ser_mantido
```

Este comando remove de fato um ou mais commits do histórico de commits. Toda a sequência de commits no histórico de commits que vierem *depois* do commit informado no comando serão removidos. Por conta da flag `--hard`, as alterações feitas por essa sequência de commits será removida do código e eles serão **também removidos do histórico** de commits. Por conta disso, é um comando perigoso, que deve ser usado com atenção para não perder conteúdo.

> O `git reset` é aplicado no seu repositório local. Cuidado caso o commit resetado já esteja no repositório remoto, pois isso altera o histórico de commits que outras pessoas têm em seu próprio repositório local. Para resetar também no repositório remoto, é um outro comando, não mencionado aqui.

```bash
git reset --soft HEAD~1
```

**Desfaz** seu último commit, removendo-o do histórico, mas **mantém** as alterações que haviam sido feitas (por conta da flag `--soft`). Útil quando, por exemplo, você commitou, mas viu que faltou uma pequena alteração. Ao invés de aplicar a alteração e criar um novo commit, você pode usar esse comando para desfazer o commit, aplicar a alteração, e então commitar. Seria como um "`Ctrl+Z`".

> Uma alternativa melhor é usar o comando [`git commit --amend`](#git-commit---amend).

### `git tag`

```bash
git tag nome_da_tag
```

Cria uma tag para o último commit dado. Serve para nomear aquele commit. Pode ser utilizado, por exemplo, para dar um nome a uma "versão" finalizada do projeto, um release do projeto. 

Imagine que você chegou a uma etapa importante do seu projeto e vai entregar a versão 1.0 dele. Neste caso, você pode usar `git tag v1.0`.

> `git tag -a nome_da_tag -m "mensagem"`: chamada de "annotated tag" (opção `-a`), é possível adicionar uma mensagem à tag criada.

   - annotated tags também criam outros metadados, como o nome do autor e data que a tag foi criada. Se quiser ver essas informações, use `git tag -v nome_da_tag` ou `git show nome_da_tag` (não funciona para tags que não são annotated, chamadas de "lightweight tags").

> `git tag -a nome_da_tag numero_do_commit`: gera uma tag para um commit específico (por exemplo, quando você esqueceu de criar uma tag e já fez outros commits posteriores). Pode ser o hash completo ou os primeiros sete dígitos.

> `git tag`: mostra todas as tags criadas.

> `git tag -d nome_da_tag`: remove a tag em seu repositório local. Para remover do repositório remoto, use `git push nome_do_repositorio_remoto --delete nome_da_tag`

> `git push nome_do_repositorio_remoto nome_da_tag`: comando necessário para enviar a tag para o repositório remoto.

> `git push nome_do_repositorio_remoto --tags`: envia para o repositório remoto todas as tags ainda não enviadas.

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

Caso deseje enviar as modificações feitas no seu projeto "forkado" para projeto original, você utiliza a funcionalidade de **Pull Request (PR)** do GitHub.

É possível enviar um título e um texto com as modificações feitas. O GitHub irá mostrar o diff dos arquivos modificados e também já avisa se consegue fazer o merge automático.

O responsável pelo repositório original poderá analisar os PR e decidir se irá fazer ou não o merge da solicitação. Tudo isso é feito dentro do GitHub.

> Você pode conferir na prática uma abertura de PR [neste vídeo](https://www.youtube.com/watch?v=cdL_F3FiSWI) do instrutor Vinicius Dias.

> É possível também usar PR dentro de um projeto para o gerenciamento de branches. Ao invés de fazer o merge via linha de comando, você pode abrir um PR para uma branch que já foi desenvolvida, solicitando que ela seja incorporada à branch principal. O processo é semelhante ao descrito acima. 

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

É o nome que se dá à ação de selecionar um commit específico e trazer para o HEAD da branch atual. Imagine, por exemplo, que há uma branch em que uma nova funcionalidade está sendo desenvolvida, mas ainda não está pronta. No entanto, uma modificação feita nessa branch está sendo necessária na branch principal (correção de um bug, por exemplo). É possível, neste caso, usar o `cherry-pick` para trazer para a branch principal somente o commit que possui essa modificação. Será feito um merge (ou deverão ser resolvidos os conflitos) e um novo commit será gerado, incluindo a mensagem do commit (se houver).

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

Apesar do nome (culpar), o comando informa o último usuário que alterou *cada linha* do código em um arquivo. Informa também o hash do commit e data da modificação.

```bash
git blame nome_do_arquivo
```

O VS Code possui extensões que mostram visualmente essas informações na tela.

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

### Continuar em...

"Basic Branching and merging" na pág. 70