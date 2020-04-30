# Criação de uma imagem usando o comando RUN para gerar o arquivo dentro do diretório do nginx
#                     e
# executar o comando COPY  para copiar o arquivo index.html para o diretório do nginx

# Entrar no diretório onde está o descritor (arquivo) Dockerfile:
curso-docker$ cd build-com-copy

# Criado o arquivo Dockerfile com os comandos COPY e RUN

# Criação da imagem à partir do descritor
# -t -> insere o nome da imagem
#  o ponto(.) é para achar o local do descritor dockerfile
curso-docker$ docker image build -t build-com-copy .

# Executar o Container à partir da imagem criada.
curso-docker$ docker container run -p 80:80 build-com-copy

# Para visualizar, acesse http://localhost:80
