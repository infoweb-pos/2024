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

### 3.2 Migrate do Prisma ORM

**em desenvolvimento**
```bash
npx prisma migrate dev --create-only --name adicionado_titutlo_trabalho

npx prisma studio
```


