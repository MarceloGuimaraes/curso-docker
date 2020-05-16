# PROJETO: email-worker-compose

# Criação o arquivo docker-compose.yml com a img do postgres
## version: '3'
## services: 
##    db:
##        image: postgres:9.6
##        environment: 
##            - POSTGRES_USER=postgres
##            - POSTGRES_PASSWORD=postgres
##            - POSTGRES_DB=db

# Criação o arquivo docker-compose.yml
# Executar o comando para ler o arquivo 'docker-compose.yml' e subir/executar todos os container/serviços configurados no arquivo 
# -d -> executa em modo deamon
docker-compose up -d

# Listar os banco de dados do serviço levantado 
docker-compose exec db psql -U postgres -c '\l'

## resultado esperado:
###                               List of databases
###   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
### -----------+----------+----------+------------+------------+-----------------------
###  db        | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
###  postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
###  template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
###            |          |          |            |            | postgres=CTc/postgres
###  template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
###            |          |          |            |            | postgres=CTc/postgres

# Outra maneira de listar as bases usando SELECT
docker-compose exec db psql -U postgres -c 'SELECT datname FROM pg_database'


# Criação do script de inicialização init.sql do postgres e o arquivo check.sql com as consultas do postgres

# Alterado o docker-compose.yml para dividir em volumes
# Execute o comando para verificar separa listar o processo
docker-compose ps

# Se estiver em execução, utilize o comando abaixo para encerrar
cusso-docker$ docker-compose down

# Executar o comando para ler o arquivo 'docker-compose.yml' e subir/executar o volume criado.
docker-compose up -d

# Executar o comando abaixo onde será executa os comandos a partir do arquivo check.sql
docker-compose exec db psql -U postgres -f ./scripts/check.sql

# Resultado esperado
## 							List of databases
##      Name     |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
## --------------+----------+----------+------------+------------+-----------------------
##  db           | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
##  email_sender | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
##  postgres     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
##  template0    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
##               |          |          |            |            | postgres=CTc/postgres
##  template1    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
##               |          |          |            |            | postgres=CTc/postgres
## (5 rows)
## 
## You are now connected to database "email_sender" as user "postgres".
##                                     Table "public.emails"
##   Column  |            Type             |                      Modifiers                      
## ----------+-----------------------------+-----------------------------------------------------
##  id       | integer                     | not null default nextval('emails_id_seq'::regclass)
##  data     | timestamp without time zone | not null default now()
##  assunto  | character varying(100)      | not null
##  mensagem | character varying(250)      | not null

# atualizar o docker-compose com a imagem do nginx
# O nginx será executado na porta 80 e ira usar o volume para mapear o conteudo do diretorio web
##  - ./web:/usr/share/nginx/html/


## Levantar a instancia:
docker-compose up -d

## Executar o comando para verificar os logs após levantar o docker com nginx que será exposto na porta 80
docker-compose logs -f -t

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log

# Atualizar o docker-compose com a imagem do python que será exposto na porta 8080
# Neste exemplo, as duas portas estarão disponíveis na camada web
# Criação um serviço em Python e fazer com que ao submeter uma página no nginx chame a rota do serviço enviando o assunto e a mensagem
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

# Criação o arquivo default.config no diretorio nginx para configurar: 
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
## Criação o metodo register_message
## Adicionar no metodo send, a chamada ao método register_message 

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log
# A msg será inseria no banco de dados postgres
# Para consultar execute:
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
docker-compose up -d

## Executar o comando para verificar os logs após levantar o docker com nginx que será exposto na porta 80
docker-compose logs -f -t


# Como escalar um único container para vários serviços, gerar várias instâncias de um único container.
## Criar o arquivo Dockerfile no diretórios worker.
## Editar o serviço worker no compose, 
### Substituir de:'image: python:3.6'  para: 'build: worker' ( p/compose procurar o dockerfile criado)
### Substituir de:'command: bash ./app.sh'  para: 'command: worker.py' ( p/ usar o ENTRYPOINT adicionado como parametro no worker.py )

## Escalando o worker com 3 instancias
docker-compose up -d --scale worker=3

## Executar o comando para verificar os logs do serviço de worker
docker-compose logs -f -t worker

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log
#### log:
#### worker_1    | 2020-05-16T01:31:44.419818360Z Encaminhando a mensagem: a
#### worker_3    | 2020-05-16T01:31:53.414823769Z Encaminhando a mensagem: b
#### worker_2    | 2020-05-16T01:32:00.903373346Z Encaminhando a mensagem: v
#### worker_1    | 2020-05-16T01:32:17.453621832Z Mensagem a Mensagem encaminhada com sucesso
#### worker_3    | 2020-05-16T01:32:24.445559747Z Mensagem b Mensagem encaminhada com sucesso
#### worker_2    | 2020-05-16T01:32:26.929310644Z Mensagem v Mensagem encaminhada com sucesso


# Usar variáveis de ambiente
## Adicionado variáveis de ambiente no arquivo sender.py para o database e para a fila
## Adicionado variáveis de ambiente no arquivo worker.py para a fila
## Adicionado as variáveis na tag enviroment do docker-compose 

## Escalando o worker com 3 instancias
docker-compose up -d --scale worker=3

## Executar o comando para verificar os logs do serviço de worker
docker-compose logs -f -t worker

# Para testar, acessar http://localhost/  e validar se a msg foi inserida no postgres
docker-compose exec db psql -U postgres -d email_sender -c 'select * from emails'
