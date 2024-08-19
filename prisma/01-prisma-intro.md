# Introdução a Prisma ORM

[vídeo da aula](https://youtu.be/-JvDDYCEZX0)



## 1. Link
- [Repositório de código](https://github.com/infoweb-pos/2024-prisma)
- tutoriais
  - [getting started](https://www.prisma.io/docs/getting-started/quickstart)
  - [migrate - getting start](https://www.prisma.io/docs/orm/prisma-migrate/getting-started)
- ferramentas
  - [prisma orm](https://www.prisma.io/)
  - [extensão do vs code](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma)



## 2. Objetivos
- Apresentar o ORM Prisma




## 3. Códigos e comandos da aula




### 3.1. Introdução ao Prisma

[arquivos finais podem ser visualizados no branch 01-introducao](https://github.com/infoweb-pos/2024-prisma/tree/01-introducao/)

**3.1.1.** Inicializando o projeto com a linha de comando
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

## Abrir o VS Code com a pasta do projeto
## Nos labs do IFRN-CNAT o comando pode ser
## vscode .
code .
```

**3.1.2** Edite o arquivo de schema do prisma `prisma/schema.prisma` para adicionar os modelos

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
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
  posts Post[]
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

**3.1.3** Execute a migração nomeando como `introducao`

```bash
## cria uma migração e gera código cliente para o prisma
## cria o arquivo /prisma/migrations e /prisma/dev.db
npx prisma migrate dev --name introducao

```

**3.1.4** Crie, edite e execute o acesso ao dados

arquivo `src/script.ts`

```ts
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

async function main() {
	// ... you will write your Prisma Client queries here
	let user = await prisma.user.create({
		data: {
			name: "Alice",
			email: "alice@prisma.io",
		},
	});
	console.log(user);
	user = await prisma.user.create({
		data: {
			name: "Bob",
			email: "bob@prisma.io",
			posts: {
				create: [
					{
						title: "Hello World",
						published: true,
					},
					{
						title: "My second post",
						content: "This is still a draft",
					},
				],
			},
		},
	});
	console.log(user);

	const users = await prisma.user.findMany();
	console.log(users);
}

main()
	.then(async () => {
		await prisma.$disconnect();
	})
	.catch(async (e) => {
		console.error(e);
		await prisma.$disconnect();
		process.exit(1);
	});

```

executar no terminal

```bash
## executa arquivo src/script.ts com o ts-node
## cria objetos em memória e persiste no banco de dados /prisma/dev.db
npx ts-node src/script.ts

```

**3.1.5** Visualize os dados numa interface web

```bash
## executa um cliente de acesso a dados na url http://localhost:5555/
npx prisma studio

```




### 3.2 Migrate do Prisma ORM - Adicionando campos

[código no branch 02-migrate-adicionar-atributos](https://github.com/infoweb-pos/2024-prisma/tree/02-migrate-adicionar-atributos)

**3.2.1** Modifica o arquivo de configuração/inicialização do prisma `prisma/schema.prisma`

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

[código no branch 04-migrate-tabelas-renomear](https://github.com/infoweb-pos/2024-prisma/tree/04-migrate-tabelas-renomear)

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



### 3.5. Mudando para postgresql e fazendo hosting no vercel

[código da aula no branc 05-postgres-hosting](https://github.com/infoweb-pos/2024-prisma/tree/05-postgres-hosting)

**3.5.1** Crie um _storage_ postgresql no Vercel

**3.5.2** Renomeie a pasta `prisma/migrations` para `prisma/sqlite`

**3.5.3** Modifique o `provider` no arquivo `prisma/schema.prisma`

```diff
generator client {
  provider = "prisma-client-js"
}

datasource db {
--  provider = "sqlite"
++  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  nickname  String   @unique
  passwword String   @default("321")
  email2    String?
  posts     Post[]
  profile   Profile?

  @@map("usuarios")
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  subtitle  String?
  published Boolean @default(false)
  author    User    @relation(fields: [authorId], references: [id])
  authorId  Int

  @@map("postagens")
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique

  @@map("perfis")
}

```

**3.5.4** Copie a URL de conexão do Vercel para o arquivo .env

Lembre que a sequência da URL é `usuário`:`senha`@`domínio`:5432/verceldb, onde estão representadas abaixo por `????`.
**Cuidado** para não publicar o arquivo `.env` e tornar público seu login e senha do postgres no vercel.

```diff
--# Environment variables declared in this file are automatically made available to Prisma.
--# See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema
--
--# Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
--# See the documentation for all the connection string options: https://pris.ly/d/connection-strings
--
--DATABASE_URL="file:./dev.db"
++DATABASE_URL="postgres://????:????@????:5432/verceldb"

```

**3.5.5** Publique os modelos/tabelas/dados no postgresql/vercel

```bash
## cria scripts SQL e gera o cliente de acesso específico para o postgres
npx prisma migrate dev --name postgres_inicializacao

# executa os códigos ts para inserir dados e consultá-los
npx ts-node src/script.ts
```
