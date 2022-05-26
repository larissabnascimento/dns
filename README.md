ASA

REPOSITORIO PARA O SERVIDOR DNS 

criar a rede DNS-net para adicionar os containers:

$ docker network create --subnet=172.20.0.0/16 DNS-net

cria a imagen:

$ docker build -t bind9 .

Execute um contêiner em segundo plano, usando o mesmo IP do arquivo db.asa-teste.com e a mesma rede Docker criada:

$ docker run -d --rm --name=dns-server --net=DNS-net --ip=172.20.0.2 bind9

Em seguida, habilite o daemon bind9:

$ docker exec -d dns-server /etc/init.d/bind9 start

Agora é possível executar os dois hosts usando o contêiner dns-server como servidor DNS

host 1
$ docker run -d --rm --name=host1 --net=DNS-net --ip=172.20.0.3 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"

host 2
$  docker run -d --rm --name=host2 --net=DNS-net --ip=172.20.0.4 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"

