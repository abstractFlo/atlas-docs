---
description: Learn more about our DI-Container
---

# DI-Container

## Introduction

Dependency Injection is the way to go, if you want to create clean and structured code. It helps you to resolve required dependencies for your classes. Keep in mind, dependency injection only works inside the constructor.

We're using [tsyringe](https://github.com/microsoft/tsyringe) from Microsoft as our di-container solution. It fits all our needs and is really lightweight.

{% hint style="info" %}
If you want more informations about tsyringe, please read the documentation on GitHub.
{% endhint %}

{% hint style="danger" %}
We will not go into how Dependency Injection works, but only how we use it.
{% endhint %}

## tsyringe Decorators

### @singleton

If you want to have the same instance of the class every time, use the **@singleton** decorator.

```typescript
@singleton()
export class MyClass {}

// resolve class from container
const resolve1 = container.resolve(MyClass);
const resolve2 = container.resolve(MyClass);

resolve1 === resolve2;
```

### @injectable

If you want to have a new instance of the class every time, use the **@injectable** decorator.

```typescript
@injectable()
export class MyClass {}

// resolve class from container
const resolve1 = container.resolve(MyClass);
const resolve2 = container.resolve(MyClass);

resolve1 !== resolve2;
```

## Custom DI Decorators

We created some custom decorators to interact with the di-container. They are needed for internal work. The entire framework uses many shared classes and helpers on both sides. The decorators help us to connect the server and client with the shared classes.

{% hint style="danger" %}
Every custom decorator must be declared after the tsyringe decorator
{% endhint %}

### @Module

The **@Module** decorator is the most used custom decorator. This decorator handles all your other module imports and components. It combines the autoloading feature and **@StringResolver**

{% tabs %}
{% tab title="index.ts" %}
```typescript
container.resolve(FirstModule)
```
{% endtab %}

{% tab title="first.module.ts" %}
```typescript
@Module({
    imports: [SecondModule],
    components: [FirstComponent]
})
export class FirstModule {}
```
{% endtab %}

{% tab title="second.module.ts" %}
```typescript
@Module({
    components: [SecondComponent]
})
export class SecondModule{}
```
{% endtab %}

{% tab title="first.component.ts" %}
```typescript
@Component()
export class FirstComponent{}
```
{% endtab %}

{% tab title="second.component.ts" %}
```typescript
@Component()
export class SecondComponent{}
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
The example above shows you the magic. Only one line of code for import. Resolve the module from container, autoload all other declared modules and components. This keep your code clean and good-looking.
{% endhint %}

### @Component

This **@Component** decorator is a simple decorator for readable code. It is the same as [@singleton](di-container.md#singleton) but fits the framework read language better

{% tabs %}
{% tab title="your.component.ts" %}
```typescript
@Component()
export class YourComponent{}
```
{% endtab %}
{% endtabs %}

### @StringResolver

The **@StringResolver** registers the decorated class as a string InjectionToken inside the DI-container.  
This is needed if you want to resolve a class based on the string constructor name

{% hint style="info" %}
This decorator is only needed, if you get errors on resolving your class. There are not many usecases.
{% endhint %}

```typescript
@StringResolver
export class MyClass {}

// resolve class from container
const resolve1 = container.resolve('MyClass');

resolve1 instanceof MyClass === true
```

