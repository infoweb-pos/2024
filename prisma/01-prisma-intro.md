# Introdução a Prisma ORM

[vídeo da aula](https://youtu.be/-JvDDYCEZX0)



## 1. Link
- [Repositório de código](https://github.com/infoweb-pos/2024-prisma)
- tutoriais
  - [getting started](https://www.prisma.io/docs/getting-started/quickstart)
  - [migrate - getting start](https://www.prisma.io/docs/orm/prisma-migrate/getting-started)
  - [Building a REST API with NestJS and Prisma](https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0)
- ferramentas
  - [prisma orm](https://www.prisma.io/)
  - [extensão do vs code](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma)



## 2. Objetivos
- Apresentar o ORM Prisma



## 3. Códigos e comandos da aula



### 3.1. Introdução ao Prisma
```bash
## baixar o repositório de código
git clone https://github.com/infoweb-pos/2024-prisma.git

## acessar a pasta do código
cd 2024-prisma/

## iniciar um projeto js
## cria o arquivo /package.json
npm init -y

## instala o typescript
## adiciona as referências das 3 bibliotecas no /package.json em devDependencies
npm install typescript ts-node @types/node --save-dev

## configura como será interpretado o ts e
## como fazer o traspiling de ts para js
## cria o arquivo /tsconfig.json
npx tsc --init

## instala o prisma
## adiciona a referência da biblioteca no /package.json em devDependencies
npm install prisma --save-dev

## configuração do prima para acessar o banco de dados com SQLite
## cria o arquivo /prisma/schema.prisma e o /.env
npx prisma init --datasource-provider sqlite

## cria uma migração e gera código cliente para o prisma
## cria o arquivo /prisma/migrations e /prisma/dev.db
npx prisma migrate dev --name introducao

## executa arquivo src/script.ts com o ts-node
## cria objetos em memória e persiste no banco de dados /prisma/dev.db
npx ts-node src/script.ts

## executa um cliente de acesso a dados na url http://localhost:5555/
npx prisma studio
```
[arquivos finais podem ser visualizados no branch 01-introducao](https://github.com/infoweb-pos/2024-prisma/tree/01-introducao/)



### 3.2 Migrate do Prisma ORM - Adicionando campos

**3.2.1** Modifica o arquivo de configuração/inicialização do prisma `prisma/schema.prisma`
[código no branch 02-migrate-adicionar-atributos](https://github.com/infoweb-pos/2024-prisma/tree/02-migrate-adicionar-atributos)
```prisma
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model User {
  id        Int     @id @default(autoincrement())
  email     String  @unique
  name      String?
  nickname  String  @unique
  passwword String  @default("321")
  posts     Post[]
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
}

```

**3.2.2** Executa o `migrate dev` para somente criar o script SQL
```bash
npx prisma migrate dev --create-only --name adicionado_apelido_senha

```

**3.2.3** Edita script SQL de migração `prisma/migrations/2024??????????_adicionado_apelido_senha/migration.sql`

```diff
--/*
--  Warnings:
--
--  - Added the required column `nickname` to the `User` table without a default value. This is not possible if the table is ---not empty.
--
--*/
-- RedefineTables
PRAGMA defer_foreign_keys=ON;
PRAGMA foreign_keys=OFF;
CREATE TABLE "new_User" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "email" TEXT NOT NULL,
    "name" TEXT,
    "nickname" TEXT NOT NULL,
    "passwword" TEXT NOT NULL DEFAULT '321'
);
--INSERT INTO "new_User" ("email", "id", "name") SELECT "email", "id", "name" FROM "User";
++INSERT INTO "new_User" ("email", "id", "name", "nickname") SELECT "email", "id", "name", "email" as "nickname" FROM "User";
DROP TABLE "User";
ALTER TABLE "new_User" RENAME TO "User";
CREATE UNIQUE INDEX "User_email_key" ON "User"("email");
CREATE UNIQUE INDEX "User_nickname_key" ON "User"("nickname");
PRAGMA foreign_keys=ON;
PRAGMA defer_foreign_keys=OFF;

```

**3.2.4** Executa o `migrate dev` para aplicar modificações no banco de dados
```bash
npx prisma migrate dev

```



### 3.3 Migrate do Prisma ORM - Adicionando novos modelos
[código no branch 03-migrate-tabelas-incluir](https://github.com/infoweb-pos/2024-prisma/tree/03-migrate-tabelas-incluir)

**3.3.1** Modifica o arquivo de configuração/inicialização do prisma `prisma/schema.prisma`
```diff
--// This is your Prisma schema file,
--// learn more about it in the docs: https://pris.ly/d/prisma-schema
--
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  nickname  String   @unique
  passwword String   @default("321")
  posts     Post[]
++  profile   Profile?
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
}
++
++model Profile {
++  id     Int     @id @default(autoincrement())
++  bio    String?
++  user   User    @relation(fields: [userId], references: [id])
++  userId Int     @unique
++}

```

**3.3.2** Executa o `migrate dev` para criar o script SQL e aplicar no banco de dados
```bash
npx prisma migrate dev --name adicionado_profile

```



### 3.4 Migrate do Prisma ORM - Modifificando nomes de tabelas

**3.4.1** Modifica o arquivo de configuração/inicialização do prisma `prisma/schema.prisma`
```diff
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  nickname  String   @unique
  passwword String   @default("321")
  posts     Post[]
  profile   Profile?
++
++  @@map("users")
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int
++
++  @@map("posts")
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
++
++  @@map("profiles")
}

```

**3.4.2** Executa o `migrate dev` para somente criar o script SQL
```bash
npx prisma migrate dev --create-only --name tabelas_renomeadas

```

**3.4.3** Edita script SQL de migração `prisma/migrations/2024??????????_tabelas_renomeadas/migration.sql`

```prisma
-- CreateTable
PRAGMA foreign_keys=off;
ALTER TABLE "User" RENAME TO "users";
ALTER TABLE "Post" RENAME TO "posts";
ALTER TABLE "Profile" RENAME TO "profiles";

-- CreateIndex
CREATE UNIQUE INDEX "users_email_key" ON "users"("email");

-- CreateIndex
CREATE UNIQUE INDEX "users_nickname_key" ON "users"("nickname");

-- CreateIndex
CREATE UNIQUE INDEX "profiles_userId_key" ON "profiles"("userId");

PRAGMA foreign_keys=on;
```

**3.4.4** Executa o `migrate dev` para aplicar modificações no banco de dados
```bash
npx prisma migrate dev

```
