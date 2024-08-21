# Nest - 2024 - Introdução

[gravação da aula](https://youtu.be/2aKe4ZYvMZU)

## 1. Objetivo
1. Introdução a desenvolvimento de API com NestJS
2. Criar novo endpoint `tarefas` com verbos CRUD

## 2. Links

- Repositório de códido da aula
  - [2024-nest](https://github.com/infoweb-pos/2024-nest)
- Tutoriais
  - [Nest - Documentation](https://docs.nestjs.com/)
    - [First steps](https://docs.nestjs.com/first-steps)
  - [Prisma - Quick start](https://www.prisma.io/docs/getting-started/quickstart)
    - [Building a REST API with NestJS and Prisma](https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0)
- Ferramentas
  - [nest js](https://nestjs.com/)
  - Rest client - HTTP Client
    - [Postman](https://www.postman.com/product/rest-client/)
    - [VS Code extension - Rest Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)


## 3. Notas de aula

### 3.1. Criar e executar projeto

[código 3.1 - branch main](https://github.com/infoweb-pos/2024-nest/tree/main)

no terminal do sistema operacional, executar o comando

```bash
npx @nestjs/cli new 2024-nest

```

a resposta para a pergunta sobre qual gerenciador usar, deve ser `npm`

```bash
## respostas

? Which package manager would you ❤️  to use? npm

```

para executar o `api`, execute o comando abaixo

```bash
cd 2024-nest
npm run start:dev

```

a saída para o comando deve ser como abaixo

```console
[15:03:04] Starting compilation in watch mode...

[15:03:06] Found 0 errors. Watching for file changes.

[Nest] 11517  - 20/08/2024, 15:03:07     LOG [NestFactory] Starting Nest application...
[Nest] 11517  - 20/08/2024, 15:03:07     LOG [InstanceLoader] AppModule dependencies initialized +16ms
[Nest] 11517  - 20/08/2024, 15:03:07     LOG [RoutesResolver] AppController {/}: +7ms
[Nest] 11517  - 20/08/2024, 15:03:07     LOG [RouterExplorer] Mapped {/, GET} route +2ms
[Nest] 11517  - 20/08/2024, 15:03:07     LOG [NestApplication] Nest application successfully started +2ms

```

lembrando que o único `endpoint` é o raiz e que a porta é `3000`.
por isso, o único endereço ativo é `http://localhost:3000/`


para testar este endereçõ, use a extensão do vs code [rest client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client).

1. crie um arquivo `./rest-client/root.http`
2. instale o [rest client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) no vs code
3. edite colando o conteúdo abaixo no arquivo
4. execute como [rest client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

```http
### root
GET http://localhost:3000/

```

a saída deve ser como abaixo, normalmente numa nova janela

```http
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-Lve95gjOVATpfV8EL5X4nxwjKHE"
Date: Tue, 20 Aug 2024 18:38:55 GMT
Connection: close

Hello World!

```
### 3.2. Criar endpoints para o CRUD de tarefas

[código 3.2 - branch 01-tarefas-crud](https://github.com/infoweb-pos/2024-nest/tree/01-tarefas-crud)

para criar os recursos para o CRUD de tarefas, execute o comando abaixo em um terminal.

NÃO é necessário encerrar a execução da api.

```bash
## criar endpoints de tarefas
nest g res tarefas

```

as respostas e saídas do comando acima, devem ser como abaixo

```bash
## perguntas e respostas
? What transport layer do you use? REST API
? Would you like to generate CRUD entry points? Yes

## saída do comando
CREATE src/tarefas/tarefas.controller.ts (936 bytes)
CREATE src/tarefas/tarefas.module.ts (262 bytes)
CREATE src/tarefas/tarefas.service.ts (637 bytes)
CREATE src/tarefas/dto/create-tarefa.dto.ts (32 bytes)
CREATE src/tarefas/dto/update-tarefa.dto.ts (177 bytes)
CREATE src/tarefas/entities/tarefa.entity.ts (23 bytes)
UPDATE src/app.module.ts (320 bytes)

```

após a execução do comando, a saída do terminal que esta executando a API deverá esta como o terminal abaixo.

```bash
[15:32:20] File change detected. Starting incremental compilation...

[15:32:20] Found 0 errors. Watching for file changes.

[Nest] 14003  - 20/08/2024, 15:32:20     LOG [NestFactory] Starting Nest application...
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [InstanceLoader] AppModule dependencies initialized +10ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [InstanceLoader] TarefasModule dependencies initialized +0ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RoutesResolver] AppController {/}: +4ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/, GET} route +3ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RoutesResolver] TarefasController {/tarefas}: +0ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/tarefas, POST} route +1ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/tarefas, GET} route +0ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/tarefas/:id, GET} route +1ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/tarefas/:id, PATCH} route +0ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [RouterExplorer] Mapped {/tarefas/:id, DELETE} route +1ms
[Nest] 14003  - 20/08/2024, 15:32:20     LOG [NestApplication] Nest application successfully started +2ms

```

para testar, use o [rest client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) com os endereços abaixo.

salve os endereços no arquivo `./rest-client/tarefas.http`

```http
### GET recupera todas as tarefas
GET http://localhost:3000/tarefas/

### GET recupera a tarefa de id 1
GET http://localhost:3000/tarefas/1

### POST cria uma nova tarefa
POST http://localhost:3000/tarefas

### DELETE apaga a tarefa de id 1
DELETE http://localhost:3000/tarefas/1

### PATCH atualiza a tarefa de id 1
PATCH http://localhost:3000/tarefas/1

```

as saídas para os comandos acima devem estar conforme as saídas abaixo.

verbo **get** para todas as tarefas
```http
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 31
ETag: W/"1f-UUEj2omBuP9Dbd1uus2e4t+2wTY"
Date: Tue, 20 Aug 2024 18:49:38 GMT
Connection: close

This action returns all tarefas

```

verbo **get** para a tarefa de id 1
```http
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 31
ETag: W/"1f-wq4ufk1bayHWCKLBCShocPfZXBA"
Date: Tue, 20 Aug 2024 18:50:47 GMT
Connection: close

This action returns a #1 tarefa

```

verbo **post** para criar nova tarefa
```http
HTTP/1.1 201 Created
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 29
ETag: W/"1d-hWfUaGusnbHS1I/1dUNR2Td+Img"
Date: Wed, 21 Aug 2024 00:59:56 GMT
Connection: close

This action adds a new tarefa

```

verbo **delete** para a tarefa de id 1
```http
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 31
ETag: W/"1f-uYyDWwZtwwU3xQzxFtb8NsZ2nuE"
Date: Tue, 20 Aug 2024 18:51:04 GMT
Connection: close

This action removes a #1 tarefa

```

verbo **patch** para atualizar a tarefa de id 1
```http
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 31
ETag: W/"1f-3MCDa+NYfMJ+6OXQbrNH7KSzWeI"
Date: Tue, 20 Aug 2024 18:51:21 GMT
Connection: close

This action updates a #1 tarefa

```

modificando o código `./src/tarefas/tarefas.service.ts` para implementar persistência em memória com `array` do typescript.

primeiro, modificar a entidade e dto do recurso `tarefas`.

`./src/tarefas/dto/create-tarefa.dto.ts` responsável por tipar os dados `json` que serão enviados para o `endpoint` de criar uma nova tarefa.

```ts
export class CreateTarefaDto {
  titulo: string;
  descricao?: string;
  concluido?: boolean = false;
}

```

`./src/tarefas/dto/update-tarefa.dto.ts` responsável por tipar os dados `json` que serão enviados para o `endpoint` de atualizar uma tarefa.

```ts
import { PartialType } from '@nestjs/mapped-types';
import { CreateTarefaDto } from './create-tarefa.dto';

export class UpdateTarefaDto extends PartialType(CreateTarefaDto) {
  titulo: string;
  descricao?: string;
}

```

`./src/tarefas/entities/tarefa.entity.ts` responsável por tipar os dados usados dentro do projeto.

```ts
export class TarefaEntity {
  id: number;
  titulo: string;
  descricao?: string;
  concluido?: boolean = false;
}

```

por último, a implementação do código do `service` para as tarefas.

```ts
import { Injectable } from '@nestjs/common';
import { CreateTarefaDto } from './dto/create-tarefa.dto';
import { UpdateTarefaDto } from './dto/update-tarefa.dto';
import { TarefaEntity } from './entities/tarefa.entity';

@Injectable()
export class TarefasService {
  tarefas: TarefaEntity[] = [];
  contador: number = 0;

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
      }
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

para testar, o arquivo `./rest-client/tarefas.http` foi atualizado com os dados `json` para criar e atualizar uma tarefa.

```http
### GET recupera todas as tarefas
GET http://localhost:3000/tarefas/

### GET recupera a tarefa de id 1
GET http://localhost:3000/tarefas/1

### POST criar uma tarefa com todos os dados
POST http://localhost:3000/tarefas/
content-type: application/json

{
  "titulo": "uma tarefa nova",
  "descricao": "descrição da nova tarefa",
  "concluido": false
}

### POST criar uma tarefa apenas com título
POST http://localhost:3000/tarefas/
content-type: application/json

{
  "titulo": "uma tarefa nova"
}

### DELETE apaga a tarefa de id 1
DELETE http://localhost:3000/tarefas/1

### PATCH atualiza a tarefa de id 1
PATCH http://localhost:3000/tarefas/1
content-type: application/json

{
  "titulo": "título modificado",
  "descricao": "uma descrição da tarefa",
  "concluido": true
}

```
