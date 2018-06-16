# Middlewares

## Creating Middlewares

To create a middleware class, must extends ```ProtonMiddleware```. A middleware may have one Middleware Function.

Example:

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

A ***Middleware Function*** is a function into ```ProtonMiddleware``` class, with ```@Middleware``` decorator and has as parameter a object of type ```MiddlewareFunctionParams```. This function defines the middleware behavior.

```typescript
@Middleware()
exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
    cosole.log(params.req);
    console.log(params.res);
    params.next();
}
```
The ```@Middleware``` decorator has the ```autoNext: boolean``` optional parameter. If is ```true ```, the ``` next() ``` function will be called implicitly as last instruction of function. Not is necessary call ``` params.next() ```. 

Example:
```typescript
@Middleware(true)
exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
    cosole.log(params.req);
    console.log(params.res);
}
```

### @Middleware
The ```@Middleware()``` decorator indicates what function contains the middleware behavior.

**Properties**

- autoNext: ```boolean```. Enables the implicit call of the ```next()``` function on middleware finsh execution.
### MiddlewareFunctionParams

***Middleware Function*** parameter

**Properties**

- **req**: The object that contains the HTTP request. Matches with [Express](http://expressjs.com/ "") request object.
- **res**: Object used to send the HTTP response. Matches with [Express](http://expressjs.com/ "") response object.
- **next**: Function used to call the next middlewere of the chain.
- **app**: Protontype application instance. Through it you can access the application properties.

## Middlewares's Scopes
The middlewares can act in diferents scopes

### Application's Scope
This middleware will act in global scope, that is, before any configured route.
To make a global midlleware, must to add it in application's bootstrap methods: ```addMiddleware ou addMiddlewareAs```:

```typescript
new ProtonApplication()
    .addMiddleware(new ExampleMiddleware())
    .addMiddlewareAs(ExampleMiddleware2)
    .start();
```

- **addMiddleware**:  Allows to pass a created instance of the middleware
- **addMiddlewareAs**: Allows to pass the middleware type, and the application will instantiate it.

### Router's Scope
This middleware will act to all ***Router Functions*** into ```ExpressRouter``` class.
To add router's scope middlewares, you must use ```@RouterClass()``` decorator.

```typescript
@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new ExampleMiddleware(), new ExampleMiddleware2() ]
})
export class TaskRouter extends ExpressRouter {
 ...
}
```

### Route's Scope (Router Function)
This middleware will act only to specific route. To add route's spcope middleware, you must use the ```@Route()``` decorator
Este middleware atuará somente para aquela rota específica. Para adicionar middlewares ao escopo da rota, este deve ser configurado no decorator ```@Route()```.

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

The ```configMiddlewares()``` can to be overwritten. In him we can access the ExpressJs instance and to make anything. 

Example:

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