---
description: Learn more about the available Decorators for both sides
---

# Both Sides

## Module Decorator

The module decorator provides a simple way to load your components and other modules

{% hint style="info" %}
It is a good practice to split out your gamemode in modules and components to keep your file structured, clean and readable
{% endhint %}

```typescript
@Module({
    imports: [SecondModule, ThirdModule],
    components: [FirstComponent, SecondComponent, ThirdComponent]
})
export class YourModule {}
```

## Basic Event Decorator

{% hint style="info" %}
Every basic event decorator has an optional parameter for the eventname  
If the eventname is not provided within the parameter, the name of the decorated method will be used as the eventname.
{% endhint %}

{% tabs %}
{% tab title="@On" %}
```typescript
@On()
public playerDisconnect(player: Player): void {
  // playerDisconnect is the eventname for listen
}

@On('yourAwesomeEvent')
public doSomeMagicStuff(): void {
  // yourAwesomeEventis the eventname for listen
}
```
{% endtab %}

{% tab title="@Once" %}
```typescript
@Once()
public yourAwesomeEvent(player: Player): void {
  // yourAwesomeEvent is the eventname for listen
}

@Once('yourAwesomeEvent')
public doSomeMagicStuff(): void {
  // yourAwesomeEventis the eventname for listen
}
```
{% endtab %}
{% endtabs %}

## Command Decorator

Creating sever console commands has never been easier. You don't need to create them or listen to specific events. Only this decorator is needed.

{% hint style="warning" %}
Avoid using multiple commands with same name. Only the first registered command will be handled.
{% endhint %}

### Basic Usage

{% hint style="info" %}
The first parameter is optional. If the parameter is not provided, the method name will be used as command name.
{% endhint %}

```typescript
@Cmd('hello')
public world(): void {
    // called if you write /hello inside your server console
}

@Cmd()
public world(): void {
    // called if you write /world inside your server console
}
```

### Example

This example shows the simple usage of this awesome decorator. The way they work is the same on both sides. This example shows a simple vehicle create command on server side.

```typescript
@Cmd('veh')
public createVehicle(name: string): void {
    const player: Player = alt.Player.local;
    const pos = new alt.Vector3({...player.pos}).add(2,2,0);
    const rot = new alt.Vector3({...player.rot}).toDegrees();
    const vehicleHash= alt.hash(name);

    const vehicle = new alt.Vehicle(
        vehicleHash, 
        pos.x, 
        pos.y, 
        pos.z, 
        rot.x, 
        rot.y, 
        rot.z
    );

    vehicle.numberPlateText = 'MyCar';
}
```

