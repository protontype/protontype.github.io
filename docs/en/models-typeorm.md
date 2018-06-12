# Models
##Creating Models

The **ProtonType** uses [TypeORM](http://typeorm.io/#/) by default to creating Models and database access. But any framewok can be used through the [DBConnectors](/db-connector)

### Creating a Model
Using default configuration, to creates a Model must be to follow documentation of [TypeORM about Entities](http://typeorm.io/#/entities)

#### Example
```typescript
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

> The model structure and mappings, will depend of [DBConnector](/db-connector) that was used