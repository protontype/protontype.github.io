# Models
##Criando Models

O **ProtonType** usa o [**ORM Sequelize**](http://docs.sequelizejs.com/en/v3/ "") para criação dos Models e acesso ao banco de dados.

Para criar um Model, deve-se criar uma classe que *extends* de **BaseModel**. O
mapeamento do banco de dados é feita a através da anotação @Model que possui os
seguntes parâmetros:

-   **name**: Nome do model
-   **definition**: Definição das colunas. O objeto usado para as definições é o mesmo de
    [definição do
    Sequelize](http://docs.sequelizejs.com/en/v3/docs/models-definition/).

Exemplo:

```javascript

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

```javascript

@LoadModel(new TaskModel())
export class UsersModel extends BaseModel<User> {

}

```

## Adicionando relacionamentos e outras configurações nos Models


Um BaseModel permite sobreescrever o método *configure()*, que permite acessar a instancia do modelo Sequelize e os modelos já carregados e adicionar lógicas e configurações:
```javascript

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
```javascript

@HasMany(modelName: string)
@HasOne(modelName: string)
@BelongsTo(modelName: string)
@BelongsToMany(modelName: string, options: Sequelize.AssociationOptionsBelongsToMany)

```

Estes podem ser usados como nos exemplos abaixo:

```javascript

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

```javascript

@BelongsTo(ModelNames.USERS)
export class TasksModel extends BaseModel<Task> {
}

```

Para mais informações sobre as possibilidades de configurações e uso dos Models, consultar a documentação do Sequelize: <http://docs.sequelizejs.com/en/v3/>