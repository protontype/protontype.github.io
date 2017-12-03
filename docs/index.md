#
<div align="center">
  <a href="https://protontype.github.io/protontype-docs/">
    <img src="assets/images/logo_small.png" width="200" height="200">
  </a>
</div>

Um simples web framework feito em TypeScript.

O ProtonType tem como objetivo tornar simples e agradável o desenvolvimento de APIs REST e criação de modelos de banco de dados.

## Documentação de referência da API
<https://linck.github.io/protontype-api-reference/>

## Instalação
```bash
npm install protontype --save
```
 
## Models

```javascript
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class TasksModel {
    @PrimaryGeneratedColumn()
    id: number;
    @Column({ nullable: true })
    title: string;
    @Column()
    done: boolean;
    @Column()
    userId: number;
}
```

## Router

```javascript
import { RouterClass, TypeORMCrudRouter, BodyParserMiddleware } from 'protontype';
import { TasksModel } from '../models/TasksModel';

@RouterClass({
    baseUrl: "/tasks",
    model: TasksModel
})
export class TasksRouter extends TypeORMCrudRouter {

}
```

## Middlewares

```javascript
import { ProtonMiddleware, Middleware, MiddlewareFunctionParams } from "protontype";

export class TasksMiddleware extends ProtonMiddleware {

    @Middleware()
    sayHello(params: MiddlewareFunctionParams) {
        console.log("Hello!");
        params.next();
    }
}
```

## Iniciando

```javascript
    import { ParticlesRouter } from './ParticlesRouter';
    import { ProtonApplication } from 'protontype';
    
    new ProtonApplication()
        .addRouter(new TasksRouter())
        .addMiddleware(new TasksMiddleware())
        .bootstrap();

```

## Exemplos

<https://github.com/protontype/protontype-sample>


[English](https://github.com/linck/protontype/blob/develop/README_en.md "")