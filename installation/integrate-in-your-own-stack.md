---
description: Learn more about the integration in your own Stack
---

# Integrate in your own Stack

## Introduction

If you create your own setup and you want to integrate the framework inside, it is possible by installing the npm packages.

```text
npm install -D @abstractFlo/client @abstractFlo/server @abstractFlo/shared
```

{% hint style="danger" %}
You have to set up the bundler for yourself. We can not give any kind of support for your own stack.
{% endhint %}

After this step, you need to create some bootstrap files. They are loaded before your first gamemode related script gets resolved.

{% tabs %}
{% tab title="Bootstrap Clientside" %}
```typescript
import { altLibRegister, setupWebviewRegistry } from '@abstractFlo/shared';
import * as alt from 'alt-client';
import { container } from 'tsyringe';

altLibRegister(alt);

// if you want to use the webviewService
setupWebviewRegistry('http://192.168.178.26:5000', 'routeChangeEventName');

```
{% endtab %}

{% tab title="Bootstrap Serverside" %}
```typescript
import * as alt from 'alt-server';
import { resolve } from 'path';
import { 
    altLibRegister, 
    setupServerConfigPath, 
    setupServerDatabaseEntities 
} from '@abstractFlo/shared';
import { container } from 'tsyringe';

altLibRegister(alt);

// Setup the path where your environment.json lives
// example: ~/config/
setupServerConfigPath(resolve('config'));

// if you want to use the DatabaseService, enter your entities here
setupServerDatabaseEntities([]);


```
{% endtab %}

{% tab title="environment.json" %}
```typescript
{
  "database": {
    "name": "default",
    "type": "mysql",
    "host": "altv-mysql",
    "port": 3306,
    "username": "gamemode",
    "password": "demo123",
    "database": "gamemode",
    "synchronize": true,
    "logging": false
  },
  "discord": {
    "client_id": "",
    "client_secret": "",
    "bot_secret": "",
    "server_id": "",
    "redirect_url": "http://127.0.0.1:1337/auth/discord",
    "auth_url": "https://discord.com/api/oauth2/authorize",
    "auth_token_url": "https://discord.com/api/oauth2/token",
    "user_me_url": "https://discord.com/api/users/@me"
  }
}

```
{% endtab %}
{% endtabs %}

Create your entry points

{% tabs %}
{% tab title="Clientside" %}
```typescript
import '@abraham/reflection';
import './bootstrap';
import { container } from 'tsyringe';
import { LoaderService, UtilsService } from '@abstractFlo/shared';
import { EventService } from '@abstractFlo/client';
import { ClientModule } from './client.module';

const loader = container.resolve(LoaderService);

UtilsService.eventOn('connectionComplete', () => {
  loader
      .bootstrap(ClientModule)
      .afterComplete(() => {
        UtilsService.log('~lg~Booting complete => ~w~Happy Playing');
      });
});

```
{% endtab %}

{% tab title="Serverside" %}
```typescript
import '@abraham/reflection';
import './bootstrap';
import { container } from 'tsyringe';
import { LoaderService, UtilsService } from '@abstractFlo/shared';
import { ServerModule } from './server.module';

const loader = container.resolve(LoaderService);

loader
    .bootstrap(ServerModule)
    .afterComplete(() => {
      UtilsService.log('~lg~Booting complete => ~w~Player can now join and have some fun');
    });


/**
 * Global Error Handler
 * not needed but nice to have
 */
process.on('uncaughtException', (err) => {
  UtilsService.logError(err.stack);
  UtilsService.logError(err.message);
  UtilsService.logError(err.name);
  UtilsService.log('~r~Please close the server and fix the problem~w~');
});

```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Now you are able to start using the Framework. Have fun with it ❤ 
{% endhint %}

