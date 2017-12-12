# Models
##Criando Models

O **ProtonType** usa o [TypeORM](http://typeorm.io/#/) por padrão para criação dos Models e acesso ao banco de dados. Qualquer outro framework pode ser usado por meio dos [DBConnectors](/db-connector)

### Criando um Model
Usando a configuração padrão, para criar um Model basta seguir a documentação do [TypeORM sobre Entities](http://typeorm.io/#/entities)

#### Exemplo
```javascript
@Entity()
export class TasksModel {
    @PrimaryGeneratedColumn()
    id: number;
    @Column({ nullable: true })
    title: string;
    @Column()
    done: boolean;
    @Column()
    userId: number;
}
```

> A criação dos modelos de banco de dados e mapeamentos, vai depender do [DBConnector](/db-connector) usado