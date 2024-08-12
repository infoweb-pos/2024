# Nest - 2024 - Introdução

[gravação da aula](https://youtu.be/2aKe4ZYvMZU)

## Objetivo
1. Introdução a desenvolvimento de API com NestJS
2. Criar novo endpoint `tarefas` com verbos CRUD

## Links

- Repositório de códido da aula
  - [2024-nest](https://github.com/infoweb-pos/2024-nest)
- Tutoriais
  - [Criando o primeiro CRUD com NestJS](https://www.treinaweb.com.br/blog/criando-o-primeiro-crud-com-nestjs)
  - [Nest - Documentation](https://docs.nestjs.com/)
  - [Prisma - Quick start](https://www.prisma.io/docs/getting-started/quickstart)
- Ferramentas
  - [Nest](https://nestjs.com/)
  - Banco de dados
    - [SQLite 3](https://www.sqlite.org/)
    - [Prisma](https://www.prisma.io/)
  - Rest client - HTTP Client
    - [Postman](https://www.postman.com/product/rest-client/)
    - [VS Code extension - Rest Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

## Passos

```bash
## Instalar o nestjs
npm i -g @nestjs/cli

## Criar novo projeto nest
nest new 2024-nest-introducao
cd 2024-nest-introducao

## instalar a biblioteca do sqlite3 e prisma
npm i sqlite3 @prisma/client

## executei a api
npm run start:dev

## criar endpoints de tarefas
nest g res tarefas
# Rest API -> modelo do endpoint
# Y -> verbos do CRUD

```

