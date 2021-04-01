---
description: 'Learn more about our custom alt:V Internal Extends'
---

# Extends

## Introduction

We created some extends for alt:V Internals. This will help you to do some basic stuff without the boilerplate. Extending means that we create new methods and properties on internal alt:V classes

{% hint style="success" %}
Extending classes in TypeScript is done by overriding the prototype of a class like **Player** or **Vehicle**.
{% endhint %}

## Our Extends

### alt.Player

We created some methods that are available on each Player Instance.

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

{% tab title="Gui Interaction" %}
```typescript
// All this methods available on each player instance

// RouteTo routeName inside webview
player.guiRouteTo(routeName: string, ...args: any[])

// Focus webview
player.guiFocus();

// Unfocus webview
player.guiUnfocus();

// Show cursor
player.guiShowCursor();

// Remove cursor
player.guiRemoveCursor();

// Remove all cursors
player.guiRemoveAllCursors();

```
{% endtab %}
{% endtabs %}

### alt.Colshape

The colshape has a new property to attach a name to it.

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

