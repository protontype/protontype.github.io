# Middlewares

## Criando Middlewares

Pra criar uma classe middleware base estender ```ProtonMiddleware```. Um middleware poderá ter um Middleware Function.

Exemplo:

```typescript
import { Middleware, MiddlewareFunctionParams, ProtonMiddleware } from 'protontype';
export class ExampleMiddleware extends ProtonMiddleware {

    @Middleware()
    exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
        cosole.log(params.req);
        console.log(params.res);
        params.next();
    }
}
```

## Middleware Function

Um ***Middleware Function*** é uma função dentro de uma classe ```ProtonMiddleware```  anotada com o decorator ```@Middleware``` e tem como parâmetro um objeto do tipo ```MiddlewareFunctionParams```. Esta função define o comportamento do middleware.

```typescript
@Middleware()
exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
    cosole.log(params.req);
    console.log(params.res);
    params.next();
}
```

O decorator ```@Middleware``` possui o parâmetro opcional ```autoNext: boolean```. Caso seja ```true ``` a função ``` next() ``` será implicitamente chamada no final da função, não sendo necessário chamar explicitamente ``` params.next() ```. 

Exemplo:
```typescript
@Middleware(true)
exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
    cosole.log(params.req);
    console.log(params.res);
}
```

### @Middleware
O decorator ```@Middleware()``` indica qual função contém o comportamento do middleware.

**Propriedades**

- autoNext: ```boolean```. Habilita a chamada implícita da função ```next()``` no final da execução do middleware
### MiddlewareFunctionParams

Parâmetros de um ***Middleware Function***

**Propriedades**

- **req**: Objeto que contém a requisição http. Corresponde ao objeto de request do [Express](http://expressjs.com/ "").
- **res**: Objeto usado para enviar a resposta http. Corresponde ao objeto de response do [Express](http://expressjs.com/ "").
- **next**: função usada para chamar o próximo middleware da cadeia
- **app**: Instância da aplicação Protontype. Por meio dela pode-se acessar as propriedades da aplicação.

## Escopo dos Middlewares
Os middlewares podem atuar em diferentes escopos

### Escopo de Aplicação
Este middleware atuará no escopo da global, ou seja antes de qualquer rota configurada.
Para tornar um middleware global, deve-se adicionar ele no bootstrap da aplicação usando os métodos ```addMiddleware ou addMiddlewareAs```:

```typescript
new ProtonApplication()
    .addMiddleware(new ExampleMiddleware())
    .addMiddlewareAs(ExampleMiddleware2)
    .start();
```

- **addMiddleware**:  Permite passar uma instância criada do middleware
- **addMiddlewareAs**: Permite passar o tipo do middleware para ser instanciado pela aplicação

### Escopo de Router
Este middleware atuará para todas as ***Router Functions*** dentro de uma classe ```ExpressRouter```.
Para adicionar middlewares para atuar no escopo do router, este deve ser configurado no decorator ```@RouterClass()```:

```typescript
@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new ExampleMiddleware(), new ExampleMiddleware2() ]
})
export class TaskRouter extends ExpressRouter {
 ...
}
```

### Escopo de Rota (Router Function)
Este middleware atuará somente para aquela rota específica. Para adicionar middlewares ao escopo da rota, este deve ser configurado no decorator ```@Route()```:

```typescript
@RouterClass({baseUrl: "/tasks"})
export class TaskRouter extends ExpressRouter {
    @Route({
        endpoint: '/',
        method: Method.GET,
        middlewares: [ new ExampleMiddleware(), new ExampleMiddleware2() ]
    })
    listTask(params: RouterFunctionParams) {
        ...
    }
}
```

## Hook Methods

 O método ```configMiddlewares()``` pode ser sobrescrito. Nele podemos acessar a instância do express e fazer qualquer configuração ou lógica necessária.

Exemplo:

```typescript
export class DefaultMiddleware extends Middleware {
    private port: number = 5555;
    private jsonSpaces: number = 2;

public configMiddlewares(): void {
    this.express.set("port", this.port);
    this.express.set("json spaces", this.jsonSpaces);
    this.express.use(bodyParser.json());
    this.express.use((req, res, next) => {
        delete req.body.id;
        next();
    })
}
```