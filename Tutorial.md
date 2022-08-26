# Tutorial de como criar um ambiente de rede com máquinas virtuais

Instituto Federal de Alagoas
<br>
Infraestrutura e Serviços de Rede
<br>
Turma: 913   |   Curso: Informática
<br>
Grupo 5 - Júlia Ferreira, Antony Gabriel, Beatriz Santos e Maria Eduarda Lima

&nbsp;

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

![redes1 ](https://user-images.githubusercontent.com/103438145/186777862-c83920e9-2d8b-4632-bbac-e179bfd1ebff.png)

* O comando "su" permite que possamos mudar de usuário.

&nbsp;

2. Criamos no root uma pasta com um nome destinado a nossa rede de computadores - trabredes - e, dentro deste diretório, criamos outras pastas com um nível hierárquico, a fim de armazenar organizadamente todos os dados necessários (A quantidade de hierarquizações é facultativa). Esses procedimentos devem ser feitos através dos comandos abaixo:

```
sudo mkdir /pasta1    
cd /pasta1             
sudo mkdir /pasta2     
cd /pasta2
sudo mkdir /pasta3
cd /
sudo mkdir pasta1/pasta4
sudo mkdir pasta1/pasta4/pasta5
sudo mkdir pasta1/pasta4/pasta5/pasta6
```

![redes3](https://user-images.githubusercontent.com/103438145/186778076-abf96678-ac26-4c18-9ff9-ff7a1a9d4929.png)

* O "sudo" é usado para que possamos efetivar comandos como se fosse o superusuário ou outro usuário.
* O "mkdir" é usado para criar diretórios.
* O "cd" é usado para mudar o diretório do trabalho.

&nbsp;

3. Em seguida, a fim de alterar as permissões necessárias para o acesso de arquivos e pastas, digitamos os comandos seguintes:

```
sudo chown –R nobody:nome_usuario /pasta1
sudo chgrp –R nome_usuario /pasta1
sudo chmod –R 771 /pasta1
```
![redes4](https://user-images.githubusercontent.com/103438145/186778091-db28a867-63d0-4a31-91a5-aeee601a2a8f.png)

![redes7](https://user-images.githubusercontent.com/103438145/186778128-e9e36367-c825-40e5-8b66-5df4228c4e56.png)

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

![redes5](https://user-images.githubusercontent.com/103438145/186778099-f55038bd-cd2d-423d-b676-42a8abf7e80f.png)

* O "apt" permite a instalação, atualização e remoção de pacotes.

&nbsp;

2. Em prosseguimento, deve ser feita a instalação das máquinas virtuais em cada computador. Em nossa exemplificação, construímos uma rede com 8 máquinas virtuais em 4 computadores, sendo 2 máquinas virtuais para cada computador. Para isso, com o VirtualBox aberto clique em “Arquivo” e selecione a opção de importar appliance. Configure sua máquina virtual com as configurações de hardware de sua preferência.

&nbsp;

![redes6](https://user-images.githubusercontent.com/103438145/186778113-c4453d7a-db58-40ea-8c66-9d6390bb407b.png)

Coloque como origem do arquivo o diretório criado /pasta1/pasta2/pasta3/ubunto-server-mini.ova e armazene sua máquina no diretório /pasta1/pasta4/pasta5/pasta6

&nbsp;

3. Reinicializamos as Vms e configuramos estaticamente o endereço IP. Para isso, instalamos as ferramentas de rede necessárias com o comando ```sudo apt installnet-tools -y```, posteriormente rodamos o comando: ```sudo nano /etc/netplan/01-netcfg.yaml```, desativando o dhcp e definindo os endereços IP e do gateway. Preste atenção com a indentação do texto e, após a finalização das configurações, aplique as alterações com o comando ```sudo netplan apply```.

* Esse último comando tem por função aplicar os arquivos de configuração.

&nbsp;

4. Desligamos as máquinas virtuais e configuramos nas configurações dessas para que as VMs usassem a mesma rede interna (nomeie essa rede com o nome que preferir - em nossa prática usamos o nome "trabredes").

![redes8](https://user-images.githubusercontent.com/103438145/186778141-545fb93d-a7ed-4ddd-af0b-43a2593d0e23.png)

&nbsp;

5. Por fim, testamos a conexão entre as nossas VMs criadas com o comando “ping” e o endereço IP da máquina que queríamos fazer a conexão. Caso algum erro ocorra, verifique a efetivação dos passos anteriores, tais como definição estática de endereços, desativação do dhcp4, entre outros.

![redes9](https://user-images.githubusercontent.com/103438145/186778150-1d6e7045-1d31-412d-a5bd-222e54069b5d.png)

* O comando ping usa o datagrama Echo Request do protocolo ICMP (Protocolo de Mensagens de Controle da Internet) a fim de testar conexão entre máquinas.

&nbsp;

### PARTE III - Ambiente de uma rede ponto a ponto física 

&nbsp;

1. Certifique-se de que os endereços IPs estão configurados corretamente e que os endereços MAC são diferentes, caso não sejam, atribua às maquinas endereços MACs aleatórios através das configurações de cada máquina (lembre-se de desligá-las).

![redes10](https://user-images.githubusercontent.com/103438145/186778189-652bf722-48d3-456b-ad7e-51c6ef6ca798.png)

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

![redes11](https://user-images.githubusercontent.com/103438145/186778192-48665374-645d-45ce-9d9d-ff8b162d41c2.png)

* O SSH permite que os usuários possam realizar o acesso e alterações em servidores remotamente.

&nbsp;

6. Verificamos, em seguida, os status das portas do sistema com o comando ```netstat -an | grep LISTEN```. As conexões TCP na porta 22 devem estar como LISTENING.

![redes12](https://user-images.githubusercontent.com/103438145/186778194-4d401a23-23fb-4c10-bb14-a20533dc5514.png)

* O comando "grep" procura padrões em um arquivo.

&nbsp;

7. Configuramos o firewall para permitir as conexões com os comandos abaixo:

```
sudo ufw allow ssh
sudo ufw status
sudo ufw enable
```

![redes13](https://user-images.githubusercontent.com/103438145/186778195-8745b12d-d839-4b9a-adbc-d5c65be0d21b.png)

* O "allow" permite as conexões.
* O "enable" habilita o ufw.

> O ufw é uma interface que simplifica a configuração do firewall.

&nbsp;

8. Confirmação dos passos feitos anteriormente. Para conectarmos nossas máquinas remotamente digitamos o comando ```ssh user@ip_remoto``` usando o ip das máquinas que queríamos realizar a conexão e o user que estava sendo usado.

&nbsp;

### PARTE V - Acesso Remoto SSH com Host-Only

&nbsp;

1. Adicionamos em uma de nossas máquinas virtuais de cada computador um novo adaptador. Para isso, primeiro é necessário clicar em Arquivo e depois em Host Network Manager, em seguida adicionar um novo adaptador em modo Host-only.

![redes14](https://user-images.githubusercontent.com/103438145/186778197-21bfa8dd-6ada-478b-9702-a85b4698af74.png)

&nbsp;

2. Ligamos a VM com o novo adaptador e digitamos o ```ifconfig -a``` para confirmar a existência do adaptador 2, observando a presença de "enp08s”. Logo após, com o comando ```sudo nano /etc/netplan/01-netcfg.yaml``` ativamos o dhcp do segundo adaptador. Lembre-se de aplicar com ```sudo netplan apply```.

&nbsp;

3. Por fim, novamente realize a conexão com ```ssh user@ip_remoto```.

&nbsp;

### PARTE VI - Configuração dos Nomes associados

1. Abrimos o arquivo /etc/hosts e editamos-o com as denominações de todas as VMs da rede com o comando ```sudo nano /etc/hosts```.

![redes16](https://user-images.githubusercontent.com/103438145/186778222-0e38e6ca-4a8b-4a6e-8f1a-37878fcd3e61.png)

&nbsp;

2. Acessamos de forma remota nossas VMs nas diferentes máquinas usando as novas denominações configuradas, com o comando: ```ssh user@hostname|FQDN|alias|IP```.

&nbsp;

### PARTE VII - Conectando os computadores através de um switch

&nbsp;

1. Nesta etapa, conectamos por fim todos os computadores através dos switchs e dos cabos de par trançado, testando logo em seguida os pings e ssh de uma das máquinas para as outras e usando os usuários que criamos.

![r2](https://user-images.githubusercontent.com/103438145/186790501-8ed899f1-e39e-40f0-bf19-925c0424ca98.png)
![r3](https://user-images.githubusercontent.com/103438145/186790667-523311c3-9b4f-40f3-9c8a-8add347a822c.png)
![r1](https://user-images.githubusercontent.com/103438145/186790685-55f0ee9d-6f98-4e65-b22b-6aba92133fd7.png)

&nbsp;

### Validações - Ping e SSH

&nbsp;

* Ping

&nbsp;

![ping1](https://user-images.githubusercontent.com/103438145/186794348-da719839-76d4-4f57-b96b-5211e831d2b4.png)

&nbsp;

![ping10](https://user-images.githubusercontent.com/103438145/186794370-fc0c538e-72cc-4938-842d-32cefc9f7f59.png)

&nbsp;

![ping9](https://user-images.githubusercontent.com/103438145/186794373-e3b67460-8ba3-43bc-9d71-fef241e640bf.png)

&nbsp;

![ping8](https://user-images.githubusercontent.com/103438145/186794377-a6dce1bc-e081-4570-943c-59455dc0620f.png)

&nbsp;

![ping7](https://user-images.githubusercontent.com/103438145/186794383-2e854b07-1ab5-47f7-a5fc-fb26aaace48d.png)

&nbsp;

![ping6](https://user-images.githubusercontent.com/103438145/186794387-de26e34c-ac87-4b97-8c48-41725a6daaf4.png)

&nbsp;

![ping5](https://user-images.githubusercontent.com/103438145/186794390-e2e37b33-7b72-4d1b-8755-d3097f2b4c1c.png)

&nbsp;

![ping4](https://user-images.githubusercontent.com/103438145/186794392-827f2fee-c11d-49cb-b5c1-9b76522dda3e.png)

&nbsp;

![ping3](https://user-images.githubusercontent.com/103438145/186794395-1ccc33d4-acec-4e49-b3d6-330c49e702f9.png)

&nbsp;

![ping2](https://user-images.githubusercontent.com/103438145/186794399-d3cffc15-7aaf-442a-be0e-6e64b449a371.png)

&nbsp;

* SSH

&nbsp;

![ssh6](https://user-images.githubusercontent.com/103438145/186794354-9d50c5e6-084f-4911-8513-c0c513c138a6.png)

&nbsp;

![ssh5](https://user-images.githubusercontent.com/103438145/186794356-acd6b639-a534-4ff5-a97a-f36a19e88012.png)

&nbsp;

![ssh4](https://user-images.githubusercontent.com/103438145/186794357-a531253f-2ebb-40af-ad83-b1a1e9997547.png)

&nbsp;

![ssh3](https://user-images.githubusercontent.com/103438145/186794358-d26b4401-4a60-4aaa-9dad-aa9b47316186.png)

&nbsp;

![ssh2](https://user-images.githubusercontent.com/103438145/186794363-a6c95faf-fc59-430a-a6bc-7afcfa232519.png)

&nbsp;

![ssh1](https://user-images.githubusercontent.com/103438145/186794368-0a2d3de0-2797-4b80-a1bd-1dbc965f05b7.png)

&nbsp;

![índice](https://user-images.githubusercontent.com/103438145/186925265-458daf46-239e-4252-88c7-2e1ac6227b72.jpeg)

&nbsp;

#### Após todos os passos terem sido realizados, e se validados, a rede de computadores está pronta.
