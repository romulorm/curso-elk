# ELASTIC SEARCH - CONTAINERS LAB


## Install Alma Linux 9

* Instale no Virtualbox: https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe
* Crie uma VM no virtualbox do tipo Linux - Redhat(64-bit) com pelo menos 16Gb RAM e 100Gb de disco
* Puxe a .iso do Alma Linux 9 do link abaixo Alma Linux 9: https://mirrors.almalinux.org/isos/x86_64/9.0.html
* Instale o S.O. na máquina virtual criada no passo anterior indo no menu Dispositivos (da VM), Discos óticos, Escolher uma imagem de disco. Aponte para o arquivo .iso.

## Instalando o Docker no Alma Linux 9

    dnf -y update
    dnf install -y yum-utils
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    dnf -y update
    dnf -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    systemctl enable docker
    systemctl start docker
    docker -v
    docker compose version

## Defina a memória virtual

        sysctl -w vm.max_map_count=262144
        echo "vm.max_map_count=262144" >> /etc/sysctl.conf

##  Caso esteja utilizando um usuário não-ROOT, adicione-o ao grupo docker
        sudo usermod -aG docker $USER

## Preparando os arquivos docker-compose e .env que criarão o cluster
        mkdir ~/elastic
        dnf install wget
        wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/docker-compose.yml
        wget https://github.com/romulorm/elk-docs/raw/master/cap02-instalacao_configuracao_elasticsearch/.env
        docker compose up -d

Do navegador web, em outra VM na mesma rede, acesse a URL http://ip_do_almalinux:5601
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
