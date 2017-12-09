# CRUD Routers

A classe ```TypeORMCrudRouter``` provê todas as funcionalidades de um ```ExpressRouter``` mais as operações básicas de CRUD, sem a necessidade de implementação adicional, usando o ```TypeORMDBConnector```.

Exemplo:

```javascript
@RouterClass({
    baseUrl: "/tasks",
    model: TasksModel
})
export class TasksRouter extends TypeORMCrudRouter {

}
```
Esta classe já proverá as rotas:

-   **GET /** - Lista todos registros
-   **POST /** - Cria um registro
-   **GET /:id** - Consulta um registro
-   **PUT /:id** - Atualiza um registro
-   **DELETE /:id** - Remove um registro