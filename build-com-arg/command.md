# Criação de uma imagem passando argumentos

# Entrar no diretório onde está o descritor (arquivo) Dockerfile:
curso-docker$ cd build-com-arg'

# Criado o arquivo Dockerfile usando a imagem do Debian e
# criando a varável 'S3_BUCKET' com o valor default files

# Criação da imagem à partir do descritor
# -t -> insere o nome da imagem
#  o ponto(.) é para achar o local do descritor dockerfile
curso-docker$ docker image build -t build-com-arg .

# Listar a imagem criada:
curso-docker$ docker image ls

# Run no container e executar o 'bash' e ver a saída da variável S3_BUCKET
curso-docker$ docker container run build-com-arg bash -c 'echo $S3_BUCKET'
# o resultado será o valor definido no dockerfile: file

# Criação da imagem alterando o valor do argumento S3_BUCKET criado no descritor
curso-docker$ docker image build --build-arg S3_BUCKET=myapp -t build-com-arg .

# Run no container e executar o 'bash' e ver a saída da variável S3_BUCKET
curso-docker$ docker container run build-com-arg bash -c 'echo $S3_BUCKET'
# o resultado será o valor passado como parametro: myapp
