---
description: All informations about folder structure used by us
---

# Folder Structure

## Introduction

There are some ways out there how to structure your gamemode. The simplest an easiest way, create one resource folder and create your code in it.

### Easy Setup

![Basic structure for simple gamemodes](../.gitbook/assets/image%20%281%29.png)

The benifit about this structure, you have no trouble with imports, assets, classes and so on. But this is not what we are looking for

### Advanced Setup

![Some better structure](../.gitbook/assets/image%20%282%29.png)

The benefit about this structure, you have splitted your gamemode into small pieces. All this peaces can combine to your master resource with the help of [DI-Container](../basic-knowledge/di-container.md). The only you thing you must be understand if your resource contains a package.json, the ResourceManager would be bundle it up in it's own resource. In most cases, this is not needed. You can split every pieces you want, every piece can have it's own components, services and submodules if you want.

#### How to register the modules

This is the easiest part of it, create your module and add to [@Module Decorator](../basic-knowledge/decorators/) import section in your main resource.

{% tabs %}
{% tab title="authentication/server/authentication.module.ts" %}
```typescript
import { Module } from '@abstractflo/atlas-shared';

/**
 * You can import custom modules and components too
 */
@Module()
export class AuthenticationModule {
  
}
```
{% endtab %}

{% tab title="gamemode/server/server.module.ts" %}
```typescript
import { Module } from '@abstractflo/atlas-shared';
import { AuthenticationModule } from '../../authentication/server/authentication.module';

@Module({
  imports: [AuthenticationModule]
})
export class ServerModule {}

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Keep in mind, you don't need the client and server folder both for each splitted module. Create only what you need. Keep it simple and stupid.
{% endhint %}

