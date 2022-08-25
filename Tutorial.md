# Tutorial de como criar um ambiente de rede com máquinas virtuais

#### O seguinte tutorial é dividido em 7 partes, a fim de que seja possível detalhar de maneira pontual as etapas necessárias para construir um ambiente de rede com 8 máquinas virtuais.
&nbsp;

### Configuração de Hardware das VMs

- Sistema Operacional: Ubuntu;
- Processadores: 1;
- Memória RAM: 512 MB;

### Softwares

- VirtualBox;

### Instrumentos
- Switch;
- 04 cabos de par trançado.

&nbsp;

### Denominações

- Os endereços IPs das nossas máquinas virtuais foram 192.168.13.65, 192.168.13.66, 192.168.13.67, 192.168.13.68, 192.168.13.69, 192.168.13.70, 192.168.13.71, 192.168.13.72;
- Criamos 04 usuários em cada máquina virtual com os nomes "julia", "eduarda", "antony", "beatriz" ou "jfo", "agmm", "bss" e "mebl".

> Outras denominações podem ser visualizadas através da tabela, acessando-a com o comando ```sudo nano /etc/hosts```.

&nbsp;

### PARTE I - Configuração para instalação das máquinas virtuais

&nbsp;

1. Com os terminais abertos, entramos com usuários predefinidos, a partir do comando ```su nome_usuario``` (Ou crie um novo usuário com o comando ```sudo adduser nome_usuario```). 

* O comando "su" permite que possamos mudar de usuário.

&nbsp;

2. Criamos no root uma pasta com um nome destinado a nossa rede de computadores - trabredes - e, dentro deste diretório, criamos outras pastas com um nível hierárquico, a fim de armazenar organizadamente todos os dados necessários (A quantidade de hierarquizações é facultativa). Esses procedimentos devem ser feitos através dos comandos abaixo:

```
Sudo mkdir /pasta1    
cd /pasta1             
sudo mkdir /pasta2     
cd /pasta2
sudo mkdir /pasta3
cd /
sudo mkdir pasta1/pasta4
sudo mkdir pasta1/pasta4/pasta5
sudo mkdir pasta1/pasta4/pasta5/pasta6
```

* O "sudo" é usado para que possamos efetivar comandos como se fosse o superusuário ou outro usuário.
* O "mkdir" é usado para criar diretórios.
* O "cd" é usado para mudar o diretório do trabalho.

&nbsp;

3. Em seguida, a fim de alterar as permissões necessárias para o acesso de arquivos e pastas, digitamos os comandos seguintes:

```
Sudo chown –R nobody:nome_usuario /pasta1
Sudo chgrp –R nome_usuario /pasta1
Sudo chmod –R 771 /pasta1
```

* O comando "chown" permite que possamos alterar o nome do dono de um arquivo.
* O "chgrp" permite que mudemos o nome de um arquivo.
* O "chmod 771" altera as permissões de um arquivo, de forma a ser possível que o dono possa ler, escrever e executar o arquivo.

&nbsp;

4. Logo após, entre no diretório que criou /pasta1/pasta2/pasta3 e aloque o arquivo ubuntu-server-mini.ova (exportação de máquinas virtuais).
Como em nossa exemplificação já tínhamos este arquivo, mas presente em outros diretórios, apenas copiamos e transferimos ele para os diretórios que estávamos utilizando, através do comando: ```sudo cp diretorio_arquivo diretorio_localCopiar```.

* O comando "cp" faz cópia de arquivos.

&nbsp;

### PARTE II – Instalação e configuração das máquinas virtuais

&nbsp;

1. Nessa ordem, deve-se iniciar a etapa de instalar a extensão do pacote do VirtualBox com o comando ```sudo apt install virtualbox-ext-pack```.

* O "apt" permite a instalação, atualização e remoção de pacotes.

&nbsp;

2. Em prosseguimento, deve ser feita a instalação das máquinas virtuais em cada computador. Em nossa exemplificação, construímos uma rede com 8 máquinas virtuais em 4 computadores, sendo 2 máquinas virtuais para cada computador. Para isso, com o VirtualBox aberto clique em “Arquivo” e selecione a opção de importar appliance. Configure sua máquina virtual com as configurações de hardware de sua preferência.

&nbsp;

Coloque como origem do arquivo o diretório criado /pasta1/pasta2/pasta3/ubunto-server-mini.ova e armazene sua máquina no diretório /pasta1/pasta4/pasta5/pasta6

&nbsp;

3. Reinicializamos as Vms e configuramos estaticamente o endereço IP. Para isso, instalamos as ferramentas de rede necessárias com o comando ```sudo apt installnet-tools -y```, posteriormente rodamos o comando: ```sudo nano /etc/netplan/01-netcfg.yaml```, desativando o dhcp4 e definindo os endereços IP e do gateway4. Preste atenção com a indentação do texto e, após a finalização das configurações, aplique as alterações com o comando ```sudo netplan apply```.

