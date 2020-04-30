# Criar o arquivo app.js no diretório backend
# Criar o arquivo index.html no diretório frontend
# Criar o arquivo docker-compose.yml na raiz do diretório node-mongo-compose

# Entrar na pasta backend e criar o arquivo descritor do Node 
# Pré requisito é q o node/npm esteja instalado:
curso-docker$ npm init -y
# O resultado será a criação do arquivo package.json

# Realizar a instalação dos pacotes 
npm i --save  body-parser@1.17.2  cors@2.8.3  express@4.15.3  mongoose@4.11.1  node-restful@0.2.6 

# Após a instalação o pacote node-modules poderá ser removido: rm -rf node-modules

# Criar o arquivo docker-compose.yml
# Executar o comando para ler o arquivo yml e subir/executar todos os container/serviços configurados no arquivo
urso-docker$ docker-compose up

# O resultado será a exibição dos LOGS do backend, mongodb e do frontend
# Para testar o frontend: 
#     - Acesse http://localhost:80 onde será exibido a página com o texto 'Frontend'

# Para testar o backend: 
#     - Acesse http://localhost:3000 onde será exibido a página com o texto 'Backend'