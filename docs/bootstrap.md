# Iniciando aplicação 

```typescript
let protonApp = new ProtonApplication();
protonApp
    .withDBConnector(new MyDBConnector())
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .start();
```

ou 

```typescript
let protonApp = new ProtonApplication();
protonApp
    .withDBConnectorAs(MyDBConnector)
    .addRouterAs(TasksRouter)
    .addMiddlewareAs(TasksMiddleware)
    .start();
```

## Configurações do Start

### Middlewares

Adiciona um middleware que atuará em todas as rotas da aplicação.

- **addMiddleware**: Adiciona uma instância criada do middleware
```typescript
let taskMiddleware = new TasksMiddleware();
taskMiddleware.setTaskName("MyTask");
taskMiddleware.setTaskOwner("Bob");

protonApp.addMiddleware(taskMiddleware);
```
- **addMiddlewareAs**: Adiciona o tipo do middleware para ser instanciado pela aplicação
```typescript
protonApp.addMiddlewareAs(TasksMiddleware);
```

### Routers

Adiciona um Router para que a apicação configure e levante suas rotas.

- **addRouter**: Adiciona uma instância criada do Router 
```typescript
protonApp.addRouter(new TasksRouter());
```

- **addRouterAs**: Adiciona o tipo do Router para ser instanciado pela aplicação
```typescript
protonApp.addRouterAs(TasksRouter);
```

### Database Connectors

Opcional. Informa a aplicação qual módulo de conexão e manipulação de banco de dados usar. Caso não seja informado usará o DBConnector padrão. Ver [DBConnectors](/db-connector)

- **withDBConnector**: Adiciona uma instância criada do DBConnector 
```typescript
protonApp.withDBConnector(new SequelizeDBConnector());
```

- **withDBConnectorAs**: Adiciona o tipo do DBConnector para ser instanciado pela aplicação
```typescript
protonApp.withDBConnectorAs(SequelizeDBConnector);
```

### Start

O método ```expressApp.start()``` inicia a aplicação, suas rotas e middlewares configurados.

## Exemplo de uso completo

<https://github.com/protontype/protontype-sample>