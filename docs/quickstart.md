#Quick Start

##Estrutura de pastas e configurações iniciais

```bash
    mkdir proton-quickstart
    cd proton-quickstart
    npm init
    npm install typescript -g
    npm install protontype --save
    npm install sqlite3 --save
    mkdir src
```

Criar o arquivo tsconfig.json na raiz do projeto

```json

    {
      "compilerOptions": {
        "target": "es6",
        "module": "commonjs",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "esModuleInterop": true,
        "outDir": "dist"
      },
      "exclude": [
        "node_modules",
        "dist"
      ]
    }
    
```

Criar arquivo proton.json na raiz do projeto
```json
{
  "servers": [
    {
      "port": 3001,
      "useHttps": false
    }
  ],
  "database": {
    "name": "defaultTestConnection",
    "type": "sqlite",
    "database": "./proton.sqlite",
    "synchronize": true,
    "logging": false,
    "entities": [
      "./dist/models/**/*.js"
    ]
  },
  "defaultRoutes": true,
}
```

##Model

Criar um arquivo **src/models/TasksModel.ts**

```typescript
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class TasksModel {
    @PrimaryGeneratedColumn()
    id: number;
    @Column({ nullable: true })
    title: string;
    @Column()
    done: boolean;
    @Column({ nullable: true })
    userId: number;
}
```

##Middleware
Criar um arquivo **src/middlewares/TasksMiddleware.ts**
```typescript
import { ProtonMiddleware, Middleware, MiddlewareFunctionParams } from "protontype";

export class TasksMiddleware extends ProtonMiddleware {

    @Middleware()
    sayHello(params: MiddlewareFunctionParams) {
        console.log("Hello!");
        params.next();
    }
}
```

##Router

Criar arquivo **src/routers/TasksRouter.ts**

```typescript
import { RouterClass, TypeORMCrudRouter, BodyParserMiddleware } from 'protontype';

import { TasksModel } from '../models/TasksModel';
import { TasksMiddleware } from '../middlewares/TasksMiddleware';

@RouterClass({
    baseUrl: "/tasks",
    model: TasksModel,
    middlewares: [new TasksMiddleware()]
})
export class TasksRouter extends TypeORMCrudRouter {

}
```

 

##Main

Criar arquivo **src/Main.ts**

```typescript
import { TasksRouter } from './routers/TasksRouter';
import { ProtonApplication } from 'protontype';
/**
 * @author Humberto Machado
 *
 */
new ProtonApplication()
    .addRouter(new TasksRouter())
    .start();
```
 

**Compilando e Rodando Aplicação**
```bash
    tsc
    node dist/Main.js
```
 
##Testando a API

Por padrão, a aplicação usará um banco de dados sqlite. 
Será criado um arquivo proton.sqlite na raiz do projeto.

Os endpoints abaixo já estarão disponíveis:

-   **GET http://localhost:3001/tasks** - Lista todos os registos da tabela Particles
-   **POST http://localhost:3001/tasks** - Cria um registro na tabela Particles
-   **GET http://localhost:3001/tasks/:id** - Consulta um registro da tabela Particles
-   **PUT http://localhost:3001/tasks/:id** - Atualiza um registro da tabela Particles
-   **DELETE http://localhost:3001/tasks/:id** - Remove um registro da tabela Particles

Poderá testar através do app [Postman](https://www.getpostman.com/ "") ou outro da sua preferência.

##Repositório

<https://github.com/protontype/protontype-sample>