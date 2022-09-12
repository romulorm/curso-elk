# ELASTIC SEARCH - CONTAINERS LAB

## Instalando o Virtualbox
* Instale no Virtualbox: https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe
* No Virtualbox, crie uma rede virtual no menu Arquivo, Preferências, Rede, botão "Acrescentar uma nova rede NAT" com um nome a sua escolha.

## Instalando o Ubuntu Desktop
* Puxe a .iso do Ubuntu Desktop do link: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso
* Crie uma VM no virtualbox do tipo Linux - Ubuntu(64-bit) com pelo menos 12Gb RAM e 80Gb de disco
* Nas configurações de rede da VM, escolha a rede NAT criada anteriormente.
* Vá no menu da máquina virtual, acesse "Dispositivos" -> "Discos óticos" -> "Escolher uma imagem de disco". Aponte para o arquivo ubuntu-22-xxx.iso.
* Inicie a VM para iniciar a instalação

## Instalando o Docker no Ubuntu 22.04

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
    
## Definindo a memória virtual

    $ sudo sysctl -w vm.max_map_count=262144
    $ sudo su -
    $ echo "vm.max_map_count=262144" >> /etc/sysctl.conf
    $ su - seu_usuario

## Preparando os arquivos docker-compose e .env que criarão o cluster
    $ mkdir ~/elastic && cd ~/elastic
    $ wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/docker-compose.yml
    $ wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/.env
    $ docker compose up -d

Depois de instalado o cluster, abra o navegador web acesse a URL http://localhost:5601 com o usuário: **elastic** e senha: **elastic123456**

## Importando os datasets para o Elastic

### Criando os índices CRIMESSP e MOVIE

Copie o conteúdo dos arquivos abaixo, um por vez, no Dev Tools do Elastic, e aplique.

CRIMESSSP: https://github.com/romulorm/elk-docs/raw/master/datasets/crimessp.txt

MOVIE: https://github.com/romulorm/elk-docs/raw/master/datasets/movie.txt

### Importando as informações para os índices

Dentro da pasta ~/elastic do servidor, faça o download dos bancos de dados em formato json e importe no Elastic:

    $ wget https://github.com/romulorm/elk-docs/raw/master/datasets/accounts.json && curl -u elastic:elastic123456 -k -H "Content-Type: application/json" -X PUT "https://localhost:9200/banco/_bulk?pretty&refresh" --data-binary "@accounts.json"
    
    $ wget https://github.com/romulorm/elk-docs/raw/master/datasets/crimessp-data.json && curl -u elastic:elastic123456 -k -H "Content-Type: application/json" -X PUT "https://localhost:9200/crimessp/_bulk?pretty&refresh" --data-binary "@crimessp-data.json"
    
    $ wget https://github.com/romulorm/elk-docs/raw/master/datasets/movie-data.json && curl -u elastic:elastic123456 -k -H "Content-Type: application/json" -X PUT "https://localhost:9200/movie/_bulk?pretty&refresh" --data-binary "@movie-data.json"
             
   
### Testando as consultas nos índices criados

Acesse o arquivo abaixo e teste as consultas nele contidas pelo Dev Tools do Kibana (Management -> Dev Tools:

https://github.com/romulorm/elk-docs/blob/master/cap02-instalacao_configuracao_elasticsearch/03-apis_de_operacao_e_query_dsl.txt


### Atualizando o cluster


    $ docker compose down

Editar o valor da variável STACK_VERSION do arquivo .env para a última versão disponível do Elastic.

Exemplo: STACK_VERSION="8.4.1"


    $ docker compose up -d


### Removendo o LAB e TODOS os dados

    $ docker compose down -v
