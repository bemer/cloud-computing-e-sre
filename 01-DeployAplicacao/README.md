# Instalando uma aplicação na Nuvem Pública

![deploy](/01-DeployAplicacao/images/deploy.png)


Uma das melhores formas para se entender como a nuvem pública funciona é interagindo com os serviços disponíveis.

Neste tutorial vamos realizar o deployment de uma aplicação web em um servidor virtual na nuvem da Amazon Web Services.


## 1. Criando o servidor virtual

Neste laboratório, vamos criar uma máquina virtual para que possamos realizar o deployment de uma aplicação web.

Para isto, logue-se em sua conta da AWS e em `AWS Services` digite `EC2`, Clique em `EC2`:

![acessar ec2](/01-DeployAplicacao/images/acessar_ec2.png)

Feito isto, você será redirecionado para a console do EC2, que é o serviço responsável por entregar máquinas virtuais na cloud da AWS. Nesta tela, clique em `Launch Instance`:

![launch instance](/01-DeployAplicacao/images/launch_instance.png)

Agora, vamos selecionar o sistema operacional a ser utilizado em nossa imagem. Neste caso, selecione a primeira opcão `Amazon Linux 2 AMI`:

![select operating system](/01-DeployAplicacao/images/select_operating_system.png)

A próxima página permite que selecionemos o tipo de máquina virtual. Neste caso, o tipo está associado à quantidade de memória e CPU disponibilizados em nosso servidor. Vamos mater a opção `t2.micro` selecionada. Esta máquina será entregue com 1 vCPU e 1 GiB de memória RAM. Após selecionar o tipo `t2.micro`, clique em `Next: Configure Instance Details` no canto inferior direito da página:

![select instance type](/01-DeployAplicacao/images/instance_type.png)


Nesta tela, clique em `Next: Add Storage` no canto direito inferior da tela:

![instance details](/01-DeployAplicacao/images/instance_details.png)

Na próxima tela, clique apenas em `Next: Add Tags`:

![add storage](/01-DeployAplicacao/images/add_storage.png)

Agora, clique em `click to add a Name tag` no centro da página. Note que este é um like, que irá permitir a criação de uma `Tag` com um nome para nosso servidor apache. Isto irá nos ajudar a identificar nosso servidor na console da AWS posteriormente. Vamos nomear nosso servidor `web-app-server`, e em seguida, clicar em `Next: Configure Security Group` no canto direito inferior da tela:

![add tags](/01-DeployAplicacao/images/add_tags.png)

Agora, vamos realizar a configuração de firewall necessária para que possamos acessar o nosso servidor apache através da porta 80 (HTTP). No passo 6, mantenha selecionada a opção `Create a **new** security group`. Para o nome de nosso security group, vamos utilizar `web-app-server`. Insira o mesmo texto também na descrição.

Altere a regra do seu security group para que a regra que permita o tráfego `SSH` permita acesso apenas a partir de seu endereço IP selecionando `My IP` em `Source` e adicione uma nova regra permitindo o tráfeto HTTP clicando em `Add Rule`. Em seguida, clique em `Review and Launch`, no canto inferior direito da tela:

![configure sg](/01-DeployAplicacao/images/configure_sg.png)

Na próxima tela, revise todas as suas configurações, e clique em `Launch`, no canto inferior direito.

Note que ao clicar em `Launch`, será apresentada uma tela para que você selecione um certificado. Nesta tela, selecione `Create a new key pair` e insira um nome para o seu certificado. Para este laboratório, vamos utilizar o padrão `seu_nome-mba-fiap`. Após preencher o nome, clique em `Download Key Pair` e salve o arquivo em algum lugar seguro.

>Note que o download deste key pair só é apresentado uma vez, durante a sua criação. Caso você perca este arquivo, você não conseguirá mais acessar suas instâncias através de SSH, portanto salve o mesmo em algum lugar seguro.

Após isto, clique em `Launch Instances`:

![create key pair](/01-DeployAplicacao/images/create_key_pair.png)

Neste ponto, seu servidor será provisionado. Clique então em `View Instances` para verificar o status de seu servidor:

![launch status](/01-DeployAplicacao/images/launch_status.png)

