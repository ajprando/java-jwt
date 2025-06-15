# Projeto Java Spring Boot - API de Autenticação JWT com Controle de Acesso por Role (Admin/User)

Este projeto é uma API REST desenvolvida com Spring Boot, que implementa autenticação e autorização baseada em JWT (JSON Web Token). Usuários podem se registrar, autenticar, atualizar perfil e, se forem administradores, gerenciar outros usuários.

## Sumário

- [Funcionalidades](#funcionalidades)
- [Instalação](#instalação)
- [Como rodar](#como-rodar)
- [Endpoints](#endpoints)
  - [Autenticação (`/api/auth`)](#autenticação-apiauth)
  - [Usuário (`/api/user`)](#usuário-apiuser)
  - [Administração (`/api/admin`)](#administração-apiadmin)
- [Exemplos de uso](#exemplos-de-uso)

---

## Funcionalidades

- Registro e login de usuários com senha criptografada.
- Geração e validação de JWT.
- Perfis de usuário: `USER` e `ADMIN`.
- Rotas protegidas por autenticação e autorização.
- Atualização de perfil de usuário.
- Administração de usuários (listar, atualizar, deletar).

## Instalação

1. **Pré-requisitos**:
   - Java 17+
   - Maven 3.9+

2. **Clone o repositório**:
   ```sh
   git clone <url-do-repo>
   cd java-jwt-master
   ```

3. **Build do projeto**:
   ```sh
   ./mvnw clean install
   ```

## Como rodar

Execute o comando abaixo para iniciar a aplicação:

```sh
./mvnw spring-boot:run
```

A API estará disponível em: `http://localhost:8080`

## Endpoints

### Autenticação `/api/auth`

- **POST /register**  
  Registra um novo usuário.  
  Corpo:
  ```json
  {
    "name": "João",
    "email": "joao@email.com",
    "password": "senha123",
    "role": "USER"
  }
  ```

- **POST /login**  
  Autentica o usuário e retorna um token JWT.  
  Corpo:
  ```json
  {
    "email": "joao@email.com",
    "password": "senha123"
  }
  ```
  Resposta:
  ```
  "eyJhbGciOiJIUzI1NiJ9..."
  ```

### Usuário `/api/user`

Necessário enviar o token JWT no header:  
`Authorization: Bearer <token>`

- **GET /profile**  
  Retorna os dados do usuário autenticado.

- **PUT /profile**  
  Atualiza nome e/ou senha do usuário autenticado.  
  Corpo:
  ```json
  {
    "name": "João da Silva",
    "password": "novasenha"
  }
  ```

### Administração `/api/admin`

Necessário ser ADMIN e enviar o token JWT.

- **GET /users**  
  Lista todos os usuários.

- **PUT /users/{id}**  
  Atualiza dados de um usuário pelo ID.  
  Corpo:
  ```json
  {
    "name": "Maria",
    "email": "maria@email.com",
    "password": "senha456",
    "role": "ADMIN"
  }
  ```

- **DELETE /users/{id}**  
  Remove um usuário pelo ID.

## Exemplos de uso

### 1. Registro

```sh
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"João","email":"joao@email.com","password":"senha123","role":"USER"}'
```

### 2. Login

```sh
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"joao@email.com","password":"senha123"}'
```

### 3. Buscar perfil

```sh
curl -H "Authorization: Bearer <token>" http://localhost:8080/api/user/profile
```

### 4. Listar usuários (ADMIN)

```sh
curl -H "Authorization: Bearer <token_admin>" http://localhost:8080/api/admin/users
```

---

## Observações

- O banco de dados utilizado é H2 em memória.
- O token JWT deve ser enviado em todas as rotas protegidas, no header `Authorization`.
- Para testar rotas de admin, registre um usuário com `"role": "ADMIN"`.

---

Projeto desenvolvido com Spring Boot, Spring Security, JPA,
