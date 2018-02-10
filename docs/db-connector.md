# Database Connectors

A conexão com banco de dados é definida através da classe **DBConnector**. Assim o **Protontype** permite o uso de qualquer framework ou ORM para manipulação de banco de dados.

Por padrão é usado o [TypeORM](http://typeorm.io/#/) para acessar o banco de dados

## Acessando Banco de Dados
O módulo **TypeORMDBConnector** disponibiliza o objeto **TypeORMDB** que provê a cesso aos objetos do [TypeORM](http://typeorm.io/#/)

```typescript
let tasksRepository = TypeORMDB.getBD().getRepository(TasksModel);
let tasks = await tasksRepository.find();
``` 

O objeto retornado pelo **TypeORMDB.getBD()** é exatamente o objeto resultante da conexão do **TypeORM**

> Para mais informações ver documentação do [TypeORM](http://typeorm.io/#/)

> Ver também <http://typeorm.io/#/undefined/using-repositories>

## Criando um DBConnector
Para criar um DBConnector basta extender a classe **DBConnector**.
```typescript
export abstract class DBConnector<OptionsType, ConnectionType> {
    abstract createConnection(config?: OptionsType): Promise<ConnectionType>;
}
```
- **OptionsType**: TIpo do objeto de configuração passado por parâmetro no método de conexão
- **ConnectionType**: Tipo do objeto de retorno. Resultado da conexão. Sempre deverá ser uma promisse

### Exemplo de DBConnector
```typescript
import { Connection, ConnectionOptions, createConnection } from 'typeorm';
import { DBConnector } from '../DBConnector';

export class TypeORMDBConnector implements DBConnector<ConnectionOptions, Connection> {

    createConnection(options?: ConnectionOptions): Promise<Connection> {
        return createConnection(options);
    }
}
```