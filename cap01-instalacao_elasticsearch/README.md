# ELASTIC SEARCH - CONTAINERS LAB

## Montando o ambiente

### Instalando o Virtualbox
* Instale no Virtualbox: https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe
* No Virtualbox, crie uma rede virtual no menu Arquivo, Preferências, Rede, botão "Acrescentar uma nova rede NAT" com um nome a sua escolha.

### Instalando o Ubuntu Desktop
* Puxe a .iso do Ubuntu Desktop do link: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso
* Crie uma VM no virtualbox do tipo Linux - Ubuntu(64-bit) com pelo menos 12Gb RAM e 80Gb de disco
* Nas configurações de rede da VM, escolha a rede NAT criada anteriormente.
* Vá no menu da máquina virtual, acesse "Dispositivos" -> "Discos óticos" -> "Escolher uma imagem de disco". Aponte para o arquivo ubuntu-22-xxx.iso.
* Inicie a VM para iniciar a instalação

### Instalando o Docker no Ubuntu 22.04

Acesse a máquina virtual e execute os comandos abaixo, um por vez:

    $ sudo apt-get remove docker docker-engine docker.io containerd runc
    $ sudo apt-get -y update
    $ sudo apt-get install ca-certificates curl gnupg lsb-release
    $ sudo mkdir -p /etc/apt/keyrings
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    $ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    $ sudo usermod -aG docker $USER
    $ sudo systemctl enable docker
    $ sudo systemctl start docker
    $ docker -v
    $ docker compose version
    
### Definindo a memória virtual

    $ sudo sysctl -w vm.max_map_count=262144
    $ sudo su -
    $ echo "vm.max_map_count=262144" >> /etc/sysctl.conf
    $ su - seu_usuario

### Preparando os arquivos docker-compose e .env que criarão o cluster
    $ mkdir ~/elastic && cd ~/elastic
    $ wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/docker-compose.yml
    $ wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/.env
    $ docker compose up -d

Depois de instalado o cluster, abra o navegador web acesse a URL do KIBANA no endereço http://localhost:5601 com o usuário: **elastic** e senha: **elastic123456**


## Como atualizar o cluster

Quando estiver disponível uma nova versão do Elastic, atualize-o com os seguintes procedimentos.

    $ docker compose down

Editar o valor da variável STACK_VERSION do arquivo .env para a última versão disponível do Elastic.

Exemplo: STACK_VERSION="8.4.1"


    $ docker compose up -d


## Como remover o LAB e TODOS os dados

Para remover todos os containers e seus respectivos volumes, execute o comando abaixo.
   
    $ docker compose down -v