* Esse último comando tem por função aplicar os arquivos de configuração.

&nbsp;

4. Desligamos as máquinas virtuais e configuramos nas configurações dessas para que as VMs usassem a mesma rede interna (nomeie essa rede com o nome que preferir - em nossa prática usamos o nome "trabredes").

&nbsp;

5. Por fim, testamos a conexão entre as nossas VMs criadas com o comando “ping” e o endereço IP da máquina que queríamos fazer a conexão. Caso algum erro ocorra, verifique a efetivação dos passos anteriores, tais como definição estática de endereços, desativação do dhcp4, os endereços MACs que precisam ser únicos, entre outros.

* O comando ping usa o datagrama Echo Request do protocolo ICMP (Protocolo de Mensagens de Controle da Internet) a fim de testar conexão entre máquinas.

&nbsp;

### PARTE III - Ambiente de uma rede ponto a ponto física 

&nbsp;

1. Certifique-se de que os endereços IPs estão configurados corretamente e que os endereços MAC são diferentes, caso não sejam, atribua às maquinas endereços MACs aleatórios através das configurações de cada máquina (lembre-se de desligá-las).

&nbsp;

2. Em seguida, configuramos as NICs como modo bridge através das configurações das VMs.

&nbsp;

3. Pingue os endereços para testar a conectividade entre as máquinas.

&nbsp;

### PARTE IV - SSH-Server

&nbsp;

1. Primeiramente, fizemos a associação dos hostnames nas nossas VMs, com o comando: ```sudo hostnamectl set-hostname nome_hostname```.

* Esse comando define o nome do host do sistema.

&nbsp;

2. Para a instalação do servidor SSH é necessário conexão com a internet, logo alteramos a configuração das VMs no adaptador1 para modo NAT.

&nbsp;

3. Comentamos o endereço IP estático e o gateway e ativamos o dhcp, usando o comando ```sudo nano /etc/netplan/01-netcfg.yaml```. Lembre-se de aplicar as alterações com ```sudo netplan apply```.

&nbsp;

4. Digitamos os comandos ```sudo apt update``` e ```sudo apt upgrade -y``` para confirmar a conexão com a internet.

> OBS: O update baixa e o upgrade instala.

&nbsp;

5. A fim de instalar o SSH server digitamos os seguintes comandos ```sudo apt-get install openssh-server``` e ```systemctl status ssh```.

* O SSH permite que os usuários possam realizar o acesso e alterações em servidores remotamente.

&nbsp;

6. Verificamos, em seguida, os status das portas do sistema com o comando ```netstat -an | grep LISTEN```. As conexões TCP na porta 22 devem estar como LISTENING.

* O comando "grep" procura padrões em um arquivo.

&nbsp;

7. Configuramos o firewall para permitir as conexões com os comandos abaixo:

```
sudo ufw allow ssh
sudo ufw status
sudo ufw enable
```

* O "allow" permite as conexões.
* O "enable" habilita o ufw.

> O ufw é uma interface que simplifica a configuração do firewall.

&nbsp;

8. Confirmação dos passos feitos anteriormente. Para conectarmos nossas máquinas remotamente digitamos o comando ```ssh user@ip_remoto``` usando o ip das máquinas que queríamos realizar a conexão e o user que estava sendo usado.

&nbsp;

### PARTE V - Acesso Remoto SSH com Host-Only

&nbsp;

1. Adicionamos em uma de nossas máquinas virtuais de cada computador um novo adaptador. Para isso, primeiro é necessário clicar em Arquivo e depois em Host Network Manager, em seguida adicionar um novo adaptador em modo Host-only.

&nbsp;

2. Ligamos a VM com o novo adaptador e digitamos o ```ifconfig -a``` para confirmar a existência do adaptador 2, observando a presença de "enp08s”. Logo após, com o comando ```sudo nano /etc/netplan/01-netcfg.yaml``` ativamos o dhcp do segundo adaptador. Lembre-se de aplicar com ```sudo netplan apply```.

&nbsp;

3. Por fim, novamente realize a conexão com ```ssh user@ip_remoto```.

&nbsp;

### PARTE VI - Configuração dos Nomes associados

1. Abrimos o arquivo /etc/hosts e editamos-o com as denominações de todas as VMs da rede com o comando ```sudo nano /etc/hosts```.

&nbsp;

2. Acessamos de forma remota nossas VMs nas diferentes máquinas usando as novas denominações configuradas, com o comando: ```ssh user@hostname|FQDN|alias|IP```.

&nbsp;

### PARTE VII - Conectando os computadores através de um switch

&nbsp;

1. Nesta etapa, conectamos por fim todos os computadores através dos switchs e dos cabos de par trançado, testando logo em seguida os pings e ssh de uma das máquinas para as outras e usando os usuários que criamos.

&nbsp;

#### Após todos os passos terem sido realizados, e se validados, a nossa rede de computadores estava pronta.
