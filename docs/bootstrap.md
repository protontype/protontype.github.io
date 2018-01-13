# Iniciando aplicação 

```javascript
let protonApp = new ProtonApplication();
protonApp
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .bootstrap();
```

## Configurações do Start

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

### Database Connectors

Opcional. Informa a aplicação qual módulo de conexão e manipulação de banco de dados usar. Caso não seja informado usará o DBConnector padrão. Ver [DBConnectors](/db-connector)
```javascript
protonApp.withDBConnector(new SequelizeDBConnector());
```

### Start

O método ```expressApp.start()``` inicia a aplicação, suas rotas e middlewares configurados.

## Exemplo de uso completo

<https://github.com/protontype/protontype-sample>