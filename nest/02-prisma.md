# Nest - 2024 - Incluindo persistência no CRUD



## 1. Objetivo
1. Incluir mecanismo de persistência na API



## 2. Links

- Tutoriais
  - [Building a REST API with NestJS and Prisma Series](https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0)
  - [Nest - Documentation](https://docs.nestjs.com/)
  - [Prisma - Quick start](https://www.prisma.io/docs/getting-started/quickstart)
    - [Queries CRUD](https://www.prisma.io/docs/orm/prisma-client/queries/crud)
- Ferramentas
  - [Nest](https://nestjs.com/)
  - Banco de dados
    - [SQLite 3](https://www.sqlite.org/)
    - [PostgreSQL]()
  - [ORM (Object-Relational Mapping)](https://pt.wikipedia.org/wiki/Mapeamento_objeto-relacional)
    - [Prisma](https://www.prisma.io/)
    - [TypeORM](https://typeorm.io/)
  - Rest client - HTTP Client
    - [Postman](https://www.postman.com/product/rest-client/)
    - [VS Code extension - Rest Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)



## 3. Notas de Aula

**sumário**

3.1. Baixar o código inicial e acessar pasta do projeto

3.2. Instalar biblioteca do prisma orm

3.3. Configurar o prisma para usar o SQlite

3.4. Criar o modelo / entidade Tarefa

3.5. Programar para inserir dados iniciais com seed



### 3.1. Baixar o código inicial e acessar pasta do projeto

**código fonte inicial** [zip](https://github.com/infoweb-pos/2024-nest/archive/refs/tags/01-Tarefa-CRUD-Memoria.zip) [branch](https://github.com/infoweb-pos/2024-nest/tree/01-tarefas-crud)

pode baixar o código zip, link acima, e descompactar no computador na sua pasta de preferência.
ou pode clonar o projeto a partir do repositório e branch remoto, link também acima.

caso use o repositório remoto, lembre de fazer `checkout` para a `branch` correta, executando o comando `git checkout -b 01-tarefas-crud origin/01-tarefas-crud`.

abra o terminal e acesse a pasta onde esta o projeto inicial.
se preferir, abra o VS Code e abra a pasta do projeto diretamente, e depois abra um terminal no próprio VS Code.

**opcional** usar git e `branch` para registrar as modificações.
```bash
git checkout -b 02-tarefas-crud-prisma

```

### 3.2. Instalar biblioteca do prisma orm

para instalar o [ORM] prisma, executar o comando abaixo no terminal.

```bash
npm i -D prisma

```

**opcional** usando um repositório, guardar a modificação no arquivo de configuração do projeto.

```bash
git add package.json
git commit -m "adicionado lib prisma ao projeto"

```



### 3.3. Configurar o prisma para usar o SQlite

configurar o prisma e sqlite para o projeto, executar o comando abaixo.

```bash
npx prisma init --datasource-provider sqlite

```

a saída deve parecer com o terminal abaixo:
```console
✔ Your Prisma schema was created at prisma/schema.prisma
  You can now open it in your favorite editor.

warn You already have a .gitignore file. Don't forget to add `.env` in it to not commit any private information.

Next steps:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Run prisma db pull to turn your database schema into a Prisma schema.
3. Run prisma generate to generate the Prisma Client. You can then start querying your database.
4. Tip: Explore how you can extend the ORM with scalable connection pooling, global caching, and real-time database events. Read: https://pris.ly/cli/beyond-orm

More information in our documentation:
https://pris.ly/d/getting-started
    
```

entendendo a executação do comando, o prisma cria 2 (dois) arquivos: 
- `./.env` - arquivo exemplo com variáveis de ambiente que são recuperados na execução, e 
- `./prisma/schema.prisma` - arquivo com configuração inicial padrão pronta para colocar os modelos/entidades necessárias a API.

**opcional** usando um repositório, guardar a modificação no arquivo de configuração do prisma.

```bash
git add prisma
git commit -m "configurado o prisma para sqlite"

```



### 3.4. Criar o modelo / entidade Tarefa

edite o arquivo `./prisma/prisma.schema` para inserir o modelo/entidade `tarefa`.

```ts
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model Tarefa {
  id              Int      @id @default(autoincrement())
  titulo          String
  descricao       String?
  concluido       Boolean  @default(false)
  dataCriacao     DateTime @default(now())
  dataAtualizacao DateTime @updatedAt
}

```

```bash
npx prisma migrate dev --name "init"

```

saída do comando acima deve parecer conforme a saída abaixo.

```console
environment variables loaded from .env
Prisma schema loaded from prisma/schema.prisma
Datasource "db": SQLite database "dev.db" at "file:./dev.db"

SQLite database dev.db created at file:./dev.db

Applying migration `20240822184748_init`

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20240822184748_init/
    └─ migration.sql

Your database is now in sync with your schema.

Running generate... (Use --skip-generate to skip the generators)

✔ Generated Prisma Client (v5.18.0) to ./node_modules/.pnpm/@prisma+client

```

considerando o SQLite da configuração do prisma, a execução do `migrate dev` fez 3 (três) coisas:
1. iniciou o hitórico da evolução do banco de dados (migrações)
   - criou a pasta `./prisma/migrations`
   - criou a 1a evolução do banco de dados com o arquivo SQL `./prisma/migrations/20240822184748_init/migration.sql`
   - criou um arquivo interno do prisma para controle de migrações `./prisma/migrations/migration_lock.toml`
2. executou a evolução do banco de dados
   - criou o arquivo do SQLite `./prisma/dev.db` e `./prisma/dev.db-journal`
   - executou o arquivo SQL `./prisma/migrations/20240822184748_init/migration.sql` no banco de dados `./prisma/dev.db`
3. gerou a biblioteca `@prisma/client`, responsável pelo acesso real entre o modelo/entidade (typecript e SQLite)

para criar dados iniciais com o comando `db seed`, é preciso:
1. criar um arquivo typescript com os dados iniciais;
2. configurar o `package.json` com um campos `prisma.seed`;
3. executar o comando `npx prisma db seed`.

**opcional** usando um repositório, guardar a modificação no arquivo de configuração do projeto.

```bash
git add package.json prisma/schema.prisma prisma/migrations
git commit -m "criado o SQL e a lib cliente de acesso ao db"

```



### 3.5. Programar para inserir dados iniciais com seed

crie o arquivo typescript `./prisma/seed.ts` e modifique conforme abaixo:

```ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

const seed = async () => {
  const tarefas = await prisma.tarefa.createManyAndReturn({
    data: [
      {
        titulo: 'Criar projeto Nestjs',
        concluido: true,
      },
      {
        titulo: 'Criar endpoints de CRUD para tarefas',
        concluido: true,
      },
      {
        titulo: 'Adicionar mecanismo de persistência',
        descricao: 'prisma orm https://www.prisma.io/docs/',
      },
    ],
  });

  console.log(tarefas);
};

seed()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    // close Prisma Client at the end
    await prisma.$disconnect();
  });

```

edite o arquivo `./package.json` inserindo as linhas conforme o `diff` abaixo:

```diff
{
  "name": "2024-nest",
  "version": "0.0.1",
  "description": "",
  "author": "",
  "private": true,
  "license": "UNLICENSED",
  "scripts": {
    "build": "nest build",
    "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\"",
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",
    "lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
    "test:e2e": "jest --config ./test/jest-e2e.json"
  },
  "dependencies": {
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "@nestjs/mapped-types": "*",
    "@nestjs/platform-express": "^10.0.0",
    "@prisma/client": "5.18.0",
    "reflect-metadata": "^0.2.0",
    "rxjs": "^7.8.1"
  },
  "devDependencies": {
    "@nestjs/cli": "^10.0.0",
    "@nestjs/schematics": "^10.0.0",
    "@nestjs/testing": "^10.0.0",
    "@types/express": "^4.17.17",
    "@types/jest": "^29.5.2",
    "@types/node": "^20.3.1",
    "@types/supertest": "^6.0.0",
    "@typescript-eslint/eslint-plugin": "^8.0.0",
    "@typescript-eslint/parser": "^8.0.0",
    "eslint": "^8.42.0",
    "eslint-config-prettier": "^9.0.0",
    "eslint-plugin-prettier": "^5.0.0",
    "jest": "^29.5.0",
    "prettier": "^3.0.0",
    "prisma": "^5.18.0",
    "source-map-support": "^0.5.21",
    "supertest": "^7.0.0",
    "ts-jest": "^29.1.0",
    "ts-loader": "^9.4.3",
    "ts-node": "^10.9.1",
    "tsconfig-paths": "^4.2.0",
    "typescript": "^5.1.3"
  },
  "jest": {
    "moduleFileExtensions": [
      "js",
      "json",
      "ts"
    ],
    "rootDir": "src",
    "testRegex": ".*\\.spec\\.ts$",
    "transform": {
      "^.+\\.(t|j)s$": "ts-jest"
    },
    "collectCoverageFrom": [
      "**/*.(t|j)s"
    ],
    "coverageDirectory": "../coverage",
    "testEnvironment": "node"
++  },
++  "prisma": {
++    "seed": "ts-node prisma/seed.ts"
  }
}

```

após isso, execute o comando `npx prisma db seed` e sua saída deverá ser parecida com o terminal abaixo:

```console
Environment variables loaded from .env
Running seed command `ts-node prisma/seed.ts` ...
[
  {
    id: 1,
    titulo: 'Criar projeto Nestjs',
    descricao: null,
    concluido: true,
    dataCriacao: 2024-08-24T12:09:49.213Z,
    dataAtualizacao: 2024-08-24T12:09:49.213Z
  },
  {
    id: 2,
    titulo: 'Criar endpoints de CRUD para tarefas',
    descricao: null,
    concluido: true,
    dataCriacao: 2024-08-24T12:09:49.213Z,
    dataAtualizacao: 2024-08-24T12:09:49.213Z
  },
  {
    id: 3,
    titulo: 'Adicionar mecanismo de persistência',
    descricao: 'prisma orm https://www.prisma.io/docs/',
    concluido: false,
    dataCriacao: 2024-08-24T12:09:49.213Z,
    dataAtualizacao: 2024-08-24T12:09:49.213Z
  }
]

🌱  The seed command has been executed.

```

execute novamente `npx prisma db seed` e veja com `npx prisma studio` que os dados foram duplicados.
para evitar isso, vamos inserir o código no início para apagar todos os dados antes de inserir novos, conforme diff abaixo:
```diff
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

const seed = async () => {
++  await prisma.tarefa.deleteMany({});
++
  const tarefas = await prisma.tarefa.createManyAndReturn({
    data: [
      {
        titulo: 'Criar projeto Nestjs',
        concluido: true,
      },
      {
        titulo: 'Criar endpoints de CRUD para tarefas',
        concluido: true,
      },
      {
        titulo: 'Adicionar mecanismo de persistência',
        descricao: 'prisma orm https://www.prisma.io/docs/',
      },
    ],
  });

  console.log(tarefas);
};

seed()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    // close Prisma Client at the end
    await prisma.$disconnect();
  });

```

agora execute novamente `npx prisma db seed` e veja o resultado no navegador do prisma studio.
dica, se for consultar sempre o prisma studio, abra um novo terminal e deixe executando `npx prisma studio`.

por úlitmo na inserção de dados, será inserido uma tarefa individualmente para podermos ter os dados completos desta tarefa, como o _id_.
lembre de executar novamente `npx prisma db seed` e conferir o resultado em prisma studio.
```diff
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

const seed = async () => {
  await prisma.tarefa.deleteMany({});

++  const tarefa = await prisma.tarefa.create({
++    data: {
++      titulo: 'Adicionar autenticação por email e senha na API',
++    },
++  });
++  console.log(tarefa);
++
  const tarefas = await prisma.tarefa.createManyAndReturn({
    data: [
      {
        titulo: 'Criar projeto Nestjs',
        concluido: true,
      },
      {
        titulo: 'Criar endpoints de CRUD para tarefas',
        concluido: true,
      },
      {
        titulo: 'Adicionar mecanismo de persistência',
        descricao: 'prisma orm https://www.prisma.io/docs/',
      },
    ],
  });

  console.log(tarefas);
};

seed()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    // close Prisma Client at the end
    await prisma.$disconnect();
  });

```

o último comando executado deverá apresentar a saída no terminal conforme abaixo:
```console
npx prisma db seed
Environment variables loaded from .env
Running seed command `ts-node prisma/seed.ts` ...
{
  id: 1,
  titulo: 'Adicionar autenticação por email e senha na API',
  descricao: null,
  concluido: false,
  dataCriacao: 2024-08-24T15:54:21.633Z,
  dataAtualizacao: 2024-08-24T15:54:21.633Z
}
[
  {
    id: 2,
    titulo: 'Criar projeto Nestjs',
    descricao: null,
    concluido: true,
    dataCriacao: 2024-08-24T15:54:21.654Z,
    dataAtualizacao: 2024-08-24T15:54:21.654Z
  },
  {
    id: 3,
    titulo: 'Criar endpoints de CRUD para tarefas',
    descricao: null,
    concluido: true,
    dataCriacao: 2024-08-24T15:54:21.654Z,
    dataAtualizacao: 2024-08-24T15:54:21.654Z
  },
  {
    id: 4,
    titulo: 'Adicionar mecanismo de persistência',
    descricao: 'prisma orm https://www.prisma.io/docs/',
    concluido: false,
    dataCriacao: 2024-08-24T15:54:21.654Z,
    dataAtualizacao: 2024-08-24T15:54:21.654Z
  }
]

🌱  The seed command has been executed.

```

**opcional** usando um repositório, guardar a modificação no arquivo de configuração do prisma.

```bash
git add package.json prisma/seed.ts
git commit -m "criado script para inserção de dados iniciais"

```



### 3.6. Criar o módulo de persistência da API

substituindo a persistência em memória pelo prisma:
1. Criar o serviço e módulo de persistência
2. Adicionar prisma ao serviço de persistência e exportar o serviço para os outros módulos da API
3. Importar módulo de persistência no módulo de tarefas
4. Importar serviço de persistência no serviço de tarefas
5. Usar o serviço de persistência no serviço de tarefas



### 3.6.1. Criar o serviço e módulo de persistência

FIXME
```bash
npx nest generate module persistencia

```

```console
CREATE src/persistencia/persistencia.module.ts (89 bytes)
UPDATE src/app.module.ts (413 bytes)

```

```bash
npx nest generate service persistencia --no-spec

```

```console
CREATE src/persistencia/persistencia.service.ts (96 bytes)
UPDATE src/persistencia/persistencia.module.ts (187 bytes)
(node:288385) [DEP0051] DeprecationWarning

```

```bash
git add src/app.module.ts src/persistencia
git commit -m "adicionado módulo e serviço de persistência"

```

### 3.6.2. Adicionar prisma ao serviço de persistência e exportar o serviço para os outros módulos da API

no serviço de tarefas, arquivo `./src/persistencia/persistencia.service.ts`, usar a lib @prisma/client para extender a classe que implementa o serviço, conforme `diff` abaixo:

```diff
import { Injectable } from '@nestjs/common';
++import { PrismaClient } from '@prisma/client';

@Injectable()
--export class PersistenciaService {}
++export class PersistenciaService extends PrismaClient {}

```


no módulo de tarefas, arquivo `./src/persistencia/persistencia.module.ts` 

```diff
import { Module } from '@nestjs/common';
import { PersistenciaService } from './persistencia.service';

@Module({
  providers: [PersistenciaService],
  exports: [PersistenciaService],
})
export class PersistenciaModule {}

```

**opcional** FIXME
```bash
git add src/persistencia/persistencia.module.ts src/persistencia/persistencia.service.ts
git commit -m "módulo de persistência programado para extender prisma"

```



### 3.6.3. Importar módulo de persistência no módulo de tarefas

`./src/tarefas/tarefas.module.ts` FIXME
```diff
import { Module } from '@nestjs/common';
import { TarefasService } from './tarefas.service';
import { TarefasController } from './tarefas.controller';
++import { PersistenciaModule } from 'src/persistencia/persistencia.module';

@Module({
  controllers: [TarefasController],
  providers: [TarefasService],
++  imports: [PersistenciaModule],
})
export class TarefasModule {}

```

**opcional** FIXME
```bash
git add src/tarefas/tarefas.module.ts`
git commit -m "módulo de persistência importado no módulo de tarefas"

```



### 3.6.4. Importar serviço de persistência no serviço de tarefas

`./src/tarefas/tarefas.service.ts` FIXME
```diff
import { Injectable } from '@nestjs/common';
import { CreateTarefaDto } from './dto/create-tarefa.dto';
import { UpdateTarefaDto } from './dto/update-tarefa.dto';
import { TarefaEntity } from './entities/tarefa.entity';
++import { PersistenciaService } from 'src/persistencia/persistencia.service';

@Injectable()
export class TarefasService {
  tarefas: TarefaEntity[] = [];
  contador: number = 0;
++
++  constructor(private persistencia: PersistenciaService) {}

  create(createTarefaDto: CreateTarefaDto) {
    const tarefa = {
      id: this.contador + 1,
      ...createTarefaDto,
    };
    this.contador += 1;
    this.tarefas.push(tarefa);
    return {
      estado: 'ok',
      dados: tarefa,
    };
  }

  findAll() {
    return {
      estado: 'ok',
      dados: this.tarefas,
    };
  }

  findOne(id: number) {
    const temp = this.tarefas.filter((tarefa) => tarefa.id === id);
    if (temp[0]) {
      return {
        estado: 'ok',
        dados: temp[0],
      };
    } else {
      return {
        estado: 'nok',
        mensagem: `tarefa com #${id} não existe!`,
      };
    }
  }

  update(id: number, updateTarefaDto: UpdateTarefaDto) {
    const index = this.tarefas.findIndex((tarefa) => tarefa.id === id);
    if (index >= 0) {
      this.tarefas[index] = {
        ...this.tarefas[index],
        ...updateTarefaDto,
      };
      return {
        estado: 'ok',
        dados: this.tarefas[index],
      };
    }
    return {
      estado: 'nok',
      mensagem: `tarefa com #${id} não existe!`,
    };
  }

  remove(id: number) {
    const index = this.tarefas.findIndex((tarefa) => tarefa.id === id);
    if (index >= 0) {
      const tarefasRemovidas = this.tarefas.splice(index, 1);
      return {
        estado: 'ok',
        dados: tarefasRemovidas[0],
      };
    } else {
      return {
        estado: 'nok',
        mensagem: `tarefa com #${id} não existe!`,
      };
    }
  }
}

```

**opcional** FIXME
```bash
git add 
git commit -m "serviço de persistência injetado no serviço de tarefas"

```

FIXME

### 3.6.5. Usar o serviço de persistência no serviço de tarefas

as mudanças agora será no arquivo `./src/tarefas/tarefas.service.ts` usando o serviço de persistência que encapsula o `PrismaClient`, conforme `diff` abaixo:
```diff
import { Injectable } from '@nestjs/common';
import { CreateTarefaDto } from './dto/create-tarefa.dto';
import { UpdateTarefaDto } from './dto/update-tarefa.dto';
import { TarefaEntity } from './entities/tarefa.entity';
import { PersistenciaService } from 'src/persistencia/persistencia.service';

@Injectable()
export class TarefasService {
  tarefas: TarefaEntity[] = [];
  contador: number = 0;

  constructor(private persistencia: PersistenciaService) {}

--  create(createTarefaDto: CreateTarefaDto) {
--     const tarefa = {
--       id: this.contador + 1,
--       ...createTarefaDto,
--     };
--     this.contador += 1;
--     this.tarefas.push(tarefa);
++  async create(createTarefaDto: CreateTarefaDto) {
++    const tarefa = await this.persistencia.tarefa.create({
++      data: createTarefaDto,
++    });
++    return {
++      estado: 'ok',
++      dados: tarefa,
++    };
  }

--  findAll() {
--     return {
--       estado: 'ok',
--       dados: this.tarefas,
--     };
++  async findAll() {
++    return {
++      estado: 'ok',
++      dados: await this.persistencia.tarefa.findMany({}),
++    };
  }

--  findOne(id: number) {
--     const temp = this.tarefas.filter((tarefa) => tarefa.id === id);
--     if (temp[0]) {
--       return {
--         estado: 'ok',
--         dados: temp[0],
--       };
--     } else {
--       return {
--         estado: 'nok',
--         mensagem: `tarefa com #${id} não existe!`,
--       };
--     }
++  async findOne(id: number) {
++    const tarefa = await this.persistencia.tarefa.findUnique({
++      where: { id: id },
++    });
++    if (tarefa) {
++      return {
++        estado: 'ok',
++        dados: tarefa,
++      };
++    } else {
++      return {
++        estado: 'nok',
++        mensagem: `tarefa com #${id} não existe!`,
++      };
    }
  }

--  update(id: number, updateTarefaDto: UpdateTarefaDto) {
--     const index = this.tarefas.findIndex((tarefa) => tarefa.id === id);
--     if (index >= 0) {
--       this.tarefas[index] = {
--         ...this.tarefas[index],
--         ...updateTarefaDto,
--       };
--       return {
--         estado: 'ok',
--         dados: this.tarefas[index],
--       };
--     }
--     return {
--       estado: 'nok',
--       mensagem: `tarefa com #${id} não existe!`,
--     };
++  async update(id: number, updateTarefaDto: UpdateTarefaDto) {
++    const resultado = await this.persistencia.tarefa
++      .update({
++        where: { id: id },
++        data: updateTarefaDto,
++      })
++      .then((tarefa) => {
++        return {
++          estado: 'ok',
++          dados: tarefa,
++        };
++      })
++      .catch((error) => {
++        return {
++          estado: 'nok',
++          mensagem: `tarefa com #${id} não existe!`,
++        };
++      });
++    return resultado;
  }

--  remove(id: number) {
--     const index = this.tarefas.findIndex((tarefa) => tarefa.id === id);
--     if (index >= 0) {
--       const tarefasRemovidas = this.tarefas.splice(index, 1);
--       return {
--         estado: 'ok',
--         dados: tarefasRemovidas[0],
--       };
--     } else {
--       return {
--         estado: 'nok',
--         mensagem: `tarefa com #${id} não existe!`,
--       };
--     }
++  async remove(id: number) {
++    const resultado = await this.persistencia.tarefa
++      .delete({ where: { id } })
++      .then((tarefa) => {
++        return { estado: 'ok', dados: tarefa };
++      })
++      .catch((error) => {
++        return {
++          estado: 'nok',
++          mensagem: `tarefa com #${id} não existe!`,
++        };
++      });
++    return resultado;
  }
}

```

FIXME explicar e linkar os métodos do prisma