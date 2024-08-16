# Introdução a Prisma ORM

[vídeo da aula](https://youtu.be/-JvDDYCEZX0)

## Link
- [Repositório de código](https://github.com/infoweb-pos/2024-prisma)
- tutoriais
  - [getting started](https://www.prisma.io/docs/getting-started/quickstart)
  - [migrate - getting start](https://www.prisma.io/docs/orm/prisma-migrate/getting-started)
  - [Building a REST API with NestJS and Prisma](https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0)
- ferramentas
  - [prisma orm](https://www.prisma.io/)
  - [extensão do vs code](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma)
## Objetivos
- Apresentar o ORM Prisma

## Códigos e comandos da aula

### Introdução ao Prisma
```bash
## baixar o repositório de código
git clone https://github.com/infoweb-pos/2024-prisma.git

## acessar a pasta do código
cd 2024-prisma/

## iniciar um projeto js
npm init -y

## instala o typescript
npm install typescript ts-node @types/node --save-dev

## colocar como será interpretado o ts e
## como fazer o traspiling de ts para js
npx tsc --init

npm install prisma --save-dev

npx prisma init --datasource-provider sqlite

npx prisma studio
```

### Migrate do Prisma ORM
```bash
npx prisma migrate dev --create-only --name adicionado_titutlo_trabalho

npx prisma studio
```


