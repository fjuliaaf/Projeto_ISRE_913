# Tutorial de como criar um ambiente de rede com máquinas virtuais

#### O seguinte tutorial é dividido em 7 partes, a fim de que seja possível detalhar de maneira pontual as etapas necessárias para construir um ambiente de rede com 8 máquinas virtuais.
&nbsp;
- PARTE I - Configuração para instalação das máquinas virtuais

&nbsp;

1. Com os terminais abertos, entre com usuários predefinidos, a partir do comando ```su nome_usuario```.

&nbsp;

2. Crie no root uma pasta com um nome destinado a sua rede de computadores e, dentro deste diretório, crie outras pastas com um nível hierárquico, a fim de armazenar organizadamente todos os dados necessários (A quantidade de hierarquizações é facultativa). Esses procedimentos devem ser feitos através dos comandos abaixo:

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

&nbsp;

3. Em seguida, a fim de alterar as permissões necessárias para o acesso de arquivos e pastas, digite os comandos seguintes:

```
Sudo chown –R nobody:nome_usuario /pasta1
Sudo chgrp –R nome_usuario /pasta1
Sudo chmod –R 771 /pasta1
```

&nbsp;

4. Logo após, entre no diretório que criou /pasta1/pasta2/pasta3 e aloque o arquivo ubuntu-server-mini.ova (exportação de máquinas virtuais).
Como em nossa exemplificação já tínhamos este arquivo, mas presente em outros diretórios, apenas copiamos e transferimos ele para os diretórios que estávamos utilizando, através do comando: ```sudo cp diretorio_arquivo diretorio_localCopia```.

&nbsp;

- PARTE II – Instalação e configuração das máquinas virtuais

&nbsp;

1. Nessa ordem, deve-se iniciar a etapa de instalar a extensão do pacote do VirtualBox com o comando ```sudo apt install virtualbox-ext-pack```.

&nbsp;

2. Em prosseguimento, deve ser feita a instalação das máquinas virtuais em cada computador. Em nossa exemplificação, construímos uma rede com 8 máquinas virtuais em 4 computadores, sendo 2 máquinas virtuais para cada computador. Para isso, com o VirtualBox aberto clique em “Arquivo” e selecione a opção de importar appliance. Configure sua máquina virtual com as configurações de hardware de sua preferência.

&nbsp;

Coloque como origem do arquivo o diretório criado /pasta1/pasta2/pasta3/ubunto-server-mini.ova e armazene sua máquina no diretório /pasta1/pasta4/pasta5/pasta6

&nbsp;

3. Reinicialize as Vms e configure estaticamente o endereço IP. Para isso, instale as ferramentas de rede necessárias com o comando ```sudo apt installnet-tools -y```, posteriormente rode o comando: ```sudo nano /etc/netplan/01-netcfg.yaml```, desative o dhcp4 e defina os endereços IP e do gateway4. Preste atenção com a indentação do texto e, após a finalização das configurações, aplique as alterações com o comando ```sudo netplan apply```.

&nbsp;

4. Desligue as máquinas virtuais e configure nas configurações dessas para que as VMs usem a mesma rede interna (nomeie essa rede com o nome que preferir).

&nbsp;

5. Por fim, teste a conexão entre as suas VMs criadas com o comando “ping” e o endereço IP da máquina que se deseja fazer a conexão. Caso algum erro ocorra, verifique a efetivação dos passos anteriores, tais como definição estática de endereços, ativação/desativação do dhcp4, os endereços MACs que precisam ser únicos, entre outros.

&nbsp;

- PARTE III - Ambiente de uma rede ponto a ponto física 

&nbsp;

1. Certifique-se de que os endereços IPs estão configurados corretamente e que os endereços MAC são diferentes, caso não sejam, atribua às maquinas endereços aleatórios através das configurações de cada máquina (lembre-se de desligá-las).

&nbsp;

2. Configure as NICs como modo bridge através das configurações das VMs.

&nbsp;

3. Pingue os endereços para testar a conectividade entre esses PCs.

&nbsp;

- PARTE IV - SSH-Server

&nbsp;

1. Primeiramente, faça a associação dos hostnames nas suas VMs, com o comando: ```sudo hostnamectl set-hostname nome_hostname```.

&nbsp;

2. Para a instalação do servidor SSH é necessário conexão com a internet, logo altere a configuração das VMs no adaptador1 para modo NAT.

&nbsp;

3. Comente o endereço IP estático e ative o dhcp, usando o comando ```sudo nano /etc/netplan/01-netcfg.yaml``` e lembre-se de aplicar as alterações com ```sudo netplan apply```.

&nbsp;

4. Digite os comando ```sudo apt update``` e ```sudo apt upgrade -y``` para confirmar a conexão com a internet.

&nbsp;

5. A fim de instalar o SSH server digite os seguintes comandos ```sudo apt-get install openssh-server``` e ```systemctl status ssh```.

&nbsp;

6. Verifique os status das portas do sistema com o comando ```netstat -an | grep LISTEN```. As conexões TCP na porta 22 devem estar como LISTENING.

&nbsp;

7. Configure o firewall para permitir as conexões com os comandos abaixo:

```
sudo ufw allow ssh
sudo ufw status
sudo ufw enable
```

&nbsp;

8. Confirmação dos passos feitos anteriormente. Para conectar sua máquina com outra remotamente digite o comando ```ssh user@ip_remoto``` usando o ip da máquina que deseja se conectar e o user que está sendo usado.

&nbsp;

- PARTE V - Acesso Remoto SSH com Host-Only

&nbsp;

1. Adicione em uma de suas máquinas virtuais um novo adaptador. Primeiro clique em Arquivo e depois em Host Network Manager, em seguida adicione um novo adaptador em modo Host-only.

&nbsp;

2. Ligue a VM com o novo adaptador e digite o ```ifconfig -a``` para confirmar a existência do adaptador 2, observando a presença de "enp08s”. Logo após, com o comando ```sudo nano /etc/netplan/01-netcfg.yaml``` ative o dhcp do segundo adaptador. Lembre-se de aplicar com ```sudo netplan apply```.

&nbsp;

3. Por fim, novamente realize a conexão com ```ssh user@ip_remoto```.

&nbsp;

- PARTE VI - Configuração dos Nomes associados

1. Abra o arquivo /etc/hosts e edite-o com as denominações de todas as VMs da rede com o comando ```sudo nano /etc/hosts```.

&nbsp;

2. Acesse de forma remota outras VMs usando as novas denominações configuradas, com o comando: ```ssh user@hostname|FQDN|alias|IP```.

&nbsp;

- PARTE VII - Conectando os computadores através de um switch

&nbsp;

1. 

&nbsp;

### Configuração de Hardware das VMs

- Sistema Operacional: Ubuntu;
- Processadores: 1;
- Memória RAM: 512 MB;

&nbsp;

### Link do tutorial com as imagens de cada etapa, incluindo testes de ping e ssh

[Tutorial com imagens das etapas](https://docs.google.com/document/d/1TM0-MaSALltTjzLK44p3Tm8edg08KEIeAwbOhXyEcOs/edit)


#### Após todos os passos terem sido realizados, e se validados, a sua rede de computadores está pronta.
