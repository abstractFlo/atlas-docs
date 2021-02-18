---
description: Learn more about the BaseService
---

# Base Service

## Introduction

To keep it simple as possible for you to work with database entities, we created a BaseService with all the basic interaction stuff.

## Included Features

Following features included inside the BaseService.

### Methods

#### getAll\(\)

#### findById\(id: number\)

#### remove\(id: number\)

#### create\(\)

#### update\(id: number, data: any\)

### Properties

#### repo

## Usage

The usage is simple. Only extend the BaseService in your own service and setup the constructor and generic type.

{% tabs %}
{% tab title="your.service.ts" %}
```typescript
import { BaseService } from '@abstractFlo/server';
import { YourEntity } from './your.entity';

export class YourService extends BaseService<YourEntity> {

    constructor() {
        super(YourEntity)
    }
    
    // Now you can access all the base service features like
    // getAll(), findById(), ....
    
    // You can add your own methods too
    public findByWhatEver(whatever: string): Promise<YourEntity> {
        return this.repo.find({whatevet});
    }
    

}
```
{% endtab %}
{% endtabs %}

