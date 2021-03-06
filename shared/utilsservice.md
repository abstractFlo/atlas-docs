---
description: Learn more about the Utils Service
---

# UtilsService

## Introduction

Sometimes you always need the same implementation of a method in different scenarios. For instance, you want to build classes and use them on client and server side.

There are a few helpers built in that should help you to keep your performance in check.

{% hint style="success" %}
UtilsService can be used on both sides
{% endhint %}

## Available Methods

This section covers all available methods included in UtilsService

#### setTimeout\(listener: CallableFunction, duration: number\)

This method creates a simple timeout and automatically clears it once the timeout is finished.

```typescript
// Create a timeout which is processed after 1000ms
const timeout = UtilsService.setTimeout(() => {
    // Do whatever you want
}, 1000)
```

#### setInterval\(listener: CallableFunction, milliseconds: number\)

This method creates a simple interval.

{% hint style="danger" %}
Please do not forget to [clear](utilsservice.md#clearinterval-interval-number) the interval if you do not need it anymore
{% endhint %}

```typescript
// Create a interval which is processed every 1000ms
const interval = UtilsService.setInterval(() => {
    // Do whatever you want
}, 1000)
```

#### everyTick\(listener: CallableFunction\)

This method creates a simple everyTick.

{% hint style="danger" %}
Please do not forget to [clear](utilsservice.md#cleareverytick-tick-number) the everyTick if you do not need it anymore
{% endhint %}

```typescript
// Create a everyTick which is processed everyTick ;)
const everyTick = UtilsService.everyTick(() => {
    // Do whatever you want
})
```

#### autoClearInterval\(listener: CallableFunction, milliseconds: number, intervalDuration: number\)

This method creates an interval that will automatically be cleared after the defined duration.

```typescript
// Create a autoClearInterval which is processed every 1000ms 
// and cleared after 5000ms
UtilsService.autoClearInterval(() => {
    // Do whatever you want
}, 1000, 5000)
```

#### nextTick\(listener: CallableFunction\)

This method creates a simple nextTick.

{% hint style="danger" %}
Please do not forget to [clear](utilsservice.md#clearnexttick-tick-number) the nextTick if you do not need it anymore
{% endhint %}

```typescript
// Create a nextTick which is processed nextTick ;)
const nextTick = UtilsService.nextTick(() => {
    // Do whatever you want
})
```

#### clearInterval\(interval: number\)

This method clears an interval based on the intervalId returned by the setInterval method.

```typescript
// Create a interval which is processed every 1000ms
const interval = UtilsService.setInterval(() => {
    // Do whatever you want
}, 1000)

// Clear the before created interval
UtilsService.clearInterval(interval)
```

#### clearTimeout\(timeout: number\)

This method clears a timeout based on the timeoutId.

```typescript
// Create a timeout which is processed after 1000ms
const timeout = UtilsService.setTimeout(() => {
    // Clear the timeout
    UtilsService.clearTimeout(timeout)
}, 1000)
```

#### clearNextTick\(tick: number\)

This method clears a nextTick based on the nextTickId

```typescript
// Create a nextTick which is processed nextTick ;)
const nextTick = UtilsService.nextTick(() => {
    // Do whatever you want
})

UtilsService.clearNextTick(nextTick)
```

#### clearEveryTick\(tick: number\)

This method clears a everyTick based on the id returned by the everyTick\(\) method

```typescript
// Create a everyTick which is processed everyTick ;)
const everyTick = UtilsService.everyTick(() => {
    // Do whatever you want
})

UtilsService.clearEveryTick(everyTick)
```

#### log\(...messages: any\[\]\)

This method logs a message

```typescript
UtilsService.log('your message')
```

#### logWarning\(...messages: any\[\]\)

This method logs a message as a warning

```typescript
UtilsService.logWarning('your message')
```

#### logError\(...messages: any\[\]\)

This method logs a message as an error

```typescript
UtilsService.logError('your message')
```

#### logLoaded\(...messages: any\[\]\)

This method logs a message formatted as loaded

```typescript
UtilsService.logLoaded('your message')
// UtilsService.log(`Loaded ~lg~${message}~w~`))
```

#### logUnloaded\(...messages: any\[\]\)

This method logs a message formatted as unloaded

```typescript
UtilsService.logUnloaded('your message')
// UtilsService.log(`Unloaded ~lg~${message}~w~`))
```

#### logRegisteredHandlers\(name: string, length: number\)

This method is used internal

```typescript
UtilsService.logRegisteredHandlersloaded('YourService', 10)
// UtilsService.log(`Registered all handlers for ~lg~${name}~w~ - ~y~[${length}]~w~`);
```

#### isProduction\(toggle: boolean = false\)

Setup the production mode only needed clientside, serverside is setup by .env variable

```typescript
UtilsService.isProduction(true)
```

#### setCommandPrefix\(prefix: string\)

Change the prefix for commands, base is `/` 

```typescript
UtilsService.setCommandPrefix('!')
```

### Event Helpers

{% hint style="danger" %}
All event methods are only called in its specific context - Client/Server. They can't communicate with the other side.
{% endhint %}

#### eventOn\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Listen for an event

```typescript
UtilsService.eventOn('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventOnce\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Listen for an event once

```typescript
UtilsService.eventOnce('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventOff\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Removes the listener of an event

{% hint style="info" %}
Keep in mind that the listener has to be in the same context as the [on](utilsservice.md#eventon-eventname-string-listener-args-any-greater-than-void) listener
{% endhint %}

```typescript
UtilsService.eventOff('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventEmit\(eventName: string, ...args: any\[\]\)

Emits an event

```typescript
UtilsService.eventEmit('eventName', 'arg1', 'arg2')
```

