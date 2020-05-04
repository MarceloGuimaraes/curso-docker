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
curso-docker$ docker-compose up -d

# Listar os banco de dados do serviço levantado 
curso-docker$ docker-compose exec db psql -U postgres -c '\l'

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
