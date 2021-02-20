---
description: Learn more about the BasePool
---

# Base Pool

## Introduction

Dealing with a huge amount of objects can be really harde. To make it easier for you, we create a Base Pool Class. The class provides the feature, that you can create a "Pool" for any kind of objects. Internaly we use a simple [ES6 Map](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Map). The usage is really simple and if you only need the basic stuff, you only need a few lines of code to create your own pool.

### The BasePool Class

The BasePool is a simple class with some methods. This class is created to use as extend not as instance self.

{% hint style="warning" %}
The BasePool has two generic types. **T** stands for the entity type you want to store, **K** stands for the key identifier.
{% endhint %}

```typescript
export abstract class BasePool<T, K = string | number> {

  /**
   * Contains the pool
   *
   * @type {Map<string|number, T>}
   * @protected
   */
  protected pool: Map<K, T> = new Map<K, T>();

  /**
   * Return the pool size
   *
   * @return {number}
   */
  public get size(): number {
    return this.pool.size;
  }

  /**
   * Create new entity inside pool if not exists
   *
   * @param {K} identifier
   * @param {T} entity
   * @return {Map<string|number, T> | void}
   */
  public add(identifier: K, entity: T): Map<K, T> | void {
    if (!this.has(identifier)) {
      this.pool.set(identifier, entity);
    }
  }

  /**
   * Get entity from pool if exists
   *
   * @param {K} identifier
   * @return {T | void}
   */
  public get(identifier: K): T | void {
    if (this.has(identifier)) {
      return this.pool.get(identifier);
    }
  }

  /**
   * Check if the pool has identifier
   *
   * @param {K} identifier
   * @return {boolean}
   */
  public has(identifier: K): boolean {
    return this.pool.has(identifier);
  }

  /**
   * Return all entries from pool
   *
   * @return {T[]}
   */
  public entries(): T[] {
    return Array.from(this.pool.values());
  }

  /**
   * Return all keys from pool
   *
   * @return {(K)[]}
   */
  public keys(): (K)[] {
    return Array.from(this.pool.keys());
  }

  /**
   * Remove entity from pool if exists
   *
   * @param {K} identifier
   * @return {boolean}
   */
  public remove(identifier: K): boolean {
    return this.pool.delete(identifier);
  }

  /**
   * Remove all entities from pool
   */
  public removeAll(): void {
    this.pool.clear();
  }
}
```

### Usage

{% tabs %}
{% tab title="player.pool.ts" %}
```typescript
import { BasePool } from '@abstractFlo/shared';
import { Player } from 'alt-client';
import { singleton } from 'tsyringe';

@singleton()
export class PlayerPool extends BasePool<Player, string> {}
```
{% endtab %}

{% tab title="your.component.ts" %}
```typescript
import { ScriptSyncedMeta } from '@shared/constants';
import { Player } from 'alt-client';
import { PlayerPool } from './player.pool';
import { singleton } from 'tsyringe';

@singleton()
export class PlayerComponent {

  constructor(
      private readonly playerPool: PlayerPool,
  ) {}

  /**
   * Add player by it's id to pool
   *
   * @param {Player} player
   */
  public addPlayerToPool(player: Player): void {
    this.playerPool.add(player.id, player);
  }

  /**
   * Remove player from pool by id
   *
   * @param {Player} player
   */
  public removePlayerFromPool(player: Player): void {
    this.playerPool.remove(player.id);
  }
}
```
{% endtab %}
{% endtabs %}

