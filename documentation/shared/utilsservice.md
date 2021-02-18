---
description: Learn more about the Utils Service
---

# UtilsService

## Introduction

It can happen that you always need the same implementation of a method in different scenarios. This can be for example if you want to build classes that you want to use on client and server side.  
There are a few helpers built in to help you keep your performance in check.

{% hint style="success" %}
UtilsService can be used on both sides
{% endhint %}

## Available Methods

This sections covered all available methods included in UtilsService

#### setTimeout\(listener: CallableFunction, duration: number\)

Create a simple timeout. This method clear the timeout after finished.

```typescript
// Create a timeout which is processed after 1000ms
const timeout = UtilsService.setTimeout(() => {
    // Do whatever you want
}, 1000)
```

#### setInterval\(listener: CallableFunction, milliseconds: number\)

Create a simple interval.

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

Create a simple everyTick.

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

Create interval they is autocleared after defined duration.

```typescript
// Create a autoClearInterval which is processed every 1000ms 
// and cleared after 5000ms
UtilsService.autoClearInterval(() => {
    // Do whatever you want
}, 1000, 5000)
```

#### nextTick\(listener: CallableFunction\)

Create a simple nextTick.

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

Clear given interval number

```typescript
// Create a interval which is processed every 1000ms
const interval = UtilsService.setInterval(() => {
    // Do whatever you want
}, 1000)

// Clear the before created interval
UtilsService.clearInterval(interval)
```

#### clearTimeout\(timeout: number\)

Clear given timeout number

```typescript
// Create a timeout which is processed after 1000ms
const timeout = UtilsService.setInterval(() => {
    // Clear the timeout
    UtilsService.clearTimeout(timeout)
}, 1000)
```

#### clearNextTick\(tick: number\)

Clear given nextTick number

```typescript
// Create a nextTick which is processed nextTick ;)
const nextTick = UtilsService.nextTick(() => {
    // Do whatever you want
})

UtilsService.clearNextTick(nextTick)
```

#### clearEveryTick\(tick: number\)

Clear given everyTick number

```typescript
// Create a everyTick which is processed everyTick ;)
const everyTick = UtilsService.everyTick(() => {
    // Do whatever you want
})

UtilsService.clearEveryTick(everyTick)
```

#### log\(...messages: any\[\]\)

Log one ore more message to console

```typescript
UtilsService.log('your message')
```

#### logWarning\(...messages: any\[\]\)

Log one ore more message to console as warning

```typescript
UtilsService.logWarning('your message')
```

#### logError\(...messages: any\[\]\)

Log one ore more message to console as error

```typescript
UtilsService.logError('your message')
```

#### logLoaded\(...messages: any\[\]\)

Log one ore more message to console formated as loaded

```typescript
UtilsService.logLoaded('your message')
// UtilsService.log(`Loaded ~lg~${message}~w~`))
```

#### logUnloaded\(...messages: any\[\]\)

Log one ore more message to console formated as unloaded

```typescript
UtilsService.logUnloaded('your message')
// UtilsService.log(`Unloaded ~lg~${message}~w~`))
```

### Event Helpers

{% hint style="danger" %}
All event methods are only called in his specific context - Client/Server. They can't communitcate with the other side.
{% endhint %}

#### eventOn\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Listen for event

```typescript
UtilsService.eventOn('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventOnce\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Listen for event once time

```typescript
UtilsService.eventOnce('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventOff\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

Remove listener for event

{% hint style="info" %}
Keep in mind the listener must be the same context as the [on](utilsservice.md#eventon-eventname-string-listener-args-any-greater-than-void) listener
{% endhint %}

```typescript
UtilsService.eventOff('eventName', (...args: any []) =>  {
    // Do whatever you want
})
```

#### eventEmit\(eventName: string, ...args: any\[\]\)

Emit event

```typescript
UtilsService.eventEmit('eventName', 'arg1', 'arg2')
```

