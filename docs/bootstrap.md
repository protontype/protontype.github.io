# Iniciando aplicação 

```javascript
let protonApp = new ProtonApplication();
protonApp
    .withAuthMiddleware(new JWTAuthMiddleware())
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .bootstrap();
```

## Configurações do bootstrap

### Middlewares

Para adicionar um middleware a aplicação. Este middleware afetará todas as rotas da aplicação

```javascript
protonApp.addMiddleware(new TasksMiddleware());
```

### Middleware de Autenticação

Para adicionar um middleware responsável para fazer a autenticação. Este middleware afetará:

 - Todos Routers anotadas com o decorator ```@UseAuth()```
 - Todas rotas dentro de um router que possua o parâmetro ```useAuth: true``` do decotarot ```@Route()```

```javascript
protonApp.withAuthMiddleware(new JWTAuthMiddleware());
```

### Routers

Adiciona um Router para que a apicação configure e levante suas rotas
```javascript
protonApp.addRouter(new TasksRouter());
```

### Bootstrap

O método ```expressApp.bootstrap()``` inicia a aplicação, suas rotas e middlewares configurados.

## Exemplo de uso completo

<https://github.com/linck/protontype-example>