Você será redirecionado novamente para a página do EC2, no entanto agora existirá uma instância em execução. O próximo passo, é obter os dados de hostname ou endereço IP público da instância para podermos realizar acesso remoto ao servidor e colocarmos nossa aplicação no ar.

Os dados de endereço IP e hostname estão disponíveis na console do EC2:

![get public dns](/01-DeployAplicacao/images/get_public_dns.png)

## 2. Acessando a instância e realizando o deployment da aplicação

O próximo passo para colocarmos nossa aplicação no ar, é realizarmos acesso remoto via SSH ao servidor que acabamos de provisionar. Caso esteja utilizando um computador com sistema operacional Windows, siga os seguinte passos para realizar acesso ao servidor:

https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/putty.html

>NOTE: Você deverá seguir este procedimento à risca para realizar o acesso via SSH à instância em questão. Os demais procedimentos deverão ser executados todos a partir do terminal do Putty conectado à instância.

### 2.1 Configurando o ambiente - Instalação do Git

O primeiro passo para a configuração do ambiente em seu servidor virtual é a atualização dos repositórios. Para isto, execute o seguinte comando:

    sudo yum update -y

Ao final, você deverá visualizar a informação de que a instalação foi realizada com sucesso:

    Complete!

Agora, vamos instalar o Git e realizar o clone do repositório para a máquina local. Para isto, utilize o seguinte comando:

    sudo yum install git -y

Instale o Node utilizando os seguintes comandos

O próximo passo, é realizar o clone do repositório com a aplicação através do comando:

    git clone https://github.com/bemer/cloud-computing-e-sre.git


### 2.2 Instalação do Node

Instale o gerenciador de versão do nó (nvm, do inglês "node version manager") digitando o seguinte na linha de comando:

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash

Ative o nvm utilizando o seguinte comando:

    . ~/.nvm/nvm.sh

Use o nvm para instalar a versão do Node.js que você pretende usar digitando o seguinte na linha de comando:

    nvm install 4.4.5

Verifique se o Node.js está instalado e funcionando corretamente ao digitar o seguinte na linha de comando:

    node -e "console.log('Running Node.js ' + process.version)"

### 2.3 Build da aplicação

Para realizar o build, vamos mover a pasta com o código da aplicação para uma nova localidade através do seguinte comando:

    cp -r cloud-computing-e-sre/00-Application/app/ ~/

Agora, acesse o diretório em que sua aplicação se encontra:

    cd ~/app/

Realize a instalação do `Gulp` a partir do seguinte comando:

    npm install --global gulp && npm install gulp

>NOTA: O Gulp é um kit de ferramentas utilizado para automatizar tasks em seu workflow de desenvolvimento. Caso queira mais informações sobre esta ferramenta, acesse o seguinte link: https://gulpjs.com/

Após instalar o Gulp, execute o seguinte comando:

    npm install

### 2.4 Instalação do Web Server

Agora que realizarmos o build de nossa aplicação, vamos realizar a instalação de um web server para podermos publicá-la. Neste laboratório, utilizaremos o Nginx para esta finalidade. Para instalar o Nginx, utilize o comando:

    sudo amazon-linux-extras install nginx1.12 -y

Inicie o Nginx através do seguinte comando:

    sudo service nginx start

### 2.5 Deploy da aplicação

Copie o conteúdo de sua aplicação web para o diretório padrão utilizado pelo Nginx através do seguinte comando:

    sudo cp -r ./ /usr/share/nginx/html

## 3. Validando a instalação

Caso tudo tenha ocorrido bem, você deverá estar apto a acessar a sua aplicação através do hostname ou endereço IP público da sua instância. Para validar o funcionamento, abra uma nova aba em seu browser e acesse o servidor utilizando os dados previamente obtidos. Você deverá visualizar a sua aplicação web:

![validate access](/01-DeployAplicacao/images/validate_access.png)

## 4. Limpando o ambiente

Após realizar o laboratório, certifique-se de deletar a instância EC2 provisionada em sua conta para que não ocorram gastos em seu cartão de crédito. Para isto, navegue até a console do serviço EC2 através da URL https://console.aws.amazon.com/ec2/v2/home.

Nesta tela, clique em `Running Instances` e selecione a instancia previamente criada, clique em `Actions`, `Instance State` e `Terminate`:

![terminate instance](/01-DeployAplicacao/images/terminate_instance.png)
