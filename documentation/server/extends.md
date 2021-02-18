---
description: 'Learn more about our custom alt:V Internal Extends'
---

# Extends

## Introduction

We have create some extends for alt:V Internals. This will help you do some basic stuff without the boilerplate. Extends means that we create new methods and properties on Internal alt:V Classes

{% hint style="success" %}
Extending classes in TypeScript, is done by override the prototype for a class like **Player** or **Vehicle**.
{% endhint %}

## Our Extends

### alt.Player

We have create 2 methods thats available on each Player Instance.

{% tabs %}
{% tab title="emit" %}
```typescript
// This is available on each player instance
// With this, you can emit event direct to player without
// needed player param
player.emit('yourEventName', 'arg1', 'arg2','arg3')
```
{% endtab %}

{% tab title="emitGui" %}
```typescript
// This is available on each player instance
// With this, you can emit event direct to player gui
// this use the client as a bridge
player.emitGui('yourEventName', 'arg1', 'arg2','arg3')
```
{% endtab %}
{% endtabs %}

### alt.Colshape

The Colshape has a new property for add a name to it.

{% tabs %}
{% tab title="name" %}
```typescript
// If you want to interact with the colshape decorators
// set name to your ColShape
const colShape = new ColshapeCylinder(0,0,0,3,2);
colShape.name = 'yourColshape';
```
{% endtab %}
{% endtabs %}

