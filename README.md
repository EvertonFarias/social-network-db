# Banco de Dados para uma Rede Social

Este projeto é parte da disciplina **Laboratório de Banco de Dados**, lecionada pelo professor **Marcos Vinícius Silva Bento**. O objetivo é criar a estrutura inicial de um banco de dados para uma rede social e documentá-lo no GitHub.

## Tecnologias Utilizadas
- PostgreSQL
- SQL
- Git/GitHub
- Dbdiagram.io

## Estrutura do Banco de Dados
O banco de dados da rede social possui as seguintes tabelas:

1. **users**: Armazena os usuários da rede social.
2. **posts**: Armazena as postagens feitas pelos usuários.
3. **friends**: Registra as amizades entre os usuários.
4. **comments**: Armazena os comentários realizados em postagens.
5. **likes**: Registra as curtidas feitas em postagens e comentários.

### Diagrama ER
![Diagrama ER](docs/MER.png)

## Pré-requisitos
- PostgreSQL instalado (versão 12 ou superior)
- Git instalado

### Instalação do PostgreSQL
Para instalar o PostgreSQL no Ubuntu, utilize os seguintes comandos:
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```
### Para verificar se o serviço está ativo:
```bash
sudo systemctl status postgresql
```
## Clonando Repositório
```bash
https://github.com/EvertonFarias/social-network-db.git
```
## Criação do Banco de Dados
### Acesse o PostgreSQL com o comando:
```bash
sudo -u postgres psql
```
### Dentro do console do PostgreSQL, crie o banco de dados:
```sql
CREATE DATABASE rede_social_cb300;
\c rede_social;
```
## Criando as Tabelas
### Execute o seguinte script SQL para criar as tabelas:
```sql


CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    profile_picture VARCHAR(255),
    bio TEXT
);


CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    image_url VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW()
);


CREATE TABLE friends (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    friend_id INT REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE (user_id, friend_id)
);


CREATE TABLE comments (
    id SERIAL PRIMARY KEY,
    post_id INT REFERENCES posts(id) ON DELETE CASCADE,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);


CREATE TABLE likes (
    id SERIAL PRIMARY KEY,
    post_id INT REFERENCES posts(id) ON DELETE CASCADE,
    comment_id INT REFERENCES comments(id) ON DELETE CASCADE,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT NOW(),
    UNIQUE (user_id, post_id, comment_id)
);
```

### Exemplos de inserção de dados:

```sql
-- Inserindo usuários
INSERT INTO users (username, email, password_hash, bio) VALUES ('marcos', 'marcos@cb300.com', 'hash123', 'Apaixonado por motos.');

-- Criando uma postagem
INSERT INTO posts (user_id, content, image_url) VALUES (1, 'Minha nova CB300!', 'http://imagem.com/cb300.jpg');

-- Adicionando um amigo
INSERT INTO friends (user_id, friend_id) VALUES (1, 2);

-- Comentando em um post
INSERT INTO comments (post_id, user_id, content) VALUES (1, 2, 'Linda moto, Marcos!');

-- Curtindo um post
INSERT INTO likes (post_id, user_id) VALUES (1, 2);

```


