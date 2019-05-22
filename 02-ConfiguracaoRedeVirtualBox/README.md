# Configuração de Rede no Virtual Box

Para que possamos executar os laboratórios no laboratório da FIAP, é importante realizarmos uma pequena alteração nas configurações de rede das máquinas virtuais, uma vez que precisamos fazer com que as duas máquinas virtuais utilizadas nos próximos passos conversem entre si e sejam acessíveis a partir de nosso computador, vamos realizar os seguintes passos.

## 1. Criar uma rede NAT

Na interface do VirtualBox, clique em `Arquivo` e em seguida em `Preferências`. Feito isto, clique na opção `Rede` no menu lateral esquerdo:

![network settings](/02-ConfiguracaoRedeVirtualBox/images/network_settings.png)

Feito isto, clique no botão com o sinal de `+` no canto direito desta tela, para criarmos uma nova rede NAT. Em seguida, clique em OK:

![new nat network](/02-ConfiguracaoRedeVirtualBox/images/new_nat_network.png)

## 2. Configurando as máquinas virtuais

Após criar a rede NAT, selecione a máquina virtual `jenkins` na interface inicial do VirtualBox e clique em `Configurações`.

![server settings](/02-ConfiguracaoRedeVirtualBox/images/server_settings.png)

Clique então em `Rede` e selecione a opção `Rede NAT`. Ao finalizar esta configuração, clique em OK:

![select nat network](/02-ConfiguracaoRedeVirtualBox/images/select_nat_network.png)


## 3. Obtendo os endereços IP

Quando configurarmos a interface de rede para utilizar a opção `Rede NAT` e iniciarmos as máquinas virtuais, o VirtualBox irá entregar um endereço IP para cada uma das máquinas virtuais utilizando um DHCP Server próprio. Devido a isto, precisaremos obter o endereço IP entregue a cada uma das máquinas virtuais para que possamos então configurar o encaminhamento de portas.

Para obter o endereço IP, inicie o `jenkins` e através da interface do VirtualBox acesse o mesmo utilizando as credenciais:

    Usuário: jenkins
    Senha: jenkins01

Após efetuar login no sistema operacional, execute o comando:

    ifconfig enp0s3

A saída do comando deverá ser semelhante a:

    enp0s3    Link encap:Ethernet  HWaddr 08:00:27:b0:90:09  
              inet addr:10.0.2.4  Bcast:10.0.2.255  Mask:255.255.255.0
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:341 errors:0 dropped:0 overruns:0 frame:0
              TX packets:294 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:287987 (287.9 KB)  TX bytes:32422 (32.4 KB)

Note que o endereço IP do servidor é exibido logo na segunda linha da saída do comando logo após `ined addr:`. Em nosso exemplo, o endereço IP é `10.0.2.4`.

> Note que o endereço IP de sua máquina virtual pode ser diferente do endereço apresentado no exemplo anterior.

Tome nota do endereço IP do `jenkins`.


## 4. Configurando o Encaminhamento de Portas

Após obter os endereços IP atribuídos via DHCP a ambas as máquinas virtuais, vamos realizar a configuração do encaminhamento de portas, para que possamos acessar as VM's utilizando o putty.

Para isto, na console do VirtualBox clique em `Arquivo`, `Preferências` e selecione a opção `Rede` no menu lateral esquerdo. Nesta tela, a nossa rede NAT será exibida. Clique então no terceiro botão na lateral direita para editar as configurações:

![nat configurations](/02-ConfiguracaoRedeVirtualBox/images/nat_configurations.png)

Em seguida, clique em `Encaminhamento de portas`:

![port forwarding](/02-ConfiguracaoRedeVirtualBox/images/port_forwarding.png)

Vamos então clicar no sinal de `+` no lado direito da tela e adicionar os mapeamentos. Deveremos criar os seguintes mapeamentos de porta:

| Porta do Host | IP do Convidado   | Porta do Convidado |
|---------------|-------------------|--------------------|
| 22            | IP do jenkins     | 22                 |
| 8080            | IP do jenkins   | 8080               |
| 5000           | IP do jenkins    | 5000               |

Conforme a imagem abaixo:

![port forwarding rules](/02-ConfiguracaoRedeVirtualBox/images/port_forwarding_rules.png)

Após salvar todas as configurações, vamos realizar o download e instalação do `Putty` para que possamos realizar acesso remoto ao servidor virtual e executar os comandos de uma forma melhor. Para isto, realize o download do Putty através do seguinte link:

    https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe

Ao executar o Putty, vamos realizar as configurações para acesso ao servidor do Jenkins. Lembre-se que o servidor virtual estará utilizando uma rede própria do Virtual Box e nós utilizaremos configurações de redirecionamento de portas para acesso ao mesmo, portanto configure o Putty da seguinte maneira:

    Host Name (or IP Address): jenkins@localhost
    Port: 22

Após realizar estas configurações, preencha o campo `Saved Sessions` com o nome `jenkins` e clique em `Save`. Isto irá facilitar o acesso ao servidor posteriormente caso a sessão seja encerrada por alguma razão:

![putty access](/02-ConfiguracaoRedeVirtualBox/images/putty_access.png)

Para realizar acesso ao servidor onde vamos realizar a instalação do Jenkins, selecione o mesmo em `Save Sessions` e clique em `Open`. Você se conectará ao servidor e o mesmo irá solicitar a senha para acesso. Neste caso, utilize a senha `jenkins01`.
