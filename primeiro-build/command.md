# Execução de um Container à partir da criação de uma Imagem 

# Entrar no diretório onde está o descritor (arquivo) Dockerfile:
curso-docker$ cd primeiro-build'

# Criação da imagem:
# -t -> insere o nome da imagem
#  o ponto(.) é para achar o local do descritor dockerfile
curso-docker$ docker image build -t primeiro-build .

# Listar a imagem criada:
curso-docker$ docker image ls

# Executar o Container à partir da imagem criada.
curso-docker$ docker container run -p 80:80 primeiro-build

# Para visualizar, acesse http://localhost:80
