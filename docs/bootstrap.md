#Iniciando aplicação 

```javascript

let expressApp = new ProtonApplication();
expressApp
    .withAuthMiddleware(new JWTAuthMiddleware())
    .addRouter(new TasksRouter())
    .addMiddleware(new TasksMiddleware())
    .bootstrap();
    
```

Exemplo de uso completo
---

**https://github.com/linck/protontype-example**