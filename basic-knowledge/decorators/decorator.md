---
description: Learn more about the available Server Decorators
---

# Server

## Basic Event Decorator

{% hint style="info" %}
Every basic event decorator has an optional parameter for the eventname to be listened  
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

Dealing with colshapes can be really frustrating. For example, you want to use colshapes in your gamemode and check which entity is inside the colshape or what entity enters or leaves the colshape. That's why we create these decorators. Working with colshapes has never been easier.

{% hint style="info" %}
Only the first parameter is required to identify the triggered colShape. If you want to be more specific, you can set the name, and the triggered entity type.
{% endhint %}

{% tabs %}
{% tab title="@EntityEnterColShape" %}
```typescript
// Would be triggered if colshape is cylinder with name myColShape 
// and the entity is a player
@EntityEnterColShape(ColShapeType.Cylinder, 'myColShape', BaseObjectType.Player)
public testEnter1(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder, myColShape, player');
}

// Would be triggered if colshape is cylinder with name myColShape
@EntityEnterColShape(ColShapeType.Cylinder, 'myColShape')
public testEnter2(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder, myColShape');
}

// Would be triggered if colshape is cylinder
@EntityEnterColShape(ColShapeType.Cylinder)
public testEnter3(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder');
}
```
{% endtab %}

{% tab title="@EntityLeaveColShape" %}
```typescript
// Would be triggered if colshape is cylinder with name myColShape 
// and the entity is a player
@EntityLeaveColShape(ColShapeType.Cylinder, 'myColShape', BaseObjectType.Player)
public testLeave1(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder, myColShape, player');
}

// Would be triggered if colshape is cylinder with name myColShape
@EntityLeaveColShape(ColShapeType.Cylinder, 'myColShape')
public testLeave2(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder, myColShape');
}

// Would be triggered if colshape is cylinder
@EntityLeaveColShape(ColShapeType.Cylinder)
public testLeave3(colshape: Colshape, entity: Entity): void {
  UtilsService.log('cylinder');
}
```
{% endtab %}
{% endtabs %}

