# Iniciar um container
docker container start <id>

# Reiniciar um container
docker container restart <id>

# Parar um container
docker container stop <id>

# Remover um container
docker container rm <id>

# Remover c/ 'Force removal' o container
docker container rm -f <id>


# Matar uma imagem
docker container kill <id>

# Logs do volume
docker container logs <id>

# Inspecionar um container
docker container inspect primeiro-build

# Executar comandos linux no docker
docker container exec primeiro-build uname -or

# Lista todas os container
docker container ps -a

# Criar um container mapeando o arquivo html local com o diretório home do servidor nginx
# -d Coloca o docker para rodar em background
# -p altera a porta defaul
# -v remapeando com volume
docker container run -d --name primeiro-build -p 8080:80 -v ./html:/usr/share/nginx/html n
ginx

# Listar as imagens
docker image ls

# Listar os volumes
docker volume ls

# Remover uma imagem
docker image rm <id>

# Remover c/ 'Force removal'a imagem
docker image rm -f <id>

# Puxar/extrair/baixar uma imagem
docker image pull redis:latest

# PROJETO: primeiro-build
# Criando imagem
# -t tag da imagem
# . Local onde esta o Dockerfile
docker image build -t primeiro-build .

# Executar o Container à partir da imagem criada.
docker container run -p 80:80 primeiro-build

# PROJETO: build-com-arg
# Imagem com args
docker image build -t build-com-arg .

# Run no container e executar o 'bash' e ver a saída da variável S3_BUCKET
docker container run build-com-arg bash -c 'echo $S3_BUCKET'
# o resultado será o valor definido no dockerfile: file

# Criação da imagem alterando o valor do argumento S3_BUCKET criado no descritor
docker image build --build-arg S3_BUCKET=myapp -t build-com-arg .

# Run no container e executar o 'bash' e ver a saída da variável S3_BUCKET
docker container run build-com-arg bash -c 'echo $S3_BUCKET'
# o resultado será o valor passado como parametro: myapp

# Filtro inspect
docker image inspect --format='{{index .Config.Labels \"maintainer\"}}' build-com-arg

# PROJETO: build-dev
# Docker com python
docker image build -t ex-build-dev .

# Execução do container remapeando e alterando a porta de execução definida no descritor
# -it -> modo interativo para ver o log no console
# -v ./build-dev:/app  -> está mapeando o WORKDIR (diretório atual da máquina q contem o arquivo run.py) para o '/app'
curso-docker$ docker container run -it -v ./build-dev:/app -p 80:8000 --name python-server ex-build-dev

# No console será exibido o log da execução do servidor
# Para visualizar a página estática, acesse http://localhost:80

# Outro Exemplo
# Criar um container para  **LER** o log/volume que o container anterior gerou
curso-docker$ docker container run -it --volumes-from=python-server debian cat /log/http-server.log

# REDES:

# Criar o container em modo HOST, usando diretamente as interfaces de rede da máquina host
# Isso tira a camada de isolamente sem usar o modo bridge, tem uma proteção menor
# Mas ganha em velocidade por estar executando direto em modo HOST
docker container run -d --name container4 --net host alpine sleep 1000

# executar o ifconfig no container4
docker container exec -it container04 ifconfig
# O resultado será a exibição das interfaces locais do host

# Criando nova tag
docker image tag primeiro-build simple-build:1.0

# Docker login
docker login --username=$(USER)

# Docker push
docker image push simple-build:1.0

# Listar drivers de rede
docker network ls

docker container run --rm alpine ash -c "ifconfig"
docker container run --rm --net none alpine ash -c "ifconfig"

docker network inspect bridge

docker container run -d --name container1 alpine sleep 1000
docker container exec -it container1 ifconfig

docker container exec -it container1 ping 172.17.0.5

docker network create --driver bridge rede_nova

docker container exec -it container3 ping 172.17.0.1

docker network connect bridge container3

docker network disconnect bridge container3



# Docker Postgres
docker-compose exec db psql -U postgres -c '\l'
docker-compose exec db psql -U postgres -f ./scripts/check.sql

docker-compose logs -f -t

docker-compose exec db psql -U postgres -d email_sender -c 'select * from emails'

# Escalando Docker
docker-compose up -d --scale worker=3