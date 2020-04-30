# Criação de um servidor em Python versão 3.6
# Criação de um novo descritor Dockerfile para fazer com que o servidor em Python seja executado 
# e sirva a página HTML

# Entrar no diretório onde está o descritor(arquivo) Dockerfile:
curso-docker$ cd build-dev'

# Criação da imagem
curso-docker$ docker image build -t ex-build-dev .

# Execução do container remapeando e alterando a porta de execução definida no descritor
# -it -> modo interativo para ver o log no console
# -v ./build-dev:/app  -> está mapeando o WORKDIR (diretório atual da máquina q contem o arquivo run.py) para o '/app'
curso-docker$ docker container run -it -v ./build-dev:/app -p 80:8000 --name python-server ex-build-dev

# No console será exibido o log da execução do servidor
# Para visualizar a página estática, acesse http://localhost:80


# Outro Exemplo
# Criar um container para  **LER** o log/volume que o container anterior gerou
curso-docker$ docker container run -it --volumes-from=python-server debian cat /log/http-server.log