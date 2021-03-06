---
description: Learn more about the Lifecycle
---

# Lifecycle

## Introduction

There are many scenarios, where you want to wait for the method to be completed before starting another one. That's why we create a custom lifecycle system for booting up the framework. In simple words, the lifecycle is kind of a queue, where each decorator runs after each other in a particular order.

{% hint style="warning" %}
This lifecycle does not affect the [alt:V](https://altv.mp/#/) lifecycle. They only help you perform a different task on different steps.
{% endhint %}

## Loader

We've created a Loaderservice to fit our needs for the lifecycle we want. Using the loader is recommended, and the setup is fairly easy. The only thing to do is, bootstrap your entry module, and the loader starts the magic.

{% hint style="info" %}
This is available on the server and client side
{% endhint %}

{% tabs %}
{% tab title="Server" %}
```typescript
import { LoaderService } from '@abstractflo/atlas-server';
const loader = container.resolve(LoaderService);

loader
    .bootstrap(ServerModule)
    .done(() => {
        // This method is called, if the lifecycle finished
    });
```
{% endtab %}

{% tab title="Client" %}
```typescript
import { LoaderService } from '@abstractflo/atlas-client';
const loader = container.resolve(LoaderService);

loader
    .bootstrap(ClientModule)
    .done(() => {
        // This method is called, if the lifecycle is finished
        // You can send an event to server to inform the player is ready
        // Example:
        UtilsService.log('~lg~Booting complete => ~w~Happy Playing');
        const eventService = container.resolve(EventService);
        eventService.emitServer('playerBootComplete');
    });
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
You don't need to load up other modules. Keep in mind, the available decorators do this for you.
{% endhint %}

## Available Decorators

The done callback has a duration within **5000ms**. If your task not over in this time, the console would show you some information about.

{% hint style="warning" %}
You can increase this time, by adding as argument to decorator
{% endhint %}

{% tabs %}
{% tab title="@Before\(\)" %}
```typescript
@Before()
public task(done: CallableFunction): void {
    setTimeout(() => {
        done();
    }, 5000)
}
```
{% endtab %}

{% tab title="@After\(\)" %}
```typescript
@After()
public task(done: CallableFunction): void {
    setTimeout(() => {
        done();
    }, 5000)
}
```
{% endtab %}

{% tab title="@AutloadBefore" %}
```typescript
// Load yourMethod before the bootstrap is started with a available timeout
// about 10000ms
@AutoloadBefore({methodName: 'yourMethod', doneCheckTimeout: 10000})
export class YourFile {

    public yourMethod(done: CallableFunction): void {
        done();
    }

}
```
{% endtab %}

{% tab title="AutloadAfter" %}
```typescript
// Load yourMethod after the bootstrap is complete with a available timeout
// about 10000ms
@AutoloadAfter({methodName: 'yourMethod', doneCheckTimeout: 10000})
export class YourFile {

    public yourMethod(done: CallableFunction): void {
        done();
    }

}
```
{% endtab %}
{% endtabs %}

