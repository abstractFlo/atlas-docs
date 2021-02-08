---
description: Learn more about the Eventservice
---

# Eventservice

## Introduction

Sending events is a key feature for each gamemode. This service will help you archive this goal.

{% hint style="info" %}
Keep in mind, our eventservice is a wrapper around the alt:V Eventsystem. If you want, you can use the way how alt:V provides it with **alt.emit**, **alt.emitClient** and so on. But in a clean code scenario, it would be better to use the eventService to keep your code clean and structured.
{% endhint %}

## Base Usage

We use the same method signature as alt:V. If you are already familiar with it, then it is very easy for you to use this service.

{% tabs %}
{% tab title="emit" %}
```typescript
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {}

    // Emit event to server
    public yourMethod(): void {
        this.eventService.emit('eventName', 'data1', 'data2', 'data3')
    }

}
```
{% endtab %}

{% tab title="emitClient" %}
```typescript
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {}

    // Send event to all players
    public yourMethod(): void {
        this.eventService.emitClient(null, 'eventName', 'data1', 'data2', 'data3')
    }

    // Send event to specific player
    public yourMethod(): void {
        const player = Player.getByID(1);
        this.eventService.emitClient(player, 'eventName', 'data1', 'data2')
    }

}
```
{% endtab %}
{% endtabs %}

## Special Usage

In our own gamemode, from time to time we need this feature, to send data directly to player GUI without the annoying part for create double code on clientside to only send data to GUI. This method will help you to solve this problem.

{% hint style="info" %}
Internal we use the client as a bridge. There is no magic, only an event that retrieves the data and send it to the GUI itself.
{% endhint %}

{% tabs %}
{% tab title="emitGui" %}
```typescript
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {}

    // Send event to all players gui
    public yourMethod(): void {
        this.eventService.emitGui(null, 'eventName', 'data1', 'data2', 'data3')
    }

    // Send event to specific player gui
    public yourMethod(): void {
        const player = Player.getByID(1);
        this.eventService.emitGui(player, 'eventName', 'data1', 'data2')
    }

}
```
{% endtab %}
{% endtabs %}

## Side Notes

The eventservice also has methods for retrieving events. But if you're using the decorators, there is no need to use them. But for the sake of completeness.

{% tabs %}
{% tab title="on" %}
```typescript
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

{% tab title="onClient" %}
```typescript
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.onClient('event', (player: Player, ...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}

{% tab title="onceClient" %}
```typescript
export class MyComponent {

    constructor(
        private readonly eventService: EventService
    ) {
        this.eventService.onceClient('event', (player: Player, ...args: any[]) => {
            // Do you stuff
        })
    }
}
```
{% endtab %}
{% endtabs %}

