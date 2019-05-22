# Pré Requisitos para Execução do Laboratório

![checkmark](/03-PreRequisitosJenkins/images/checkmark.png)


O próximo laboratório a ser executado gira em torno da instalação do `Jenkins` e da configuração de um job para build de uma imagem Docker com nossa aplicação para deploy em ambientes de nuvem. Para isto, precisaremos preencher alguns requisitos mínimos que são:

- Criação de conta no Docker Hub;
- Criação de uma conta no GitHub;
- Criação de um novo repositório no GitHub para a aplicação;
- Clone dos repositórios do laboratório para a VM;
- Commit do código fonte da aplicação para o novo repositório.

## 01. Criando uma conta no Docker Hub

O Docker Hub é um `Container Registry` executado em ambiente de nuvem e mantido pela própria Docker. Quando criamos uma conta no Docker Hub, temos acesso à criação de repositórios públicos ilimitados e um repositório privado gratuitamente. Por ser o registro padrão utilizado pelo Docker, vamos utilizar este serviço em nosso laboratório.

Crie sua conta no Docker Hub utilizando a seguinte URL:

    https://hub.docker.com/signup

Para a criação de sua conta, você precisará de apenas 3 informações:

- Um Docker ID, que será o seu username;
- Uma senha;
- O seu endereço de email.

> Dica: quando geramos imagens Docker e enviamos a mesma para algum registry, o seu username fará parte do nome da imagem para futura identificação, desta maneira manter o Docker ID curto facilitará processos de pull e push futuramente.


## 02. Criando uma conta no GitHub

O GitHub é uma plataforma de hospedagem de código-fonte com controle de versão usando o Git. Ele permite que programadores, utilitários ou qualquer usuário cadastrado na plataforma contribuam em projetos privados e/ou Open Source de qualquer lugar do mundo. Em nosso laboratório, vamos utilizar o GitHub para armazenar todo o código fonte da nossa aplicação bem como arquivos utilizados para o processo de Build (Dockerfile).

Crie sua conta no GitHub através do seguinte link:

    https://github.com/join?source=header-home


## 03. Criação de novo repositório para a aplicação

Após realizar a criação de sua conta no GitHub, vamos criar um novo repositório para armazenarmos o código fonte da aplicação utilizada. Para isto, clique no botão `New` no lado esquerdo da tela:

![new repo](/03-PreRequisitosJenkins/images/new-repo.png)

Nomeie seu repositório como `fiap-app` e clique em `Create repository`:

![create repo](/03-PreRequisitosJenkins/images/create-repo.png)

Você será redirecionado para a página de seu repositório. Nesta página é exibida a URL utilizada para `Clone` do repositório, que é basicamente o processo de realizar o download do código fonte presente no repositório para o seu computador. Tome nota desta URL pois a mesma será utilizada nos próximos passos:

![repo main screen](/03-PreRequisitosJenkins/images/repo-main-screen.png)

## 04. Clone dos repositórios para a VM

Neste momento você deverá possuir todas as suas contas devidamente criadas e estar acessando sua VM, onde vamos realizar o clone do repositório do laboratório.

>Note que na VM fornecida o Git já está devidamente instalado, porém caso você queira realizar o processo em outro computador basta seguir o passo-a-passo da instalação de acordo com seu sistema operacional.

Para realizar o clone do repositório do laboratório para a VM, utilize o seguinte comando na janela do Putty:

    git clone https://github.com/bemer/cloud-computing-e-sre.git

A saída deste comando deverá ser semelhante a:

    Cloning into 'cloud-computing-e-sre'...
    remote: Enumerating objects: 224, done.
    remote: Counting objects: 100% (224/224), done.
    remote: Compressing objects: 100% (190/190), done.
    remote: Total 224 (delta 41), reused 211 (delta 28), pack-reused 0
    Receiving objects: 100% (224/224), 41.08 MiB | 5.25 MiB/s, done.
    Resolving deltas: 100% (41/41), done.
    Checking connectivity... done.

Agora, vamos realizar o clone do seu repositório recém criado. Você irá utilizar o mesmo comando, porém substituindo a URL pela obtida no [passo 3](/03-PreRequisitosJenkins):

    git clone URL_DO_SEU_REPOSITORIO

O Git irá devolver uma mensagem de aviso dizendo que o repositório em questão está vazio. Não se preocupe, pois de fato ainda não adicionamos nenhum conteúdo ao mesmo.

## 05. Criação de um novo repositório no GitHub para a aplicação

Agora, vamos acessar o diretório gerado pelo processo de clone do repositório que você criou. Para isto, utilize o comando:

    cd fiap-app

Agora, vamos copiar apenas o conteúdo da aplicação presente no repositório do laboratório para o repositório da sua aplicação através do comando:

    cp -r ~/cloud-computing-e-sre/00-Application/* .

Liste o conteúdo da pasta através do comando `ls`. A saída deverá ser semelhante a esta:

    app  Dockerfile

isto significa que o conteúdo foi devidamente copiado. Vamos agora enviar o conteúdo para o GitHub. Vamos agora configurar o Git na VM para que possamos realizar o upload dos arquivos. Execute o seguinte comando, inserindo o endereço de email utilizado na criação de sua conta no GitHub:

    git config --global user.email "SEU_ENDEREÇO_DE_EMAIL"

Agora, utilize os seguintes comandos para enviar os arquivos da aplicação para o GitHub:

    git add --all
    git commit -m 'Initial commit'
    git push

 O Git irá pedir novamente os seus dados de email e senha de acesso. Preencha as informações e aguarde o upload ser finalizado. A saída do comando deverá ser semelhante a esta:

    Counting objects: 79, done.
    Compressing objects: 100% (75/75), done.
    Writing objects: 100% (79/79), 28.14 MiB | 1.17 MiB/s, done.
    Total 79 (delta 5), reused 0 (delta 0)
    remote: Resolving deltas: 100% (5/5), done.
    To https://github.com/bemer/fiap-app.git
     * [new branch]      master -> master

Valide o commit de suas alterações acessando o repositório através da interface Web do GitHub. Você deverá visualizar os arquivos da aplicação:

![github commit](/03-PreRequisitosJenkins/images/github-commit.png)
