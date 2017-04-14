# Iniciando aplicação 

```javascript
let expressApp = new ProtonApplication();
expressApp
    .withAuthMiddleware(new JWTAuthMiddleware())
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .bootstrap();
```

## Configurações do bootstrap

### Middlewares

Para adicionar um middleware a aplicação. Este middleware afetará todas as rotas da aplicação

```javascript
expressApp.addMiddleware(new TasksMiddleware());
```

### Middleware de Autenticação

Para adicionar um middleware responsável para fazer a autenticação. Este middleware afetará:

 - Todos Routers anotadas com o decorator ```@UseAuth()```
 - Todas rotas dentro de um router que possua o parâmetro ```useAuth: true``` do decotarot ```@Route()```

```javascript
expressApp.withAuthMiddleware(new JWTAuthMiddleware());
```

### Routers

Adiciona um Router para que a apicação configure e levante suas rotas
```javascript
expressApp.addRouter(new TasksRouter());
```

### Boostrap

O método ```expressApp.bootstrap()``` inicia a aplicação, suas rotas e middlewares configurados.

## Exemplo de uso completo

[https://github.com/linck/protontype-example](https://github.com/linck/protontype-example)