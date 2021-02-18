---
description: Learn more about the available Server Decorators
---

# Server

## Basic Event Decorator

{% hint style="info" %}
Every basic event decorator can have a parameter for the eventname to be listen  
If the parameter is not provided, the name of the decorated method will be used as the eventname.
{% endhint %}

{% tabs %}
{% tab title="@OnClient" %}
```typescript
// Please notice, the first parameter inside the method is everytime
// the player they send the event to server

@OnClient('customEventname')
public doFancyStuff(player: Player): void {
  // customEventname is the eventname for listen
}

@OnClient()
public youAwesomMethod(player: Player): void {
  // yourAwesomeMethod is the eventname for listen
}
```
{% endtab %}

{% tab title="@OnceClient" %}
```typescript
// Please notice, the first parameter inside the method is everytime
// the player they send the event to server

@OnceClient('customEventname')
public doFancyStuff(player: Player): void {
  // customEventname is the eventname for listen
}

@OnceClient()
public youAwesomMethod(player: Player): void {
  // yourAwesomeMethod is the eventname for listen
}
```
{% endtab %}
{% endtabs %}

## Colshape Decorator

If you want using colshapes in your gamemode, that can be frustrate you every time if you want to check which entity is in, if any entity enter or leave the colshape and many more usecases. That's why we create this decorators. Working with colshapes was never easier before.

 

