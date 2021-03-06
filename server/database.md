---
description: Learn more about the Database
---

# Database

## Introduction

Our Framework has support for a wide range of database systems. This is provided by [TypeORM](https://typeorm.io/#/).  
The DatabaseService is ready to use for your gamemode. Set up your credentials inside the environment.json and start using the service.

{% hint style="warning" %}
We don't teach you the interaction and creation of databases. If you have any questions about database basics, you should take a look at the [docs](https://typeorm.io/#/).
{% endhint %}

## How to use

Using the DatabaseService is fairly simple. Create your entities, add the new [@AutoAdd](database.md#autoadd) decorator and use your entity as normal.

{% tabs %}
{% tab title="your.entity.ts" %}
```typescript
@AutoAdd()
@Entity('yours')
export class YourEntity {

  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;
}
```
{% endtab %}

{% tab title="example.component.ts" %}
```typescript
import { On } from '@abstractFlo/shared';
import { ExampleService } from './example.service';

export class ExampleComponent {

  constructor(
    private readonly exampleService: ExampleService
  ) {}

  /**
   * Load entries sync
   */
  @On('yourEvent')
  public loadAll(): void {
    this.exampleService
      .loadAll()
      .then((yourEntities: YourEntity[]) => {
        // Do whatever you want
      })
      .catch((err) => {
        // Do what ever you want if error
      })

  }

  /**
   * Load entries async
   */
  @On('yourEvent')
  public async loadAllAsync(): void {
    try {
      const yourEntities = await this.exampleService.loadAll();

      // do whatever you want
    } catch(err) {
      // do whatever you want if error
    }
  }

}
```
{% endtab %}

{% tab title="example.service.ts" %}
```typescript
import { injectable } from 'tsyringe';
import { YourEntity } from './your.entity';
import { getRepository, Repository } from 'typeorm';

@injectable()
export class ExampleService {

  /**
   * Get repository
   *
   * @type {Repository<YourEntity>}
   * @protected
   */
  private repo: Repository<YourEntity> = getRepository(YourEntity);


  /**
   * Return all entries
   *
   * @return {Promise<YourEntity[]>}
   */
  public getAll(): Promise<YourEntity[]> {
    return this.repo.find();
  }

}
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
If you're not familiar with TypeORM, feel free to check out our sample gamemode and see how we use it.
{% endhint %}

### @AutoAdd

This is a new decorator to provide a simple way for adding new Entities to your database service. Only add this to your TypeORM Entity and use this entity as expected. This decorator registered the entity for you.

{% tabs %}
{% tab title="your.entity.ts" %}
```typescript
@AutoAdd()
@Entity('yours')
export class YourEntity {

  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;
}
```
{% endtab %}
{% endtabs %}

