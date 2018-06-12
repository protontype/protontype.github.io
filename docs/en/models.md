# Models
##Creating Models

The **ProtonType** uses [**ORM Sequelize**](http://docs.sequelizejs.com/en/v3/ "") for creating Models and database access.

To build a Model, you have to create a class that extends from **BaseModel**. The database mapping is make throught @Model decorator, with parameters:

-   **name**: Name of the Model
-   **definition**: Column definition. This object is same of the [Sequelize definition](http://docs.sequelizejs.com/en/v3/docs/models-definition/).

Example:

```typescript

import { ModelNames } from './ModelNames';
import { BaseModel, BelongsTo, DataTypes, Model, SequelizeBaseModelAttr } from 'protontype';

@Model({
    name: ModelNames.TASKS,
    definition: {
        title: {
            type: DataTypes.STRING,
            allowNull: false,
            validate: {
                notEmpty: true
            }
        },
        done: {
            type: DataTypes.BOOLEAN,
            allowNull: false,
            defaultValue: false
        }
    }
})
export class TasksModel extends BaseModel<Task> {

}

export interface Task extends SequelizeBaseModelAttr {
    title: string;
    done: boolean;
    user_id: number;
}

```

##Carregamento dos Models

Cada **BaseModel** será carregado automaticamente na hora da sua instanciação. Geralmente o model sera carregado quando for usado por um ***Router***, porém o carregamento poderá ser forçado através o **@LoadModel** decorator ou simplemente através do **new**  

```typescript

@LoadModel(new TaskModel())
export class UsersModel extends BaseModel<User> {

}

```

## Adicionando relacionamentos e outras configurações nos Models


Um BaseModel permite sobreescrever o método *configure()*, que permite acessar a instancia do modelo Sequelize e os modelos já carregados e adicionar lógicas e configurações:
```typescript

export class UsersModel extends BaseModel<User> {
    public configure(): void {
        this.getInstance().beforeCreate((user: any) => {
            let salt: string = bcrypt.genSaltSync();
            user.password = bcrypt.hashSync(user.password, salt);
        });

        this.getInstance().hasMany(this.ProtonDB.getModel("Tasks").getInstance());
    }
}

```

###Usando decorators para criar relacionamentos

Alguns decorators estãos disponíveis para facilitar a adição dos relacionamentos:
```typescript

@HasMany(modelName: string)
@HasOne(modelName: string)
@BelongsTo(modelName: string)
@BelongsToMany(modelName: string, options: Sequelize.AssociationOptionsBelongsToMany)

```

Estes podem ser usados como nos exemplos abaixo:

```typescript

@HasMany(ModelNames.TASKS)
export class UsersModel extends BaseModel<User> {
    public configure(): void {
        this.getInstance().beforeCreate((user: any) => {
            let salt: string = bcrypt.genSaltSync();
            user.password = bcrypt.hashSync(user.password, salt);
        });
    }
}

```

```typescript

@BelongsTo(ModelNames.USERS)
export class TasksModel extends BaseModel<Task> {
}

```

Para mais informações sobre as possibilidades de configurações e uso dos Models, consultar a documentação do Sequelize: <http://docs.sequelizejs.com/en/v3/>

## Hook Methods

Um BaseModel permite sobreescrever o método ```configure()```, nele podemos acessar a instancia do Model Sequelize, os Models já carregados e adicionar lógicas e configurações:
```typescript

export class UsersModel extends BaseModel<User> {
    public configure(): void {
        this.getInstance().beforeCreate((user: any) => {
            let salt: string = bcrypt.genSaltSync();
            user.password = bcrypt.hashSync(user.password, salt);
        });

        this.getInstance().hasMany(this.ProtonDB.getModel("Tasks").getInstance());
    }
}

```