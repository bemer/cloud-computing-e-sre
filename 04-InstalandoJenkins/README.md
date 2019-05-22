# Instalando o Jenkins

![logo](/04-InstalandoJenkins/images/logo.png)


Agora que já temos a configuração de rede de nossas máquinas virtuais devidamente efetuadas e todos os pré-requisitos devidamente preenchidos, vamos passar ao processo de instalação do Jenkins.

Jekins é uma ferramenta de automação open source escrita em Java e será utilizado para gerar novas versões de nossa aplicação automaticamente, sendo o principal recurso para a implementação de *Continuous Integration* em nosso laboratório.

Siga os passos a seguir na ordem exata para realizar a instalação do Jenkins na VM `Jenkins` entregue.

## 01. Instalação do Java

Como o Jenkins é desenvolvido em Java, é necessário realizarmos a instalação do mesmo na máquina virtual. Para tanto, utilize os seguintes comandos:

    sudo apt-get update
    sudo apt-get install openjdk-8-jdk -y

## 02. Instalação e configuração do Docker

Nos próximos laboratórios, vamos gerar imagens Docker e, para isto, precisamos instalar o Docker em nossa VM. Para instalar o Docker, execute os seguintes comandos:

    sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y

Adicione a chave GPG oficial:

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Em seguida, adicione o repositório do Docker em seu sistema através do comando:

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

Atualize seus repositórios:

    sudo apt-get update

E finalmente execute a instalação do Docker:

    sudo apt-get install docker-ce docker-ce-cli containerd.io -y

Para validar a instalação do Docker, execute o seguinte comando:

    sudo docker run hello-world

A saída deverá ser semelhante a:

    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    1b930d010525: Pull complete
    Digest: sha256:92695bc579f31df7a63da6922075d0666e565ceccad16b59c3374d2cf4e8e50e
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
    3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/

isto significa que o Docker foi devidamente instalado em sua VM.

<!-- Vamos agora configurar o Daemon do docker para que possamos habilitar conexões remotas ao serviço. Isto será necessário para que possamos utilizar o plugin do Jenkins que realizará o build de nossas imagens posteriormente. Para isto, edite o arquivo `vi /lib/systemd/system/docker.service` utilizando o seguinte comando:

    sudo vi vi /lib/systemd/system/docker.service

Altere a linha 15 deste arquivo, mudando de:

    ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Para:

    ExecStart=/usr/bin/dockerd -H fd:// -H=tcp://0.0.0.0:2375

> Esta configuração irá habilitar conexões via socket ao Docker Daemon. Mais informações sobre esta alteração podem ser encontradas no seguinte link: https://docs.docker.com/engine/reference/commandline/dockerd/

Feito isto, reinicie o serviço do Docker através dos seguintes comandos:

    systemctl daemon-reload
    sudo service docker restart

Para validar se a alteração foi realizada com sucesso, podemos observar o status do processo dockerd através do seguinte comando:

    ps -ef | grep docker

A saída deverá ser semelhante a:

    root      2340     1  2 23:43 ?        00:00:00 /usr/bin/dockerd -H fd:// -H=tcp://0.0.0.0:2375 -->



## 03. Adicionando o repositório do Jenkins

O próximo passo, é adicionar o repositório do Jenkins. Para isto, importe as chaves GPG do Jenkins através do seguinte comando:

    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -

A saída do comando anterior deverá ser `OK`.
Agora, vamos adicionar o repositório do Jenkins ao sistema através do seguinte comando:

    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

Este comando não irá gerar nenhuma saída.

## 03. Instalando o Jenkins

Após instalar os pré-requisitos e adicionar o repositório do Jenkins ao nosso sistema, vamos efetuar a instalação do Jenkins. Faça isto utilizando os seguinte comandos:

    sudo apt-get update
    sudo apt-get install jenkins -y

Após sua instalação o Jenkins deverá ser iniciado automaticamente. Valide o funcionamento do Jenkins através do seguinte comando:

    systemctl status jenkins

A saída deverá ser similar a esta:

    ● jenkins.service - LSB: Start Jenkins at boot time
       Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
       Active: active (exited) since Sun 2019-04-21 00:00:30 -03; 2min 19s ago
         Docs: man:systemd-sysv-generator(8)

Após o processo de instalação, acesse o Jenkins através de seu Web Browser utilizando o endereço:

    http://localhost:8080

Você será redirecionado para uma página de login do Jenkins que solicitará a senha criada durante a instalação:

![unlock jenkins](/04-InstalandoJenkins/images/unlock-jenkins.jpg)

Durante o processo de instalação, o Jenkins gerou uma senha com 32 caracteres e salvou no arquivo `/var/lib/jenkins/secrets/initialAdminPassword`. Para obter esta senha, volte ao terminal e utilize o seguinte comando:

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

A senha será exibida. Copie a mesma a partir do terminal e cole-a na interface. Feito isto, clique em `Continue`.

O próximo passo é realizar a instalação dos plugins do Jenkins. Clique na opção `Install suggested plugins`:

