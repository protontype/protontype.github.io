# BaseCrudRouter

A classe ```BaseCrudRouter``` provê todas as funcionalidades de um ```ExpressRouter``` mais as operações básicas de CRUD, sem a necessidade de implementação adicional.

Exemplo:

```javascript
import { TasksModel } from '../models/TasksModel';
import { BaseCrudRouter, RouterClass, UseAuth } from 'protontype';

@UseAuth()
@RouterClass({
    baseUrl: "/tasks",
    modelInstances: [new TasksModel()]
})
export class TasksRouter extends BaseCrudRouter {

}
```
Esta classe já proverá as rotas:

-   **GET /** - Lista todos registros
-   **POST /** - Cria um registro
-   **GET /:id** - Consulta um registro
-   **PUT /:id** - Atualiza um registro
-   **DELETE /:id** - Remove um registro

Caso um **BaseCrudRouter** possua mais de uma instacia de Models, serão criadas as rotas para cada instancia, sendo o padrão da url:

```html
/baseUrl/modelName/...
```

Exemplo:

```html
/tasks/tasksmodel/
```

## Configurando autenticação

Para hablititar a autenticação em um ```BaseCrudRouter``` deve-se usar o decorator ```@UseAuth()```. Este pode conter os parametros abaixo:

```javascript

@UseAuth({
    create: boolean, //Habilita a autenticação para rotas de criação
    update: boolean, //Habilita a autenticação para rotas de atualização
    read: boolean,   //Habilita a autenticação para rotas de leitura
    delete: boolean  //Habilita a autenticação para rotas de remoção
})

```