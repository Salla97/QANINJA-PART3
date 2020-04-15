# QANINJA-PART3


Este projeto tem como princípio a automação do 0 ao avançado de uma plataforma para cadastros de Filmes. 

O projeto está com infraestrutura Docker, segue abaixo os comandos para configuração.

### Comandos para configurar Docker Toolbox (partindo de que o Docker Toolbox esteja instalado)

docker network create --driver bridge skynet
docker run --name pgdb --network=skynet -e "POSTGRES_PASSWORD=qaninja" -p 5432:5432 -v var/lib/postgresql/data -d postgres 
docker run --name pgadmin --network=skynet -e "PGADMIN_DEFAULT_EMAIL=root@qaninja.io" -e "PGADMIN_DEFAULT_PASSWORD=qaninja" -p 15432:80 -d dpage/pgadmin4

docker pull papitoio/nflix-api-movies
docker run --name nflix-api-movies --network=skynet -e "DATABASE=pgdb" -p 3002:3002 -d papitoio/nflix-api-movies
docker pull papitoio/nflix-api-gateway
docker run --name nflix-api-gateway --network=skynet -e "API_USERS=http://nflix-api-users:3001" -e "API_MOVIES=http://nflix-api-movies:3002" -p 3000:3000 -d
docker pull papitoio/nflix-web
docker run --name nflix-web --network=skynet -e "VUE_APP_API=http://192.168.99.100:3000" -p 8080:8080 -d papitoio/nflix-web


## Principais comandos:

### Parar e remover os seguintes containers:

docker stop nflix-api-movies
docker rm nflix-api-movies
docker stop nflix-api-gateway
docker rm nflix-api-gateway
docker stop nflix-web
docker rm nflix-web


### Atualizar a imagem dos containers:
docker pull papitoio/nflix-api-movies
docker pull papitoio/nflix-api-gateway
docker pull papitoio/nflix-web

### Subir novos containers:

#### API Movies
docker run --name nflix-api-movies --network=skynet -e "DATABASE=pgdb" -p 3002:3002 -d papitoio/nflix-api-movies

#### API Gateway
docker run --name nflix-api-gateway --network=skynet -e "API_USERS=http://nflix-api-users:3001" -e "API_MOVIES=http://nflix-api-movies:3002" -p 3000:3000 -d

#### WebApp

#### Docker Normal:​

docker run --name nflix-web --network=skynet -e "VUE_APP_API=http://localhost:3000" -p 8080:8080 -d papitoio/nflix-web

#### Docker ToolBox

docker run --name nflix-web --network=skynet -e "VUE_APP_API=http://192.168.99.100:3000" -p 8080:8080 -d papitoio/nflix-web
