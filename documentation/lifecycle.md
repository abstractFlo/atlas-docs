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
const loader = container.resolve(LoaderService);

loader
    .bootstrap(ServerModule)
    .afterComplete(() => {
        // This method is called, if the lifecycle finished
    });
```
{% endtab %}

{% tab title="Client" %}
```typescript
const loader = container.resolve(LoaderService);

loader
    .bootstrap(ClientModule)
    .afterComplete(() => {
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

The done callback has a duration within **5000ms**. If your task not over in this time, the console would show you some informations about.

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

{% tab title="@AfterBootstrap\(\)" %}
```typescript
@AfterBootstrap()
public task(done: CallableFunction): void {
    setTimeout(() => {
        done();
    }, 5000)
}
```
{% endtab %}

{% tab title="@Before\(\) with higher duration time" %}
```typescript
// The done callback must be called within 25000ms
@Before(25000)
public heavyTask(done: CallableFunction): void {
    setTimeout(() => {
        done();
    }, 20000)
}
```
{% endtab %}
{% endtabs %}

## Example

### NEU ÜBERLEGEN WAS WIR HIER MACHEN!!!!

```typescript
class YourModule {

    private loadedVehicles: number = 0;
    private loadedPedPositions: number = 0;

    // Push init the database as first step to queue
    @Before
    public initDatabase(done: CallableFunction): void {
        this.initYourDatabase().then(() => done());
    }

    // push load all server vehicles to queue after database is initialized
    @After
    public loadServerVehicles(done: CallableFunction): void {
        this.loadAllServerVehicles().then((loadedVehicles: number) => {
            this.loadedVehicles = loadedVehicles;
            done();
        });
    }

    // Push load all ped positions to queue after the vehicles loaded
    @After
    public loadPedPositions(done: CallableFunction): void {
        this.loadAllPedPositions().then((pedPositions: number) => {
            this.loadedPedPositions = pedPositions;
            done();
        });
    }

    // Last step, print out the loaded vehiles and ped positions
    @AfterBootstrap
    public printLoadedVehicles(done: CallableFunction): void {
        UtilsService.log(`Successfully loaded ${this.loadedVehicles}`);
        UtilsService.log(`Successfully loaded ${this.loadedPedPositions}`);
        done();
    }

}
```

{% hint style="info" %}
You can push as much as you want on each queue step. You should only keep in mind, that any step would be call if the before step is finished and in this order:  **@Before** -&gt; **@After** -&gt; **@AfterBootstrap**
{% endhint %}

