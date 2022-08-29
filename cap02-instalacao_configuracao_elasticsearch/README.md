# ELASTIC SEARCH - CONTAINERS LAB

## Instale o Virtualbox e crie duas VMs
* Instale no Virtualbox: https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe
* No Virtualbox, crie uma rede virtual no menu Arquivo, Preferências, Rede, botão "Acrescentar uma nova rede NAT"
* Crie uma VM no virtualbox do tipo Linux - Ubuntu(64-bit) com pelo menos 16Gb RAM e 100Gb de disco

## Install Ubuntu Desktop
* Puxe a .iso do Ubuntu Desktop do link: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-desktop-amd64.iso
* Nas configurações de rede da VM, escolha a rede NAT criada anteriormente.
* Instale o S.O. na  máquina virtual criada, indo no menu Dispositivos (da VM), Discos óticos, Escolher uma imagem de disco. Aponte para o arquivo ubuntu-22-xxx.iso.
* Inicie a VM para iniciar a instalação

## Instalando o Docker no Alma Linux 9

Acesse a máquina virtual
    sudo apt-get remove docker docker-engine docker.io containerd runc
    sudo apt-get -y update
    sudo apt-get install ca-certificates curl gnupg lsb-release
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    sudo usermod -aG docker $USER
    sudo systemctl enable docker
    sudo systemctl start docker
    docker -v
    docker compose version

## Defina a memória virtual

    sudo sysctl -w vm.max_map_count=262144
    sudo echo "vm.max_map_count=262144" >> /etc/sysctl.conf

## Preparando os arquivos docker-compose e .env que criarão o cluster
    mkdir ~/elastic
    dnf install wget
    wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/docker-compose.yml
    wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/.env
    docker compose up -d

Do navegador web, em outra VM na mesma rede, acesse a URL http://localhost:5601
Usuário: elastic
Pass: elastic123456

--------------------------------------
Importando os datasets para o Elastic
--------------------------------------

Dentro da pasta elastic:
    wget https://github.com/romulorm/elk-docs/raw/master/datasets/accounts.json
    wget https://github.com/romulorm/elk-docs/raw/master/datasets/crimessp.json
    wget https://github.com/romulorm/elk-docs/raw/master/datasets/crimessp-data.json
    wget https://github.com/romulorm/elk-docs/raw/master/datasets/movie.json
    wget https://github.com/romulorm/elk-docs/raw/master/datasets/movie-data.json

POST /banco/_doc/_bulk?pretty&refresh
-binary @accounts.json

--------------------------

PUT /crimessp?pretty
-binary @crimessp.json

PUT /crimessp/_doc/_bulk?pretty&refresh
-binary @crimessp-data.json

--------------------------

PUT /movie?pretty
-binary @movie.json

PUT /movie/_doc/_bulk?pretty&refresh
-binary @movie-data.json
