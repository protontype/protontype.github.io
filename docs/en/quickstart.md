#Quick Start

##Project configuration

```bash
    mkdir proton-quickstart
    cd proton-quickstart
    npm init
    npm install typescript -g
    npm install protontype --save
    npm install sqlite3 --save
    mkdir src
```

Create tsconfig.json file in project root folder

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
Create proton.json file in project root folder
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

Create file **src/models/TasksModel.ts**

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

Create file **src/middlewares/TasksMiddleware.ts**

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

Create file **src/routers/TasksRouter.ts**

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

Create file **src/Main.ts**

```typescript
import { TasksRouter } from './routers/TasksRouter';
import { ProtonApplication } from 'protontype';

new ProtonApplication()
    .addRouter(new TasksRouter())
    .start();
```
 
**Compiling and Running**
```bash
    tsc
    node dist/Main.js
```
 
##Testing API

By default, the application uses a sqlite database.
Will be created proton.sqlite file in root project file.

The endpoints below will now be available:

-   **GET http://localhost:3001/tasks** - List all records of the Particles table
-   **POST http://localhost:3001/tasks** - Creates a record in the Particles table
-   **GET http://localhost:3001/tasks/:id** - Queries a record of the Particles table
-   **PUT http://localhost:3001/tasks/:id** - Updates a record of the Particles table
-   **DELETE http://localhost:3001/tasks/:id** - Removes a record of the Particles table

Will be test using [Postman](https://www.getpostman.com/ "") app or other.

##Repository

<https://github.com/protontype/protontype-example>