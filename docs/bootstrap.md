# Iniciando aplicação 

```javascript
let protonApp = new ProtonApplication();
protonApp
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .bootstrap();
```

## Configurações do bootstrap

### Middlewares

Adiciona um middleware que atuará em todas as rotas da aplicação

```javascript
protonApp.addMiddleware(new TasksMiddleware());
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