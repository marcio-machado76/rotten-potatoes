# rotten-potatoes
## Sistema de avaliação de filmes

## **Como utilizar**
  - Para o uso dessa aplicação são necessárias algumas das configurações abaixo:

### Configuração

|**Variáveis de ambiente do mongodb**|
|------------------------------------|
|  Variável   |     Descrição        |
|-------------|----------------------|
| MONGODB_DB  | Nome do database|
|MONGODB_HOST | Host do MongoDB |
|MONGODB_PORT | Porta de acesso ao MongoDB|
|MONGODB_USERNAME | Usuário do MongoDB |
|MONGODB_PASSWORD | Senha do MongoDB |



### Arquivo Dockerfile
```Dockerfile
FROM python:3.9-slim-buster
WORKDIR /app
COPY requirements.txt .
RUN python3 -m pip install --upgrade pip && \
python -m pip install -r requirements.txt 

COPY . .
EXPOSE 5000
CMD ["gunicorn", "--workers=3", "--bind", "0.0.0.0:5000", "app:app"]

```

### Arquivo docker-compose.yaml
```docker-compose
version: "3.9"
services:
  db:
    image: mongo:4.4.3
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: mongouser
      MONGO_INITDB_ROOT_PASSWORD: mongopasswd
    volumes:
      - mongovol:/data/db
    networks:
      - net-app-mongo

  app:
    image: machado1976/rotten-potatoes:latest
    build:
      dockerfile: ./Dockerfile
      context: .
    ports:
      - 5000:5000
    environment:
      MONGODB_DB: admin
      MONGODB_HOST: db
      MONGODB_PORT: 27017
      MONGODB_USERNAME: mongouser
      MONGODB_PASSWORD: mongopasswd
    depends_on:
      - db
    networks:
      - net-app-mongo

volumes:
  mongovol:

networks:
  net-app-mongo:
    driver: bridge
```

### Comandos 
  - Acesso a pasta 'src/', `cd src/`
  - Execute o comando `docker-compose up -d`
  - Aguarde concluir todo o processo e depois acesse a 
  aplicação no browser através do endereço <localhost:5000>
  - Para terminar a aplicação execute o comando `docker-compose down`
