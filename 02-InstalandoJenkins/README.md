# Instalando o Jenkins

Agora que já temos a configuração de rede de nossas máquinas virtuais devidamente efetuadas, vamos passar ao processo de instalação do Jenkins.

Jekins é uma ferramenta de automação open source escrita em Java e será utilizado para gerar novas versões de nossa aplicação automaticamente, sendo o principal recurso para a implementação de *Continuous Integration* em nosso laboratório.

Siga os passos a seguir na ordem exata para realizar a instalação do Jenkins na VM `Jenkins` entregue.

## 01. Instalação do Java

Como o Jenkins é desenvolvido em Java, é necessário realizarmos a instalação do mesmo na máquina virtual. Para tanto, utilize os seguintes comandos:

    sudo apt-get update
    sudo apt-get install openjdk-8-jdk -y

## 02. Adicionando o repositório do Jenkins

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

![unlock jenkins](/02-InstalandoJenkins/images/unlock-jenkins.jpg)
