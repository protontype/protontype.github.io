# Database Connectors

The database connection is defined through **DBConnector** class. So the **Protontype** allow any ORM or framework to database manipulation.

[TypeORM](http://typeorm.io/#/) is default.

##Accessing Database
The **TypeORMDBConnector** module provides **TypeORMDB** object which provides [TypeORM](http://typeorm.io/#/) objects access.

```typescript
let tasksRepository = TypeORMDB.getBD().getRepository(TasksModel);
let tasks = await tasksRepository.find();
``` 

The **TypeORMDB.getBD()** rutuned object is exactly the connection object of the **TypeORM**

> See for more informations [TypeORM](http://typeorm.io/#/)

> See too <http://typeorm.io/#/working-with-repository>

## Creating a DBConnector
To create a DBConnector must to extend **DBConnector** class.

```typescript
export abstract class DBConnector<OptionsType, ConnectionType> {
    abstract createConnection(config?: OptionsType): Promise<ConnectionType>;
}
```

- **OptionsType**: Connection options object type.
- **ConnectionType**: Result object of connection. Always will be a promisse.

### DBConnector example
```typescript
import { Connection, ConnectionOptions, createConnection } from 'typeorm';
import { DBConnector } from '../DBConnector';

export class TypeORMDBConnector implements DBConnector<ConnectionOptions, Connection> {

    createConnection(options?: ConnectionOptions): Promise<Connection> {
        return createConnection(options);
    }
}
```