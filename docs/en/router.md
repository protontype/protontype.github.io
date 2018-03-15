# Router

A API do Protontype disponibiliza a classe ```ExpressRouter``` para ser base dos routers.

## Criando um Router

Para criar uma classe router basta estender a classe ```ExpressRouter```

```typescript
import { ExpressRouter } from 'protontype';

export class TasksRouter extends ExpressRouter {
    ...
}
```

Para configurar uma classe Router deve-se usar o decorator ```@RouterClass()```

```typescript
import { ExpressRouter, RouterClass } from 'protontype';

@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new MyMiddleware(), new OtherMiddleware() ]
})
export class TasksRouter extends ExpressRouter {
        ...
}
```

Uma classe Router poderá conter várias funções de rotas, chamadas ***Router Functions***. Para definir e configurar uma ***router function*** deve-se usar o decorator ```@Route()```. Uma ***Router Function*** recebe como parâmetro um objeto do tipo ```RouterFunctionParams```

```typescript
import { ExpressRouter, RouterFunctionParams, Method, Route } from 'protontype';

@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new MyMiddleware(), new OtherMiddleware() ]
})
export class TasksRouter extends ExpressRouter {
    
    @Route({
        endpoint: '/',
        method: Method.GET
    })
    listTasks(params: RouterFunctionParams) {
        params.res.send({id: 1, title: 'Task de teste'});
    }
}
```

### @RouterClass()

Usado para definir uma classe que contém rotas. Todas classes do tipo ```ExpressRouter``` suportam esta anotação.

```typescript
@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new MyRouterMiddleware(), new OtherRouterMiddleware() ]
})
```
**Propriedades:**

- **baseUrl**: Url base do roteador. Todas rodas serão criadas no padrão ***baseUrl + endpoint***
- **middlewares**: Middlewares que atuarão para todas as rotas definidas neste router

### @Route()

Usado para definir e configurar as rotas dentro de um router.
```typescript
@Route({
    endpoint: '/list',
    method: Method.GET,
    middlewares: [ new MyRouteMiddleware(), new OtherRouteMiddleware() ]
})
```

**Propriedades:**

- **endpoint**: Define o endpoint da rota. A url desta rota será formada pela **baseUrl** (definida na ```@RouterClass```) + **endpoint**. Exemplo: ***http://locathost/tasks/list***
- **method**: Verbo HTTP usado para esta rota. GET, POST, DELETE...
- **middlewares**: Middlewares que atuarão somente para esta rota específica

É possivel também usar o decorator ```@Route()``` sem parâmetros, assim a router function deve criar as rotas diretamente usando o router do express.

```typescript
    @Route()
    public rootRoute(): void {
        this.router.get("", (req, res) =>
            res.sendFile('routes.html', { "root": "./src/views" })
        );
    }
```

### RouterFunctionParams

Toda **Router Function** deve ter como parâmetro um objeto do tipo ```RouterFunctionParams```

```typescript
@Route({
    endpoint: '/',
    method: Method.GET
})
listTasks(params: RouterFunctionParams) {
    console.log(params.req);
    console.log(params.res);
    console.log(params.app);
}
```
**Propriedades:**

- **req**: Objeto que contém a requisição http. Corresponde ao objeto de request do [Express](http://expressjs.com/ "").
- **res**: Objeto usado para enviar a resposta http. Corresponde ao objeto de response do [Express](http://expressjs.com/ "").
- **app**: Instância da aplicação Protontype. Por meio dela pode-se acessar as propriedades da aplicação.