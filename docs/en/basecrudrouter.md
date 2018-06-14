# CRUD Routers

The ```TypeORMCrudRouter``` class provides all ```ExpressRouter``` features, but adds the CRUD operations, without additional implementation. It uses the ```TypeORMDBConnector```

Example:

```typescript
@RouterClass({
    baseUrl: "/tasks",
    model: TasksModel
})
export class TasksRouter extends TypeORMCrudRouter {

}
```
This class will provide the routes bellow:

-   **GET /** - Lists all records
-   **POST /** - Creates a record
-   **GET /:id** - Queries a record
-   **PUT /:id** - Updates a record
-   **DELETE /:id** - Removes a record