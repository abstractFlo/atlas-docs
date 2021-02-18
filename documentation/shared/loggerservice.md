---
description: Learn more about Logger Service
---

# LoggerService

## Introduction

The LoggerService is a wrapper around the alt:V log features. It's created to fit the Object Oriented needs.  
We have improve some parts for logging to help you out.

{% hint style="success" %}
LoggerService is available on both sides
{% endhint %}

### Available Methods

This section describe the available methods that you can use.

#### info\(...messages: any\[\]\)

Simple log one or messages

```typescript
@singleton()
export class YourClass {

    constructor(
        private readonly loggerService: LoggerService
    ) {}
    
    public yourMethod(): void {
        this.loggerService.info('Your message')
    }
}
```

#### warning\(...messages: any\[\]\)

Simple log one or messages as warning

```typescript
@singleton()
export class YourClass {

    constructor(
        private readonly loggerService: LoggerService
    ) {}
    
    public yourMethod(): void {
        this.loggerService.warning('Your message')
    }
}
```

#### error\(...messages: any\[\]\)

Simple log one or messages as error

```typescript
@singleton()
export class YourClass {

    constructor(
        private readonly loggerService: LoggerService
    ) {}
    
    public yourMethod(): void {
        this.loggerService.error('Your message')
    }
}
```

#### starting\(message: string\)

Simple log a message formated as starting

```typescript
@singleton()
export class YourClass {

    constructor(
        private readonly loggerService: LoggerService
    ) {}
    
    public yourMethod(): void {
        this.loggerService.starting('DatabaseService')
    }
}
```

![Console printed message](../../.gitbook/assets/loggerservice_starting.png)

#### started\(message: string\)

Simple log a message formated as started

```typescript
@singleton()
export class YourClass {

    constructor(
        private readonly loggerService: LoggerService
    ) {}
    
    public yourMethod(): void {
        this.loggerService.started('DatabaseService')
    }
}
```

![Console printed message](../../.gitbook/assets/loggerservice_started.png)

