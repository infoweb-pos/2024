# Nest - 2024 - Incluindo persistência no CRUD



## 1. Objetivo
1. Incluir mecanismo de persistência na API



## 2. Links

- Tutoriais
  - [Building a REST API with NestJS and Prisma: Authentication](https://www.prisma.io/blog/nestjs-prisma-authentication-7D056s1s0k3l)
  - [Nestjs docs](https://docs.nestjs.com/)
    - [Authentication](https://docs.nestjs.com/security/authentication)
    - [Authorization](https://docs.nestjs.com/security/authorization)


## 3. Notas de Aula

- **autentica** - provar que você é quem diz ser
- **autorização** - com a identificação apresentada, tem acesso ao recurso solicitado

**sumário**

[zip](https://github.com/infoweb-pos/2024-nest/archive/refs/tags/02-Tarefa-Crud-Prisma.zip) [branch](https://github.com/infoweb-pos/2024-nest/tree/02-tarefas-crud-prisma)

```console
### criar  novo branch local 03-autenticacao
$ git checkout -b 03-autenticacao
Switched to a new branch '03-autenticacao'


### criar modulo de usuario
$ npx @nestjs/cli g module usuarios
CREATE src/usuarios/usuarios.module.ts (85 bytes)
UPDATE src/app.module.ts (490 bytes)


### criar servico (regras de negócio) de usuario
$ npx @nestjs/cli g service usuarios --no-spec
CREATE src/usuarios/usuarios.service.ts (92 bytes)
UPDATE src/usuarios/usuarios.module.ts (171 bytes)
(node:394233) [DEP0051] DeprecationWarning


### criar controlador (url) de usuário
$ npx @nestjs/cli g controller usuarios --no-spec
CREATE src/usuarios/usuarios.controller.ts (105 bytes)
UPDATE src/usuarios/usuarios.module.ts (268 bytes)


### adicionar entity e dto (criar) para usuário
### ./src/usuarios/dto/create-usuario.dto.ts
### ./src/usuarios/entities/usuario.entity.ts


### editar o módulo, controlador e serviço de usuario
### ./src/usuarios/usuarios.controller.ts
### ./src/usuarios/usuarios.module.ts
### ./src/usuarios/usuarios.service.ts


### evoluir e aplicar schema prisma com modelo usuario
### evoluir seed do prisma
$ npx prisma migrate dev --name usuarios
Environment variables loaded from .env
Prisma schema loaded from prisma/schema.prisma
Datasource "db": SQLite database "dev.db" at "file:./dev.db"

Applying migration `20240826162654_usuarios`

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20240826162654_usuarios/
    └─ migration.sql

Your database is now in sync with your schema.

✔ Generated Prisma Client (v5.18.0) to ./node_modules/.pnpm/@prisma+client@5.18.0_prisma@5.18.0/node_modules/@prisma/client in 58ms

$ npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed.ts` ...
...
[
  {
    id: 1,
    apelido: 'leonardo',
    email: 'leonardo.minora@gmail.com',
    senha: '123'
  },
  {
    id: 2,
    apelido: 'minora',
    email: 'leonardo.minora@ifrn.edu.br',
    senha: '321'
  }
]

🌱  The seed command has been executed.


### editar o serviço de usuario
### ./src/usuarios/usuarios.service.ts

### instalar as libs necessárias para hoje
### passaport, jwt e bcrypt
$ npm install --save @nestjs/passport passport @nestjs/jwt passport-jwt bcrypt
$ npm install --save-dev @types/passport-jwt @types/bcrypt

$ npx @nestjs/cli generate resource autenticacao --no-spec
? What transport layer do you use? REST API
? Would you like to generate CRUD entry points? No
CREATE src/autenticacao/autenticacao.controller.ts (252 bytes)
CREATE src/autenticacao/autenticacao.module.ts (297 bytes)
CREATE src/autenticacao/autenticacao.service.ts (96 bytes)
UPDATE src/app.module.ts (583 bytes)

### editar ./src/autenticacao/autenticacao.module.ts
### para configurar passport e importar os outros módulos 
### (passport, jwt, persistencia)


### criar ./src/autenticacao/dto/login.dto.ts


### criar ./src/autenticacao/entity/autenticacao.entity.ts

### editar ./src/autenticacao/autenticacao.service.ts
### para criar método de autenticação


### editar ./src/autenticacao/autenticacao.controller.ts
### para adicionar um endpoint

### criar o arquivo ./rest-client/autenticacao.http

### criar uma estratégia de verificação ./src/autenticacao/jwt.strategy.ts

```
