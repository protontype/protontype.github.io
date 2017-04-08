# ProtonType

Um simples web framework feito em TypeScript.

O ProtonType tem como objetivo tornar simples e agradável o desensolvimento de APIs REST e criação de modelos de banco de dados. Utilizando [Express](http://expressjs.com/ "") e [Sequelize ORM](http://docs.sequelizejs.com/ "") ajuda na criação de aplicações web robustas.


## Instalação
```bash
npm install protontype --save
```


## Criação de Models

Criar um arquivo ParticlesModel.ts

```javascript
import { BaseModel, SequelizeBaseModelAttr, Model, DataTypes } from 'protontype';

@Model({
    name: "Particles",
    definition: {
        name: {
            type: DataTypes.STRING
        },
        symbol: {
            type: DataTypes.STRING
        },
        mass: {
            type: DataTypes.BIGINT
        }

    }
})
export class ParticlesModel extends BaseModel<Particle> {

}

export interface Particle extends SequelizeBaseModelAttr {
    name: string;
    symbol: string;
    mass: number;
}
```

## Criação de Routers

Criar arquivo ParticlesRouter.ts

```javascript
import { ParticlesModel } from './ParticlesModel';
import { BaseCrudRouter, RouterClass } from 'protontype';

@RouterClass({
    baseUrl: '/particles',
    modelInstances: [new ParticlesModel()]
})
export class ParticlesRouter extends BaseCrudRouter {

}
```

## Criação de Middlewares

```javascript
import { Middleware, MiddlewareFunctionParams } from './../decorators/MiddlewareConfig';
import { ProtonMiddleware } from './ProtonMiddleware';
import * as bodyParser from 'body-parser';

export class JsonContentMiddleware extends ProtonMiddleware {

    @Middleware()
    jsonContentMiddlewareFunc(params: MiddlewareFunctionParams) {
        params.app.getExpress().set("json spaces", 2);
        params.app.getExpress().use(bodyParser.json());
        params.next();
    }
}
```

## Bootstrap

```javascript
import { ParticlesRouter } from './ParticlesRouter';
import { ProtonApplication } from 'protontype';

new ProtonApplication()
    .addRouter(new ParticlesRouter())
    .bootstrap();   
```