Git/Git-Flow
=======================================================


Lifecicle da tarefa até a conclusão (Git-Flow)
-------------

Toda a tarefa desenvolvida deverá seguir um ciclo desde seu início até sua conclusão e publicação, para este fim iremos utilizar o Git-Flow.

###Iniciando o Git-Flow

Para instruções de como instalar o Git-Flow em seu ambiente e realizar a sua configuração inicial pode-se utilizar o [Git-Flow Cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/)

###Fluxo de Trabalho

O processo de desenvolvimento deve sempre se iniciar apartir de uma tarefa criada no clickup, a partir desta tarefa uma branch com a solução será aberta porém dependendo do tipo da tarefa ela terá algumas diferenças.

- **Novas Funcionalidades.** Deverá ser aberto uma branch do tipo `feature` a partir da `develop`
- **Bugs.** Deverá ser aberto uma branch do tipo `bugfix` a partir da `develop`
- **Bugs ou Funcionalidades Urgentes.** Deverá ser aberto uma branch do tipo `hotfix` a partir da `master`

**OBS.** Todas as tarefas no clickup deverão ter uma tag sinalizando sua categoria, caso não tenha pode-se cobrar o responsável pelo projeto.

Assim que aberta a branch o fluxo de desenvolvimento começa com a seguinte ordem:

1. Com a branch aberta todo o desenvolvimento da tarefa deverá ser feito unicamente nela, nenhuma nova branch deverá ser aberta para a mesma tarefa.
   1. Durante o desenvolvimento, as boas práticas quanto a atualização da branch e commits deverão ser seguidos, estas boas práticas são detalhadas mais a frente.
2. Uma vez finalizada a tarefa, deverá ser aberto um **Pull Request** dentro do repositório do projeto com a branch criada.
   1. Se assegure que a branch alvo de sua **Pull Request** é a correta.
   2. Faça uma última verificação no resumo das mudanças que serão exibidas para se certificar que não está indo nenhum fragmento de código indesejado.

![alt text](assets/Git%20Flow%20Passo%201.png)

![alt text](assets/Git%20Flow%20Passo%202.png)


Padrão para nomenclatura das branchs
-------------

Para facilitar a identificação da sua branch ela sempre deverá ser criada com o nome da tarefa que a originou em padrão **snake_case**.

Por exemplo, uma tarefa chamada `Criar cadastro de clientes` deverá ter sua branch criada como `criar_cadastro_de_clientes`.

Mantenha a branch atualizada
-------------

É recomendável, principalmente se a tarefa levar mais que um dia para ser finalizada, que sua branch esteja sempre atualizada com a branch que deu origem a ela seja a `develop` ou a `master`.

Para fazer isto utilize os seguintes comandos:

- `git checkout <develop/master>`
- `git pull`
- `git checkout <branch>`
- `git merge <develop/master>`

É recomendável também que ao criar a **Pull Request** e até depois disso, enquanto ela ainda não foi fechada, que se verifique se não foi alertado conflitos com a branch raiz.

Criação do comentário para os commits
-------------

O comentário no momento do commit deve refletir o que realmente está sendo "comitado", uma boa prática para o comentário é que ele seja feito começando com um termo no **infinitivo**. Exemplo `Criando ação para disparo de e-mails`.

### Quando realizar o Commit?
Para que o comentário possa ser feito da forma correta, de forma a expressar o que realmente foi desenvolvido e que não fique muito extenso é necessário que o fluxo de commits seja coerente.

Para isso, assim como em alguns padrões de denvolvimento, recomenda-se utilizar a abordagem do **Princípio da Responsabilidade Única**, este princípio pode também ser aplicado aos commits fazendo com que cada commit seja composto de linhas que façam sentido juntas para uma mesma lógica.
t