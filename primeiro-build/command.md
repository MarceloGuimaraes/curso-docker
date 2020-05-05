# Execução de um Container à partir da criação de uma Imagem 

# Entrar no diretório onde está o descritor (arquivo) Dockerfile:
cd primeiro-build'

# Criação da imagem:
# -t -> insere o nome da imagem
#  o ponto(.) é para achar o local do descritor dockerfile
docker image build -t primeiro-build .

# Listar a imagem criada:
docker image ls

# Executar o Container à partir da imagem criada.
docker container run -p 80:80 primeiro-build

# Para visualizar, acesse http://localhost:80
