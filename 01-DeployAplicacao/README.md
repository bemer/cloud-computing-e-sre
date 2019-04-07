# Instalando uma aplicação na Nuvem Pública

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
