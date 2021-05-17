---
description: Learn more about the Eventservice
---

# Event Service

## Introduction

Sending events is a key feature for each gamemode. This service will help you archive this goal.

{% hint style="info" %}
Keep in mind, our event service is a wrapper around the alt:V event system. If you want, you can use it the way alt:V provides it with **alt.emit**, **alt.emitServer** and so on. In a clean code scenario, it would be better to use the eventService to keep your code clean and structured.
{% endhint %}

## Base Usage

We use the same method signature as alt:V. If you are already familiar with it, then it will be very easy for you to use this service.

{% tabs %}
{% tab title="emit" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {}

    // Emit event to client
    public yourMethod(): void {
        this.eventService.emit('eventName', 'data1', 'data2', 'data3')
    }

}
```
{% endtab %}

{% tab title="emitServer" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {}

    // Send event to server
    public yourMethod(): void {
        this.eventService.emitServer('eventName', 'data1', 'data2', 'data3')
    }


}
```
{% endtab %}
{% endtabs %}

## Side Notes

The event service also has methods for retrieving events. If you're using the decorators, there is no need to use them. For the sake of completeness, they're included.

{% tabs %}
{% tab title="on" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.on('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="once" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.once('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="onServer" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.onServer('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="onceServer" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.onceServer('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="onGui" %}
```typescript
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.onGui('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}
{% endtabs %}

### Net to know

We provide some other methods for **offServer** and **off** Eventlistener.

{% tabs %}
{% tab title="offServer" %}
```typescript
// Keep in mind, offServer need the same function context as onServer to remove
// the eventListener
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.offServer('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="off" %}
```typescript
// Keep in mind, offServer need the same function context as on to remove
// the eventListener
@Component()
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.off('event', (...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}
{% endtabs %}

