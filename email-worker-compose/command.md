# PROJETO: email-worker-compose

# Criar o arquivo docker-compose.yml com a img do postgres
## version: '3'
## services: 
##    db:
##        image: postgres:9.6
##        environment: 
##            - POSTGRES_USER=postgres
##            - POSTGRES_PASSWORD=postgres
##            - POSTGRES_DB=db

# Criar o arquivo docker-compose.yml
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


# Adicionando o nginx para usar como proxy reverso
# O nginx ira usar o volume para mapear o conteudo do diretorio web
##  - ./web:/usr/share/nginx/html/

# Levantar a instancia:
docker-compose up 

# Executar o comando para verificar os logs após levantar o docker com nginx
docker-compose logs -f -t

# Para testar, acessar http://localhost/ onde será possível acompanhar a requisição no log
