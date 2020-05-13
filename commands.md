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
 docker container run -it -v ./build-dev:/app -p 80:8000 --name python-server ex-build-dev

# No console será exibido o log da execução do servidor
# Para visualizar a página estática, acesse http://localhost:80

# Outro Exemplo
# Criar um container para  **LER** o log/volume que o container anterior gerou
 docker container run -it --volumes-from=python-server debian cat /log/http-server.log

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


# PROJETO: cadastro Simples
# Criar o arquivo app.js no diretório backend
# Criar o arquivo index.html no diretório frontend
# Criar o arquivo docker-compose.yml na raiz do diretório node-mongo-compose

# Entrar na pasta backend e criar o arquivo descritor do Node 
# Pré requisito é q o node/npm esteja instalado:
 npm init -y
# O resultado será a criação do arquivo package.json

# Realizar a instalação dos pacotes 
npm i --save  body-parser@1.17.2  cors@2.8.3  express@4.15.3  mongoose@4.11.1  node-restful@0.2.6 

# Após a instalação o pacote node-modules poderá ser removido: rm -rf node-modules

# Criar o arquivo docker-compose.yml
# Executar o comando para ler o arquivo yml e subir/executar todos os container/serviços configurados no arquivo
docker-compose up

# O resultado será a exibição dos LOGS do backend, mongodb e do frontend
# Para testar o frontend: 
#     - Acesse http://localhost:80 onde será exibido a página com o texto 'Frontend'

# Para testar o backend: 
#     - Acesse http://localhost:3000 onde será exibido a página com o texto 'Backend'

# PROJETO: email-worker-compose
# Docker Postgres
# Listar os banco de dados do serviço levantado 
docker-compose exec db psql -U postgres -c '\'

# Outra maneira de listar as bases usando SELECT
docker-compose exec db psql -U postgres -c 'SELECT datname FROM pg_database'

# Executar o comando abaixo onde será executa os comandos a partir do arquivo check.sql
docker-compose exec db psql -U postgres -f ./scripts/check.sql

# Executar o comando para verificar os logs após levantar o docker com nginx que será exposto na porta 80
docker-compose logs -f -t

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log

# Atualizar o docker-compose com a imagem do python que será exposto na porta 8080
# Neste exemplo, as duas portas estarão disponíveis na camada web
# criar um serviço em Python e fazer com que ao submeter uma página no nginx chame a rota do serviço enviando o assunto e a mensagem
## criação do arquivo 'app.sh' para instalação de libraries do python
## criação do arquivo 'sender.py' com o serviço send na rota '/' na porta 8080
## para testar o serviço : 
### curl --location --request POST 'http://localhost:8080' \
### --header 'Content-Type: application/json' \
### --header 'Content-Type: text/plain' \
### --data-raw '{ "assunto": "Assunto teste 1", "mensagem": "mensagem teste 1" }'

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log

# Configuração do nginx como proxy reverso para redirecionar as requisições para a aplicação Sender.py
# O que melhora a camada de segurança

# criar o arquivo default.config no diretorio nginx para configurar: 
## direcionamentos da pagina index.html/index.htm
## direcionamento para as páginas de erro 500
## direcionamento para a rota '/api', fazendo o proxy reverso (proxy_pass) para 'app', que foi configurado no compose
## OBS: app é o nome do serviço definido no compose, não é necessário saber o IP do serviço,
##      pode acessar pelo nome dele, ex.: pode usar o 'db', 

# Adicionar a configuração do proxy reverso no compose: 
## - ./nginx/default.conf:/etc/nginx/conf.d/default.conf

# Remover a porta 8080 da aplicação sender.
# testar a api via curl:
### curl --location --request POST 'http://localhost/api' \
### --header 'Content-Type: application/json' \
### --header 'Content-Type: text/plain' \
### --data-raw '{ "assunto": "Assunto teste 1", "mensagem": "mensagem teste 1" }'

# Para testar a pagina, acesse http://localhost/ onde será possível acompanhar a requisição no log

# Configuração das redes : WEB e BANCO
# Armazenar a mensagem no postgres
# Atualizar o docker-compose com as configurações de networks para segregar a rede
# A tag 'depends_on' serve para orquestrar inicialização a inicialização dos serviços no container

# Após as tag, adicionar a instalação do pacote psycopg2 no script app.sh
## Editar o script sender.py adicionando o import do psycopg2
## Adicionar a conexão com o postgres na variável DSN
## Adicionar a consulta no postgres na variável  SQL
## Criar o metodo register_message
## Adicionar no metodo send, a chamada ao método register_message 

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log
# A msg será inseria no banco de dados postgres
# Para consultar o banco de dados execute:
docker-compose exec db psql -U postgres -d email_sender -c 'select * from emails'


# Adicionar o worker e a fila :
## Editar o compose adicionando a rede fila.
## Permitir acesso ao serviço 'app' em todas as redes
## Adicionado os serviços queue e worker 
## Adicionar o redis no arquivo requirements.txt do aplicativo
## Alteração o arquivo sender.py da aplicação para ser uma classe e adicionar o envio da msg para a fila
# Criação do diretorio worker
## Criação do arquivo app.sh para realizar a instalação do redis e execução do worker
## Criação do script worker.sh de execução da fila
### mkdir worker && cd worker&& touch app.sh && chmod 777 app.sh
### touch worker.py && chmod 777 worker.py

## Levantar a instancia:
docker-compose up 

## Executar o comando para verificar os logs após levantar o docker com nginx que será exposto na porta 80
docker-compose logs -f -t




# Escalando Docker
docker-compose up -d --scale worker=3