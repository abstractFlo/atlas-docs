---
description: Learn more about the Database
---

# Database

## Introduction

Our Framework has support for a wide range of database systems. This is provided by [TypeORM](https://typeorm.io/#/).  
The DatabaseService is ready to use for your gamemode. Setup your credentials inside the environment.json and start using the service.

{% hint style="warning" %}
We don't teach you the interaction and creation for databases. If you have any questions about database basics, consult the [docs](https://typeorm.io/#/).
{% endhint %}

## How to use

Using the DatabaseService is fairly simple. Only add database entities and the service is starting for you.

{% hint style="warning" %}
If you don't need a database, remove **setupServerDatabaseEntities** from **bootstrap.ts**
{% endhint %}

{% tabs %}
{% tab title="index.ts" %}
```typescript
import '@abraham/reflection';
import './bootstrap';
// ... other imports and stuff for server start
```
{% endtab %}

{% tab title="bootstrap.ts" %}
```typescript
import { setupServerDatabaseEntities } from '@abstractFlo/shared';
import { YourEntity } from 'path/to/your/your.entity';

setupServerDatabaseEntities([
  YourEntity
]);
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
If you're not familiar at TypeORM yet, feel free to check out our sample gamemode and see how we use it.
{% endhint %}

