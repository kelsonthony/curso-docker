# curso-docker
curso-docker

Comandos

docker run --name web01 -p 80:80 -d nginx

docker version
docker info
docker ps
docker ps -a
docker run
docker --help
docker <comando> <submcomando> <opcoes>
docker container ls
docker container ls -a
docker container --help
docker image --help
docker network ls
docker network --help
docker volume
docker volume --help
docker container rm --help

Executar um container

docker container run <OPTIONS> <IMAGEM:TAG>

docker container stop 
doker container b86 (as iniciais do container)

Remover todos os containers

docker container ls -a -q

docker container run -d -P (Todas as portas do container e publicar pro host em portas aleatórias)

docker container run -d -p 80:80 --name webhost nginx

docker container run -d -p 81:80 --name webhost2 httpd

docker container logs webhost

docker containter logs -f webhost

processos que um container está rodando

docker container top web01

docker container stats web01

docker container inspect web01

docker container inspect --help

Exemplo de informações de redes 
docker container inspect -f {{.NetworkSettings}} web01

docker container inspect -f {{.NetworkSettings.Networks}} web01 

docker container inspect -f {{.Mounts}} web01 

docker container inspect -f {{.Name}} web01 
docker container inspect -f {{.Platform}} web01

Conceitos de Redes Docker

Redes padrão

Bridge
None
Host

Bridge em Docker é a NAT

my_web = mysql, apache, php
my_api = flask, nodejs

docker network ls

docker network inspect bridge

iptables -t nat -L

docker inspect host 

docker inspect none

Exemplo prático

docker container run --name net_host1 -d --network host nginx:alpine

checar os logs na mesma porta 
docker logs -f bdef666d995f
docker network ls

pegar o ID da rede host

docker network inspect 1d01eb115aab

Matar todos os containers

docker container rm -f $(docker container ls -a -q)

Criar uma rede

docker network create --driver bridge isolated_nw

docker container run -d --name h_bridge1 nginx:alpine

docker network inspect bridge

docker container run -d --name h_ambiente1 --network ambiente1 nginx:alpine

docker container exec -it h_ambiente1 sh

docker container exec -it postgres /bin/bash

docker container run -d --name h_none --network none nginx:alpine

docker container inspect h_none

docker network create my_custom_net

docker network inspect my_custom_net

 docker network create --help

 docker network create my_custom_net2 --subnet 192.168.134.0/24 --gateway 192.168.134.1

 docker network create my_custom_net3 --subnet 192.168.1.100/24 --gateway 192.168.1.1

 Remover uma rede
 docker network rm  my_custom_net3

 Remover redes não utilizadas

docker network --help

Conectar um container a uma rede

docker network prune

Docker DNS

docker container run -d --name webhost1 nginx:alpine
docker container run -d --name webhost2 nginx:alpine

docker container exec -it webhost1 ping webhost2

Linkar containers

docker container run -d --name webhost2 --link webhost1 nginx:alpine
docker container exec -it webhost2 ping webhost1

docker network create -d bridge network_web

docker container run -d --name webhost1 --network network_web nginx:alpine

docker container exec -it webhost1 ping webhost2

Volume Docker

docker volume ls

docker volume rm ID-VOLUME

docker volume create mysql-db

docker volume inspect mysql-db

docker container run -d --name mysql-db-container -v mysql-db-volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password mysql

docker container inspect f27 visualizar o mount

somente leitura ro 

docker container run -d --name mysql-db-container -v mysql-db-volume:/var/lib/mysql:ro -e MYSQL_ROOT_PASSWORD=password mysql

docker container inspect 7a9 -f '{{json $.Mounts}}'

criar um volume na hora de criar um container

docker container run -d --name mysql-db-container-2 -v mysql-db-volume-2:/var/lib/mysql:ro -e MYSQL_ROOT_PASSWORD=password mysql

Exemplo com o --mount

 docker container run -d --name mysql-db-container-3 --mount 'type=volume,source=mysql-db-volume-3,target=/var/lib/mysql' -e MYSQL_ROOT_PASSWORD=password mysql

docker container inspect 2d8

Bind Volumes

docker container run -d --name nginx1 -p 80:80 nginx

docker container cp nginx1:/usr/share/nginx/html/index.html .

docker container run -d --name nginx2 -v $(pwd)/index.html:/usr/share/nginx/html/index.html -p 81:80 nginx

