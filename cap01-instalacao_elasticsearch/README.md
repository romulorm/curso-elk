# Instalação do Elastic + Kibana

Serão apresentados dois tipos de instalação do Elastic: 
1) Instalação Single-node utilizando os binários para Windows;
2) Instalação em cluster utilizando conteineres em máquina Linux.

# A) MS-WINDOWS LAB

## Montando o ambiente no Microsoft Windows

* Puxe a versão 8.4.3 do Elastic em:
https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.4.3-windows-x86_64.zip

* Descompacte na raiz do disco C: ou D:
* Com o bloco de notas, inclua no final do arquivo **config/elasticsearch.yml** o texto abaixo, não deixando espaços em branco no início das linhas:

~~~Parâmetros
cluster.name: elastic-local
discovery.type: single-node
network.host: localhost
http.port: 9200
~~~

* Com o bloco de notas, descomente os parâmetros abaixo no arquivo **config/jvm.options**, retirando os # e os espaços e ajustando de acordo com a quantidade de memória que pretende disponibilizar ao JVM. No exemplo abaixo será utilizado 4Gb de memória RAM para a JVM.

~~~Parâmetros
-Xms4g
-Xmx4g
~~~

* Execute o arquivo **bin/elasticsearch.bat**.
* Em outro prompt de comando, entre no diretório /bin da sua instalação do elastic. Exemplo: **cd C:\elasticsearch-8.4.3\bin**
* Execute o comando: **elasticsearch-reset-password -i -u elastic**
* Confirme teclando a letra **Y**;
* Defina a senha do usuário **elastic** para **elastic123456**
* Agora vamos utilizar o mesmo comando para alterar a senha do usuário kibana_system
* Execute o comando: **elasticsearch-reset-password -i -u kibana_system**
* Confirme teclando a letra **Y**;
* Defina a senha do usuário **kibana_system** para **kibana123456**

* Puxe a versão 8.4.3 do Kibana em:
https://artifacts.elastic.co/downloads/kibana/kibana-8.4.3-windows-x86_64.zip

* Descompacte na raiz do disco C: ou D:
* Com o bloco de notas, inclua no final do arquivo  **config/kibana.yml** o texto abaixo, não deixando espaços em branco no início das linhas:

~~~Parâmetros
server.port: 5601
server.host: "localhost"
server.maxPayload: 100000000
elasticsearch.hosts: ["https://localhost:9200"]
elasticsearch.username: "kibana_system"
elasticsearch.password: "kibana123456"
elasticsearch.requestTimeout: 120000
elasticsearch.ssl.verificationMode: none
~~~

# B) CONTAINERS LAB

## Montando o ambiente

Caso já tenha uma máquina Ubuntu Linux, avance para o passo 3.

### 1) Instalando o Virtualbox
* Instale no Virtualbox: https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe
* No Virtualbox, crie uma rede virtual no menu Arquivo, Preferências, Rede, botão "Acrescentar uma nova rede NAT" com um nome a sua escolha.

### 2) Instalando o Ubuntu Desktop
* Puxe a .iso do Ubuntu Desktop do link: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso
* Crie uma VM no virtualbox do tipo Linux - Ubuntu(64-bit) com pelo menos 12Gb RAM e 80Gb de disco
* Nas configurações de rede da VM, escolha a rede NAT criada anteriormente.
* Vá no menu da máquina virtual, acesse "Dispositivos" -> "Discos óticos" -> "Escolher uma imagem de disco". Aponte para o arquivo ubuntu-22-xxx.iso.
* Inicie a VM para iniciar a instalação

### 3) Instalando o Docker no Ubuntu 22.04

Acesse a máquina virtual e execute os comandos abaixo, um por vez:

~~~shellscript
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get -y update
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
docker -v
docker compose version
sudo apt-get install ca-certificates curl gnupg lsb-release
~~~
    
### 4) Definindo a memória virtual

~~~shellscript
sudo sysctl -w vm.max_map_count=262144
sudo su -
echo "vm.max_map_count=262144" >> /etc/sysctl.conf
su - seu_usuario
~~~

### 5) Preparando os arquivos docker-compose e .env que criarão o cluster
~~~shellscript
mkdir ~/elastic && cd ~/elastic
wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/docker-compose.yml
wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/.env
docker compose up -d
~~~

Depois de instalado o cluster, abra o navegador web acesse a URL do KIBANA no endereço http://localhost:5601 com o usuário: **elastic** e senha: **elastic123456**

---

## Como atualizar o cluster

Quando estiver disponível uma nova versão do Elastic, atualize-o com os seguintes procedimentos.

~~~shellscript
docker compose down
~~~

Editar o valor da variável STACK_VERSION do arquivo .env para a última versão disponível do Elastic.

Exemplo: STACK_VERSION="8.4.1"

~~~shellscript
docker compose up -d
~~~

---

## Como remover o LAB e TODOS os dados

Para remover todos os containers e seus respectivos volumes, execute o comando abaixo.
   
~~~shellscript
docker compose down -v
~~~