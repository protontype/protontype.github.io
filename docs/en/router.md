# Router

The Protontype API contains ```ExpressRouter``` class to be routers base.

## Creating a Router

To create a router class must to extend ```ExpressRouter``` class

```typescript
import { ExpressRouter } from 'protontype';

export class TasksRouter extends ExpressRouter {
    ...
}
```

To configure a Router class must use the ```@RouterClass()``` decorator

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

A Router class can contains multiple ***Router Functions***. To define and configure a ***router function*** must use the ```@Route()``` decorator. A ***Router Function*** have as parameter a object of type ```RouterFunctionParams```.

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
        params.res.send({id: 1, title: 'Task test'});
    }
}
```

### @RouterClass()

Used to define a class tha contains routes. All classes of type ```ExpressRouter``` supports this decorator.

```typescript
@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new MyRouterMiddleware(), new OtherRouterMiddleware() ]
})
```
**Properties:**

- **baseUrl**: Base Url of the router. All routes will be creates of pattern: ***baseUrl + endpoint***
- **middlewares**: Middlewares that will act for all routes in router.

### @Route()

Used to define and configure a routes into a router

```typescript
@Route({
    endpoint: '/list',
    method: Method.GET,
    middlewares: [ new MyRouteMiddleware(), new OtherRouteMiddleware() ]
})
```

**Properties:**

- **endpoint**: Defines the routes's endpoint. The url of this route will be formed by **baseUrl** (defined in ```@RouterClass```) + **endpoint**. Example: ***http://locathost/tasks/list***
- **method**: HTTP Verbs. GET, POST, DELETE...
- **middlewares**: Middlewares that will act only for this specific route.

It's also possible use the ``@Route()``` decotator without parameters, this way the router function must create the routes using the ExpressJS Router directly.

```typescript
    @Route()
    public rootRoute(): void {
        this.router.get("", (req, res) =>
            res.sendFile('routes.html', { "root": "./src/views" })
        );
    }
```

### RouterFunctionParams

All **Router Function** must have a object of type ```RouterFunctionParams``` as parameter 

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
**Properties:**

- **req**: Object that contains the HTTP request. Matches to [Express](http://expressjs.com/ "") request object.
- **res**: Object used to send the HPPT response. Matches to [Express](http://expressjs.com/ "") response object.
- **app**: Protontype application instance. Through it you can access the aplication properties.