![customize jenkins](/04-InstalandoJenkins/images/customize-jenkins.jpg)

A instalação iniciará automaticamente:

![getting started](/04-InstalandoJenkins/images/jenkins-getting-started.jpg)

Quando a instalação for finalizada, será apresentada uma tela para a criação do usuário administrador do Jenkins. Preencha com as seguintes informações:

    Nome de usuário: admin
    Senha: admin01
    Confirmar a senha: admin01
    Nome completo: Administrator
    Endereço de e-mail:	seuemail@provedor.com

E clique em `Save and continue`:

![create user](/04-InstalandoJenkins/images/create-user.png)

A próxima página irá pedir pela URL utilizada pelo Jenkins e já virá populada. Apenas clique em `Save and finish`:

![create user](/04-InstalandoJenkins/images/instance-configuration.png)

Feito isto, sua instalação do Jenkins estará completa e você poderá continuar para a página de login clicando no botão `Start using Jenkins`:

![jenkins ready](/04-InstalandoJenkins/images/jenkins-ready.png)

![jenkins main](/04-InstalandoJenkins/images/jenkins-main.png)

## 04. Adicionando permissões

Durante o processo de instalação do Jenkins, o usuário `jenkins` é criado no sistema operacional. Como vamos utilizar o Jenkins para realizar o build das imagens, precisamos que este usuário tenha permissões para executar o Docker. Para isto, precisamos adicionar o usuário `jenkins` no grupo `docker`. Utilize o seguinte comando:

    sudo groupadd docker
    sudo usermod -a -G docker jenkins

Em seguida, reinicie o processo do Jenkins:

    sudo service jenkins restart


## 05. Criando um novo Job

Agora que temos o plugin para build instalado, devemos realizar a criação de um novo job para que o repositório seja acessado a partir do Jenkins. Para isto, na tela inicial, clique em `Novo Job`. Nesta tela, defina o nome `build-fiap-app`, selecione `Construir um projeto de software free-style` e em seguida clique em `OK`:

![job creation](/04-InstalandoJenkins/images/job-creation.png)

Na próxima tela, selecione `Git` em `Gerenciamento de código fonte` e adicione a URL do seu repositório no GitHub:

![repo selection](/04-InstalandoJenkins/images/repo-selection.png)

Agora, ainda nesta tela, clique em `Adicionar passo no build` no sub-menu `Build` e selecione `Docker Build and Publish`:

![add build step](/04-InstalandoJenkins/images/add-build-step.png)

Na nova tela, preencha as informaçoes da seguinte maneira:

    Repository Name: seu_dockerhub_username/fiap-app
    Tag: $BUILD_NUMBER

Adicionalmente, vamos precisar informar as credenciais do Docker Hub para que o Jenkins possa realizar o upload de nossa imagem. Para isto, em `Registry Credentials` clique no botão `Add` e em seguinda selecione `Jenkins`:

![add credentials](/04-InstalandoJenkins/images/add-credentials.png)

Na nova tela, preencha as informações da seguinte maneira:

    Username: seu usuário do Docker Hub
    Password: sua senha do Docker Hub
    ID: docker-hub-credentials

Em seguida, clique em `Add`:

![adding credentials](/04-InstalandoJenkins/images/adding-credentials.png)

Feito isto, na tela de configuração do processo de Build, selecione as credenciais recém criadas, e em seguida clique em `Salvar`:

![save build](/04-InstalandoJenkins/images/salve-build.png)

Você será redirecionado para a tela do projeto `build-fiap-app`. Para realizar o build, clique em `Construir agora` no menu lateral esquerdo:

![build now](/04-InstalandoJenkins/images/build-now.png)

O processo de build será iniciado e você poderá acompanhar a execução do mesmo, clicando no histórico de builds, na parte esquerda da tela:

![build history](/04-InstalandoJenkins/images/build-history.png)

Caso queira acompanhar as mensagens geradas pelo processo, clique em `Saída do console` no menu lateral esquerdo:

![console output](/04-InstalandoJenkins/images/console-output.png)

Ao final do processo, caso tudo esteja correto, na página inicial do Jenkins, será exibido o status de seu job com um ícone azul e o indicador de sucesso:

![success build](/04-InstalandoJenkins/images/success-build.png)

## 06. Validando o build

Agora que a build é apresentada no Jenkins com o status de sucesso, o próximo passo é saber se a imagem Docker foi devidamente criada e enviada para o Docker Hub. Para isto acesse a sua console do Docker Hub através da URL https://hub.docker.com, insira suas credenciais, e você deverá visualizar uma imagem Docker disponível:

![docker hub console](/04-InstalandoJenkins/images/docker-hub-console.png)

Clique na imagem em questão e verifique se as tags foram devidamente preenchidas. Você deverá identificar a tag `latest` e a tag `1`, que representa o ID do processo de build do Jenkins:

![docker tags](/04-InstalandoJenkins/images/docker-tags.png)
